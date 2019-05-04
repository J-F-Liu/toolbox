# 使用Arch Linux

## 1、文件目录操作
```
pwd 查看当前工作目录的路径
cd  更改当前工作目录
ls 列出当前目录中的文件
ls -ltr 显示详细信息并按时间排序
```

> drwxr-xr-x 7 root root 4096 Aug 11 01:22 app

第一个字符的含义:<br />
\- regular file<br />
c character special file<br />
d directory<br />
l link<br />

修改文件属主
```
chown -R root:mfadmin Model/001/XJ-02
```

修改文件权限
```
chmod -R 3770 Model
```

4 = r (read)           list files inside the directory, only read the names of files<br />
2 = w (write/modify)   modify entries in the directory(create, delete, and rename files).<br />
1 = x (execute)        grants the ability to access file contents and meta-information if its name is known

To represent r-- triplet use 4+0+0=4<br />
To represent r-x triplet use 4+0+1=5<br />
To represent rw- triplet use 4+2+0=6<br />
To represent rwx triplet use 4+2+1=7

4 Octal Mode Numbers<br />
mode owner group others<br />
ugs  rwx   rwx   rwx

1000  Sets the sticky bit, only the file's owner, the directory's owner, or root user can rename or delete the file.<br />
2000  Sets the setgid bit, set group ID on execution<br />
4000  Sets the setuid bit, set user ID on execution

Symbolic modes<br />
u user  the owner of the file<br />
g group users who are members of the file's group<br />
o others  users who are neither the owner of the file nor members of the file's group<br />
a all     all three of the above, same as ugo

+ adds the specified modes to the specified classes<br />
- removes the specified modes from the specified classes<br />
= the modes specified are to be made the exact modes for the specified classes

获取和设置额外的文件权限
```
getfacl dir
setfacl -R -m u:user:rwx dir
setfacl -R -d --set g:group:rwx dir
```

清空文件内容
```
echo -n > /path/to/file
```

用less查看文件内容，h帮助信息，-N显示行号，/搜索，v编辑，R重新加载，q退出
g跳到开头，G跳到结尾，输入行号再按g跳到指定行，b上一页，z下一页
```
less /path/to/text/file
```

用十六进制方式查看或编辑文件
```
pacman -S hexedit
hexedit /path/to/file
```

将压缩文件text.zip在当前目录下解压缩。
```
unzip test.zip
```

将压缩文件text.zip在指定目录/tmp下解压缩，如果已有相同的文件存在，要求unzip命令不覆盖原先的文件。
```
unzip -n test.zip -d /tmp
```

将压缩文件test.zip在指定目录tmp下解压缩，如果已有相同的文件存在，要求unzip命令覆盖原先的文件。
```
unzip -o test.zip -d tmp/
```

查看压缩文件目录，但不解压。
```
unzip -v test.zip
```

计算MD5校验码
```
md5sum /path/to/file
```

切分文件
```
split -l 20000 bigfile.log
split -b 120M compact.file
```

## 2、帐号管理

查看帐号id及所在的用户组
```
id
id user_name
```

查看用户列表
```
less /etc/passwd
```

查看用户组
```
less /etc/group
```

新建用户组，比如gituser
```
groupadd gituser
```

将用户加入用户组
```
usermod -aG group user
```

列出用户所属的组
```
groups user
```

列出属于某个用户组的用户
```
packer -S libuser
lid -g group
```

查看当前已登录的用户
```
who
```

## 3、网络配置

查看端口
```
netstat -an | grep 8080
sudo netstat -tnlp
sudo ss -tnlp
```

启动防火墙
```
cp /etc/iptables/empty.rules /etc/iptables/iptables.rules
systemctl enable iptables
```

查看防火墙
```
iptables -nvL
```
打开防火墙80端口(cat /etc/sysconfig/iptables):
```
iptables -I INPUT -p tcp --dport 80 -j ACCEPT
iptables -A OUTPUT -p tcp --dport 587 -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A OUTPUT -p tcp --sport 587 -m state --state ESTABLISHED,RELATED -j ACCEPT
systemctl reload iptables
```

下载文件
```
curl url -o file
```

## 4、进程管理
```
nohup cmd & 后台运行命令
ps -ef 列出所有进程
ps auxww 列出进程及其命令
ps aux | grep {{string}}
pstree -p 展示进程树
kill pid 停止进程
time cmd 执行命令，并计算执行时间
top      显示进程列表
shutdown 关机
```

## 5、内核模块
```
lsmod | sort 显示当前装入的内核模块
modinfo module_name 显示模块信息
modprobe -c | grep module_name 显示某个模块的配置信息
modprobe --show-depends module_name 显示模块的依赖关系
modprobe module_name 手动装入模块的话
rmmod module_name
```
使用 /etc/modprobe.d/中的文件配置内核模块参数

## 6、其他

查看系统信息
```
uname -a
lscpu
cat /proc/cpuinfo
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c
cat /proc/meminfo
fdisk -l
lsblk -f
free -h 查看内存状态
df -h -a 查看磁盘剩余空间
du -sh /image 查看文件夹占用空间
du -kh --max-depth=1 /image
df -i 查看inode数量，即使磁盘有剩余空间，inode满了也无法写入文件
```

查看Shell程序
```
echo $SHELL
```

默认的编辑器
```
echo $EDITOR
export EDITOR=/usr/bin/nano
```

设置PATH变量和alias
```
nano /etc/profile
nano ~/.profile
```

>
```
PATH=$PATH:/home/vagrant/.gem/ruby/2.2.0/bin
alias ll='ls -l --color=auto'
alias ssh="TERM=xterm ssh"
```

```
source /etc/profile
```

识别命令
```
type <cmd>  显示命令的类型
which <cmd> 查看命令的文件路径
whatis <cmd> 显示非常简洁的命令说明
man <cmd> 显示命令手册
info <cmd> 显示程序 Info 条目
which -a <cmd> 显示所有匹配的可执行文件
whereis <cmd>  在全部路径中查找
```

&& chain tasks (Run one task after another)<br />
& run two commands at the same time<br />
< input file contents to a command<br />
\> redirect command output to a file<br />
| redirect command output to another command

```
man ascii 查看ASCII表
bc 命令行计算器
cal 日历
date 日期和时间
uptime 系统已运行了多长时间
last -x | less 系统登录和重启的记录
```
