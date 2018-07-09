# Pre-hooks和post-hooks

在启动ELK服务之前，如果脚本存在并且是可执行的，容器将在`/usr/local/bin/elk-pre-hooks.sh`中运行脚本。

这可以用于将自定义环境变量（除了[由镜像支持的默认变量](./var.md)）暴露到Elasticsearch和Logstash中，通过修改相应的`/etc/default`文件。

例如，为了将自定义`MY_CUSTOM_VAR`环境变量暴露给Elasticsearch，将一个可执行文件`/usr/local/bin/elk-pre-hooks.sh`添加到容器中（例如，通过`ADD`将其添加到扩展基础镜像的自定义`Dockerfile`，或者通过在运行时绑定文件），如下内容：

```sh
cat << EOF >> /etc/default/elasticsearch
MY_CUSTOM_VAR=$MY_CUSTOM_VAR
export MY_CUSTOM_VAR 
EOF
```

在启动ELK服务之后，容器将在`/usr/local/bin/elk-post-hooks.sh`中运行脚本，如果它存在并且是可执行的。

例如，可以在服务开始后给Elasticsearch添加索引模板或向Kibana添加索引模式。