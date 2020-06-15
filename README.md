# docker-宝塔

基于官方centos 8.1，宝塔7.2.0封装，默认只安装了nginx服务

## 获取 宝塔
```
$ docker pull adetianming/centos8_baota:latest

```

## 使用说明
CentOS Linux release 8.1.1911 (Core)
- 宝塔Linux正式版 7.2.0
- 默认安全入口YourIP:8888/bt_login
- 登录账号bt_root
- 密码：bt123456
- 默认端口开放8081，宝塔8888
- 进入后手动重启宝塔面板，否则服务无响应。

## Docker构建宝塔linux面板镜像
- 拉取纯净系统镜像：
```
  $ docker search centos
  $ docker pull centos
```
- 启动镜像，映射主机与容器内8888端口
```
  $ docker run -d -it -v /Users/ming/work-code:/home/workspace -p 8888:8888 centos
```
- 查看容器id，并进入容器
```
  $ docker ps -a
```
- 得到容器id，并进入容器
```
$ docker exec -it 容器ID bash

```
- 执行宝塔面板Centos安装命令
```
$ yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
```
- 期间会有一个安装确认，输入y。然后就等着吧。
```
+----------------------------------------------------------------------
| Bt-WebPanel FOR CentOS/Ubuntu/Debian
+----------------------------------------------------------------------
| Copyright © 2015-2099 BT-SOFT(http://www.bt.cn) All rights reserved.
+----------------------------------------------------------------------
| The WebPanel URL will be http://SERVER_IP:8888 when installed.
+----------------------------------------------------------------------

Do you want to install Bt-Panel to the /www directory now?(y/n): y

```

- 安装成功后，会提示网址，用户名和密码。改ip是公网ip，主机运行需要替换成主机ip
```
Congratulations! Installed successfully!
==================================================================
Bt-Panel: IP:8888/bt_login
username: bt_root
password: bt123456
Warning:
If you cannot access the panel,
release the following port (8888|888|80|443|20|21) in the security group
==================================================================
Time consumed: 4 Minute!
```
- 停止容器
```
  1.查看面板登录入口信息：/etc/init.d/bt default

  2.关闭安全入口：rm -f /www/server/panel/data/admin_path.pl
  
```
- 停止容器
```
  $ docker stop 容器ID
  
```
- 重命名容器
```
  $ docker rename oldName newName
  
```
- 删除容器
```
  $ docker rm -f 容器ID
  
```
### 利用docker commit新构镜像
docker commit：把一个容器的文件改动和配置信息commit到一个新的镜像。这个在测试的时候会非常有用，把容器所有的文件改动和配置信息导入成一个新的docker镜像，然后用这个新的镜像重起一个容器，这对之前的容器不会有任何影响。
 - 1、停止docker容器
```
docker stop 
```
 - 2、commit该docker容器 tag 默认latest
```
docker commit 容器id new_image:tag
```
 - 3、用前一步新生成的镜像重新起一个容器
```
docker run --name 容器Name -p 80:80 new_image:tag
```

 - 4、查看运行容器
```
    docker ps
```
 - 5、查看所有容器
```
    docker ps -a
```
 - 6、进入容器 其中字符串为容器ID:
```
    docker exec -it 容器ID /bin/bash
```
 - 7、停用全部运行中的容器:
```
    docker stop $(docker ps -q)
```
 - 8、.删除全部容器：
```
    docker rm $(docker ps -aq)
```
 - 9、一条命令实现停用并删除容器：
    docker stop $(docker ps -q) & docker rm $(docker ps -aq)
    
    

