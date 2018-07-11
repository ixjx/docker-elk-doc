# 使用方法

使用以下命令运行容器：

`$ sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it --name elk sebp/elk`

注意：整个ELK stack将被启动。请参阅[有选择地启动服务](usage/start.md)部分，以选择性地启动stack的一部分。

此命令发布下列端口，这是正确操作ELK stack所需的：

- 5601（Kibana Web界面）。
- 9200（Elasticsearch JSON接口）。
- 5044（Logstash Beats接口，从Filebeat等Beats接收日志）—— 见[使用Filebeat转发日志](logs/filebeat.md)。

注意:镜像还暴露了Elasticsearch传输接口的端口9300。使用上面的`docker`命令`-p 9300:9300`选项发布它。这个传输接口被[Elasticsearch的Java客户端API](https://www.elastic.co/guide/en/elasticsearch/client/java-api/current/index.html)使用，并在集群中运行Elasticsearch。

下图显示了这些零件是如何装配在一起的。

![](https://imgur.com/Og5eps4)

访问Kibana的Web界面：浏览 `http://<your-host>:5601`，其中`<your-host>`是主机Docker的主机名或IP地址（见注释），例如运行本地版本Docker的主机，或者运行VM宿主版本Docker的虚拟机的IP地址。

注意：配置或查找VM宿主版本Docker安装的IP地址，若使用Boot2Docker，参见 https://docs.docker.com/installation/windows/ (Windows)、https://docs.docker.com/installation/mac/ (OS X)。若使用[Vagrant](https://www.vagrantup.com/)，需要设置端口转发(见 https://docs.vagrantup.com/v2/networking/forwarded_ports.html)

使用`Ctrl+C`暂停容器，然后用`sudo docker start elk`重新启动。

从Kibana版本4.0.0以后，您将无法看到任何东西（甚至是一个空仪表盘），直到有东西被记录（见下面的[创建虚拟日志条目](usage/log.md)，关于如何测试您的设置），以及[转发日志](logs/README.md)部分，关于如何从常规应用程序中转发日志。

当Kibana配置索引时（默认为logstash-*），请注意，在该镜像中Logstash使用输出插件，该插件被配置与为Beat源输入（例如由Filebeat生成，见[使用Filebeat转发日志](logs/filebeat.md)）一起工作。并且日志将以\<beatname>-prefix索引（例如filebeat-当使用Filebeat）。