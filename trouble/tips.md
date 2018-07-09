# 附加提示

如果上面给出的建议不能解决你的问题，那么你应该看看：

- 您的日志转发客户端的日志。

- ELK的日志，由`docker exec`到运行容器（见[创建虚拟日志条目](../usage/log.md)），打开stdout日志（见[plugins-outputs-stdout](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-stdout.html)），并检查Logstash的日志（位于`/var/log/logstash`），Elasticsearch的日志（`/var/log/elasticsearch`）和Kibana的日志（`/var/log/kibana`）。

注意，ELK的日志使用logrotate每天转储，并在一周后删除。您可以通过在`/etc/logrotate.d`中覆盖`elasticsearch`、`logstash`和`kibana`文件来改变这种行为。