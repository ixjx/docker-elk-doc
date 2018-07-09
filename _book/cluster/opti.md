# 优化Elasticsearch节点

您可以使用ELK镜像来运行一个Elasticsearch集群，特别是如果您只是在测试，但要优化您的设置，您可能需要：

- 一个使用ELK镜像的节点运行完整的ELK stack。

- 几个节点只运行Elasticsearch（[有选择地启动服务](../usage/start.md)）。


在多个节点或主机上分布式部署Elasticsearch、Logstash和Kibana的更为理想的方式是只在适当的节点或主机上运行所需的服务（例如，在多个主机上运行Elasticsearch，在专用主机上运行Logstash，以及在另一个专用主机上运行Kibana）。