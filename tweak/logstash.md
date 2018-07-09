# 更新Logstash参数

该镜像包含几个用于Logstash的配置文件（例如`01-lumberjack-input.conf,02-beats-input.conf`），都位于`/etc/logstash/conf.d`。

若要修改现有配置文件，可以在运行时将本地配置文件bind-mount容器内的配置文件。例如，如果您想用本地文件`/path/to/your-30-output.conf`替换镜像的`30-output.conf`Logstash配置文件，那么您将向您的Docker命令行添加以下`-v`选项：

`$ sudo docker run ... \
    -v /path/to/your-30-output.conf:/etc/logstash/conf.d/30-output.conf \
    ...`

要用更新的或附加的配置文件创建自己的镜像，可以创建一个扩展原始镜像的Dockerfile，内容如下：

```dockerfile
FROM sebp/elk

# overwrite existing file
ADD /path/to/your-30-output.conf /etc/logstash/conf.d/30-output.conf

# add new file
ADD /path/to/new-12-some-filter.conf /etc/logstash/conf.d/12-some-filter.conf
```

然后使用Docker构建语法构建扩展镜像。