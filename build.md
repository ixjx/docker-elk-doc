# 构建镜像

要从源文件构建Docker镜像，首先克隆[Git仓库](https://github.com/spujadas/elk-docker)，转到克隆目录的根（即包含`Dockerfile`的目录），以及：

- 如果使用vanilla `docker`命令，则运行`sudo docker build -t <repository-name> .`，其中`<repository-name>`是要应用于镜像的仓库名称，然后您可以使用它来使用`docker run`命令运行容器。

- 如果您正在使用Compose，那么运行`sudo docker-compose build elk`，它使用源码库中的`docker-compose.yml`文件来构建镜像。然后您可以运行`sudo docker-compose up`容器。