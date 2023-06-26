---
title: 使用Docker部署Streamlit应用
slug: /knowledge-base/tutorials/deploy/docker
---

# 使用Docker部署Streamlit应用

## 简介

您有一个了不起的应用程序，想要与其他人分享，该怎么办？您有几个选择。首先，您想在哪里运行Streamlit应用程序？您如何访问它？

- **在您的企业网络上** - 大多数企业网络对外部世界关闭。通常您会使用VPN登录到企业网络并访问资源。出于安全考虑，您可以在企业网络中的服务器上运行您的Streamlit应用程序，以确保只有公司内部的人员可以访问它。
- **云端部署** - 如果您想从公司网络之外访问Streamlit应用程序，或者与家庭网络或笔记本之外的人共享应用程序，您可以选择这个选项。在这种情况下，取决于您的托管提供商。我们有来自Heroku、AWS和其他提供商的[社区提交的指南](/knowledge-base/deploy/deploy-streamlit-heroku-aws-google-cloud)。

无论您决定在哪里部署您的应用程序，您首先需要将其容器化。本指南将引导您使用Docker来部署您的应用程序。如果您更喜欢使用Kubernetes，请参阅[使用Kubernetes部署Streamlit](/knowledge-base/tutorials/deploy/kubernetes)。

## 先决条件

1. [安装Docker Engine](#安装Docker Engine)
2. [检查网络端口可访问性](#检查网络端口可访问性)

### 安装Docker Engine

如果尚未安装，请在服务器上安装 [Docker](https://docs.docker.com/engine/install/#server)。Docker 提供了许多 Linux 发行版的 `.deb` 和 `.rpm` 软件包，包括：

- [Debian](https://docs.docker.com/engine/install/debian/)
- [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

通过运行 `hello-world` Docker 镜像来验证 Docker Engine 是否已正确安装：

```bash
sudo docker run hello-world
```

<Tip>

按照Docker官方提供的[Linux安装后的步骤](https://docs.docker.com/engine/install/linux-postinstall/)来以非root用户身份运行Docker，这样你就不需要在`docker`命令前加上`sudo`了。

</Tip>

### 检查网络端口的可访问性

由于您和您的用户都在公司的 VPN 后面，您需要确保所有人都能访问某个特定的网络端口。假设端口为 `8501`，因为它是 Streamlit 默认使用的端口。请联系您的 IT 团队，并请求为您和您的用户开放 `8501` 端口的访问权限。

## 创建 Dockerfile

Docker通过读取来自`Dockerfile`的指令来构建镜像。`Dockerfile`是一个文本文档，包含用户可以在命令行上调用的所有命令，用于组装镜像。详细了解请参阅[Dockerfile参考](https://docs.docker.com/engine/reference/builder/)。`docker build`命令通过`Dockerfile`构建一个镜像。`docker run`命令首先在指定的镜像上创建一个容器，然后使用指定的命令启动它。

以下是一个示例的 `Dockerfile`，您可以将其添加到根目录中，例如在 `/app/` 中。

```docker
# app/Dockerfile

FROM python:3.9-slim

WORKDIR /app

RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    software-properties-common \
    git \
    && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/streamlit/streamlit-example.git .

RUN pip3 install -r requirements.txt

EXPOSE 8501

HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health

ENTRYPOINT ["streamlit", "run", "streamlit_app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

### Dockerfile 指南

让我们逐行了解 Dockerfile 的每一行：

1. `Dockerfile` 必须以 [`FROM`](https://docs.docker.com/engine/reference/builder/#from) 指令开始。它设置容器的[基础镜像](https://docs.docker.com/glossary/#base-image)（类似于操作系统）：

   ```docker
   FROM python:3.9-slim
   ```

   Docker有一些基于不同Linux发行版的官方Docker基础镜像。它们还有一些带有特定语言模块的基础镜像，例如[Python](https://hub.docker.com/_/python)。`python`镜像有多种类型，每种类型都针对特定的使用场景。在这里，我们使用`python:3.9-slim`镜像，它是一个轻量级的镜像，带有最新版本的Python 3.9。

   <提示>

   您还可以使用自己的基础镜像，只要您使用的镜像中包含适用于Streamlit的[支持的Python版本](/knowledge-base/using-streamlit/sanity-checks#check-0-are-you-using-a-streamlit-supported-version-of-python)。对于使用任何特定的基础镜像，也没有一种适用于所有情况的方法，也没有官方的Streamlit特定基础镜像。

   </Tip>

2. `WORKDIR`指令为`Dockerfile`中接下来的`RUN`、`CMD`、`ENTRYPOINT`、`COPY`和`ADD`指令设置工作目录。让我们将其设置为`app/`：

   ```docker
   WORKDIR /app
   ```

   <Important>

   如[开发流程](/library/get-started/main-concepts#development-flow)中所述，对于Streamlit 1.10.0及更高版本，Streamlit应用程序无法从Linux发行版的根目录运行。您的主要脚本应该存放在根目录以外的其他目录中。如果您尝试从根目录运行Streamlit应用程序，Streamlit将抛出`FileNotFoundError: [Errno 2] No such file or directory`错误。有关更多信息，请参阅GitHub问题[#5239](https://github.com/streamlit/streamlit/issues/5239)。

   如果您使用的是Streamlit版本1.10.0或更高版本，则必须将`WORKDIR`设置为根目录以外的目录。例如，您可以将`WORKDIR`设置为`/app`，如上面的示例`Dockerfile`所示。
   
3. 安装`git`，以便我们可以从远程仓库克隆应用代码:

   ```docker
   RUN apt-get update && apt-get install -y \
       build-essential \
       curl \
       software-properties-common \
       git \
       && rm -rf /var/lib/apt/lists/*
   ```

4. 克隆远程仓库中的代码到 `WORKDIR` 目录下：

   a. 如果你的代码在公共仓库中：

   ```docker
   RUN git clone https://github.com/streamlit/streamlit-example.git .
   ```

   克隆完成后，`WORKDIR` 目录下的文件结构如下所示：

   ```bash
   app/
   - requirements.txt
   - streamlit_app.py
   ```
```

   其中 `requirements.txt` 文件包含了所有的[Python依赖项](https://docs.streamlit.io/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies#add-python-dependencies)。例如：

   ```
   altair
   pandas
   streamlit
   ```

   `streamlit_app.py` 是您的主要脚本。例如：

   ```python
   from collections import namedtuple
   import altair as alt
   import math
   import pandas as pd
   import streamlit as st

   """
   # 欢迎使用Streamlit!
   ```

   编辑 `/streamlit_app.py` 文件，根据您的需求自定义此应用程序 :heart:

如果您有任何问题，请查阅我们的 [文档](https://docs.streamlit.io) 和 [社区论坛](https://discuss.streamlit.io)。

同时，以下是使用只需几行代码即可完成的示例：
"""

with st.echo(code_location='below'):
   total_points = st.slider("螺旋中的点的数量", 1, 5000, 2000)
      num_turns = st.slider("螺旋中的转数", 1, 100, 9)

Point = namedtuple('点', 'x y')
数据 = []

每个转数的点数 = 总点数 / 转数

对于当前点数的范围内的每个点：
   当前转数, i = divmod(当前点数, 每个转数的点数)
   角度 = (当前转数 + 1) * 2 * math.pi * i / 每个转数的点数
   半径 = 当前点数 / 总点数
   x = 半径 * math.cos(角度)
   y = 半径 * math.sin(角度)
         ```python
data.append(Point(x, y))

st.altair_chart(alt.Chart(pd.DataFrame(data), height=500, width=500)
   .mark_circle(color='#0068c9', opacity=0.5)
   .encode(x='x:Q', y='y:Q'))
```


   b. 如果您的代码位于私有存储库中，请阅读[使用SSH访问构建中的私有数据](https://docs.docker.com/develop/develop-images/build_enhancements/#using-ssh-to-access-private-data-in-builds)，并相应地修改Dockerfile - 安装SSH客户端、下载[github.com](https://github.com)的公钥，并克隆您的私有存储库。如果您使用的是其他版本控制系统，如GitLab或Bitbucket，请查阅该版本控制系统的文档，了解如何将您的代码复制到Dockerfile的`WORKDIR`中。

   如果您的代码与Dockerfile位于同一目录中，请使用以下命令将您的应用程序文件从服务器复制到容器中，包括`streamlit_app.py`、`requirements.txt`等，将`git clone`行替换为：

```docker
COPY . .
```

更一般地，将您的应用程序代码从服务器上的任何位置复制到容器中。如果代码与Dockerfile不在同一目录中，请修改上述命令以包含代码的路径。

5. 在容器中从克隆的 `requirements.txt` 安装您的应用的 [Python 依赖项](/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies#add-python-dependencies)：

   ```docker
   RUN pip3 install -r requirements.txt
   ```

6. [`EXPOSE`](https://docs.docker.com/engine/reference/builder/#expose) 指令告知 Docker 容器在运行时监听指定的网络端口。您的容器需要监听 Streamlit 的 (默认) 端口 8501：

   ```docker
   EXPOSE 8501
   ```

7. [`HEALTHCHECK`](https://docs.docker.com/engine/reference/builder/#expose) 指令告诉 Docker 如何测试容器以确保其正常工作。您的容器需要监听 Streamlit 的（默认）端口 8501：

   ```docker
   HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health
   ```

8. [`ENTRYPOINT`](https://docs.docker.com/engine/reference/builder/#entrypoint)允许您配置一个作为可执行文件运行的容器。在这里，它还包含了整个`streamlit run`命令用于运行您的应用程序，因此您无需从命令行调用它：

   ```docker
   ENTRYPOINT ["streamlit", "run", "streamlit_app.py", "--server.port=8501", "--server.address=0.0.0.0"]
   ```

## 构建Docker镜像

[`docker build`](https://docs.docker.com/engine/reference/commandline/build/) 命令从 `Dockerfile` 文件构建镜像。在服务器上的 `app/` 目录下运行以下命令来构建镜像：

```docker
docker build -t streamlit .
```

`-t`标志用于给镜像打标签。在这里，我们给镜像打了一个名为`streamlit`的标签。如果你运行：

```docker
docker images
```

您应该在REPOSITORY列下看到一个`streamlit`图像。例如：

```
REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
streamlit    latest    70b0759a094d   About a minute ago   1.02GB
```

## 运行Docker容器

现在您已经构建了镜像，可以通过执行以下命令来运行容器：

```docker
docker run -p 8501:8501 streamlit
```

`-p`标志将容器的端口8501发布到服务器的8501端口。

如果一切顺利，您应该看到类似以下的输出:

```
docker run -p 8501:8501 streamlit

  You can now view your Streamlit app in your browser.

  URL: http://0.0.0.0:8501
```

要查看您的应用程序，用户可以浏览到 `http://0.0.0.0:8501` 或 `http://localhost:8501`

<注意>

根据您的服务器网络配置，您可以映射到端口80/443，这样用户可以使用服务器IP地址或主机名来查看您的应用程序。例如：`http://your-server-ip:80` 或 `http://your-hostname:443`。

</注意>
