# 调整镜像

有几种调整镜像的方法：

- 使用镜像作为基础镜像并扩展它，添加文件（例如，配置文件来处理由日志产生的应用程序发送的日志、用于 Elasticsearch的插件）以及根据需要重写文件（例如配置文件、证书和私钥文件）。请参阅Docker的[Dockerfile Reference page](https://docs.docker.com/engine/reference/builder/)了解更多关于编写Dockerfile的信息。

- 通过将本地文件绑定到容器中的文件来替换现有文件。请参阅Docker[ Manage data in containers](https://docs.docker.com/engine/userguide/containers/dockervolumes/)，了解更多关于卷的信息，特别是卷的绑定。

- Fork Git源仓库然后修改

接下来的几个小节介绍了一些典型的用例。