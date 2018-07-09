# Elasticsearch无法运行

如果在[经常遇到的问题](../issues.md)中列出的建议没有帮助，那么另一种解决Elasticsearch无法启动的方法是：

- 用`bash`命令启动容器：

    `$ sudo docker run -it docker_elk bash`

- 手动启动Elasticsearch以查看它输出的内容：

    ```sh
    $ gosu elasticsearch /opt/elasticsearch/bin/elasticsearch \
    -Edefault.path.logs=/var/log/elasticsearch \
    -Edefault.path.data=/var/lib/elasticsearch \
    -Edefault.path.conf=/etc/elasticsearch
    ```