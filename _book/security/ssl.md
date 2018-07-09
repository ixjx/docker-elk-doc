# 禁用SSL/TLS

基于证书的服务器身份验证需要生成日志的客户端信任服务器的根证书颁发机构的证书，这在零临界环境（例如演示环境、沙盒）中可能是不必要的麻烦。

若要禁用基于证书的服务器身份验证，请删除Logstash的输入插件配置文件中的所有`SSL`和`ssl-prefixed`指令（例如`ssl_certificate`、`ssl_key`）。

例如，使用镜像中的默认配置文件，将`02-beats-input.conf`（用于Beats发射器）的内容替换为：

```conf
input {
  beats {
    port => 5044
  }
}
```