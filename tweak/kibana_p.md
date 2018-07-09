# 安装Kibana插件

镜像中的Kibana主目录的名称存储在`KIBANA_HOME`环境变量中（在基础镜像中设置为`/opt/kibana`）。Kibana的插件管理脚本（`kibana-plugin`）位于`bin`子目录中，插件安装在`installedPlugins`子目录中。

Kibana使用用户`kibana`运行。为了避免权限的问题，因此建议使用`kibana`安装Kibana插件，使用`gosu`命令（参见下面的示例，并参考进一步的细节）。

下面的`Dockerfile`可以用来扩展基本镜像并安装最新版本的[Sense plugin](https://www.elastic.co/guide/en/sense/current/index.html)，这是一个与Elasticsearch的REST API交互的便利控制台：

```dockerfile
FROM sebp/elk

WORKDIR ${KIBANA_HOME}
RUN gosu kibana bin/kibana-plugin install elastic/sense
```

请参阅上面[构建镜像](../build.md)部分，以说明如何构建新的镜像。然后，可以使用与[使用方法](../usage/README.md)中的命令行相同的命令行基于此镜像运行容器。在`http://<your-host>:5601/apss/sense`（例如 http://localhost:5601/app/sense， 用于本地实例的Docker），可以访问Sense接口。