# linux学习



## linux软件安装
### rpm

#### RPM概念

1. RPM包命名规则

httpd-2.2.15-15.el6.centos. 1.i686.rpm

| 名称       | 意义            |
| ---------- | --------------- |
| httpd      | 软件包名        |
| 2.2.15     | 软件版本        |
| 15         | 软件发布的次数  |
| el6.centos | 适合的Linux平台 |
| i686       | 适合的硬件平台  |
| rpm        | 包扩展名        |

2.  RPM依赖

- 树形依赖: a>b>c
- 环形依赖:>b>c>a
- 模块依赖:模块依赖查询网站:  www.rpmfind.net

3. 包全名与包名
   包全名:操作的包是没有安装的软件包时，
   使用包全名。而且要注意路径
   包名:操作已经安装的软件包时，使用包名。
   是搜索/var/lib/rpm/中的数据库

#### RPM操作命令

- RPM安装

  rpm  -ivh 包全名
  选项:

  | 参数           | 作用         |
  | -------------- | ------------ |
  | -i ( install)  | 安装         |
  | -V ( verbose ) | 显示详细信息 |
  | -h(hash)       | 显示进度     |
  | --nodeps       | 不检测依赖性 |
  |                |              |

- RPM 升级

  rpm    -Uvh包		全名

  | 参数         | 作用 |
  | ------------ | ---- |
  | -U (upgrade) | 升级 |

- rpm 卸载

  rpm -e 包名

  | 参数         | 作用          |
  | ------------ | ------------- |
  | -e ( erase ) | 卸载          |
  | --nodeps     | 不检查依赖性. |



##### rpm查询

1. 查询是否安装
   rpm -q 包名

| 参数 | 作用        |
| ---- | ----------- |
| -q   | 查询(query) |

 rpm -qa	 #查询所有已经安装的RPM包



| 参数 | 作用        |
| ---- | ----------- |
| -q   | 查询(query) |
| -a   | 所有(all)   |

2. 查询软件包详细信息
   rpm -qi 包名
   选项:
   -i 	查询软件信息( information)
   -P	查询未安装包信息(package)



3. 查询包中文件安装位置
    rpm - ql包名
   选项: 
   -l		列表(list)
   -P		查询未安装包信息( package)



4. 查询系统文件属于哪个RPM包
   rpm- qf 系统文件名
   选项:
   -f查询系统文件属于哪个软件包(file)



5. 查询软件包的依赖性
   rpm - qR包名
   选项:
   -R 查询软件包的依赖性(requires)
   -p 查询未安装包信息( package)

##### RPM 包校验

用来判断系统文件是否被人更改



命令： rpm -V已安装的包名
选项:
	-V校验指定RPM包中的文件( verify)

验证内容中的8个信息的具体内容如下:
◆S		文件大小是否改变
◆M		文件的类型或文件的权限(rwx) 是否被改变
◆5		文件MD5校验和是否改变(可以看成文件内容是否改变)
◆D		设备的中，从代码是否改变
◆L		文件路径是否改变
◆U		文件的属主(所有者)是否改变
◆G		文件的属组是否改变
◆T		文件的修改时间是否改变



文件类型:
配置文件(config file)
◆d		普通文档(documentation )
◆g		鬼”文件(ghost file)，很少见，就是该文件不应该被这个RPM包包含
◆1		授权文件( license file)
◆r		描述文件(rread me )



示例：

```bash
[lqh@localhost ~]$ rpm -q docker
docker-1.13.1-102.git7f2769b.el7.centos.x86_64

[lqh@localhost ~]$ rpm -V docker
S.5....T.  c /etc/docker/daemon.json
S.5....T.  c /etc/sysconfig/docker-storage
```





##### RPM包中文件提取

提取RPM包中部分文件

```bash
[root@localhost ~] rpm2cpio包全名|\
cpio -idv .文件绝对路径

## 注意这里的 \ 表示一行太长，需要写在第二行，可以回车
## -idv 后面的.  表示提取到当前目录
```



rpm2cpio	#将rpm包转换为cpio格式的命令
cpio	#是一个标准工具，它用于创建软件档案文件和从档案文件中提取文件



[root@localhost ~]# cpio选项< [文件|设备]
选项:
-i: copy-in模式， 还原
-d:还原时自动新建目录
-V: 显示还原过程





```bash
[root@localhost ~]# rpm -qf /bin/ls
#查询s命令属于哪个软件包
[root@localhost ~]# mv /bin/Is /tmp/
#造成s命令误删除假象
[root@localhost ~]# rpm2cpio /mnt/cdrom/Packages/coreutils-
8.4-19.el6.i686.rpm| cpio -idv ./bin/ls
#提取RPM包中1s命令到当前目录的/bin/ls下
[root@localhost ~]# cp /root/bin/ls /bin/
#把Is命令复制会/bin/目录，修复文件丢失
```



### yum学习

####  查询

- yum list 查询所有可用的软件包列表

