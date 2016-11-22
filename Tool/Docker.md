# Docker

## 安装
```
pacman -S docker
systemctl start docker
systemctl enable docker
```

## allows IP forwarding from the container
```
nano /etc/systemd/network/<interface>.network
```
```
[Network]
...
IPForward=kernel
...
```

## 常用命令
```
docker version 查看docker的版本信息
docker run hello-world 测试docker能否正常运行

docker pull image_name 从仓库获取所需要的镜像
docker images 列出镜像
docker ps -a 列出容器
docker ps --format "{{.ID}}: {{.Command}}"

# 新建并启动容器，-t分配一个伪终端并绑定到容器的标准输入上， -i则让容器的标准输入保持打开，-d则进入后台运行
docker run -it image_name:tag_name /bin/bash

# run multiple commands in docker, use /bin/bash -c and semicolon ;
docker run image /bin/bash -c "cd /path/to/somewhere; python a.py"

docker commit container_id image_name:tag_name 提交容器中的改动到镜像文件中
docker start container_id 启动容器
docker stop container_id 停止容器
docker restart container_id 重启动容器

docker logs container_id 查看容器的日志
docker attach container_id 进入运行中的容器
exit 从容器中退出

docker rm 删除容器
docker rmi 删除镜像

docker volume ls 列出分卷
docker volume create --name volume_name 创建分卷
docker volume inspect volume_name 查看分卷
docker volume rm volume_name 删除分卷
```

## Swarm 集群
```
# 在管理节点上创建一个新的swarm，其他节点以worker或manager的方式加入swarm
docker swarm init --advertise-addr 192.168.33.101 (在管理节点上运行)
docker swarm join \
--token SWMTKN-1-4d8z30svnj6wi8y64ttzirpildro5vego1qrrldepj8auwxa6l-4l4b6o7q1wnjyiwsubkpffkkn \
192.168.33.101:2377 (在worker节点上运行)

docker swarm join-token worker 查看加入swarm的命令
docker node ls 查看集群中的所有节点
docker node promote <worker_node> worker节点上升为manager节点(在管理节点上运行)
```

### 在Swarm集群上运行service
服务模式包括replicated和global。默认是replicated。
- replicated模式，根据指定的数量运行任务。
- global模式，任务运行在集群中所有活跃的节点上。
```
docker service create --name <service name> --mode <mode> alpine ping 8.8.8.8
docker service ls
docker service ps <service name> 查看服务到底跑在哪个节点服务器上
docker service scale <service name>=<副本个数>
```

### 发布端口
- 公共的端口会暴露在每一个swarm集群中的节点服务器上.
- 请求进如公共端口后会负载均衡到所有的sevice实例上.
```
docker service create --name search --publish 9200:9200 --replicas 7 elasticsearch
watch docker service ps search 监控 service 创建过程
docker service rm <service ID or Name> 删除服务
docker service ls -q | xargs docker service rm 删除所有servcie
```
一个service副本的创建过程,会经历以下几个状态:
* accepted 任务已经被分配到某一个节点执行
* preparing 准备资源, 现在来说一般是从网络拉取image
* running 副本运行成功
* shutdown 呃, 报错,被停止了…

### Rolling Update
--update-parallelism指定每次update的容器数量, --update-delay 每次更新之后的等待时间.
回滚和更新的命令是相同的，只是指定不同的镜像.
```
docker service update <service name> --update-parallelism 2 --update-delay 5s --image <updated image>
```

### overlay网络
保证不同主机上的容器网络互通
```
docker network create --driver overlay <network name>
docker network ls
docker service create --network <network name> --name <service name> <image name> 在指定网络上创建service
```
