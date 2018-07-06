# 前提

若要使用此镜像运行容器，您需要以下内容：

- **Docker**
安装[Docker](https://docker.com/)，或者使用本地包(Linux)或在虚拟机(Windows,OS X中使用[Boot2Docker](http://boot2docker.io/)或[Vagrant](https://www.vagrantup.com/))。

- **最少分配给Docker 4GB内存**
单独使用Elasticsearch至少需要2GB的内存。
使用Mac的Docker，可以使用UI设置Docker专用内存的数量：[How to increase docker-machine memory Mac](https://stackoverflow.com/questions/32834082/how-to-increase-docker-machine-memory-mac/39720010#39720010)(Stack Overflow).

- **mmap计数大于等于262144的限制**
<font color=red>!!这是自从Elasticsearch V5发布后，Elasticsearch服务启动失败的最常见原因。</font>
Linux系统中使用`sysctl vm.max_map_count`查看当前值，见[Elasticsearch's documentation on virtual memory](https://www.elastic.co/guide/en/elasticsearch/reference/5.0/vm-max-map-count.html#vm-max-map-count)如何更改这个值。注意，**必须在宿主机上更改**，不能从容器内更改。
如果使用Docker for Mac,需要用`MAX_MAP_COUNT`环境变量启动容器(使用`docker的-e`选项)，见[Overriding start-up variables](usage/var.md),设置为至少262144，以使Elasticsearch设置mmap计数在启动时的限制。

- **从日志发送客户端访问TCP端口5044**
其他端口可能需要显式地打开：请参见已暴露的完整端口列表的[用法](usage/README.md)。
