# 在不同主机上运行Elasticsearch节点

要在不同的主机上运行群集节点，需要在Docker镜像中更新`/etc/elasticsearch/elasticsearch.yml`文件，以便节点可以互相发现：

- 配置[zen发现模块](http://www.elastic.co/guide/en/elasticsearch/reference/current/modules-discovery.html)，通过添加`discovery.zen.ping.unicast.hosts`指令，指向在每个节点上启动Elasticsearch时应该轮询的主机的IP地址或主机名。

- 建立`network.*` 指令如下：

    ```yml
    network.host: 0.0.0.0
    network.publish_host: <reachable IP address or FQDN>
    ```

    `reachable IP address`指的是其他节点可以到达的IP地址（例如，公共IP地址，或路由的私有IP地址，但不是Docker分配给内部的172.x.x.x地址）。

- 发布端口9300

作为一个例子，在一个主机上像往常一样启动ELK容器，它将作为第一个主机。假设主机被称为*elk-master.example.com*。

查看集群的健康状况：

```sh
$ curl http://elk-master.example.com:9200/_cluster/health?pretty
{
  "cluster_name" : "elasticsearch",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 6,
  "active_shards" : 6,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 6,
  "delayed_unassigned_shards" : 6,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 50.0
}
```

这表明，此时只有一个节点正在启动，黄色状态指示所有主分片都是活动的，但并非所有副本分片都是活动的。

然后，在另一个主机上，创建一个名为`elasticsearch-slave.yml`的文件（假设它在`/home/elk`），内容如下：

```yml
network.host: 0.0.0.0
network.publish_host: <reachable IP address or FQDN>
discovery.zen.ping.unicast.hosts: ["elk-master.example.com"]
```

现在可以使用以下命令启动ELK容器，使用以下命令（将主机上的配置文件挂载到容器中）：

`$ sudo docker run -it --rm=true -p 9200:9200 -p 9300:9300 \
  -v /home/elk/elasticsearch-slave.yml:/etc/elasticsearch/elasticsearch.yml \
  sebp/elk`

一旦Elasticsearch启动，则显示原始主机上的群集的健康状况：

```sh
$ curl http://elk-master.example.com:9200/_cluster/health?pretty
{
  "cluster_name" : "elasticsearch",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 2,
  "number_of_data_nodes" : 2,
  "active_primary_shards" : 6,
  "active_shards" : 12,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}
```