# 创建虚拟日志条目

如果您还没有任何日志，并希望手动创建一个虚拟日志条目以供测试目的（例如，查看仪表盘），首先像往常一样启动容器`sudo docker run ... ` or `docker-compose up ...)`

在另一个终端窗口中，查找ELK容器的名称，其显示在`sudo docker ps`命令输出的最后一列。

```
$ sudo docker ps
CONTAINER ID        IMAGE                  ...   NAMES
86aea21cab85        elkdocker_elk:latest   ...   elkdocker_elk_1
```

在容器中打开一个shell，并用容器的名称替换`<container-name>`，例如上面例子中的`elkdocker_elk_1`：

`$ sudo docker exec -it <container-name> /bin/bash`

在提示符中输入：

`# /opt/logstash/bin/logstash --path.data /tmp/logstash/data \
    -e 'input { stdin { } } output { elasticsearch { hosts => ["localhost"] } }'`

等待Logstash启动（如消息所示，`The stdin plugin is now waiting for input:`），然后键入一些虚拟文本，然后输入Enter来创建日志条目：

`this is a dummy entry`

注意：您可以创建尽可能多的条目。使用`^C`返回BASH提示符。

(译者注：如果提示已经启动了Logstash进程，使用`service logstash stop`停止当前进程)

浏览`http://<your-host>:9200/_search?pretty`，您将看到Elasticsearch已经有了索引条目。

```json
{
  ...
  "hits": {
    ...
    "hits": [ {
      "_index": "logstash-...",
      "_type": "logs",
      ...
      "_source": { "message": "this is a dummy entry", "@version": "1", "@timestamp": ... }
    } ]
  }
}
```

现在您可以在`http://<your-host>:5601`浏览到Kibana的Web界面。

确保下拉的"Time Filter field name"字段以值`@timestamp`预先填充，然后点击“创建”，您就可以走了(原文you're good to go...不知道怎么翻译>_<)。