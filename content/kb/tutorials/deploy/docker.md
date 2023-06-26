---
slug: /knowledge-base/tutorials/deploy/docker
title: Deploy Streamlit using Docker
---

# 使用Docker部署Streamlit应用

## 简介

您拥有一个令人惊叹的应用程序，并且想要与其他人分享，那么你该怎么做呢？您有几个选择。首先，您想在哪里运行您的Streamlit应用程序，以及如何访问它？

- **在您的企业网络中** - 大多数企业网络对外部世界是封闭的。通常您需要使用VPN来登录到企业网络并访问资源。出于安全考虑，您可以在企业网络中的服务器上运行您的Streamlit应用程序，以确保只有公司内部的人员可以访问。
- **云上部署** - 如果您希望从企业网络之外访问您的Streamlit应用程序，或者与家庭网络或笔记本之外的人分享您的应用程序，您可能会选择此选项。在这种情况下，这将取决于您的托管提供商。我们有来自Heroku、AWS和其他提供商的[社区提交的指南](/knowledge-base/deploy/deploy-streamlit-heroku-aws-google-cloud)。

无论您决定在何处部署应用程序，您首先需要将其容器化。本指南将引导您使用Docker部署应用程序。如果您更喜欢Kubernetes，请参阅[使用Kubernetes部署Streamlit](/knowledge-base/tutorials/deploy/kubernetes)。

## 先决条件

1. [安装Docker引擎](#install-docker-engine)
2. [检查网络端口的可访问性](#check-network-port-accessibility)

### 安装Docker引擎

如果您还没有安装Docker，请在服务器上安装[Docker](https://docs.docker.com/engine/install/#server)。Docker提供了许多Linux发行版的`.deb`和`.rpm`软件包，包括：

- [Debian](https://docs.docker.com/engine/install/debian/)
- [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

通过运行`hello-world` Docker镜像来验证Docker Engine是否正确安装：

```bash
sudo docker run hello-world
```

<Tip>

按照Docker官方的[Linux后安装步骤](https://docs.docker.com/engine/install/linux-postinstall/)来以非root用户身份运行Docker，这样您就不需要在`docker`命令之前加上`sudo`。

</Tip>

### 检查网络端口的可访问性

由于您和您的用户都在公司的VPN后面，您需要确保您所有的用户都能够访问特定的网络端口。假设端口号为`8501`，因为这是Streamlit默认使用的端口。请联系您的IT团队，请求为您和您的用户开放`8501`端口的访问权限。

## 创建一个Dockerfile

Docker通过读取`Dockerfile`中的指令来构建镜像。`Dockerfile`是一个文本文档，包含了用户在命令行上调用的所有命令，用于组装镜像。在[Dockerfile参考](https://docs.docker.com/engine/reference/builder/)中了解更多信息。`docker build`命令可以通过`Dockerfile`构建一个镜像。`docker run`命令首先在指定的镜像上创建一个容器，然后使用指定的命令启动它。

以下是一个示例`Dockerfile`，您可以将其添加到您的目录根目录中，例如`/app/`。

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

### Dockerfile指南

让我们逐行解读Dockerfile的内容：

1. `Dockerfile`必须以 [`FROM`](https://docs.docker.com/engine/reference/builder/#from) 指令开始。它为容器设置了基础镜像（类似于操作系统）：

   ```docker
   FROM python:3.9-slim
   ```

   Docker有一系列官方的基于各种Linux发行版的Docker基础镜像。它们还有带有特定语言模块的基础镜像，例如[Python](https://hub.docker.com/_/python)。`python`镜像有很多种，每种都针对特定的使用场景。在这里，我们使用`python:3.9-slim`镜像，它是一个轻量级镜像，带有最新版本的Python 3.9。

<Tip>

   您还可以使用自己的基础镜像，只要您使用的镜像包含了Streamlit所支持的Python版本。使用特定的基础镜像没有一种适用于所有情况的方法，也没有官方的Streamlit特定基础镜像。

</Tip>

2. `WORKDIR` 指令用于设置 `Dockerfile` 中接下来的 `RUN`、`CMD`、`ENTRYPOINT`、`COPY` 和 `ADD` 指令的工作目录。让我们将它设置为 `app/`：

   ```docker
   WORKDIR /app
   ```

   <重要提示>

   如在[开发流程](/library/get-started/main-concepts#development-flow)中提到的，对于Streamlit 1.10.0及更高版本，无法从Linux分发版的根目录运行Streamlit应用程序。您的主要脚本应该位于根目录之外的目录中。如果尝试从根目录运行Streamlit应用程序，Streamlit将抛出`FileNotFoundError: [Errno 2] No such file or directory`错误。有关更多信息，请参阅GitHub问题[#5239](https://github.com/streamlit/streamlit/issues/5239)。

   如果您使用的是Streamlit 1.10.0或更高版本，则必须将`WORKDIR`设置为根目录以外的目录。例如，您可以将`WORKDIR`设置为`/app`，如上面的示例`Dockerfile`所示。
</Important>

3. 安装`git`，以便我们可以从远程仓库克隆应用程序代码：

   ```docker
   RUN apt-get update && apt-get install -y \
       build-essential \
       curl \
       software-properties-common \
       git \
       && rm -rf /var/lib/apt/lists/*
   ```

4. 将位于远程仓库中的代码克隆到 `WORKDIR` 目录中:

   a. 如果您的代码位于公共仓库中:

   ```docker
   RUN git clone https://github.com/streamlit/streamlit-example.git .
   ```

   克隆完成后，`WORKDIR` 目录将如下所示:

   ```bash
   app/
   - requirements.txt
   - streamlit_app.py
   ```

   `requirements.txt`文件包含了您的所有[Python依赖项](https://docs.streamlit.io/streamlit-community-cloud/get-started/deploy-an-app/app-dependencies#add-python-dependencies)。例如：

   ```
   altair
   pandas
   streamlit
   ```

   `streamlit_app.py`是您的主要脚本。例如：

   ```python
   from collections import namedtuple
   import altair as alt
   import math
   import pandas as pd
   import streamlit as st

   """
   # 欢迎使用Streamlit！
   ```

   编辑`/streamlit_app.py`以根据您的喜好自定义此应用程序 :heart:

如果您有任何问题，请查看我们的[文档](https://docs.streamlit.io)和[社区论坛](https://discuss.streamlit.io)。

同时，以下是一些您可以用几行代码做的示例：
"""

with st.echo(code_location='below'):
   total_points = st.slider("螺旋线上的点数", 1, 5000, 2000)
      num_turns = st.slider("螺旋中的转数", 1, 100, 9)

Point = namedtuple('Point', 'x y')
data = []

points_per_turn = total_points / num_turns

for curr_point_num in range(total_points):
    curr_turn, i = divmod(curr_point_num, points_per_turn)
    angle = (curr_turn + 1) * 2 * math.pi * i / points_per_turn
    radius = curr_point_num / total_points
    x = radius * math.cos(angle)
    y = radius * math.sin(angle)
         data.append(Point(x, y))

st.altair_chart(alt.Chart(pd.DataFrame(data), height=500, width=500)
   .mark_circle(color='#0068c9', opacity=0.5)
   .encode(x='x:Q', y='y:Q'))
```

   b. 如果您的代码存储在私有仓库中，请阅读[使用SSH访问构建中的私有数据](https://docs.docker.com/develop/develop-images/build_enhancements/#using-ssh-to-access-private-data-in-builds)并相应地修改Dockerfile -- 安装SSH客户端，下载[github.com](https://github.com)的公钥，并克隆您的私有仓库。如果您使用的是其他版本控制系统，如GitLab或Bitbucket，请参考该版本控制系统的文档，了解如何将代码复制到Dockerfile的`WORKDIR`中。

   c. 如果您的代码与Dockerfile位于同一个目录中，请使用以下命令将您的所有应用程序文件（包括`streamlit_app.py`、`requirements.txt`等）从服务器复制到容器中，替换`git clone`行：

   ```docker
   COPY . .
   ```

   更一般地说，将您的应用程序代码从服务器上的任何位置复制到容器中。如果代码不在与Dockerfile相同的目录中，请修改上述命令以包括代码的路径。

5. 在容器中从克隆的`requirements.txt`文件中安装您的应用程序的Python依赖项：

   ```docker
   RUN pip3 install -r requirements.txt
   ```

6. [`EXPOSE`](https://docs.docker.com/engine/reference/builder/#expose)指令告知Docker容器在运行时监听指定的网络端口。您的容器需要监听Streamlit的(默认)端口8501：

   ```docker
EXPOSE 8501
```

7. [`HEALTHCHECK`](https://docs.docker.com/engine/reference/builder/#expose) 指令告诉Docker如何测试容器以检查其是否仍在工作。您的容器需要监听Streamlit的（默认）端口8501：

   ```docker
   HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health
   ```

8. [`ENTRYPOINT`](https://docs.docker.com/engine/reference/builder/#entrypoint) 允许您配置一个可作为可执行文件运行的容器。在这里，它还包含了您的应用程序的整个 `streamlit run` 命令，因此您不必从命令行调用它：

   ```docker
   ENTRYPOINT ["streamlit", "run", "streamlit_app.py", "--server.port=8501", "--server.address=0.0.0.0"]
   ```

## 构建 Docker 镜像

[`docker build`](https://docs.docker.com/engine/reference/commandline/build/) 命令从 `Dockerfile` 构建一个镜像。在服务器的 `app/` 目录下运行以下命令来构建镜像：

```docker
docker build -t streamlit .
```

`-t`标志用于为镜像打上标签。在这里，我们给镜像打上了`streamlit`的标签。如果你运行：

```docker
docker images
```

在“REPOSITORY”列下，您应该看到一个`streamlit`的图片。例如：

```
REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
streamlit    latest    70b0759a094d   About a minute ago   1.02GB
```

## 运行Docker容器

现在您已经构建了镜像，可以通过执行以下命令来运行容器:

```docker
docker run -p 8501:8501 streamlit
```

`-p`标志将容器的端口8501发布到服务器的8501端口。

如果一切顺利，您应该看到类似以下的输出：

```
docker run -p 8501:8501 streamlit

  You can now view your Streamlit app in your browser.

  URL: http://0.0.0.0:8501
```

要查看您的应用程序，用户可以浏览到 `http://0.0.0.0:8501` 或 `http://localhost:8501`

<注意>

根据您的服务器网络配置，您可以将端口映射到 80/443，以便用户可以使用服务器 IP 或主机名查看您的应用程序。例如：`http://your-server-ip:80` 或 `http://your-hostname:443`。

</注意>