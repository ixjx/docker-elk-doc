# 安全注意事项

正如它所代表的，这个镜像是用于本地测试使用的，因此没有得到保证：对ELK服务的访问是不受限制的，并且默认的身份验证服务器证书和Logstash输入插件的私钥与镜像捆绑在一起。

为了加固这个镜像，至少你想：

- 限制访问ELK服务到授权的主机/网络，如[Elasticsearch Scripting and Security](https://www.elastic.co/blog/scripting-security) 和 [Elastic Security: Deploying Logstash, ElasticSearch, Kibana "securely" on the Internet](http://blog.eslimasec.com/2014/05/elastic-security-deploying-logstash.html)。

- 密码保护对Kibana和Elasticsearch的访问（见[SSL和Kibana密码保护](http://technosophos.com/2014/03/19/ssl-password-protection-for-kibana.html)）。

- 为Logstash输入插件生成新的自签名身份验证证书（参见[证书注释](./cert.md)）或（更好）从商业提供者（称为证书颁发机构）获得适当证书，并保持私钥私有。

使用X-PACK扩展ELK镜像的[sebp/elkx](https://hub.docker.com/r/sebp/elkx/)镜像可能是提高ELK服务安全性的一个有用的起点。

另一方面，如果您想禁用基于证书的服务器身份验证（例如在演示环境中），请参阅[禁用SSL/TLS](./ssl.md)。