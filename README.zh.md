# Docker SDK for Python

[![Build Status](https://github.com/docker/docker-py/actions/workflows/ci.yml/badge.svg)](https://github.com/docker/docker-py/actions/workflows/ci.yml)

Docker 引擎 API 的 Python 库。它允许您在 Python 应用程序中执行 `docker` 命令可以做的任何事情 - 运行容器、管理容器、管理 Swarms 等。

## 安装

最新稳定版本[可在 PyPI 上获取](https://pypi.python.org/pypi/docker/)。使用 pip 安装：

    pip install docker

> 旧版本(< 6.0)需要安装 `docker[tls]` 以支持 SSL/TLS。
> 这不再必要，并且是无操作(no-op)，但为了向后兼容仍然支持。

## 使用方法

使用默认套接字或您环境中的配置连接到 Docker：

```python
import docker
client = docker.from_env()
```

您可以运行容器：

```python
>>> client.containers.run("ubuntu:latest", "echo hello world")
'hello world\n'
```

您可以在后台运行容器：

```python
>>> client.containers.run("bfirsh/reticulate-splines", detach=True)
<Container '45e6d2de7c54'>
```

您可以管理容器：

```python
>>> client.containers.list()
[<Container '45e6d2de7c54'>, <Container 'db18e4f20eaa'>, ...]

>>> container = client.containers.get('45e6d2de7c54')

>>> container.attrs['Config']['Image']
"bfirsh/reticulate-splines"

>>> container.logs()
"Reticulating spline 1...\n"

>>> container.stop()
```

您可以流式传输日志：

```python
>>> for line in container.logs(stream=True):
...   print(line.strip())
Reticulating spline 2...
Reticulating spline 3...
...
```

您可以管理镜像：

```python
>>> client.images.pull('nginx')
<Image 'nginx'>

>>> client.images.list()
[<Image 'ubuntu'>, <Image 'nginx'>, ...]
```

[阅读完整文档](https://docker-py.readthedocs.io) 以查看您可以执行的所有操作。
