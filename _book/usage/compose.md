# 使用Docker Compose运行容器

如果你使用Docker Compose管理Docker Services（你真应该这么做，因为它会让你的生活更加轻松！），然后可以创建如下docker-compose.yml：

```yml
elk:
  image: sebp/elk
  ports:
    - "5601:5601"
    - "9200:9200"
    - "5044:5044"
```

然后你可以像这样启动ELK容器：

`$ sudo docker-compose up elk`