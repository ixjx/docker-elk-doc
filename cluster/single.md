# 在单机上运行Elasticsearch节点

设置在单个主机上运行的Elasticsearch节点类似于运行不同主机上的节点，但是为了使节点彼此发现，需要将容器连接起来。

使用主机上常用的`docker`命令启动第一个节点：

`$ sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it --name elk sebp/elk`

现在，创建一个包含以下几行的基本`elasticsearch-slave.yml`文件：

```yml
network.host: 0.0.0.0
discovery.zen.ping.unicast.hosts: ["elk"]
```

使用以下命令启动节点：

` sudo docker run -it --rm=true \
  -v /var/sandbox/elk-docker/elasticsearch-slave.yml:/etc/elasticsearch/elasticsearch.yml \
  --link elk:elk --name elk-slave sebp/elk`

注意，Elasticsearch的端口没有被发布到主机的端口9200，因为它已经由最初的ELK容器发布。