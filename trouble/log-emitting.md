# 日志发送客户端无法连接到Logstash

**注意**：类似的故障排除步骤适用于将日志直接发送到Elasticsearch的设置。

确保：

- 使用正确的端口启动容器（例如5044 for Beats）。

- 如果您正在使用Filebeat，其版本与ELK镜像的版本相同。

- 端口从客户端机器可达（例如，确保在您的防火墙上设置了适当的规则来授权来自客户端和您的ELK托管机上的入站流的出站流）。

- 您的客户端被配置为使用TLS（或SSL）连接到Logstash，并且它信任Logstash的自签名证书（或证书权威机构，如果您用适当的证书替换默认证书——请参阅[安全注意事项](../security/README.md)）。

若要检查Logstash是否使用正确的证书进行身份验证，请检查输出中的错误。

`$ openssl s_client -connect localhost:5044 -CAfile logstash-beats.crt`

其中`logstash-beats.crt`是包含Logstash的自签名证书文件的名称。