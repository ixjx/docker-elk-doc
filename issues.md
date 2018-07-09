# 经常遇到的问题

- Elasticsearch is not starting (1): `max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]`

    如果容器停止，其日志包括`max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]`，那么是mmap计数的限制太低，见[前提](prerequisites.md)。

- Elasticsearch is not starting (2): `cat: /var/log/elasticsearch/elasticsearch.log: No such file or directory`

    如果Elasticsearch的日志不被转储（即，您得到以下消息：`cat: /var/log/elasticsearch/elasticsearch.log: No such file or directory`），那么Elasticsearch没有足够的内存启动，见[前提](prerequisites.md)。

- Elasticsearch is not starting (3): bootstrap tests

    自从版本5，如果Elasticsearch不再启动，即`waiting for Elasticsearch to be up (xx/30)`，并且`Couln't start Elasticsearch. Exiting.`。然后将Elasticsearch的日志转储，然后读取日志中的建议，并考虑它们*必须*被应用。

    特别是，在情况（1）中，该消息`max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]`意味主机的mmap计数限制**必须**至少为262144。

- Elasticsearch在正确启动后突然停止。

    使用默认镜像，这通常是由于在其他服务启动后Elasticsearch耗尽内存，并且相应地进程（静默）被杀死。

    作为提醒（见[前提](prerequisites.md)），您应该使用不少于3GB或更多的内存来运行容器。