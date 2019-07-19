---
layout: post
title: "Docker 笔记"
subtitle: 'docker常用命令'
author: "zhhray"
header-style: text
tags:
  - docker
  - 学习笔记

---

查看容器进程 : `docker top`可以查看运行容器中运行的进程  

查看容器信息: `docker inspect` 容器名/容器ID可以查看容器的配置信息：容器名，环境变量， 运行命令， 主机配置，网络配置，数据卷配置。  

拉取镜像 ：`docker pull centos:lastest` 从docker.io获取最新的centos系统镜像到本地, :lastest可以不写，为默认版本。  

交互式运行： `docker run -t -i  centos:lastest  /bin/bash`  

后台运行： `docker run -d centos:lastest  sleep 1000000`  

操作运行中的容器：`docker exec -t -i $container_id /bin/bash`  

给容器取名字： `docker run --name` 容器名  

端口映射：  
    `docker run -d -P(大写) $image_name [$cmd]`  
    docker可以通过Dockerfile在制作镜像时暴露一些端口出来，如web的80端口。 那么就需要在宿主机中指定一个端口映射到容器中的端口。 
    使用-P参数让容器启动时自动分配宿主机的端口到容器的端口。  
    `docker run -d -p(小写) 主机端口:容器暴露的端口 $image_name [$cmd]`与-P类似，小写的-p可以显示指定主机端口  

文件映射：  
容器中产生的文件随着容器被删除也会被删除，这时有两种方法来持久化数据：  
  1. 容器删除前做一次commit，将容器的最新状态及数据做成一个镜像，然后更新到镜像仓库。  
  2. 使用这里说的文件映射，将主机中的一个目录映射到容器中，这种方法有很多使用场景，如在宿主机上看到容器的log文件，或者将mysql的data数据关联到宿主机的某个目录下，这样即使是容器被删，数据还是保留着的。  

（注意-v的两个目录都需要为绝对路径）：  
  `docker run -d --name=gogs -v /root/gogs_data:/data -p 10080:3000 -p 22222:22 songlijun/gogs`  

环境变量：  
  `docker run -e VAR_1=xxx -e VAR_2=yyy $image_name [$cmd]` -e后面可以指定一些环境变量值，在容器运行的时候可以读到这些变量，容器的启动脚本可以使用这些变量做一些宏替换等操作，达到读取启动参数的目的。  

容器连接：  
  容器1： `docker run --name container_1_name ...`  
  容器2:  `docker run --link container_1_name:container_1_name_in_container2 ...`  
这样容器2就可以读到容器1的所有环境变量, 以及一些暴露出去的端口信息等。 这种连接方式有效的避免了将一些变量暴露到这两个容器之外。
 
举例：官方的phpmyadmin镜像与mysql数据库就使用这种方式进行关联：   
  `docker run --name mysql -e MYSQL_ROOT_PASSWORD=my_password -d mysql`  
  `docker run --link mysql:mysql -p 1234:80 nazarpc/phpmyadmin`  

制作镜像：  
  `docker commit $container_id $image_name`  
  即将一个容器（运行中或者Exit状态都可以) 做一次commit, 就可以做成一个本地镜像， $image_name是取的新镜像名。 
  注：这种方式做镜像比较方便，但是项目的其它成员就不太清楚这个镜像的具体生成细节，不易维护。 

使用Dockerfile：  
  `docker build -t $image_name -f dockerfile`  
  Dockerfile是一种脚本化描述生成镜像过程的方法，将镜像的生成指令写到文件中，便于分享给其它人，具体语法不展开说，参见官方文档： https://docs.docker.com/engine/reference/builder/  

提交镜像:  
  `docker push $image_name`  
提交名为$image_name的镜像， 镜像名中包含了镜像仓库的地址，默认为docker.io  
