# 重写启动变量

下列环境变量可用于重写用于启动服务的默认值：

- `TZ`: 容器的时区（见[有效时区列表](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)），例如`America/Los_Angeles`（默认为`Etc/UTC`，即UTC）.

- `ES_HEAP_SIZE`: Elasticsearch堆大小（默认值为最小256MB，最大1G）

    指定堆大小（例如`2G`）将MIN和MAX设置为所提供的值。若要分别设置最小值和最大值，请参阅下面的`ES_JAVA_OPTS`.

- `ES_JAVA_OPTS`: 用于Elasticsearch的附加Java选项（默认值：""）

    例如，为了将最小和最大堆大小设置为512MB和2G，将此环境变量设置为`-Xms512m -Xmx2g`.

- `ES_CONNECT_RETRY`: 在开始Logstash和/或Kibana之前等待Elasticsearch的秒数（默认值：30）

- `ES_PROTOCOL`: 用于ping Elasticsearch的JSON接口URL的协议（默认：HTTP）

    请注意，此变量仅用于测试在启动服务时Elasticsearch是否启动。它不用于更新Logstash和Kibana的配置文件中的Elasticsearch URL。

- `CLUSTER_NAME`: Elasticsearch集群的名称（默认情况下：如果Elasticsearch不需要用户身份验证，则容器启动时自动解析）。

    Elasticsearch集群的名称用于设置运行时容器显示的Elasticsearch日志文件的名称。默认情况下，通过匿名查询Elasticsearch的REST API，在启动时自动解析群集的名称（并填充`CLUSTER_NAME`）。然而，当Elasticsearch要求用户身份验证（例如，在运行X-PACK时默认情况下），这个查询失败，并且容器假设它不正确运行，则容器停止。因此，`CLUSTER_NAME`环境变量可以用来指定集群的名称，并绕过（失败的）自动解决。

- `LS_HEAP_SIZE`: Logstash堆大小（默认值：“500m”）

- `LS_OPTS`: Logstash选项（默认：`"--auto-reload"`）带有标签`es231_l231_k450`和`es232_l232_k450`，"" in `latest`；见[版本变化](../change.md)

- `NODE_OPTIONS`: 节点Kibana选项（默认：`"--max-old-space-size=250"`）

- `MAX_MAP_COUNT`: 限制mmap计数（默认值：系统默认值）

    警告-此设置依赖于系统：并非所有系统都允许从容器内设置此限制，您可能需要在启动容器之前将其设置为主机（参见[前提](../prerequisites.md)）。

- `MAX_OPEN_FILES`: 打开文件的最大数目（默认值：系统默认值；Elasticsearch需要至少等于65536）

- `KIBANA_CONNECT_RETRY`: 在运行post-hook脚本之前等待Kibana启动的秒数（参见[Pre-hooks和post-hooks](./hooks.md)）（默认值：30）

作为一个示例，下面的命令启动stack，运行具有2GB堆大小的Elasticsarch和1GB堆大小的Logstash：

`$ sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it \
    -e ES_HEAP_SIZE="2g" -e LS_HEAP_SIZE="1g" --name elk sebp/elk`