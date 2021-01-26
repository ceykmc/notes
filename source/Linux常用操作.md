# Linux常用操作

**用户相关**

```shell
cat /etc/passwd  # 查看所有用户
sudo adduser user_name  # 增加用户
sudo userdel -r user_name  # 删除用户
sudo usermod -a -G group_name user_name  # 将用户添加到组中
umask u=rwx,g=rwx,o=  # 经过这样的设置，该用户创建的文件或者文件夹，组内可以共享，其它用户不能访问
```

**用户组相关**

```shell
groups  # 列出所有的组
sudo groupadd group_name  # 增加组
groups user_name  # 列出用户所属的组
```

**文件权限相关**

```shell
sudo chgrp -R group_name file_path  # 将文件权限赋给组
```

**Samba**

[Samba配置方法](https://tutorials.ubuntu.com/tutorial/install-and-configure-samba#0)

```shell
# Samba配置共享目录，赋予组内权限
create mode = 0770
force create mode = 0770
directory mode = 0770
force directory mode = 0770
# 在[global]中增加如下配置，避免软链接的文件不能访问
wide links = yes
symlinks = yes
unix extensions = no
```

**磁盘操作**

[磁盘分区和格式化](https://www.cnblogs.com/jyzhao/p/4778657.html)

**挂载**

```shell
sudo mount -t cifs -o username=user_name,password=user_password //10.20.0.61/alg_team_share/public_dataset/FaceReID /mnt/data/FaceReid
```



