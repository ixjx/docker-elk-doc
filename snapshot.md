# 快照与还原

将`/var/backups`目录注册为快照存储库（使用`elasticsearch.yml`配置文件中的`path.repo`参数）。可以使用卷或bind-mount来访问该目录和容器外部的快照。

有关快照和还原操作的进一步信息，请参阅有关[快照和还原](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html)的官方文档。