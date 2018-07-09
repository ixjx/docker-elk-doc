# 安装Logstash插件

镜像中的Logstash主目录的名称存储在`LOGSTASH_HOME`环境变量中（在基础镜像中设置为`/opt/logstash`）。Logstash的插件管理脚本（`logstash-plugin`）位于`bin`子目录中。

Logstash使用用户`logstash`运行。为了避免权限的问题，因此建议使用`logstash`安装Logstash插件，使用`gosu`命令（参见下面的示例，并参考进一步的细节）。

下面的`Dockerfile`可以用来扩展基本镜像并安装[RSS输入插件](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-rss.html)：

```dockerfile
FROM sebp/elk

WORKDIR ${LOGSTASH_HOME}
RUN gosu logstash bin/logstash-plugin install logstash-input-rss
```

请参阅上面[构建镜像](../build.md)部分，以说明如何构建新的镜像。然后，可以使用与[使用方法](../usage/README.md)中的命令行相同的命令行基于此镜像运行容器。