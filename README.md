### 更新yum
```
yum update -y
```
### 安装Docker
相关手册
`https://docs.docker.com/install/linux/docker-ce/centos/`

配置开启自启动
```
systemctl enable docker
```
安装docker-compose
`https://docs.docker.com/compose/install`

设置中文镜像
https://www.docker-cn.com/registry-mirror

### 创建HOST机相关用户
由于Linux的权限是基于uid和gid的，而不是username和groupname。类似官方mysql镜像在容器中的mysql用户和mysql组的uid为999，组id为999，所以改变官方镜像中的默认用户名，只需要改变容器中对应用户的uid和组id即可，也就是改成我们host机对应的用户的uid和组id，我的host机的mysql用户uid为997，mysql用户的组id为993，这个改变，只需要我们在Dockerfile中作出修改即可！

 __创建www用户__ 
```
sudo useradd www
id www
// 官方文档说道，只需要普通用户加入docker组，该用户即可以运行docker
// uid=1000(www) gid=1000(www) groups=1000(www),994(docker)
```
__创建mysql用户__
```
sudo useradd -r mysql // 创建为系统用户
id mysql
// uid=997(mysql) gid=993(mysql) groups=993(mysql),1000(www)
```
__创建redis用户__
```
sudo useradd -r redis // 用于redis容器
id redis
// uid=1001(redis) gid=1001(redis) groups=1001(redis)
```
## mysql环境变量的官方说明

> When you start the mysql image, you can adjust the configuration of the MySQL instance by passing one or more environment variables on the docker run command line. Do note that none of the variables below will have any effect if you start the container with a data directory that already contains a database: any pre-existing database will always be left untouched on container startup.

> See also https://dev.mysql.com/doc/refman/5.7/en/environment-variables.html for documentation of environment variables which MySQL itself respects (especially variables like MYSQL_HOST, which is known to cause issues when used with this image).

> **MYSQL_ROOT_PASSWORD**<br>
> This variable is mandatory and specifies the password that will be set for the MySQL root superuser account. In the above example, it was set to my-secret-pw.

> **MYSQL_DATABASE**<br>
> This variable is optional and allows you to specify the name of a database to be created on image startup. If a user/password was supplied (see below) then that user will be granted superuser access (corresponding to GRANT ALL) to this database.

> **MYSQL_USER, MYSQL_PASSWORD**<br>
> These variables are optional, used in conjunction to create a new user and to set that user's password. This user will be granted superuser permissions (see above) for the database specified by the MYSQL_DATABASE variable. Both variables are required for a user to be created.

> Do note that there is no need to use this mechanism to create the root superuser, that user gets created by default with the password specified by the MYSQL_ROOT_PASSWORD variable.

> **MYSQL_ALLOW_EMPTY_PASSWORD**<br>
> This is an optional variable. Set to yes to allow the container to be started with a blank password for the root user. NOTE: Setting this variable to yes is not recommended unless you really know what you are doing, since this will leave your MySQL instance completely unprotected, allowing anyone to gain complete superuser access.

> **MYSQL_RANDOM_ROOT_PASSWORD**<br>
> This is an optional variable. Set to yes to generate a random initial password for the root user (using pwgen). The generated root password will be printed to stdout (GENERATED ROOT PASSWORD: .....).

> **MYSQL_ONETIME_PASSWORD**<br>
> Sets root (not the user specified in MYSQL_USER!) user as expired once init is complete, forcing a password change on first login. NOTE: This feature is supported on MySQL 5.6+ only. Using this option on MySQL 5.5 will throw an appropriate error during initialization.

目录结构说明：
```
docker-lnmp/
├── build // 统一用于存放Dockerfile文件
│   ├── mysql
│   │   └── Dockerfile
│   ├── nginx
│   │   └── Dockerfile
│   ├── php
│   │   ├── Dockerfile
│   │   └── Dockerfile.bak
│   └── redis
│       └── Dockerfile
├── data
│   ├── mysql // 用于存储mysql数据文件
│   └── redis // 用户存储redis数据
├── docker-compose.yml // docker-compose.yml文件
├── etc // 配置文件目录
│   ├── mysql // mysql相关配置文件
│   │   ├── conf.d // 扩展配置目录
│   │   └── my.cnf // mysql配置文件，这里是个空文件
│   ├── nginx // nginx相关配置
│   │   ├── conf.d // 扩展配置目录
│   │   │   └── vhosts.conf // 虚拟主机定义
│   │   └── nginx.conf // 主配置文件
│   ├── php // php配置文件存放目录
│   │   ├── conf.d // 扩展存放目录
│   │   │   └── opcache.ini.bak // 配置opcache，我没有启用他
│   │   └── php.ini // php主配置文件
│   └── redis // redis 配置文件
│       └── redis.conf // redis主配置文件
└── README.md
```

## 创建应用目录
在上面的`docker-lnmp/etc/nginx/conf.d/vhosts.conf`中，我已经定义了一个`test.web`的虚拟主机，其根目录为`/www/test.web`，现在来创建下这个目录

```
mkdir -pv /www/nginx/html  /www/test.web
```
## 启动
```
docker-compose up -d --build
```
注意：如果修改了类似`./etc/nginx/conf.d/vhost.conf`的配置文件，如果需要生效必须执行:
```
docker-compose up -d --force-recreate
```
而不是
```
docker-compose up -d
```

## 访问测试
* 编辑`/etc/hosts`，追加添加如下行：
```
127.0.0.1 test.web
```
* 如果在其它机器访问，也需要配对应机器的`/etc/hosts`文件，指向该host机的ip地址即可，同时，需要注意`防火墙`和`selinux`的影响。
* 最后
curl test.web // 提示403, 因为我们应用目录是空目录


:stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye:

