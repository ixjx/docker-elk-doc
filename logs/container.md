# 同主机上其他容器连接ELK容器

如果要将日志从Docker容器转发到主机上的ELK容器，则需要连接两个容器。

注意：日志发出的Docker容器必须在其中运行Filebeat。

首先，创建一个隔离的、用户自定义的`桥接`网络（我们将称之为`elknet`）：

`$ sudo docker network create -d bridge elknet`

现在启动ELK容器，使用-name选项给它命名（例如`elk`），并指定它必须连接到的网络（在这个例子中的`elknet`）：

`$ sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it \
    --name elk --network=elknet sebp/elk`

然后在同一网络上启动日志转发容器（用您转发日志的 ilebeat-enabled镜像的名称替换`your/image`）：

`$ sudo docker run -p 80:80 -it --network=elknet your/image`

从日志转发容器的角度来看，ELK容器现在被称为`elk`，它是在`filebeat.yml`配置文件中的`host`下使用的主机名。

有关与Docker网络的更多信息，请参见[Docker关于使用网络命令的文档](https://docs.docker.com/engine/userguide/networking/work-with-networks/)。

## 无用户自定义的网络连接容器

*这是连接容器在Docker的默认`bridge`网络上的传统方式，这是[Docker遗留下来的遗留特征，最终可能被移除](https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/)。*

首先，使用`--name`选项给ELK容器一个名称（例如`elk`）：

`$ sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it --name elk sebp/elk`

然后用`--Link`选项启动日志转发容器（用您转发日志的 ilebeat-enabled镜像的名称替换`your/image`）：

`$ sudo docker run -p 80:80 -it --link elk:elk your/image`

从日志转发容器的角度来看，ELK容器现在被称为`elk`，它是在`filebeat.yml`配置文件中的`host`下使用的主机名。

下面是一个（本地构建的日志生成）容器和一个ELK容器的示例条目在`docker-compose.yml`文件中的样子。

```yml
yourapp:
  image: your/image
  ports:
    - "80:80"
  links:
    - elk

elk:
  image: sebp/elk
  ports:
    - "5601:5601"
    - "9200:9200"
    - "5044:5044"
```