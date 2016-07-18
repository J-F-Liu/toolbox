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

# 新建并启动容器，-t分配一个伪终端并绑定到容器的标准输入上， -i则让容器的标准输入保持打开，-d则进入后台运行
docker run -it image_name:tag_name /bin/bash

# run multiple commands in docker, use /bin/bash -c and semicolon ;
docker run image /bin/bash -c "cd /path/to/somewhere; python a.py"

docker commit container_id image_name:tag_name 提交容器中的改动到镜像文件中
docker start container_id 启动容器
docker stop container_id 停止容器

docker attach container_id 进入运行中的容器
exit 从容器中退出

docker rm 删除容器
docker rmi 删除镜像

docker volume ls 列出分卷
docker volume create --name volume_name 创建分卷
docker volume inspect volume_name 查看分卷
docker volume rm volume_name 删除分卷
```
