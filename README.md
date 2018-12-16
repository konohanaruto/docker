# docker
忽略了data目录<br />
用于存放docker文件

* 如果修改了`./etc/nginx/conf.d/vhost.conf`文件，如果需要生效必须执行:<br />
```
docker-compose up -d --force
```
而不是
```
docker-compose up -d
```
