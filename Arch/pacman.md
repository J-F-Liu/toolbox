## pacman

配置文件 /etc/pacman.conf

安装软件包

```
pacman -S package_name1 package_name2
```

卸载软件包，以及没有被其他软件所依赖的依赖包

```
pacman -Rs package_name
```

升级系统，同步所有软件到最新状态

```
pacman -Syu
sudo pacman-mirrors --geoip && sudo pacman -Syyu
```

同步所有软件包数据

```
pacman -Sy
```

列出显式安装的软件包

```
pacman -Qe
```

搜索软件包

```
pacman -Ss package_name
pacsearch package_name
```

搜索已安装的软件包

```
pacman -Qs package_name
```

查询软件包的基本信息

```
pacman -Si package_name
pacman -Qi package_name
```

列出软件包所包含的文件

```
pacman -Ql package_name
```

搜索服务器上的软件包

```
pacman -Ss string1 string2
```

查询文件所属的软件包

```
pacman -Qo /path/to/file
```

安装 AUR 软件

```
yay -S --noconfirm --noedit package_name
yay -Su package_name 同步AUR数据并更新软件包
```

下载最新的镜像服务器列表并且按照速度排序

```
pacman -S reflector
reflector --verbose --latest 5 --protocol http --sort rate --save /etc/pacman.d/mirrorlist
```

每周自动运行一次 reflector

```
yay -S reflector-timer-weekly
systemctl enable reflector.timer
nano /etc/reflector.conf
```

> ```
>   --country China
>   --country 'United States'
>   --sort rate
>   --save /etc/pacman.d/mirrorlist
> ```

删除 yay 缓存的文件

```
rm -rf /tmp/yaybuild-\*
```

删除缓存的安装包

```
pacman -Sc
```

安装 pacman-static， Statically-compiled pacman (to fix or install systems without libc)

```
yay -S pacman-static
```
