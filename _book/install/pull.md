# 拉取特定版本组合

可以通过使用标签来拉取Elasticsearch、Logstash和Kibana的特定版本组合。

例如，包含Elasticsearch 1.7.3、Logstash 1.5.5和Kibana 4.1.2的镜像（这是使用Elasticsearch 1.x和Logstash 1.x分支的最新镜像）标签是`E1L1K4`，因此可以使用`sudo docker pull sebp/elk:E1L1K4`拉取镜像。

可用标签在[Docker Hub's sebp/elk](https://hub.docker.com/r/sebp/elk/)或GITHUB仓库页面上列出。

默认情况下，如果没有指定标签（或者使用最新标签），则会拉取镜像的最新版本。