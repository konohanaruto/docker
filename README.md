# docker(lnmp)
> 以下假设你已经安装了[Docker](https://www.docker.com/)
### 目录结构
```
├── build
│   ├── mysql
│   │   └── Dockerfile
│   ├── nginx
│   │   └── Dockerfile
│   └── php
│       └── Dockerfile
├── data
│   └── mysql
│       ├── 1aeb68324fb9.pid
│       ├── auto.cnf
│       ├── ca-key.pem
│       ├── ca.pem
│       ├── client-cert.pem
│       ├── client-key.pem
│       ├── ib_buffer_pool
│       ├── ibdata1
│       ├── ib_logfile0
│       ├── ib_logfile1
│       ├── ibtmp1
│       ├── mysql [error opening dir]
│       ├── performance_schema [error opening dir]
│       ├── private_key.pem
│       ├── public_key.pem
│       ├── server-cert.pem
│       ├── server-key.pem
│       ├── sys [error opening dir]
│       └── test [error opening dir]
├── docker-compose.yml
├── etc
│   ├── mysql
│   │   ├── conf.d
│   │   └── my.cnf
│   ├── nginx
│   │   ├── conf.d
│   │   │   └── vhosts.conf
│   │   └── nginx.conf
│   └── php
│       └── php.ini
├── mysql.env
└── README.md
```
## 目录结构和文件说明
* `build` 目录为context上下文目录，用于`docker-compose.yml`文件中用到`build`指令的存放`Dockerfile`目录
* `build/nginx/Dockerfile`、`build/mysql/Dockerfile`以及`build/php/Dockerfile`仅仅简单的参照了官方镜像，没有做额外配置
* `data` 目录为mysql数据目录
* `docker-compose.yml` 参考：[docker composer documentation](https://docs.docker.com/compose/compose-file)
* `etc` 为统一存放配置文件的目录
* `etc/nginx/conf.d/vhosts.conf` 配置了相关演示地址，并且能访问成功
* `mysql.env` 用于存放mysql官方镜像要求的必须存在的环境变量[docker/mysql from hub.docker.com](https://hub.docker.com/_/mysql)

## mysql环境变量的官方说明

> When you start the mysql image, you can adjust the configuration of the MySQL instance by passing one or more environment variables on the docker run command line. Do note that none of the variables below will have any effect if you start the container with a data directory that already contains a database: any pre-existing database will always be left untouched on container startup.
>
> See also https://dev.mysql.com/doc/refman/5.7/en/environment-variables.html for documentation of environment variables which MySQL itself respects (especially variables like MYSQL_HOST, which is known to cause issues when used with this image).
>
> MYSQL_ROOT_PASSWORD
> This variable is mandatory and specifies the password that will be set for the MySQL root superuser account. In the above example, it was set to my-secret-pw.
>
> MYSQL_DATABASE
> This variable is optional and allows you to specify the name of a database to be created on image startup. If a user/password was supplied (see below) then that user will be granted superuser access (corresponding to GRANT ALL) to this database.
>
> MYSQL_USER, MYSQL_PASSWORD
> These variables are optional, used in conjunction to create a new user and to set that user's password. This user will be granted superuser permissions (see above) for the database specified by the MYSQL_DATABASE variable. Both variables are required for a user to be created.
> 
> Do note that there is no need to use this mechanism to create the root superuser, that user gets created by default with the password specified by the MYSQL_ROOT_PASSWORD variable.
> 
> MYSQL_ALLOW_EMPTY_PASSWORD
> This is an optional variable. Set to yes to allow the container to be started with a blank password for the root user. NOTE: Setting this variable to yes is not recommended unless you really know what you are doing, since this will leave your MySQL instance completely unprotected, allowing anyone to gain complete superuser access.
>
> MYSQL_RANDOM_ROOT_PASSWORD
> This is an optional variable. Set to yes to generate a random initial password for the root user (using pwgen). The generated root password will be printed to stdout (GENERATED ROOT PASSWORD: .....).
>
> MYSQL_ONETIME_PASSWORD
> Sets root (not the user specified in MYSQL_USER!) user as expired once init is complete, forcing a password change on first login. NOTE: This feature is supported on MySQL 5.6+ only. Using this option on MySQL 5.5 will throw an appropriate error during initialization.
```

## 快速开始

1. 创建相关目录
```
mkdir -pv /www/nginx/html /www/test.web /var/log/nginx /var/log/mysql
```

2. 添加相关用户
```
groupadd -r mysql
useradd -r -g mysql mysql
useradd -r www-data
```

## 启动
```
docker-compose up -d --build
```

注意：如果修改了类似`./etc/nginx/conf.d/vhost.conf`的配置文件，如果需要生效必须执行:<br />

```
docker-compose up -d --force
```
而不是
```
docker-compose up -d
```

:stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye::stuck_out_tongue_winking_eye:
