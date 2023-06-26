---
title: 使用 Kubernetes 部署 Streamlit
slug: /knowledge-base/tutorials/deploy/kubernetes
---

# 使用 Kubernetes 部署 Streamlit

## 简介

您有一个令人惊叹的应用程序，并且想要与其他人共享，该怎么办？您有几个选择。首先，您想在哪里运行您的 Streamlit 应用程序，以及如何访问它？

- **在您的企业网络上** - 大多数企业网络对外部世界是封闭的。通常您需要使用VPN连接到企业网络并访问其中的资源。出于安全考虑，您可以在企业网络中的服务器上运行您的Streamlit应用程序，以确保只有公司内部的人员可以访问它。
- **云端部署** - 如果您想从公司网络之外访问您的Streamlit应用程序，或者与家庭网络或笔记本之外的人分享您的应用程序，您可以选择云端部署选项。在这种情况下，具体步骤将取决于您选择的托管提供商。我们有来自Heroku、AWS和其他提供商的[社区提交的指南](/knowledge-base/deploy/deploy-streamlit-heroku-aws-google-cloud)。

无论您决定将应用部署在何处，您首先需要将其容器化。本指南将指导您使用Kubernetes部署应用。如果您更喜欢Docker，请参阅[使用Docker部署Streamlit](/knowledge-base/tutorials/deploy/docker)。

## 先决条件

1. [安装Docker Engine](#install-docker-engine)
2. [安装gcloud CLI](#install-the-gcloud-cli)

### 安装Docker Engine

如果您尚未安装Docker，请在服务器上进行安装。Docker提供了来自许多Linux发行版的.deb和.rpm软件包，包括：

- [Debian](https://docs.docker.com/engine/install/debian/)
- [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

通过运行`hello-world` Docker镜像来验证Docker Engine是否正确安装：

```bash
sudo docker run hello-world
```

<Tip>

按照Docker官方的[Linux安装后步骤](https://docs.docker.com/engine/install/linux-postinstall/)，以非root用户身份运行Docker，这样您就无需在`docker`命令前加上`sudo`。

</Tip>

### 安装gcloud CLI

在本指南中，我们将使用Kubernetes编排Docker容器，并将Docker镜像托管在Google Container Registry（GCR）上。由于GCR是一个由Google支持的Docker注册表，我们需要将[`gcloud`](https://cloud.google.com/sdk/gcloud/reference)注册为Docker凭据助手。

请按照官方文档中的说明[安装gcloud CLI](https://cloud.google.com/sdk/docs/install)并进行初始化。

## 创建一个Docker容器

我们需要创建一个包含所有依赖项和应用程序代码的Docker容器。下面您可以看到入口点（即容器启动时运行的命令）和Dockerfile的定义。

### 创建入口点脚本

创建一个名为`run.sh`的脚本，内容如下：

```bash
#!/bin/bash

APP_PID=
stopRunningProcess() {
    # Based on https://linuxconfig.org/how-to-propagate-a-signal-to-child-processes-from-a-bash-script
    if test ! "${APP_PID}" = '' && ps -p ${APP_PID} > /dev/null ; then
       > /proc/1/fd/1 echo "Stopping ${COMMAND_PATH} which is running with process ID ${APP_PID}"

       kill -TERM ${APP_PID}
       > /proc/1/fd/1 echo "Waiting for ${COMMAND_PATH} to process SIGTERM signal"

        wait ${APP_PID}
        > /proc/1/fd/1 echo "All processes have stopped running"
    else
        > /proc/1/fd/1 echo "${COMMAND_PATH} was not started when the signal was sent or it has already been stopped"
    fi
}

trap stopRunningProcess EXIT TERM

source ${VIRTUAL_ENV}/bin/activate

streamlit run ${HOME}/app/streamlit_app.py &
APP_ID=${!}

wait ${APP_ID}
```

### 创建一个 Dockerfile

Docker通过读取`Dockerfile`中的指令来构建镜像。`Dockerfile`是一个文本文档，包含了用户可以在命令行上调用的所有命令，用于组装镜像。在[Dockerfile参考文档](https://docs.docker.com/engine/reference/builder/)中可以了解更多信息。`docker build`命令通过`Dockerfile`构建一个镜像。`docker run`命令首先在指定的镜像上创建一个容器，然后使用指定的命令启动它。

这是一个示例的 `Dockerfile`，您可以将其添加到您的目录根目录中。

```docker
FROM python:3.7-slim

RUN groupadd --gid 1000 appuser \
    && useradd --uid 1000 --gid 1000 -ms /bin/bash appuser

RUN pip3 install --no-cache-dir --upgrade \
    pip \
    virtualenv

RUN apt-get update && apt-get install -y \
    build-essential \
    software-properties-common \
    git

USER appuser
WORKDIR /home/appuser

RUN git clone https://github.com/streamlit/streamlit-example.git app

ENV VIRTUAL_ENV=/home/appuser/venv
RUN virtualenv ${VIRTUAL_ENV}
RUN . ${VIRTUAL_ENV}/bin/activate && pip install -r app/requirements.txt

EXPOSE 8501

COPY run.sh /home/appuser
ENTRYPOINT ["./run.sh"]
```

<Important>

如在[开发流程](/library/get-started/main-concepts#development-flow)中所述，对于Streamlit版本1.10.0及更高版本，Streamlit应用程序不能在Linux发行版的根目录中运行。您的主要脚本应该存在于根目录以外的目录中。如果您尝试从根目录运行Streamlit应用程序，Streamlit将抛出`FileNotFoundError: [Errno 2] No such file or directory`错误。有关更多信息，请参阅GitHub问题[#5239](https://github.com/streamlit/streamlit/issues/5239)。

如果您使用的是Streamlit版本1.10.0或更高版本，则必须将`WORKDIR`设置为根目录之外的目录。例如，您可以将`WORKDIR`设置为`/home/appuser`，如上面的示例`Dockerfile`所示。
</重要提示>

### 构建Docker镜像

将上述文件（`run.sh`和`Dockerfile`）放在同一个文件夹中，并构建Docker镜像：

```docker
docker build --platform linux/amd64 -t gcr.io/$GCP_PROJECT_ID/k8s-streamlit:test .
```

<重要>

请将上述命令中的`$GCP_PROJECT_ID`替换为您的Google Cloud项目的名称。

</重要>

### 将Docker镜像上传到容器注册表

下一步是将Docker镜像上传到容器注册表。在本示例中，我们将使用[Google容器注册表（GCR）](https://cloud.google.com/container-registry)。首先启用容器注册表API。登录Google Cloud，并导航到您的项目的**容器注册表**，然后点击**启用**。

现在我们可以从上一步构建Docker镜像并将其推送到我们项目的GCR。请确保在docker push命令中将`$GCP_PROJECT_ID`替换为您的项目名称。

```bash
gcloud auth configure-docker
docker push gcr.io/$GCP_PROJECT_ID/k8s-streamlit:test
```

## 创建一个Kubernetes部署

在这一步中，您需要以下条件：

- 运行中的Kubernetes服务
- 可以生成TLS证书的自定义域名
- 可以将自定义域名配置为将其指向应用程序IP的DNS服务

由于镜像已在上一步中上传到容器注册表中，我们可以使用以下配置在Kubernetes中运行它。

### 安装和运行Kubernetes

确保您的[Kubernetes客户端](https://kubernetes.io/docs/tasks/tools/#kubectl) `kubectl`已安装并在您的计算机上运行。

### 配置Google OAuth客户端和oauth2-proxy

要配置Google OAuth Client，请参阅[Google Auth Provider](https://oauth2-proxy.github.io/oauth2-proxy/docs/configuration/oauth_provider#google-auth-provider)。配置oauth2-proxy以使用所需的[OAuth Provider Configuration](https://oauth2-proxy.github.io/oauth2-proxy/docs/configuration/oauth_provider)，并更新配置映射中的oauth2-proxy配置。

下面的配置包含一个ouath2-proxy sidecar容器，用于处理与Google的身份验证。您可以从[oauth2-proxy存储库](https://github.com/oauth2-proxy/oauth2-proxy)了解更多信息。

### 创建Kubernetes配置文件

创建一个名为`k8s-streamlit.yaml`的[YAML](https://yaml.org/) [配置文件](https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/#organizing-resource-configurations)：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: streamlit-configmap
data:
  oauth2-proxy.cfg: |-
    http_address = "0.0.0.0:4180"
    upstreams = ["http://127.0.0.1:8501/"]
    email_domains = ["*"]
    client_id = "<GOOGLE_CLIENT_ID>"
    client_secret = "<GOOGLE_CLIENT_SECRET>"
    cookie_secret = "<16, 24, or 32 bytes>"
    redirect_url = <REDIRECT_URL>

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: streamlit-deployment
  labels:
    app: streamlit
spec:
  replicas: 1
  selector:
    matchLabels:
      app: streamlit
  template:
    metadata:
      labels:
        app: streamlit
    spec:
      containers:
        - name: oauth2-proxy
          image: quay.io/oauth2-proxy/oauth2-proxy:v7.2.0
          args: ["--config", "/etc/oauth2-proxy/oauth2-proxy.cfg"]
          ports:
            - containerPort: 4180
          livenessProbe:
            httpGet:
              path: /ping
              port: 4180
              scheme: HTTP
          readinessProbe:
            httpGet:
              path: /ping
              port: 4180
              scheme: HTTP
          volumeMounts:
            - mountPath: "/etc/oauth2-proxy"
              name: oauth2-config
        - name: streamlit
          image: gcr.io/GCP_PROJECT_ID/k8s-streamlit:test
          imagePullPolicy: Always
          ports:
            - containerPort: 8501
          livenessProbe:
            httpGet:
              path: /_stcore/health
              port: 8501
              scheme: HTTP
            timeoutSeconds: 1
          readinessProbe:
            httpGet:
              path: /_stcore/health
              port: 8501
              scheme: HTTP
            timeoutSeconds: 1
          resources:
            limits:
              cpu: 1
              memory: 2Gi
            requests:
              cpu: 100m
              memory: 745Mi
      volumes:
        - name: oauth2-config
          configMap:
            name: streamlit-configmap

---
apiVersion: v1
kind: Service
metadata:
  name: streamlit-service
spec:
  type: LoadBalancer
  selector:
    app: streamlit
  ports:
    - name: streamlit-port
      protocol: TCP
      port: 80
      targetPort: 4180
```

<重要>

尽管上述配置可以逐字复制，但您仍需要自行配置`oauth2-proxy`并使用正确的`GOOGLE_CLIENT_ID`、`GOOGLE_CLIENT_ID`、`GCP_PROJECT_ID`和`REDIRECT_URL`。

</重要>

现在，使用[`kubectl create`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create)命令在Kubernetes中从文件创建配置。

```bash
kubctl create -f k8s-streamlit.yaml
```

### 设置TLS支持

由于您正在使用Google身份验证，您需要设置TLS支持。请在[TLS配置](https://oauth2-proxy.github.io/oauth2-proxy/docs/configuration/tls)中了解如何设置。

### 验证部署

一旦部署和服务创建完成，我们需要等待几分钟以使公共IP地址可用。我们可以通过运行以下命令来检查是否准备就绪：

```bash
kubectl get service streamlit-service -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

在分配了公共IP之后，您需要在DNS服务中配置一个`A记录`，将其指向上述IP地址。
