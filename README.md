# 介绍

官方支持网站：https://www.llstack.com/ols/

**OLStack 社区容器版**，是基于 Docker 容器化编排的 OpenLiteSpeed 环境。性能比Nginx更胜一筹，基本兼容 Apache HTTPD 生态，主要是不支持自动加载 .htaccss 文件，该版本对操作系统环境没有限制，未来可以应用到非常多的场景中。

OpenLiteSpeed 是 LiteSpeed EnterPrise 的社区版本，相较 Nginx 很多扩展如 Brotli、nginx*-*cache*-*purge 等扩展，会因为更新的不及时导致对最新Stable版本的不支持，同时也没有企业级的保障。 而 OpenLiteSpeed 的组件有官方进行主要维护和更新，提供商用企业级的体验。

在性能上，LiteSpeed Tech 提供的 BenchMark 中，在 WordPress、Joomla、OpenCart、ModSecurity、小型静态文件、HTTP/2、HTTP/3 的测试上都比 Apache HTTPD 和 Nginx 有这更好的表现，这不仅仅是跑个 Hello World 而是进行一个完整的测试。

这是 [litespeedtech](https://github.com/litespeedtech)/**[ols-docker-env](https://github.com/litespeedtech/ols-docker-env)** 的一个复克（Fork）。

# 安装环境

## 国内服务器准备环境

一、安装 Docker 环境，已有可以跳过

```bash
curl -sSL https://get.daocloud.io/docker | sh   
```

二、安装 Docker-Compose 环境，其中`1.25.3`  可以根据 [**最新版本**](https://github.com/docker/compose/releases) 修改，已有可以跳过

```bash
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.3/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

三、下载 OLStack

```bash
## 没有下载 git 的可以通过 apt install git -y 或者 yum install git -y 安装
git clone https://gitee.com/LLStack/OLStack.git
cd OLStack
```

## 海外服务器准备环境

一、安装 Docker 环境，已有可以跳过

```bash
curl -s https://get.docker.com | sudo sh
```

二、安装 Docker-Compose 环境，其中`1.25.4`  可以根据 [**最新版本**](https://github.com/docker/compose/releases) 修改，已有可以跳过

```bash
curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

三、下载 OLStack

```bash
## 没有下载 git 的可以通过 apt install git -y 或者 yum install git -y 安装
git clone https://github.com/LLStack/OLStack.git
cd OLStack
```

## 编辑配置文件

四、编辑 `.env` 和 `docker-compose.yml`文件：

 `.env` 文件包括了对一些版本的定义。 可以具体看 .env 说明解析。

`docker-compose.yml`文件则是定义具体安装什么容器组件，包括 Redis、phpmyadmin 等。 可以具体看 docker-compose.yml 解析。

```bash
vi .env
vi docker-compose.yml
```

::: tip 提示
不会 vi 的同学，可以用 FileZilla、XFTP 这类的支持 SFTP 协议的软件，将文件下载后编辑再上传。
:::

五、 启动容器

```bash
docker-compose up -d
```

六、 启动说明：

```bash
docker-compose up ## 临时启动所有容器
docker-compose up -d ## 持久化启动所有容器
docker-compose stop ## 停止容器运行
docker-compose down ## 停止和删除所有容器
```

# 配置说明

## .ENV配置

 `.env` 文件包括了对一些版本的定义，由于是 `.` 开头的文件，可能在部分电脑显示中是隐藏的，所以需要开放显示隐藏的文件。

**说明如下：**

**一、时区设置**，定义所在业务的时区。默认是 `Asia/Shanghai`，例如服务的是美国东部的，则可以修改为 `America/New_York` 纽约时间。

```bash
TimeZone=Asia/Shanghai
```

**二、OpenLiteSpeed 版本**，目前 OLS 提供了 1.6.X 和 1.5.X 两个版本，未来可能提供更多的版本。

```bash
LITESPEED=ols1.6.9
```

可供修改的选项：`ols1.6.9` 、`ols1.5.11`以及以上版本，版本查看：

https://openlitespeed.org/release-log/

**三、PHP版本**，由 LiteSpeed 官方提供支持的 LSPHP 版本，和很多虚拟主机使用的企业版是一样的。

```bash
PHPVER=php73
```

目前提供了：`php74`、`php73`、`php72`、`php71`、`php70`、`php56`、`php55`、`php54`、`php53`

不同的 PHP 版本底层基于的 Ubuntu 版本也不一样。

- php70～74 底层系统为 Ubuntu 18.04。
- php54～56 底层系统为 Ubuntu 16.04。 **PHP不受官方支持**
- php53 底层系统为 Ubuntu 14.04。 **PHP和系统均不受官方支持，仅建议测试**

::: tip 提示
PHP 每个版本的官方生命支持周期是三年，如果程序支持建议安装最新版本
查看PHP版本支持情况：http://php.net/supported-versions.php
:::

**四、MySQLTYPE**，运行数据库的类型。

```bash
MySQLTYPE=mariadb
```

可供修改的选项：`mariadb` 、`percona`

MariaDB 和 Percona 更开发并且提供更多的功能选项比默认的 MySQL 好用。

**五、MySQLVER**，数据库的具体版本。

```bash
MySQLVER=10.3
```

MariaDB 目前提供了：`10.4` 、`10.3`、 `10.2`  （兼容 MySQL5.7）  `10.1` （兼容MySQL5.6）

Percona 目前提供了：`8.0` 、`5.7`、 `5.6`  （这个兼容关系，不说你也知道）

::: tip 提示
由于 Docker 容器的便利性，大家如果需要 PostGreSQL、SQL Server、MongoDB、Elasticsearch 都可以直接修改 `docker-compose.yml`文件来进行实现的。
:::

**六、创建的默认数据库和用户**

```bash
## 默认数据库名称
MYSQL_DATABASE=llstack
## 默认数据库 root 账号密码
MYSQL_ROOT_PASSWORD=password
## 默认的新建用户名
MYSQL_USER=llstack
## 默认的新建用户密码
MYSQL_PASSWORD=password
```

**七、REDIS_VERSION**，Redis的版本配置

```bash
REDIS_VERSION=5.0-alpine
```

可供修改的选项：`6.0-rc-alpine`、`5.0-alpine`、`4.0-alpine`、`3.2-alpine`、`2.8`

**八、DOMAIN**，默认配置的域名，可以保持默认，也可以输入为自己的默认域名，建议后面新建主机。

```bash
DOMAIN=localhost
```

## docker-compose.yml 配置

`docker-compose.yml` 模板文件是使用 Docker Compose 的核心，涉及到的指令关键字也比较多。

如果有需要学习的同学可以查看文档：**[Compose 模板文件](https://yeasy.gitbooks.io/docker_practice/compose/compose_file.html)**

这里举例几个 OLStack 的修改方案：

**一、挂载目录**

```yaml
    volumes:
        - ./config/lsws/conf:/usr/local/lsws/conf
        - ./config/lsws/admin-conf:/usr/local/lsws/admin/conf
        - ./bin/container:/usr/local/bin
        - ./sites:/var/www/vhosts/
        - ./acme:/root/.acme.sh/
        - ./logs/lsws/:/usr/local/lsws/logs/
```

`:`前的是宿主机（这台服务器）的对应目录，这里使用相对路径。`:`后的是容器主机所对应的目录，如果有其他的目录挂载需求可以修改`volumes:`进行挂载。

二、开放的端口**

```yaml
    ports:
      - 80:80
      - 443:443
      - 443:443/udp
      - 7080:7080
```

这里开放了三个TCP端口：80（HTTP）、443（HTTPS）和7080（OLS后台）。

还有一个UDP端口：443（QUIC、HTTP/3）

如果有更多的需求，可以新增新的端口。

安全起见，可以将`- 7080:7080` 修改为更安全的例如：`- 27080:7080` 这样的非默认端口，减少被安全攻击的可能。

**三、启动带#的功能**

默认绿的带 `#` 的都是不启用的功能：

```yaml
#  phpmyadmin:
#    image: phpmyadmin/phpmyadmin:latest
#    container_name: phpmyadmin
#    ports:
#      - "8081:80"
#    environment:
#      - PMA_HOST=mysql
#      - PMA_PORT=3306
#      - TZ=${TimeZone}
```

像 phpmyadmin、phpredisadmin、memcached 目前都是默认关闭的。 如果有需要需要去掉最前面的`#`，然后重新运行容器编排。

::: warning 警告
adminer、phpmyadmin、phpredisadmin 在不使用的时候，建议关闭。
:::

# 目录结构

### LiteSpeed 容器

```yaml
    volumes:
        - ./config/lsws/conf:/usr/local/lsws/conf  ## OLS的配置文件目录
        - ./config/lsws/admin-conf:/usr/local/lsws/admin/conf  ## OLS的管理控制台目录
        - ./bin/container:/usr/local/bin  ## 相关工具文件
        - ./sites:/var/www/vhosts/  ## 虚拟主机存放的位置
        - ./acme:/root/.acme.sh/  ## Let's Encrypt 生成的证书存放地址
        - ./logs/lsws/:/usr/local/lsws/logs/  ##OLS 的日志地址
```

最重要的是 `- ./sites:/var/www/vhosts/` 和 `- ./logs/lsws/:/usr/local/lsws/logs/`

这里是一个相对路径，如果 OLStack 的目录在 `/home/webserver/OLStack` 那么`./sites`的实际路径就是 `/home/webserver/OLStack/OLStack`。

如果有额外数据盘的服务器，那么建议将 OLStack 目录放在数据盘下运行。

### MySQL 容器

```yaml
    volumes:
      - "./data/mysql:/var/lib/mysql:delegated"
```

`./data/mysql`存放数据库物理文件的目录。

有自定义修改 `my.cnf` 需求的同学，可以修改 docker-compose.yml 文件挂载对应文件。

### Redis-Server 容器

```yaml
    volumes:
      - ./config/redis/redis.conf:/etc/redis.conf
      - ./data/redis/data:/data
      - ./logs/redis/:/var/log/redis/
```

`- ./config/redis/redis.conf:/etc/redis.conf` 配置文件，有中文注释

` - ./data/redis/data:/data` 持久化物理文件目录

`- ./logs/redis/:/var/log/redis/` Redis-Server日志目录

# 使用说明

::: warning 提示
使用下面的命令好，首先得进入`OLStack` 目录
:::

### 修改 LiteSpeed WebAdmin 密码

```bash
bash bin/webadmin.sh <your_password>
```

例如我想要修改为`123456` 那么输入：

```bash 
bash bin/webadmin.sh 123456
```

### 创建虚拟主机

```bash
bash bin/domain.sh -add <your_domain.com>
```

例如我想要创建域名为 `mf8.biz` 的虚拟主机那么输入，自带 `www.` 不需要重复输入：

```bash
bash bin/domain.sh -add mf8.biz
```

### 删除虚拟主机

```bash
domain.sh -del <your_domain.com>
```

### 创建数据库

下面命令会自动生成用户名、密码和数据库名。使用以下内容自动生成：

```bash
bash bin/database.sh -domain <your_domain.com>
```

用如下方式进行自定义用户名、密码和数据库名，替换`user_name`，`my_password`以及`database_name`为想要的值：

```bash
bash bin/database.sh -domain <your_domain.com> -user user_name -password my_password -database database_name
```

### 连接数据库

正常使用数据库，在站库不分离的场景下一般数据库连接地址都是填写：`127.0.0.1`或者`localhost`。

在 Docker 环境中，因为数据库和执行语言是分开运行的，所以并不是在同一台“服务器”当中，自然无法使用本地连接地址。我们需要使用 `mysql` 来进行代替。

# 使用说明

### 配置SSL证书

SSL 证书通过 ACME 申请 Let's Encrypt 免费证书实现，首次运行需要安装 ACME。

#### 安装ACME

仅 **第一次** 运行需要安装ACME，带电子邮件通知运行：

```bash
./bin/acme.sh --install -email <EMAIL_ADDR>
```

例如：

```bash
./bin/acme.sh --install -email cert@mf8.biz
```

不需要电子邮件通知运行：

```
./bin/acme.sh --install --no-email
```

#### 申请证书

在此命令中使用根域名，不需要填写 `www.` 会自动添加`www.`：

```
./bin/acme.sh -domain <yourdomain.com>
```

例如：

```bash
./bin/acme.sh -domain mf8.biz
```

则会自动签发` www.mf8.biz` 和 `mf8.biz` 两个证书

### 更新OLS版本

要将 OpenLiteSpeed 升级到最新的稳定版本，请运行

```bash
bash bin/webadmin.sh -lsup
```

### 安装WAF防火墙

使用 ModSecurity 实现防火墙WAF功能：

Web服务器上启用WAF ：

```bash
bash bin/webadmin.sh -modsec enable
```

Web服务器上禁用WAF ：

```
bash bin/webadmin.sh -modsec disable
```

### phpMyAdmin

访问地址：

http://yourip:8080

http://yourip:8443

默认用户名是`root`，密码与您在`.env`文件中提供的密码相同。

### 进入容器内部

```bash
docker exec -it litespeed /bin/sh # 进入 OpenLiteSpeed、PHP 容器
docker exec -it mysql /bin/bash # 进入 MariaDB/Percona Server容器
docker exec -it redis /bin/sh # 进入 Redis Server容器
```

只要定义了容器名称：container_name ，那么替换 <container_name> 为容器名称的名字即可进入。

```bash
docker exec -it <container_name> /bin/sh 
```

# 容器教程

## Docker 使用教程

前面 docker run 后面 `–name litespeed` 中的 `litespeed` 为 `$name`，其代表容器识别符，也就是 `$name=litespeed`。

一、定义name变量，也可以修改为 mysql、redis 等

```bash
name=litespeed
```

二、查看容器在线状态及大小

```bash
docker ps -as
```

三、查看容器的运行输出日志

```bash
docker logs $name
```

四、重新启动容器，一般在修改除端口外的配置后使用使修改生效

```bash
docker restart $name
```

五、停止容器的运行

```bash
docker stop $name
```

六、移除容器

```bash
docker rm $name
```

七、查看 docker 容器占用 CPU，内存等信息

```bash
docker stats --no-stream
```

## Docker-Compose 使用教程

::: warning 提示
首先得进入有 `docker-compose.yml` 模板文件的目录。
:::

```bash
docker-compose up ## 临时启动所有容器
docker-compose up -d ## 持久化启动所有容器
docker-compose stop ## 停止容器运行
docker-compose down ## 停止和删除所有容器
```

如果修改过`docker-compose.yml`文件，则需要重新构建。

```bash
docker-compose up --build
```

# 其他链接

[LLStack OLStack 社区版容器镜像](https://github.com/LLStack/OLStack-Dockerfiles)

[米饭粑](https://www.mf8.biz/)