# 证书注释

虚拟服务器认证证书（`/etc/pki/tls/certs/logstash-*.crt`）和私钥（`/etc/pki/tls/private/logstash-*.key`）被包含在镜像中。

**注意**-对于Logstash 2.4.0，必须使用PKCS×8格式化的私钥（参见[版本变化](../change.md)）

证书被分配给主机名`*`，这意味着它们都将工作，如果你从客户端使用一个单一的部分（即没有点）域名来引用服务器。

**示例**-在客户端（例如Filebeat）中，将日志发送到主机名`elk`，`elk.mydomain.com`将不会（产生错误：`x509: certificate is valid for *, not elk.mydomain.com`）是有效的，IP地址（如192.168.0.1）也不会（`x509: cannot validate certificate for 192.168.0.1 because it doesn't contain any IP SANs`）。

如果你不能使用单个域名，那么你可以考虑：

- 使用下面给出的命令的变体发布具有正确主机名的自签名证书。

- 即使在IP地址很可能改变的情况下，[IP address of the ELK stack in the subject alternative name field](https://github.com/elastic/logstash-forwarder/issues/221#issuecomment-48390920)，尽管这通常是不好的做法。

- 将单个部件主机名（例如`elk`）添加到客户端`/etc/hosts`文件中。

下面的命令将生成一个私钥和一个10年的自签名证书，该服务器以主机名`elk`发送给Beats输入插件：

```sh
$ cd /etc/pki/tls
$ sudo openssl req -x509 -batch -nodes -subj "/CN=elk/" \
    -days 3650 -newkey rsa:2048 \
    -keyout private/logstash-beats.key -out certs/logstash-beats.crt
```

作为另一个例子，当在一个集群中运行一个非预定义数量的容器时，它的主机名直接在`.mydomain.com`域（例如，`elk1.mydomain.com`，`elk2.mydomain.com`等）；而不是`elk1.subdomain.mydomain.com`，`elk2.othersubdomain.mydomain.com`等），您可以创建一个证书。通过使用下面的命令分配给通配符主机名`*.example.com`（所有其他参数与前面示例中的所有参数相同）。

```sh
$ cd /etc/pki/tls
$ sudo openssl req -x509 -batch -nodes -subj "/CN=*.example.com/" \
    -days 3650 -newkey rsa:2048 \
    -keyout private/logstash-beats.key -out certs/logstash-beats.crt
```

为了使Logstash使用生成的证书对Beats客户机进行身份验证，扩展ELK镜像以重写（例如使用`Dockerfile`指令`ADD`）：

- 证书(`logstash-beats.crt`) 路径`/etc/pki/tls/certs/logstash-beats.crt`.

- 私钥(`logstash-beats.key`) 路径`/etc/pki/tls/private/logstash-beats.key`

另外，请记住配置Beats客户端以使用证书文件管理器信任新创建的证书，如在[使用Filebeat转发日志](../logs/filebeat.md)中所示。