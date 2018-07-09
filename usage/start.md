# 有选择地启动服务

默认情况下，当启动容器时，所有三个ELK服务（Elasticsearch、Logstash、Kibana）都已启动。

以下环境变量可用于选择性地启动服务的子集：

- ELASTICSEARCH_START: 如果设置并设置为1以外的任何东西，则将不启动Elasticsearch.
- LOGSTASH_START: 同上
- KIBANA_START: 同上

例如，以下命令仅启动Elasticsearch：

`$ sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it \
    -e LOGSTASH_START=0 -e KIBANA_START=0 --name elk sebp/elk`

请注意，如果容器是禁用Elasticsearch而启动的，那么：

- 如果启用Logstash，那么您需要确保Logstash的Elasticsearch输出插件（`/etc/logstash/conf.d/30-output.conf`）的配置文件指向属于Elasticsearch集群的主机，而不是`localhost`（默认为ELK镜像中的主机），因为默认Elasticsearch和Logstash一起运行，例如：

```yml
output {
  elasticsearch { hosts => ["elk-master.example.com"] }
}
```

- 类似地，如果启用了Kibana，则必须首先更新Kibana的kibana.yml配置文件以使`elasticsearch.url`设置（默认值：`"http://localhost:9200"`）指向一个正在运行的Elasticsearch实例。