```shell
##关机
shutdown
init 0
halt
##设定5分钟之后关机，同时发出警告信息给用户
shutdown +5"System will shutdown after 5 minutes"
##重启
init 6
shutdown -r
reboot
##取消前一个shutdown命令
shutdown -c 
```



#  Linux安装与基础配置

**软件安装设置**

SOFTWARE SELECTION

+ 初次使用，选择GNOME Desktop，提供Linux桌面环境
+ 需要程序开发 Development and Creative Workstation,该环境提供开发需要的软硬件、图形等工具
+ 只需要一个Linux环境，选择Minimal Install，该环境仅包含Linux系统必须的一些基础软件
+ 需要在Linux系统上运行虚拟化程序，选择Virtualiaztion Host，该环境包含虚拟化程序必须的软件和应用
+ 需要搭建一个Linux服务器，建议选择Server with GUI，此环境包含了基础的网络服务设施以及GUI桌面

**系统安装设置**

Linux系统下必须的分区为根分区（用`/`标识）和交换（标识为`swap`）分区。

swap分区相当于windows中的虚拟内存，也就是内存数据和硬盘的交换。

以下分区建议在安装系统时独立分配：

+ /boot：存储系统的引导信息和内核信息
+ /usr：存储系统应用软件安装信息
+ /var：存储系统日志信息

> 根分区包含Linux系统所有的目录，如果在安装时只分配了根分区，那上述的分区都会包含在根分区中

安装时选择**我要配置分区**(20gb)

+ /10gb

+ /boot 500mb
+ swap 4096mb
+ /usr 如果安装的应用软件多，可以分大一些
+ /var 建议分大一些 ,因为系统运行时间长后，日志文件很多

**配置网络**

先把右上角的按钮打开，然后选配置 -> 常规-> 第一项和第二项勾上，然后选IPv4设置，配一个地址



## 文件系统结构介绍

+ /etc 存放系统管理相关的配置文件以及子目录（系统初始化文件，用户信息文件等）
+ /usr存放应用程序和文件，该目录一般大一些
+ /var存放系统运行以及软件运行的日志信息
+ /dev包含系统所有的设备文件
+ /proc此目录是一个虚拟目录，目录中所有信息都是内存的映射，通过这个虚拟的内存映射目录，可以和内核内部数据结构进行交互（cpuinfo,meminfo,filesystems,devices
+ /boot 存放Linux启动时的核心文件（重要）
+ /bin /sbin 这两个目录存放的都是可执行的二进制文件，/bin目录存放的时常使用的Linux命令，如文件操作命令ls、cd、cp，/sbin(super user bin）只有超级用户才能执行这些命令，如fcsk、fdisk
+ /home该目录是系统中每个用户的工作目录。如用户user1，他的默认目录是/home/user1
+ /tmp该目录为临时文件目录



## 系统服务工具Systemd

启动、停止、重启服务

```shell
systemctl start httpd.service
systemctl stop httpd.service
systemctl restart httpd.service
systemctl try-restart httpd.service
systemctl reload httpd.service
```

开机启动、禁止、查看

```shell
systemctl enable httpd.service
systemctl disable httpd.service
systemctl status httpd.service
```



# Linux常用命令及使用技巧

**Shell的命令格式**

`command [optionals] [arguments]`

`\` 将命令持续到下一行

```shell
```



**shell的通配符**

`*`  匹配一或多个字符

`? ` 匹配任意单一字符

`[] ` 匹 配任何包含在方括号内的单字符

**重定向**

Linux系统默认打开三个文件：标准输入（默认为键盘）、标准输出（屏幕）、标准错误输入（屏幕）。

所谓的重定向，就是不使用系统默认的输入/输出，通过重定向符来操作

+ 输入重定向

  + `<`

  + `<<` 将一对分隔符之间的内容作为输入

```shell
--将/etc/shadow的内容作为命令输入给wc
wc < /etc/shadow
```

+ 输出重定向

  `>`和`>>`

```shell
--把file123里面的内容输入到file文件中
more file1 file2 file3 > file
```

+ 错误重定向
+ `2>`和`2>>`

**管道**

`|` 把左边的命令当作右边命令的输入

`ps -ef|grep httpd|wc -l`

## 系统管理与维护

**ls**：列出当前目录下所有文件，包括隐藏文件

```shell
ls -al
```

+ -a：显示指定目录下的所有文件以及子目录，包含隐藏文件（.开头的文件或目录为隐藏文档）

**pwd**

**date**

**cd**

**passwd**

**su：**用于改变用户身份

+ `-`加载相应用户下的环境变量
+ `-m`改变用户身份，但不改变环境变量

**clear**

**man** 显示帮助信息

**who** 显示目前登录到系统的用户

**w** 显示登录到系统的用户信息，详细版的who

**uname** 显示操作系统的相关信息

**uptime** 输出系统任务队列信息

**last** 列出目前与过去登入系统的用户相关信息，会默认读取/var/log目录下的wtmp文件

**dmesg** 显示开机信息

**free** 显示内存状态

+ -b 以字节为单位
+ -m 以mb为单位
+ -K kb
+ -t  totall
+ -s 5 隔5秒显示一次

**top**  类似于任务管理器

+ -d 指定刷新的时间间隔

交互式命令说明：

+ m 切换显示内存

+ t 切换显示进程和cpu

  

分区信息：

+ 统计信息区
+ 进程信息区
  + PID 进程ID
  + USER进程所有者用户名
  + PR进程优先级
  + NI nice值。负值表示高优先级，正值表示低优先级
  + VIRT 进程使用的虚拟内存总量 KB
  + RES 进程使用的、未被唤醒的物理内存大小
  + SHR 共享内存大小，单位KB
  + S进程状态 R 表示运行，D表示不可中断的睡眠，S表示睡眠，T表示跟踪/停止，Z表示僵死

## 文件管理与编辑 

**mkdir**

+ -m 设置权限
+ -p 路径不存在时**自动创建**

**more **将长文件内容输出到标准输出

+ -d 提示友好信息
+ -p 先清屏，再显示文本信息
+ -c 每次都清屏
+ -10 一个屏幕显示10行

**cat** 将文件内容输出到标准输出，或者拼接

`cat file1 file2 > file3`

**diff** 用来比较文件差异

+ -c 显示全部内容，并标出不同
+ -s 当两个文件相同时，显示相同的信息

**grep** 文本过滤工具

`grep 需要查找的字符串 文件名`

+ -i 忽略大小写
+ -n 行号

**rm** 删除目录或文件

+ -r 递归删除目录以及子目录
+ -f 忽略不存在的问题

**touch** 修改文件的创建时间，**不存在则创建此文件**

**ln **创建连接，默认时创建的是硬链接

`ln 源文件 目标链接名`

Linux下有两种链接，硬链接（Hard Link）和软链接（Symbolic Link）

+ 硬链接：是指通过文件的inode来进行链接，在Linux系统中，保存在磁盘的所有类型的文件都会分配一个编号，inode号（index node）。多个文件指向同一个inode在Linux系统中是允许的，这就是硬链接。删除一个硬链接不会影响inode和文件本身，只有当所有的链接都被删除了，该文件才会真正被删除
+ 软链接 类似于windows的快捷方式

- -b 链接出现同名文件覆盖或删除时进行备份 `xxx.txt~`



**file** 显示文件的类型



**cp **复制

+ -r 若源文件是目录，递归复制
+ -a 复制目录时使用，保留所有信息



**find** 在指定路径下查找文件

```
find / -path "/usr/bin" -name "main.c"
```

文件类型：

+ b 块设备文件
+ c 字符设备文件
+ f 普通文件
+ l 链接
+ d 目录
+ p 管道
+ s socket文件



**split** 分割文档

```shell
split -b 10m -a(以数字命名) -n(文件长度，默认1000行) access_log(input_file) access_bak(output_file)
```

分割成access_baka，access_bakb，access_bakc



**mv**  移动文件或目录



## 压缩与解 压

**zip** 压缩

```shell
zip 压缩文件名 要压缩的文档 
```

+ -r 递归压缩
+ -d 从压缩文件内删除指定文件
+ -x "文件列表"
+ -u 把文件更新到压缩文件中

**unzip**

**gzip** 压缩或解压

**tar** 归档工具，对文件或者目录进行打包归档为一个文件，但不进行压缩。

```shell
#将/etc中的所有文件打包到/usr/中并命名为haha.tar
tar -zcvf /usr/haha.tar /etc
```

+ -c 创建新文件
+ -z 打包时调用 gzip来压缩
+ -v 指定在创建归档文件的过程中，显示各个归档文件的名称
+ -f 必须为最后一个选项

## 磁盘管理与维护

**df** 检查磁盘的使用情况

+ -m 
+ -T 显示分区的系统类型
+ -i 显示分区的inode信息



**du** 显示文件或目录占用磁盘的空间情况

+ -s 显示文件或整个目录的大小，单位kb
+ -sh 以人性化的方式~
+ -sm 以mb显示文件或整个目录的大小

fsck

## 网络设置与维护

**ifconfig** 配置网络或显示当前网络接口状态

```shell
ifconfig ens33:0 192.168.118.129 netmask 255.255.255.0
```

给ens33网卡再配置一个ip地址



**scp** 将文件从一个Linux系统复制到另一个Linux系统下，传输数据采用SSH协议



**netstat** 显示本机网络连接、运行端口和路由表等信息。

+ -a 显示本机所有监听端口
+ -r 显示路由表
+ -t tcp
+ -u udp
+ -i 显示自动配置接口
+ -l 显示"LISTEN"
+ -p 显示对应的PID
+ -n 以网络ip的形式显示当前建立的有效连接和端口



**traceroute**

+ -s 设置源Ip

**telnet** 通过telnet协议与远程主机通信或获取远程主机对应端口的信息

```shell
telnet 192.168.60.123 23
```



## **本文编辑工具vi**

~



# 软件安装和管理

## **源文件安装**

wget http://mirrors~ 下载文件

./configure 进行软件安装的环境测试

make 编译

make install 安装



## RPM安装

*.rpm 的软件包 是rpm文件，这些文件将软件源码文件进行编译、安装，然后进行封装，就成了rpm文件，类似于windows中的.exe 

不会自动装依赖

使用：

```shell
rpm -i xxx.rpm
```



查询

```shell
rpm -q rpm
rpm -qa|grep yum
```



更新

```shell
rpm -U file.rpm
```

删除

```shell
rpm -e file.rpm
```





## YUM安装

会自动装依赖

安装/删除

```shell
yum install/remove dhcp
```

检查可更新的包

```shell
yum ckeck-update
```

更新所有/指定的包

```shell
yum update [包名]
```

查看包/已安装的包/可升级的包信息

```shell
yum info [包名]/[installed]/[updates]
```

列出所有已安装的包

```shell
yum list installed
```





## 二进制安装

对于`*.tar.gz`软件格式

```shell
tar -zxvf xxx.tar.gz
```

对于`*.bz2`软件格式

```shell
tar -jxvf xxx.bz2
```





# 用户管理







