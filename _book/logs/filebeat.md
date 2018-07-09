# 使用Filebeat转发日志

在您需要收集和转发日志的主机上安装[Filebeat](https://www.elastic.co/products/beats/filebeat)，（参见[参考](../ref.md)部分中的详细说明的链接）。

注意：确保Filebeat的版本与ELK镜像的版本相同。

## 示例Filebeat设置和配置

注意：[GITHUB上的源Git仓库](https://github.com/spujadas/elk-docker)的`nginx-filebeat`文件名子目录包含一个示例`Dockerfile`，使您能够创建实现以下步骤的Docker镜像。

这里是一个示例`/etc/filebeat/filebeat.yml`配置文件，用于转发syslog和authentication日志以及nginx日志。

```yml
output:
  logstash:
    enabled: true
    hosts:
      - elk:5044
    ssl:
      certificate_authorities:
        - /etc/pki/tls/certs/logstash-beats.crt
    timeout: 15

filebeat:
  prospectors:
    -
      paths:
        - /var/log/syslog
        - /var/log/auth.log
      document_type: syslog
    -
      paths:
        - "/var/log/nginx/*.log"
      document_type: nginx-access
```

在示例配置文件中，确保用ELK服务主机的主机名或IP地址替换`elk`中的`elk:5044`。

您还需要复制`logstash-beats.crt`文件（它包含证书颁发机构的证书或服务器证书，作为证书是自签名的），用于Logstash的Beats输入插件；从[ELK镜像的源代码库](https://github.com/spujadas/elk-docker)中查看关于证书`/etc/pki/tls/certs/logstash-beats.crt`的更多信息的[安全注意事项](../security/README.md)。

**备注**-当在Windows机器上使用Filebeat时，而不是使用`certificate_authorities`配置选项，可以将来自`logstash-beats.crt`的证书安装在Windows的可信根证书颁发机构存储中。

**备注**- ELK镜像包括配置项（`/etc/logstash/conf.d/11-nginx.conf`和`/opt/logstash/patterns/nginx`）解析Nginx访问日志，如上面的Filebeat实例所转发的那样。

如果您是第一次启动Filebeat，则应在Elasticsearch中加载默认索引模板。*在编写该文档时，Filebeat版本6，在Elasticsearch中加载索引模板不起作用，请参阅[已知问题](../known.md)*。

启动Filebeat:

`  sudo /etc/init.d/filebeat start`

## 关于处理多行日志条目的注释

为了处理多行日志条目（例如堆栈跟踪），使用Filebeat作为单个事件，您可能需要考虑在Beats 1.1.0中引入的[Filebeat's multiline option](https://www.elastic.co/blog/beats-1-1-0-and-winlogbeat-released)，作为更改Logstash的配置文件以使用[Logstash's multiline codec](https://www.elastic.co/guide/en/logstash/current/plugins-codecs-multiline.html)
