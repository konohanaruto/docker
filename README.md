# docker(lnmp)
以下假设你已经安装了[Docker](https://www.docker.com/)
## 快速开始

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

如果修改了`./etc/nginx/conf.d/vhost.conf`文件，如果需要生效必须执行:<br />
```
docker-compose up -d --force
```
而不是
```
docker-compose up -d
```
