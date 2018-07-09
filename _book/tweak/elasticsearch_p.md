# 安装Elasticsearch插件

镜像中的Elasticsearch主目录是`/opt/elasticsearch`，其[插件管理脚本](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-plugins.html)（`elasticsearch-plugin`）位于`bin`子目录中，插件安装在`plugins`中。

Elasticsearch使用用户`Elasticsearch`运行。为了避免权限的问题，因此建议使用`elasticsearch`安装Elasticsearch插件，使用`gosu`命令（参见下面的示例，并参考进一步的细节）。

像下面这样的`Dockerfile`将扩展基本镜像并安装[GeoIP处理器插件](https://www.elastic.co/guide/en/elasticsearch/plugins/master/ingest-geoip.html)（它添加了关于IP地址的地理位置的信息）：

```dockerfile
FROM sebp/elk

ENV ES_HOME /opt/elasticsearch
WORKDIR ${ES_HOME}

RUN CONF_DIR=/etc/elasticsearch gosu elasticsearch bin/elasticsearch-plugin \
    install -b ingest-geoip
```

现在，您可以构建新的镜像（见上面的[构建镜像](../build.md)部分），并以与基本镜像相同的方式运行容器。