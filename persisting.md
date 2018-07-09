# 日志数据持久化

为了保证日志数据跨容器重新启动，该镜像挂载`/var/lib/elasticsearch`——这是Elasticsearch将其数据存储为卷的目录。

但是，您可能希望使用专用数据卷来保存此日志数据，例如便于备份和恢复操作。

这样做的一种方法是使用`Docker`的`-v`选项来挂载Docker命名卷，如：

`$ sudo docker run -p 5601:5601 -p 9200:9200  -p 5044:5044 \
    -v elk-data:/var/lib/elasticsearch --name elk sebp/elk`

这个命令将命名卷`elk-data`挂载到`/var/lib/elasticsearch`（如果不存在，则自动创建卷；也可以使用`docker volume create elk-data`手动创建）。

**注意**-设计上，Docker从不自动删除卷（例如，当不再被任何容器使用）。虽然这避免了数据意外丢失，但这也意味着，如果您没有正确管理卷的话，事情可能变得混乱（例如，在使用`docker rm`删除容器时也使用`-v`选项来删除卷……记住，只要至少一个容器仍然引用它，即使它没有运行，实际卷也不会被删除。您可以使用`docker volume ls`跟踪现有卷。

参见[Managing Data in Containers](https://docs.docker.com/engine/userguide/containers/dockervolumes/)和[Docker In-depth: Volumes](http://container42.com/2014/11/03/docker-indepth-volumes/)了解更多信息。

在权限方面，Elasticsearch数据由镜像的`elasticsearch`用户创建，使用UID 991和GID 991。

有一种[已知情况](https://github.com/spujadas/elk-docker/issues/69)，当SELinux在强制模式下运行时，拒绝访问挂载的卷。解决方法是使用`setenforce 0`命令在*permissive*模式下运行SELinux。