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
docker pull image_name 从仓库获取所需要的镜像
docker images 列出镜像
docker ps -a 列出容器
docker ps --format "{{.ID}}: {{.Command}}"
docker run -it image_name:tag_name /bin/bash
# 新建并启动容器，-t分配一个伪终端并绑定到容器的标准输入上， -i则让容器的标准输入保持打开，-d则进入后台运行

docker commit container_id image_name:tag_name 提交容器中的改动到镜像文件中
docker start container_id 启动容器
docker stop container_id 停止容器

docker rm 删除容器
docker rmi 删除镜像

docker volume ls 列出分卷
docker volume create --name volume_name 创建分卷
docker volume inspect volume_name 查看分卷
docker volume rm volume_name 删除分卷
```
