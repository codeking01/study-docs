## Git的补充

`删除文件提交`

```git
#第一步：先pull远程代码，保持同步
git pull

#第二步：删除文件
# 删除单个文件
git rm 文件名 --cached
# 删除文件夹 -r是递归的删除
git rm -r 文件夹名 --cached

#第三步：提交
git commit -m "备注信息"
#第四步：推送
# 这个git push 前提是你已经使用过这个命令git push -u origin master
git push
```

git常见命令

```git
# 初始化
git init

# 提交到缓存区(所有文件)
git add .

#提交文件
git commit -m "文件说明"

# 定义远程
git remote add orignal(别名) http://XXXXXX

#修改远程
git remote set-url orignal(别名) http://XXXXXX

# 推送
git push orignal(远程) master(默认本地的)

#拉取
git pull http://xxxx
```

## yarn配置

```
配置国内镜像-淘宝镜像
yarn config set registry https://registry.npm.taobao.org

设置代理
yarn config set proxy http://username:password@server:port
yarn confit set https-proxy http://username:password@server:port

例：npm config set https-proxy http://xiaoming:123456@192.168.1.1:8080

取消代理
yarn config delete proxy
yarn config delete https-proxy
# 原文链接：https://blog.csdn.net/lubin100/article/details/89512260
```

```
yarn的配置项：

yarn config list // 显示所有配置项
yarn config get <key> //显示某配置项
yarn config delete <key> //删除某配置项
yarn config set <key> <value> [-g|--global] //设置配
# https://blog.csdn.net/yw00yw/article/details/81354533
```

```
# 全局安装
yarn global add @vue/cli
```

## Docker安装Vim,并解决乱码

安装

```
apt-get update
apt-get install vim
# 出错了就接着install
```

解决乱码

```
cd etc/vim
vim vimrc
```

```
# 文件最后加上
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
```

## 一、配置YUM

`查看yum配置源`

一、验证网络是否可以连接阿里云镜像

```bash
# 验证网络是否可以连接阿里云镜像
ping mirrors.aliyun.com
```

二、 手动配置

1、删除原yum源

```bash
cd /etc/yum.repos.d

# 删除原yum源
rm -rf /etc/yum.repos.d/*
```

2、下载阿里云Centos-7.repo文件 

```bash
# wget命令下载: wget [options] [url]
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

# curl命令下载: curl [options] [url]
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

3、清除及生成缓存

```bash
# 清除yum缓存
yum clean all

# 缓存阿里云镜像
yum makecache
```

4、查看yum源信息，已经更换为了阿里云镜像源

```bash
yum repolist
# 在源名称后面的 base后面都出现了mirros.aliyun.com 这个代表成功了
```

补充：如何关闭防火墙

```bash
systemctl stop firewalld.service

# 永久关闭·

systemctl disable firewalld.service
```

二、docker下载

关闭参考`https://www.kuangstudy.com/bbs/1570972251397271554`

`[《狂神说Java》docker教程通俗易懂-KuangStudy-文章](https://www.kuangstudy.com/bbs/1552836707509223426)`

1、更新yum源

```bash
yum update
```

2、安装所需环境

```bash
yum install -y yum-utils device-mapper-persistent-data lvm2
```

3、配置yum仓库

```bash
# 因为我之前升级了python3.8 所有这个地方需要改一下版本
vim /usr/bin/yum-config-manager

#升级yum的参考链接
# https://blog.csdn.net/bisal/article/details/104853055?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166511518016782428695885%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166511518016782428695885&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-104853055-null-null.142^v51^control,201^v3^control_2&utm_term=%E5%A6%82%E4%BD%95%E5%8D%87%E7%BA%A7yum&spm=1018.2226.3001.4187

# 第一行修改
`#!/usr/bin/python2 -tt`
```

```bash
# 国外的
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.re 
# 国内的
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

```bash
# 安装docker
yum install docker-ce
```

`配置镜像加速`

对于使用 upstart 的系统而言，请在 /etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件）：

```bash
{"registry-mirrors":["https://yxzrazem.mirror.aliyuncs.com"]}
```

```bash
# 重启服务
sudo systemctl daemon-reload
sudo systemctl restart docker
```

5、启动Docker 、添加自启动

```bash
systemctl start docker
systemctl enable docker
```

## 二、Docker入门使用

1、Docker基础命令

`docker的启停`

```bash
systemctl start docker ## 启动
systemctl stop         ## 停止
systemctl restart       ## 重启
systemctl enable docker   ##跟随服务启动而自启
```

`docker的基本信息`

```bash
systemctl status docker ##docke运行状态
## docker版本信息
docker version
docker info
```

2、Docker镜像命令

`查看本地镜像列表`

```bash
docker images
```

`搜索镜像`

```bash
docker search 镜像名
docker search --filter=STARS=9000 mysql 搜索 STARS >9000的 mysql 镜像
```

`拉取镜像 不加tag(版本号) 即拉取docker仓库中 该镜像的最新版本latest 加:tag 则是拉取指定版本`

```bash
docker pull 镜像名 
docker pull 镜像名:tag
##运行镜像 
docker run 镜像名
```

`删除镜像 ———当前镜像没有被任何容器使用才可以删除`

```bash
##删除一个
docker rmi -f 镜像名/镜像ID
##删除多个 其镜像ID或镜像用用空格隔开即可 
docker rmi -f 镜像名/镜像ID 镜像名/镜像ID 镜像名/镜像ID
##删除全部镜像  -a 意思为显示全部, -q 意思为只显示ID
docker rmi -f $(docker images -aq）
##强制删除镜像
docker image rm 镜像名称/镜像ID
```

`保存镜像 将我们的镜像 保存为tar 压缩文件 这样方便镜像转移和保存 ,然后 可以在任何一台安装了docker的服务器上 加载这个镜像`

```bash
docker save 镜像名/镜像ID -o 镜像保存在哪个位置与名字
##exmaple:
docker save tomcat -o /myimg.tar
##保存镜像任务执行完毕，我们来看下指定位置下是否有该tar？
## 加载镜像
#任何装 docker 的地方加载镜像保存文件,使其恢复为一个镜像
docker load -i 镜像保存文件位置
```

镜像标签 有的时候呢，我们需要对一个镜像进行分类或者版本迭代操作，比如我们一个微服务已经打为docker镜像，但是想根据环境进行区分为develop环境与alpha环境，这个时候呢，我们就可以使用Tag，来进对镜像做一个标签添加，从而行进区分；版本迭代逻辑也是一样，根据不同的tag进行区分 

app:1.0.0 基础镜像

分离为开发环境：app:develop-1.0.0

分离为alpha环境：app:alpha-1.0.0

```bash
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
docker tag 源镜像名:TAG 想要生成新的镜像名:新的TAG
如果省略TAG 则会为镜像默认打上latest TAG
docker tag aaa bbb
上方操作等于 docker tag aaa:latest bbb:test
```

示例

```bash
我们根据镜像 quay.io/minio/minio 添加一个新的镜像 名为 aaa 标签Tag设置为1.2.3
docker tag quay.io/minio/minio:1.2.3 aaa:1.2.3
我们根据镜像 app-user:1.0.0 添加一个新的镜像 名为 app-user 标签Tag设置为alpha-1.0.0
docker tag app-user:1.0.0 app-user:alpha-1.0.0
```

3、Docker容器命令

`查看正在运行容器列表`

```bash
docker ps
```

`查看所有容器 ——-包含正在运行 和已停止的`

```bash
docker ps -a
```

`运行一个容器`

容器怎么来呢 可以通过run 镜像 来构建 自己的容器实例

```bash
## -it 表示 与容器进行交互式启动 -d 表示可后台运行容器 （守护式运行）  --name 给要运行的容器 起的名字  /bin/bash  交互路径
docker run -it -d --name 别名 镜像名:Tag  /bin/bash 
例如我们要启动一个redis 把它的别名取为redis001 并交互式运行 需要的命令 —我这里指定版本号为5.0.5
#1. 拉取redis 镜像
docker pull redis:5.0.5
#2.命令启动
docker run -it -d --name redis001 redis:5.0.5 /bin/bash
#器端口与服务器端口映射： -p 宿主机端口:容器端口
# -p 8888:6379 解析 将容器内部的 6379端口与docker 宿主机（docker装在哪台服务器 哪台服务器就是宿主机）8888 端口进行映射 那通过外部访问宿主机8888端口 即可访问到 docker 容器 6379 端口了
docker run -itd --name redis002 -p 8888:6379 redis:5.0.5 /bin/bash
```

`删除容器`

```bash
#删除一个容器
docker rm -f 容器名/容器ID
#删除多个容器 空格隔开要删除的容器名或容器ID
docker rm -f 容器名/容器ID 容器名/容器ID 容器名/容器ID
#删除全部容器
docker rm -f $(docker ps -aq)
```

`进入、退出容器方式`

```bash
docker exec -it 容器名/容器ID /bin/bash
docker exec -it redis001 /bin/bash
```

`直接退出 未添加 -d(持久化运行容器) 时 执行此参数 容器会被关闭`

```bash
exit
优雅退出 --- 无论是否添加-d 参数 执行此命令容器都不会被关闭
Ctrl + p + q
#停止容器
docker stop 容器ID/容器名
#重启容器
docker restart 容器ID/容器名
#启动容器
docker start 容器ID/容器名
#kill 容器
docker kill 容器ID/容器名
```

`容器文件拷贝 —无论容器是否开启 都可以进行拷贝`

```bash
docker cp 容器ID/名称:文件路径  要拷贝到外部的路径   |     要拷贝到外部的路径  容器ID/名称:文件路径
#从容器内 拷出
docker cp 容器ID/名称: 容器内路径  容器外路径
#从外部 拷贝文件到容器内
docker  cp 容器外路径 容器ID/名称: 容器内路径
```

`查看容器日志`

```bash
docker logs -f --tail=要查看末尾多少行 默认all 容器ID
#启动容器时，使用docker run命令时 添加参数--restart=always 便表示，该容器随docker服务启动而自动启动
docker run -itd --name redis002 -p 8888:6379 --restart=always  redis:5.0.5 /bin/bash
```

`docker 文件分层与数据卷挂载`

命令:  
-v 宿主机文件存储位置:容器内文件位置  
如此操作，就将 容器内指定文件挂载到了宿主机对应位置，-v命令可以多次使用，即一个容器可以同时挂载多个文件

运行一个docker redis 容器 进行 端口映射 两个数据卷挂载 设置开机自启动

```bash
docker run -d -p 6379:6379 --name redis505 --restart=always  -v /var/lib/redis/data/:/data -v /var/lib/redis/conf/:/usr/local/etc/redis/redis.conf  redis:5.0.5 --requirepass "password"
```

4、自己提交一个镜像

我们运行的容器可能在镜像的基础上做了一些修改，有时候我们希望保存起来，封装成一个更新的镜像，这时候我们就需要使用 commit 命令来构建一个新的镜像

```bash
docker commit -m="提交信息" -a="作者信息" 容器名/容器ID 提交后的镜像名:Tag
例如：
docker commit -m="gitlab初始化" -a="Colin.huang" 8e5e89512ffd gitlab_colin:15.3.3
```

我们拉取一个tomcat镜像 并持久化运行 且设置与宿主机进行端口映射

```bash
docker pull tomcat
docker run -itd -p8080:8080 --name tom tomcat /bin/bash
```

## 三、Docker部署安装gitlab

1、安装gitlab

1.1下载

```bash
docker pull gitlab/gitlab-ce:14.3.6-ce.0
```

1.2创建并启动容器

`创建容器`

```bash
docker run -d -p 8443:443 -p 8080:80 -p 8022:22 --restart always --name gitlab -v /data/gitlab/etc:/etc/gitlab -v /data/gitlab/log:/var/log/gitlab -v /data/gitlab/data:/var/opt/gitlab --privileged=true gitlab/gitlab-ce
```

`命令解释`

```bash
docker run 
-d                #后台运行，全称：detach
-p 8443:443      #将容器内部端口向外映射
-p 8090:80       #将容器内80端口映射至宿主机8090端口，这是访问gitlab的端口
-p 8022:22       #将容器内22端口映射至宿主机8022端口，这是访问ssh的端口
--restart always #容器自启动
--name gitlab    #设置容器名称为gitlab
-v /data/gitlab/etc:/etc/gitlab    #将容器/etc/gitlab目录挂载到宿主机/usr/local/gitlab/etc目录下，若宿主机内此目录不存在将会自动创建
-v /data/gitlab/log:/var/log/gitlab    #与上面一样
-v /data/gitlab/data:/var/opt/gitlab   #与上面一样
--privileged=true         #让容器获取宿主机root权限
gitlab/gitlab-ce    #镜像的名称，这里也可以写镜像ID
```

`查看容器是否启动`

```bash
docker ps #查看已经启动的容器
docker ps -all 查看所有创建的容器
```

2、修改配置

2.1进入容器

```bash
docker exec -it gitlab bash
```

2.2修改gitlab.rb文件

```bash
//先进入到gitlab目录
cd /etc/gitlab   
//编辑gitlab.rb文件  
vi gitlab.rb
```

2.3修改gitlab.rb文件中的IP与端口号

```bash
// 在gitlab创建项目时候http地址的host(不用添加端口)
external_url 'http://192.168.189.129'
//配置ssh协议所使用的访问地址和端口
gitlab_rails['gitlab_ssh_host'] = '192.168.189.129' //和上一个IP输入的一样
gitlab_rails['gitlab_shell_ssh_port'] = 8022 // 此端口是run时22端口映射的8022端口
:wq //保存配置文件并退出
```

2.4配置gitlab.yml文件

```bash
## 文件路径 /opt/gitlab/embedded/service/gitlab-rails/config
cd /opt/gitlab/embedded/service/gitlab-rails/config
## 打开编辑gitlab.yml文件
vi gitlab.yml
## 修改host 与上面.rb文件修改的一致
## 修改port 为8090
```

3、启动、访问gitlab

```bash
//容器中应用配置，让修改后的配置生效
gitlab-ctl reconfigure
//容器中重启服务
gitlab-ctl restart 
## 访问gitlab
http://192.168.189.129:8080
## 查看root 账户密码
docker exec -it gitlab /bin/bash
cat /etc/gitlab/initial_root_password
```

`修改root账户密码`

```bash
# 进入容器内部
docker exec -it gitlab /bin/bash
# 进入控制台
gitlab-rails console -e production
# 查询id为1的用户，id为1的用户是超级管理员
user = User.where(id:1).first
# 修改密码为colin123456
user.password='colin123456'
# 保存
user.save!
# 退出
exit
```

重新输入账户密码，成功访问页面啦！

## 四、Docker部署Django项目（Nginx,usgi）

`python项目打包库`

打包使用的库

下载pipreqs模块

```python
pip install pipreqs
```

1.进入项目根目录，然后打包

```
pipreqs ./ --encoding=utf8
```

即可在项目根目录下看见requirements.txt文件，里面带有依赖库和版本号

2.复制 requirements.txt 到新机器，在所在目录执行命令安装：pip install -r requirements.txt

`Docker方面`

1.拉取镜像

```bash
docker pull nginx
```

2.使用命令`docker run --name my-nginx -p 80:80 -d nginx:latest`，创建一个容器，命令为my-nginx，`-d`表示后台运行并返回容器的ID，`-p`将端口指定到宿主的80端口。打开浏览器访问80端口

3.用命令`docker exec -it my-nginx bash`开启一个交互模式终端 其中第一步不是必须，在执行`docker run`命令时找不到镜像时会自动从仓库拉取

4.项目内容设置完后，在项目根目录输入命令(后面有一个 **.**)

`docker build -t django_docker_img:v1 .`

创建镜像，使用命令`docker images`查看镜像，可以看见刚刚创建的镜像

## 五、Docker 启动redis6

```bash
# myredis 是自己的容器名称
docker exec -it myredis redis-cli
```

进入容器以后

输入 `auth 密码`完成登录

在 idea中直接输入密码，不要输入用户名（因为没设置）

```bash
Docker容器的重启策略如下：

如：--restart=always

no，默认策略，在容器退出时不重启容器
on-failure，在容器非正常退出时（退出状态非0），才会重启容器
on-failure:3，在容器非正常退出时重启容器，最多重启3次
always，在容器退出时总是重启容器
unless-stopped，在容器退出时总是重启容器，但是不考虑在Docker守护进程启动时就已经停止了的容器。

```

## 六、Docker部署mysql8.0.27

**排错**

```
 # 查看端口，注意要有分号
show global variables like 'port'; 
# docker 部署的时候 -p 外部暴露端口:容器端口
```

1.拉取镜像

2.运行容器

```parms
# 参数解释
-net 设置模式 host,none,等
-m 分配内存
-v 某个容器的目录:映射centos上的某个目录(根据实际的设置)
-e 设置环境变量
MYSQL_ROOT_PASSWORD 指定数据库密码，账户名默认是root
lower_case_table_names=1 关闭数据库名大小写区分
```

```
# 参考模板
docker run -it -d -p 3308:3306  --name mysql8_sec  \
-m 500m -v /data/mysql8_sec/data:/var/lib/mysql \
-v /data/mysql8_sec/config:/etc/mysql/conf.d  \
-e MYSQL_ROOT_PASSWORD=123456 \
-e TZ=Asia/Shanghai mysql:latest \
--lower_case_table_names=1
```

3.配置支持远程

进入容器

```
docker exec -it mysql8_sec /bin/bash
mysql -uroot -p123456
```

```
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
flush privileges;
# 刷新数据库
flush privileges;
```

## 六、Docker部署mysql8.0 主从数据库（存在问题）

```bash
docker pull mysql:8.0
#docker pull mysql:latest
```

1.详细配置参数，下面有一个简单版本

记得改参数！

```docker
docker run -p 3307:3306 --name mysql8-slave  -v /data/mysql-slave/log:/var/log/mysql  -v /data/mysql-slave/data:/var/lib/mysql  -v /data/mysql-slave/conf:/etc/mysql  -v /data/mysql-slave/mysql-files:/var/lib/mysql-files  -e MYSQL_ROOT_PASSWORD=123456  --restart=no -d mysql:latest  --lower_case_table_names=1
```

参数详细解释

```docker
docker run -p 3306:3306 --name mysql8 \

-v /data/mysql/log:/var/log/mysql \

-v /data/mysql/data:/var/lib/mysql \

-v /data/mysql/conf:/etc/mysql \

-v /data/mysql/mysql-files:/var/lib/mysql-files \

-e MYSQL_ROOT_PASSWORD=123456 \

--restart=no -d mysql:latest \

--lower_case_table_names=1

参数说明

-p 3306:3306：将容器的3306端口(冒号后面的)映射到主机的3306端口（冒号前面的）

-v /mydata/mysql/log:/var/log/mysql：将日志文件夹挂载到主机

-v /mydata/mysql/data:/var/lib/mysql\：将mysql产生的数据挂载到主机

-v /mydata/mysql/conf:/etc/mysql\：将配置文件夹挂载到主机

-v /mydata/mysql/mysql-files:/var/lib/mysql-files \ 【mysql8.0需指定 /var/lib/mysql-files的外部目录】

-e MYSQL_ROOT_PASSWORD=root：初始化root用户的密码【mysql8.0不生效，可能需手动配置root用户的密码】

-d mysql:8.0：后台运行mysql

--lower_case_table_names=1：忽略大小写【linux下mysql默认区分大小写，mysql8及以上版本需在创建mysql时设置，否则无效】

```

2.简单配置即可

```bash
docker run -p 3306:3306 \
--name mysql8 \
-e MYSQL_ROOT_PASSWORD=123456 \
--restart=always -d mysql:latest \
--lower_case_table_names=1
# 忽略大小写【linux下mysql默认区分大小写，mysql8及以上版本需在创建mysql时设置，否则无效】
```

3.配置 配置文件

进入 之前设定目录 /data/mysql/conf

`新建配置文件`

```bash
touch my.cnf
```

`设置配置文件的内容`

```mysql
[mysqld]

## 设置server_id，同一局域网中需要唯一

server_id=101 

## 指定不需要同步的数据库名称

binlog-ignore-db=mysql  

## 开启二进制日志功能

log-bin=mall-mysql-bin  

## 设置二进制日志使用内存大小（事务）

binlog_cache_size=1M  

## 设置使用的二进制日志格式（mixed,statement,row）

binlog_format=mixed  

## 二进制日志过期清理时间。默认值为0，表示不自动清理。

expire_logs_days=7  

## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。

## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致

slave_skip_errors=1062
## 原文链接：https://blog.csdn.net/weixin_52938172/article/details/123929866
```

5.重启docker的mysql8服务

```docker
docker restart mysql8
```

6.进入docker中这个mysql，进行交互

```docker
# 用 /bin/bash命令 进操作个容器
docker exec -it mysql8 /bin/bash
```

然后登录mysql,这个就是普通的进入mysql的指令

```mysql
mysql -uroot -p123456
```

7.创建一个用户可以操作这个数据库

```sql
# 创建 codeking
CREATE USER 'codeking'@'%' identified by '123456';
# 授权
GRANT REPLICATION SLAVE,REPLICATION CLIENT ON *.* TO 'codeking'@'%';
# by 后面的是密码
GRANT REPLICATION SLAVE ON *.* TO  'codeking'@'%';
```

```sql
# 这个是在瑞吉外卖项目的里面看见的操作，但是失败了，但是这个是操作mysql5.7版本
GRANT REPLICATION SLAVE ON *.* TO 'codeking'@'%' IDENTIFIED BY '123456';
#注：上面SQL的作用是创建一个用户 xiaoming ，密码为 Root@123456 ，并且给xiaoming用户授予REPLICATION SLAVE权限。常用于建# 立复制时所需要用到的用户权限，也就是slave必须被master授权具有该权限的用户，才能通过该用户复制。
```

8.进入第二个数据库的配置文件的位置 /data/mysql-slave/conf/

```bash
# 新建文件
touch my.cnf
```

```mysql
[mysqld]

## 设置server_id，同一局域网中需要唯一

server_id=102

## 指定不需要同步的数据库名称

binlog-ignore-db=mysql  

## 开启二进制日志功能，以备Slave作为其它数据库实例的Master时使用

log-bin=mall-mysql-slave1-bin  

## 设置二进制日志使用内存大小（事务）

binlog_cache_size=1M  

## 设置使用的二进制日志格式（mixed,statement,row）

binlog_format=mixed  

## 二进制日志过期清理时间。默认值为0，表示不自动清理。

expire_logs_days=7  

## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。

## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致

slave_skip_errors=1062  

## relay_log配置中继日志

relay_log=mall-mysql-relay-bin  

## log_slave_updates表示slave将复制事件写进自己的二进制日志

log_slave_updates=1  

## slave设置为只读（具有super权限的用户除外）

read_only=1
## 原文链接：https://blog.csdn.net/weixin_52938172/article/details/123929866
```

重启这个容器

```docker
docker restart mysql8-slave
```

9.在主数据库中查看主从同步状态

```sql
show master status;
```

10.进入从数据库（mysql8-slave），并配置主从关系

```bash
# 进入容器
docker exec -it mysql8-slave /bin/bash
```

登录数据库

```docker
mysql -uroot -p123456
```

11.配置主从关系

为了方便复制，写了一个模板

```sql
change master to master_host='192.168.1.101', master_user='codeking', master_password='123456', master_port=3306, master_log_file='mall-mysql-bin.000002', master_log_pos=156, master_connect_retry=30;
```

```sql
change master to master_host='宿主机ip', master_user='slave', master_password='123456', master_port=3303, master_log_file='mall-mysql-bin.000001', master_log_pos=617, master_connect_retry=30;

## 参数解释
master_host='宿主机ip'：自己查询主库宿主机的ip

master_user='slave'：刚刚你在mysql-master容器实例内创建的用于数据同步的用户名。

master_password='123456'：刚刚你在mysql-master容器实例内创建的用于数据同步的用户密码。

master_port=3303：主数据库在宿主机上的映射端口号，之前docker映射出去宿主ip端口。

master_log_file='mall-mysql-bin.000001', master_log_pos=617：根据命令： show master status;

master_log_file：指定从数据库要复制数据的日志文件，通过查看主数据的状态，获取File参数；

master_log_pos：指定从数据库从哪个位置开始复制数据，通过查看主数据的状态，获取Position参数；

master_connect_retry：连接失败重试的时间间隔，单位为秒。
## 参考链接： https://blog.csdn.net/weixin_52938172/article/details/123929866
```

查看主从同步状态

```sql
show slave status \G;

加了个 \G 结果展示不一样。
查看这个参数如下：
Slave_IO_Running: NO
Slave_SQL_Running: NO

这两个No说明同步还没开始，主库都数据都没更新。
```

开启同步

```sql
start slave;
```

可以再次查看同步状态

```sql
show slave status \G;
```

出问题的时候,考虑清楚主从关系，如果是MySQL8.0密码认证问题，可以修改插件

```
1.注意主从都保持开启log-bin

2.从库执行stop slave;

3.主库执行RESET MASTER;

4.从库执行reset slave;

5.分别重启mysql
```

修改插件

因为mysql8.0版本使用的plugin是 `caching_sha2_password`插件

做法是去数据库里面，将这个改成 `mysql_native_password`。或者可以直接删除这个用户，重写建立一个用户。

前提是，去配置文件（my.cnf）中去加一行，然后重启服务

```sql
[mysqld]

# 修改认证方式，这就可以和以前的mysql5的认证方式一样
default_authentication_plugin=mysql_native_password
```

**mysql重置root密码**

<u>如果密码输入不对，极有可能是因为你在数字键盘输入的，去你字母键盘上面那个位置输入！！！</u>

```
cd etc/mysql
vim my.cnf
# 在最后加上，跳过密码验证
skip-grant-tables
```

```
docker restart mysql8
# 进入mysql验证密码直接回车即可
```

mysql新建用户

```
CREATE USER 'codeking'@'host' IDENTIFIED BY '123456';
```

## 七、Docker部署nginx

1.部署命令改天补充

2.如何查看nginx的配置文件

```docker
# 先进入容器
docker start my-nginx

# 进入容器内部
docker exec -it my-nginx /bin/bash
#直接修改配置,有可能进不去，但是目录差不多
vim /etc/nginx/nginx.conf 

# 如果进不去
cd /etc/nginx 
# 根据具体位置查看，我的是这个
cd /etc/nginx/conf.d
```

## 八、Docker部署RabbitMQ

1.安装name为rabbitmq的这里是直接安装最新的，如果需要安装其他版本在rabbitmq后面跟上版本号即可

```docker
docker pull rabbitmq
```

2.**需要注意**的是`-p 5673:5672` 解释：-p 外网端口：docker的内部端口 ，你们可以改成自己的外网端口号，我这里映射的外网端口是5673那么程序连接端口就是用5673

```docker
docker run -d --hostname my-rabbit --name rabbit -p 15672:15672 -p 5672:5672 rabbitmq
```

```
-d 后台运行容器；
--name 指定容器名；
-p 指定服务运行的端口（5672：应用访问端口；15672：控制台Web端口号）；
-v 映射目录或文件；--hostname 主机名（RabbitMQ的一个重要注意事项是它根据所谓的 “节点名称” 存储数据，默认为主机名）；
-e 指定环境变量；（RABBITMQ_DEFAULT_VHOST：默认虚拟机名；RABBITMQ_DEFAULT_USER：默认的用户名； RABBITMQ_DEFAULT_PASS：默认用户名的密码）
#https://blog.csdn.net/yaobo2816/article/details/113822937
```

3.进入容器，开启Web插件:

```docker
rabbitmq-plugins enable rabbitmq_management
```

开启UI插件

```docker
#查看配置文件
cat /etc/rabbitmq/conf.d/management_agent.disable_metrics_collector.conf
#将配置文件内容，true改为false：
cd  /etc/rabbitmq/conf.d/
echo management_agent.disable_metrics_collector = false > management_agent.disable_metrics_collector.conf
```

如果开启UI没有效果需要安装这个插件,然后重启这个容器

```docker
docker exec -it {rabbitmq容器名称或者id} rabbitmq-plugins enable rabbitmq_management
docker restart {rabbitmq容器id}
```

注意：有些用`docker exec -it 容器id /bin/bash` 会报错:

那你可以把**脚本类型 /bin/bash，尝试换为 /bin/sh** 试一下

4.登录测试，IP+15672 账户和密码都默认是guest

5.添加一个新的用户创建账号

`rabbitmqctl add_user admin 123`

设置用户角色

`rabbitmqctl set_user_tags admin administrator`

设置用户权限

`set_permissions [-p ] rabbitmqctl set_permissions -p "/" admin ".*" ".*" ".*"`

用户 user_admin 具有 /vhost1 这个 virtual host 中所有资源的配置、写、读权限

当前用户和角色 

`rabbitmqctl list_users`

关闭应用的命令为

`rabbitmqctl stop_app`

清除的命令为

`rabbitmqctl reset`

重新启动命令为

`rabbitmqctl start_app`

**消息中间件RabbitMQ常用的的6个端口的作用：**
端口    作用
15672                 管理界面ui使用的端口
15671                 管理监听端口
5672，5671       AMQP 0-9-1 without and with TLSclient端通信口
4369                （epmd)epmd代表 Erlang端口映射守护进程，erlang发现口
25672                ( Erlang distribution） server间内部通信口
**注意**：为了省事，推荐直接拉取有managment的镜像，它自带web管理，不需要再手动安装;
如果docker pull rabbitmq后面不带management，启动rabbitmq后是会报错的的（，所以要下载带management插件的rabbitMQ。

例如：安装指定版本有managment的镜像，也可不带版本编号，`docker pull rabbitmq:management`

```docker
docker pull rabbitmq:3.20-management
```

`参考连接 https://blog.csdn.net/m0_49246190/article/details/126781033`

6. 安装延迟队列插件 `rabbitmq_delayed_message_exchange`，找到对应的版本，我这个是 `3.9.x版本`
   
   下载地址 ：https://github.com/rabbitmq/rabbitmq-delayed-message-exchange/releases[Releases · rabbitmq/rabbitmq-delayed-message-exchange · GitHub](https://github.com/rabbitmq/rabbitmq-delayed-message-exchange/releases)
   
   6.1 进入容器，然后到 容器的plugins目录,查看容器内容，然后将插件拷贝进去
   
   在容器外部执行命令
   
   ```docker
   docker cp "主机的文件" "docker容器名称或者id":/"要拷贝到的目录"/  
   docker cp /root/all_softwares/plugins/rabbitmq_delayed_message_exchange-3.9.0.ez rabbit:/plugins/
   ```
   
   6.2 开启插件，先赋权限
   
   ```docker
   # 赋权限
   chmod 777 rabbitmq_delayed_message_exchange-3.9.0.ez 
   # 开启插件，别带版本号
   rabbitmq-plugins enable rabbitmq_delayed_message_exchange
   ```

    6.3 如果exchange的type中没出现x-delayed-message,考虑重启一下容器

7.利用docker搭建集群

    7.1启动三个相互独立的RabbitMQ服务

```docker
docker run -d --hostname localhost --name myrabbit1 -p 15672:15672 -p 5672:5672 rabbitmq:3.8-management
docker run -d --hostname localhost --name myrabbit2 -p 15673:15672 -p 5673:5672 rabbitmq:3.8-management
docker run -d --hostname localhost --name myrabbit3 -p 15674:15672 -p 5674:5672 rabbitmq:3.8-management
```

`这样我们就可以通过：http://ip:15672、http://ip:15673、http://ip:15674来访问各个单例服务；`

首先删除上一步骤中生成的三个RabbitMQ镜像集群，然后再操作如下指令，生成有三个节点的集群

```docker
docker run -d --hostname rabbit1 --name myrabbit1 -p 15673:15672 -p 5673:5672 -e
RABBITMQ_ERLANG_COOKIE='rabbitcookie' rabbitmq:3.8-management

docker run -d --hostname rabbit2 --name myrabbit2 -p 15674:15672 -p 5674:5672 --link myrabbit1:rabbit1 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' rabbitmq:3.8-management

docker run -d --hostname rabbit3 --name myrabbit3 -p 15675:15672 -p 5675:5672 --link myrabbit1:rabbit1 --link myrabbit2:rabbit2 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' rabbitmq:3.8-management

-d: 后台进程运行
hostname: RabbitMQ主机名称
name:容器名称
-p 15672:15672 访问HTTP API客户端，容器对外的接口和内部接口的映射
-p 5672:5672 由不带TLS和带TLS的AMQP 0-9-1和1.0客户端使用
```

注意点：

1.多个容器之间使用“–link”连接，此属性不能少；

2.Erlang Cookie值必须相同，也就是RABBITMQ_ERLANG_COOKIE参数的值必须相同，因为RabbitMQ是用Erlang实现的，Erlang Cookie相当于不同节点之间相互通讯的秘钥，Erlang节点通过交换Erlang Cookie获得认证。

7.2 `其他方式搭建`：挂载配置文件创建集群方式：

```docker
docker run -d --hostname rabbit1 --name rabbitmq1 -p 15673:15672 -p 5673:5672 -v D:/rabbitmq1/conf:/etc/rabbitmq -v D:/rabbitmq1/log:/var/log/rabbitmq/log -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' rabbitmq:3.8.0-management

docker run -d --hostname rabbit2 --name rabbitmq2 -p 15674:15672 -p 5674:5672 -v D:/rabbitmq2/conf:/etc/rabbitmq -v D:/rabbitmq2/log:/var/log/rabbitmq/log --link rabbitmq1:rabbit1 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' rabbitmq:3.8.0-management

docker run -d --hostname rabbit3 --name rabbitmq3 -p 15675:15672 -p 5675:5672 -v D:/rabbitmq3/conf:/etc/rabbitmq -v D:/rabbitmq3/log:/var/log/rabbitmq/log --link rabbitmq1:rabbit1 --link rabbitmq2:rabbit2 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' rabbitmq:3.8.0-management

docker run -d --hostname rabbit4 --name rabbitmq4 -p 15676:15672 -p 5676:5672 -v D:/rabbitmq4/conf:/etc/rabbitmq -v D:/rabbitmq4/log:/var/log/rabbitmq/log --link rabbitmq1:rabbit1 --link rabbitmq2:rabbit2 --link rabbitmq3:rabbit3 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' rabbitmq:3.8.0-management
```

7.3.1  将RabbitMQ节点加入集群

设置节点1

```docker
docker exec -it myrabbit1 bash
rabbitmqctl stop_app
rabbitmqctl reset
rabbitmqctl start_app
exit
```

会报一个警告：

```docker
RABBITMQ_ERLANG_COOKIE env variable support is deprecated and will be REMOVED in a future version. Use the $HOME/.erlang.cookie file or the --erlang-cookie switch instead.
```

7.3.2 设置节点2，加入到集群：

```docker
docker exec -it myrabbit2 bash
rabbitmqctl stop_app
rabbitmqctl reset
rabbitmqctl join_cluster --ram rabbit@rabbit1
rabbitmqctl start_app
exit
```

参数“–ram”表示设置为内存节点，忽略此参数默认为磁盘节点。

7.3.3 设置节点3，加入集群：

```docker
docker exec -it myrabbit3 bash
rabbitmqctl stop_app
rabbitmqctl reset
rabbitmqctl join_cluster --ram rabbit@rabbit1
rabbitmqctl start_app
exit
```

设置好之后，使用http:物理机IP:15672,默认账号密码是：guest/guest。

7.4 RabbitMQ集群常用命令

查看集群的命令

```bash
# 查看集群的状态
rabbitmqctl cluster_status
# 停止节点
rabbitmqctl stop_app
# 启动节点
rabbitmqctl start_app
# 重置节点
rabbitmqctl reset
```

 我们可以远程删除节点，例如，在必须处理无响应的节点时，这很有用，例如可以删除rabbit@rabbit1节点从rabbit@rabbit2节点

```docker
# on rabbit1
rabbitmqctl stop_app
# on rabbit2
rabbitmqctl forget_cluster_node rabbit@rabbit1
```

注意：此时rabbit1仍然认为和rabbit2是在同一个集群，并且试图启动它将会报错，我们将会重置之后再重启。

```docker
# on rabbit1
rabbitmqctl start_app
# => Starting node rabbit@rabbit1 ...
# => Error: inconsistent_cluster: Node rabbit@rabbit1 thinks it's clustered with node rabbit@rabbit2, but rabbit@rabbit2 disagrees

rabbitmqctl reset
# => Resetting node rabbit@rabbit1 ...done.

rabbitmqctl start_app
# => Starting node rabbit@rabbit1 ...
# => ...done.
```

集群节点重置（reset）
有时可能需要重置节点（擦除其所有数据），然后使其重新加入集群。一般来说，有两种可能情况：节点正在运行时，以及节点由于诸如 ERL-430之 类的问题而无法启动或无法响应CLI工具命令时。

重置节点将删除其所有数据，集群成员信息，已配置的运行时参数，用户，虚拟主机以及任何其它节点数据。它还将从该群集中永久删除该节点。

要重置一个正在运行的响应节点，请首先使用rabbitmqctl stop_app停止RabbitMQ,然后使用rabbitmqctl reset对其进行重置：

```docker
# on rabbit1
rabbitmqctl stop_app
# => Stopping node rabbit@rabbit1 ...done.
rabbitmqctl reset
# => Resetting node rabbit@rabbit1 ...done.
```

对于无响应的节点，必须先使用任何必要的方法将其停止。对于无法启动的节点，情况也是如此。

已重置并重新加入其原始集群的节点将同步所有虚拟主机，用户，权限和拓扑（队列，交换，绑定），运行时参数和策略。如果选择托管副本，他可能会同步镜像队列的内容。重置节点上的非镜像队列内容将丢失。

`更改节点的类型disk|ram`

```docker
# on rabbit3
rabbitmqctl stop_app
# => Stopping node rabbit@rabbit3 ...done.

rabbitmqctl change_cluster_node_type ram
# => Turning rabbit@rabbit3 into a ram node ...done.

rabbitmqctl start_app
# => Starting node rabbit@rabbit3 ...done.
```

`GitHub地址：[GitHub - mingyang66/spring-parent: 数据库动态切换多数据源SDK、Redis多数据源SDK、全链路日志追踪SDK、RabbitMQ多虚拟主机多集群支持SDK、日志组件SDK、埋点扩展点、Netty、微服务、开发基础框架支持、异常统一处理、返回值、跨域、API路由、监控等；](https://github.com/mingyang66/spring-parent)`

`直接用带管理插件的去安装 rabbitmq:3.9-management`

```docker
#拉取镜像
docker pull rabbitmq:3.9-management
```

集群模式

1.安装容器

```docker
docker run -d --hostname rabbit1 --name rabbitmq1 -p 15673:15672 -p 5673:5672 -e  \ RABBITMQ_ERLANG_COOKIE='rabbitmq_cookie' rabbitmq:3.9-management

docker run -d --hostname rabbit2 --name rabbitmq2 -p 15674:15672 -p 5674:5672 --link rabbitmq1:rabbit1 -e \ RABBITMQ_ERLANG_COOKIE='rabbitmq_cookie' rabbitmq:3.9-management

docker run -d --hostname rabbit3 --name rabbitmq3 -p 15675:15672 -p 5675:5672 --link rabbitmq1:rabbit1 --link \ rabbitmq2:rabbit2 -e RABBITMQ_ERLANG_COOKIE='rabbitmq_cookie' rabbitmq:3.9-management
```

2. 将RabbitMQ节点加入集群

设置节点1

    docker exec -it rabbitmq1 bash
    rabbitmqctl stop_app
    rabbitmqctl reset
    rabbitmqctl start_app
    exit

可能会报一个警告，~~说明cookie可能有问题，最好重操作一下~~，中间还有error日志，忽视即可：

    RABBITMQ_ERLANG_COOKIE env variable support is deprecated and will be REMOVED in a future version. Use the $HOME/.erlang.cookie file or the --erlang-cookie switch instead.

设置节点2，加入到集群：

    docker exec -it rabbitmq2 bash
    rabbitmqctl stop_app
    rabbitmqctl reset
    rabbitmqctl join_cluster --ram rabbit@rabbit1
    rabbitmqctl start_app
    exit

参数“–ram”表示设置为内存节点，忽略此参数默认为磁盘节点。

设置节点3，加入集群：

    docker exec -it rabbitmq3 bash
    rabbitmqctl stop_app
    rabbitmqctl reset
    rabbitmqctl join_cluster --ram rabbit@rabbit2
    rabbitmqctl start_app
    exit

设置好之后，使用http:物理机IP:15672,默认账号密码是：guest/guest。

 此时安装完成的为普通集群模式

Exchange 的元数据信息在所有节点上是一致的，而 Queue（存放消息的队列）的完整数据则只会存在于创建它的那个节点上。其他节点只知道这个 queue 的 metadata 信息和一个指向 queue 的 owner node 的指针；
RabbitMQ 集群会始终同步四种类型的内部元数据（类似索引）：
1.队列元数据：队列名称和它的属性；
2.交换器元数据：交换器名称、类型和属性；
3.绑定元数据：一张简单的表格展示了如何将消息路由到队列；
4.vhost元数据：为 vhost 内的队列、交换器和绑定提供命名空间和安全属性；因此，当用户访问其中任何一个 RabbitMQ 节点时，通过 rabbitmqctl 查询到的元数据信息都是相同的。
 无法实现高可用性，当创建 queue 的节点故障后，其他节点是无法取到消息实体的。如果做了消息持久化，那么得等创建 queue 的节点恢复后，才可以被消费。如果没有持久化的话，就会产生消息丢失的现象。
`原文链接：https://blog.csdn.net/weixin_39555954/article/details/120406404`

4.配置镜像集群模式
    概念：
    把队列做成镜像队列，让各队列存在于多个节点中，属于 RabbitMQ 的高可用性方案。镜像模式和普通模式的不同在于，queue和 message 会在集群各节点之间同步，而不是在 consumer 获取数据时临时拉取。

    特点：
（1）实现了高可用性。部分节点挂掉后，不会影响 rabbitmq 的使用。
（2）降低了系统性能。镜像队列数量过多，大量的消息同步也会加大网络带宽开销。
（3）适合对可用性要求较高的业务场景。

## 九、Docker部署mongodb

#拉取镜像

`docker pull mongo:latest`

#创建和启动容器

```
# docker run -d --restart=always -p 27017:27017 --name mymongo -v /data/db:/data/db-d mongo
docker run -d --restart=no -p 27017:27017 --name mymongo -v /data/db:/data/db-d mongo
```

#进入容器

docker exec -it **mymongo**/bin/bash

#使用MongoDB客户端进行操作

`mongo`

show dbs #查询所有的数据库

```
admin 0.000GB

config 0.000GB

local 0.000GB
```

常见的mongodb命令

```
1、 Help查看命令提示 
db.help();
2、 切换/创建数据库
use test
如果数据库不存在，则创建数据库，否则切换到指定数据库
3、 查询所有数据库 
show dbs;
4、 删除当前使用数据库 
db.dropDatabase();
5、 查看当前使用的数据库 
db.getName();
6、 显示当前db状态 
db.stats();
7、 当前db版本 
db.version();
8、 查看当前db的链接机器地址 
db.getMongo〇;
```

## 十、Docker部署Nacos

如果已经部署过，过来一段时间部署上不去了，检查ip变化

在`/data/nacos/conf/application.properties`修改ip

```
# 这个地方的ip会变化
db.url.0=jdbc:mysql://<ip>:3308/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
```



参数介绍(不要用这个)

```
# 指定 nacos 对外映射端口，例如：8000:8848
docker run -d -p 8848:8848 \

# docker 容器名称
--name nacos \

# 自定义分配网络，可忽略
--network woniu_network \

# 自定义分配 IP 地址，可忽略
--ip 172.0.0.28 \

# nacos 单机实例
--env MODE=standalone \

# nacos 数据源 MySQL
--env SPRING_DATASOURCE_PLATFORM=mysql \

# MySQL 主机地址，记得改成自己数据库地址
--env MYSQL_SERVICE_HOST=172.0.0.5 \

# MySQL 端口号
--env MYSQL_SERVICE_PORT=3306 \

# MySQL 数据库名称，记得在对应的数据库执行从 github 下载的 SQL 文件
--env MYSQL_SERVICE_DB_NAME=nacos \

# MySQL 用户名称，例如：root
--env MYSQL_SERVICE_USER=nacos \

# MySQL 服务密码，例如：123456
--env MYSQL_SERVICE_PASSWORD=nacos \

# docker 文件映射，把 nacos 容器中文件和本地路径映射起来，方便操作和持久化
# nacos 配置文件目录
-v /home/docker/nacos/conf:/home/nacos/conf \

# nacos 日志文件目录
-v /home/docker/nacos/logs:/home/nacos/logs \

# nacos 数据文件目录
-v /home/docker/nacos/data:/home/nacos/data \

# 指定 docker nacos 版本，示例：nacos/nacos-server:v2.0.4
nacos/nacos-server:latest
# https://blog.csdn.net/u011374856/article/details/109204466
```

**执行命令**

0.先连接一个数据库

下载

[nacos/nacos-db.sql at master · alibaba/nacos (github.com)](https://github.com/alibaba/nacos/blob/master/config/src/main/resources/META-INF/nacos-db.sql)

然后执行文件内容(查看数据库文件内容，指明了数据库名字**nacos_config**,这个会和后面的**MYSQL_SERVICE_DB_NAME**对应)。

```
docker run -d --name mynacos -p 8848:8848 -e PREFER_HOST_MODE=hostname -e MODE=standalone nacos/nacos-server
```

1.先创建容器，然后一会再复制nacos文件，然后删除容器，重创建

```
docker run -d --name mynacos -p 8848:8848 -e PREFER_HOST_MODE=hostname -e MODE=standalone \
nacos/nacos-server
```

2.复制相关文件

```
# 把容器中的 nacos 文件复制出来
docker cp -a mynacos:/home/nacos /data/

# 删除 nacos 容器
docker rm -f mynacos
```

3.自定义启动容器

```
 用这个
docker run -d --name mynacos -p 8848:8848 -e PREFER_HOST_MODE=hostname -e MODE=standalone \
-v /data/nacos:/home/nacos \
nacos/nacos-server


### 另一个配置
docker run -d -p 8848:8848 \
--name mynacos \
--env MODE=standalone \
-e PREFER_HOST_MODE=hostname \
--env SPRING_DATASOURCE_PLATFORM=mysql \
--env MYSQL_SERVICE_HOST=127.0.0.1 \
--env MYSQL_SERVICE_PORT=3306 \
--env MYSQL_SERVICE_DB_NAME=nacos_config \
--env MYSQL_SERVICE_USER=root \
--env MYSQL_SERVICE_PASSWORD=123456 \
--restart=no \
-v /home/docker/nacos/conf:/home/nacos/conf \
-v /home/docker/nacos/logs:/home/nacos/logs \
-v /home/docker/nacos/data:/home/nacos/data \
nacos/nacos-server:latest

```

4.修改配置文件

```
# spring
server.contextPath=/nacos
server.servlet.contextPath=/nacos
server.port=8848
management.metrics.export.elastic.enabled=false
management.metrics.export.influx.enabled=false
server.tomcat.accesslog.enabled=true
server.tomcat.accesslog.pattern=%h %l %u %t "%r" %s %b %D %{User-Agent}i
server.tomcat.basedir=/
nacos.security.ignore.urls=/,/**/*.css,/**/*.js,/**/*.html,/**/*.map,/**/*.svg,/**/*.png,/**/*.ico,/console-fe/public/**,/v1/auth/login,/v1/console/health/**,/v1/cs/**,/v1/ns/**,/v1/cmdb/**,/actuator/**,/v1/console/server/**
spring.datasource.platform=mysql
db.num=1
# 这个地方不知道为什么必须写主机的ip,可以设定数据库的ip为固定，参考 http://t.csdn.cn/PF0Y5http://t.csdn.cn/PF0Y5
db.url.0=jdbc:mysql://192.168.11.96:3308/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=123456
```

```
 1，搜索nacos
docker search nacos    #搜索nacos的镜像
2,下载image
docker  pull nacos/nacos-server:1.3.1  #推荐稳定版版本(官方推荐1.3.1),如果不指定版本的话则就是latest版本(对应nacos的1.4版本)
3,检查镜像
docker images  #查看docker中的所有镜像包
4，启动
prefer_host_mode hostname/ip 默认是ip。
-v centos上的某个目录(根据实际的设置别模仿我的)：某个容器的目录
-p 外部访问端口:内部被映射端口(根据实际的设置别模仿我的)
-e 环境变量设置
-d 后台运行
--name 容器的名称
--restart 重启策略 always no 等


docker run -d   -p 8848:8848  --name mynacos -e MODE=standalone  --restart=no \
-v  /data/nacos/logs:/home/nacos/logsnacos/nacos-server 
#使用的时候合成一段即可 docker启动镜像命令


```

| name                          | description                     | option                                 |
| ----------------------------- | ------------------------------- | -------------------------------------- |
| MODE                          | cluster模式/standalone模式          | cluster/standalone default **cluster** |
| NACOS_SERVERS                 | nacos cluster地址                 | eg. ip1,ip2,ip3                        |
| PREFER_HOST_MODE              | 是否支持hostname                    | hostname/ip default **ip**             |
| NACOS_SERVER_PORT             | nacos服务器端口                      | default **8848**                       |
| NACOS_SERVER_IP               | 多网卡下的自定义nacos服务器IP              |                                        |
| SPRING_DATASOURCE_PLATFORM    | standalone 支持 mysql             | mysql / empty default empty            |
| MYSQL_MASTER_SERVICE_HOST     | mysql 主节点host                   |                                        |
| MYSQL_MASTER_SERVICE_PORT     | mysql 主节点端口                     | default : **3306**                     |
| MYSQL_MASTER_SERVICE_DB_NAME  | mysql 主节点数据库                    |                                        |
| MYSQL_MASTER_SERVICE_USER     | 数据库用户名                          |                                        |
| MYSQL_MASTER_SERVICE_PASSWORD | 数据库密码                           |                                        |
| MYSQL_SLAVE_SERVICE_HOST      | mysql从节点host                    |                                        |
| MYSQL_SLAVE_SERVICE_PORT      | mysql从节点端口                      | default :3306                          |
| MYSQL_DATABASE_NUM            | 数据库数量                           | default :2                             |
| JVM_XMS                       | -Xms                            | default :2g                            |
| JVM_XMX                       | -Xmx                            | default :2g                            |
| JVM_XMN                       | -Xmn                            | default :1g                            |
| JVM_MS                        | -XX:MetaspaceSize               | default :128m                          |
| JVM_MMS                       | -XX:MaxMetaspaceSize            | default :320m                          |
| NACOS_DEBUG                   | 开启远程调试                          | y/n default :n                         |
| TOMCAT_ACCESSLOG_ENABLED      | server.tomcat.accesslog.enabled | default :false                         |

```
docker run -d -p 8848:8848  \
-e MODE=standalone \
-e PREFER_HOST_MODE=hostname \
-e SPRING_DATASOURCE_PLATFORM=mysql \
-e MYSQL_SERVICE_HOST=127.0.0.1 \
-e MYSQL_SERVICE_PORT=3306 \
-e MYSQL_SERVICE_DB_NAME=nacos_config \
-e MYSQL_SERVICE_USER=root \
-e MYSQL_SERVICE_PASSWORD=root \
-e MYSQL_DATABASE_NUM=1 \
-v /data/nacos/init.d/custom.properties:/home/nacos/init.d/custom.properties \
-v /data/nacos/logs:/home/nacos/logs \
--restart no --name mynacos nacos/nacos-server
```
