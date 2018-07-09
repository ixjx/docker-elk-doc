# 已知问题

在使用Filebeat时，使用[索引模板文件](https://www.elastic.co/guide/en/beats/filebeat/6.0/filebeat-template.html)连接到Elasticsearch以定义确定字段应如何分析的设置和映射。

在版本5中，在第一次启动Filebeat之前，您将运行此命令（用适当的主机名替换`elk`），以在Elasticsearch中加载默认索引模板：

` curl -XPUT 'http://elk:9200/_template/filebeat?pretty' -d@/etc/filebeat/filebeat.template.json`

然而，在第6版中，`filebeat.template.json`模板文件已被`fields.yml`文件所替代，该文件用于根据`filebeat setup --template`[as per the official Filebeat instructions](https://www.elastic.co/guide/en/beats/filebeat/6.0/filebeat-template.html#load-template-manually)。不幸的是，这目前不起作用，并导致以下消息：

`Exiting: Template loading requested but the Elasticsearch output is not configured/enabled`

试图在不设置模板的情况下启动Filebeat会产生以下消息：

```json
Warning: Couldn't read data from file "/etc/filebeat/filebeat.template.json",
Warning: this makes an empty POST.
{
  "error" : {
    "root_cause" : [
      {
        "type" : "parse_exception",
        "reason" : "request body is required"
      }
    ],
    "type" : "parse_exception",
    "reason" : "request body is required"
  },
  "status" : 400
}
```

可以假设，在稍后的Filebeat版本中，将阐明指令以指定如何手动将索引模板加载到Elasticsearch的特定实例中，并且警告消息将消失，不再适用于版本6。