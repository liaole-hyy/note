##### 一、菜鸟教程安装与学习地址

1.https://www.runoob.com/linux/linux-tutorial.html

vmware安装https://blog.51cto.com/u_14473285/2428255

2.liunx基本知识

###### 1)文件结构

![image-20210413211641917](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210413211641917.png)

###### 2)目录解释

/bin : bin 是Binaries的缩写 ，这里面存放的是经常用的命令

/boot : 这里存放的是启动linux 时使用的一些核心文件，包括一些链接文件以及镜像文件

/dev ：dev是Device设备的缩写,该目录下存放的是linux的外部设备，在linux中访问设备的方式和访问文件的方式相同的

/etc : etc 是 Etcetera(等等) 的缩写,这个目录用来存放所有的系统管理所需要的配置文件和子目录

/home：
用户的主目录，在 Linux 中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的，如上图中的 alice、bob 和 eve

/lib :

lib 是 Library(库) 的缩写这个目录里存放着系统最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件。几乎所有的应用程序都需要用到这些共享库。

/lost+found : 

这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。

/media : linux 系统会自动识别一些设备，例如U盘、光驱等等，当识别后，Linux 会把识别的设备挂载到这个目录下

/mnt :

系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在 /mnt/ 上，然后进入该目录就可以查看光驱里的内容了

/opt :

opt 是 optional(可选) 的缩写，这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。

/proc :

/proc 是一种伪文件系统（也即虚拟文件系统），存储的是当前内核运行状态的一系列特殊文件，这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。

/root :该目录为系统管理员，也称作超级权限者的用户主目录

/sbin : 是 Superuser Binaries (超级用户的二进制文件) 的缩写，这里存放的是系统管理员使用的系统管理程序

/selinux :Selinux 是一个安全机制，类似于 windows 的防火墙，但是这套机制比较复杂，这个目录就是存放selinux相关的文件的。

/srv : 该目录存放一些服务启动之后需要提取的数据。

/sys : 该目录下安装了 2.6 内核中新出现的一个文件系统 sysfs.sysfs 文件系统集成了下面3种文件系统的信息：针对进程信息的 proc 文件系统、针对设备的 devfs 文件系统以及针对伪终端的 devpts 文件系统,该文件系统是内核设备树的一个直观反映。当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。

/tmp: 是 temporary(临时) 的缩写这个目录是用来存放一些临时文件的。

/usr : unix shared resources(共享资源) 的缩写，这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于 windows 下的 program files 目录。

/usr/bin :系统用户使用的应用程序。

/usr/sbin :超级用户使用的比较高级的管理程序和系统守护程序。

/usr/src :内核源代码默认的放置目录

/var :这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。

/run :是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。如果你的系统上有 /var/run 目录，应该让它指向 run。

###### 3)文件类型

1.普通文件，可以看到有类似-rwxrwxrwx，值得注意的是第一个符号是 -，这样的文件在Linux中就是普通文件。这些文件一般是用一些相关的应用程序创建

2.目录文件，看到有类似 drwxr-xr-x ，这样的文件就是目录，目录在Linux是一个比较特殊的文件。

3.字符设备或块设备文件，区块(block)设备文件 ：就是一些储存数据， 以提供系统随机存取的接口设备，举例来说，硬盘与软盘等就是啦。 你可以随机的在硬盘的不同区块读写，这种装置就是成组设备。你可以自行查一下/dev/sda看看， 会发现第一个属性为[ b ]。

字符(character)设备文件：亦即是一些串行端口的接口设备， 例如键盘、鼠标等等。这些设备的特色就是一次性读取的，不能够截断输出。 举例来说，你不可能让鼠标跳到另一个画面，而是滑动到另一个地方。第一个属性为 [ c ]。

4.数据接口文件(sockets)：数据接口文件（或者：套接口文件），这种类型的文件通常被用在网络上的数据承接了。我们可以启动一个程序来监听客户端的要求， 而客户端就可以透过这个socket来进行数据的沟通了。第一个属性为 [ s ]， 最常在/var/run这个目录中看到这种文件类型了。

5.符号链接文件：当我们查看文件属性时，会看到有类似 lrwxrwxrwx，注意第一个字符是l，这类文件是链接文件。是通过ln -s 源文件名 新文件名创建的。这和Windows操作系统中的快捷方式有点相似。

###### 4)文件权限

- chown (change ownerp) ： 修改所属用户与组。

- chmod (change mode) ： 修改用户的权限。

  chown 相当于是授权用户。chmod相当于给用户权限

  ![image-20210413225241845](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210413225241845.png)

​       从左至右用 **0-9** 这些数字来表示。

第 **0** 位确定文件类型，第 **1-3** 位确定属主（该文件的所有者）拥有该文件的权限。

第4-6位确定属组（所有者的同组用户）拥有该文件的权限，第7-9位确定其他用户拥有该文件的权限。

其中，第 **1、4、7** 位表示读权限，如果用 **r** 字符表示，则有读权限，如果用 **-** 字符表示，则没有读权限；

第 **2、5、8** 位表示写权限，如果用 **w** 字符表示，则有写权限，如果用 **-** 字符表示没有写权限；第 **3、6、9** 位表示可执行权限，如果用 **x** 字符表示，则有执行权限，如果用 **-** 字符表示，则没有执行权限。

######   5)文件相关命令

 1.chgrp：更改文件属组 

```
chgrp [-R] 属组名 文件名
```

其中-R：递归更改文件属组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属组都会更改

2.chown：更改文件属主，也可以同时更改文件属组

```
chown [–R] 属主名 文件名
chown [-R] 属主名：属组名 文件名
查看文件类型以及编码命令
file -i 文件
```

3.chmod：更改文件9个属性

Linux文件属性有两种设置方法，一种是数字，一种是符号。

Linux 文件的基本权限就有九个，分别是 **owner/group/others(拥有者/组/其他)** 三种身份各有自己的 **read/write/execute** 权限。

每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为： **-rwxrwx---** 分数则是：

- owner = rwx = 4+2+1 = 7
- group = rwx = 4+2+1 = 7
- others= --- = 0+0+0 = 0

###### 6)处理目录的常用命令 

```
ls（英文全拼：list files）: 列出目录及文件名
ls -ll :显示详情列表
ls -al :显示所有，包括隐藏文件
ls -lh ：显示文件大小M
cd（英文全拼：change directory）：切换目录
pwd（英文全拼：print work directory）：显示目前的目录
mkdir（英文全拼：make directory）：创建一个新的目录
mkdir -p /usr/local/redis/logs
rmdir（英文全拼：remove directory）：删除一个空的目录
cp（英文全拼：copy file）: 复制文件或目录
rm（英文全拼：remove）: 删除文件或目录 rm -rf 文件或者非空目录
mv（英文全拼：move file）: 移动文件与目录，或修改文件与目录的名称
```

###### 7)压缩与解压

```
1.压缩 c
tar -cvf jpg.tar *.jpg
tar -czf jpg.tar.gz *.jpg
rar a jpg.rar *.jpg 
zip jpg.zip *.jpg
2.解压 x
tar -xzf jpg.tar.gz
tar -xvf jpg.tar
tar -xzvf jpg.tar.gz或者tar -xzvf jpg.tar
*.zip 用unzip xxxx.zip
*.rar 用unrar e 
3压缩成.war包 c
jar -cvfM0 file2.war ./
4.解压缩.war包到当前目录 x
jar -xvf file.war
```



##### 二、虚拟机账号与密码

root ： root  @liaole123456

root用户登录 当前目录在/root ,里面是没有内容的 

cd ../ 到 根目录下，可以看到所有目录

###### 1)用户相关命令

```
1.新增账号
useradd 选项 用户名
选项
-c comment 指定一段注释性描述。
-d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
-g 用户组 指定用户所属的用户组。
-G 用户组，用户组 指定用户所属的附加组。
-s Shell文件 指定用户的登录Shell。
-u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。
useradd –d  /home/sam -m liaole 或者useradd liaole,/etc/passwd文件会显示所有用户数据
2.删除账号
userdel 选项 用户名
userdel -r mysql正确删除用户
3.修改账号
usermod 选项 用户名
4.用户口令的管理
指定和修改用户口令的Shell命令是passwd。超级用户可以为自己和其他用户指定口令，普通用户只能用它修改自己的口令
passwd 选项 用户名
5cat /etc/passwd|grep 用户名 查看当前用户相关信息
6.groups
groups 用户名
whoami
```

###### 2)虚拟机网卡开启至联网

1.网络主机更改成nat模式

![image-20210414204629693](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210414204629693.png)

2.开启虚拟机网卡配置

vi  /etc/sysconfig/network-scripts/ifcfg-ens33

更改onboot=no ---> onboot=yes

输入命令service network restart重启网络服务.

centos后面版本使用systemctl命令来设置某个服务

```
启动一个服务：systemctl start firewalld.service
关闭一个服务：systemctl stop firewalld.service
重启一个服务：systemctl restart firewalld.service
显示一个服务的状态：systemctl status firewalld.service
在开机时启用一个服务：systemctl enable firewalld.service
在开机时禁用一个服务：systemctl disable firewalld.service
查看服务是否开机启动：systemctl is-enabled firewalld.service;echo $?
systemctl get-default //获取当前系统启动模式
systemctl set-default graphical.target由命令行模式更改为图形界面模式
systemctl set-default multi-user.target由图形界面模式更改为命令行模式
默认模式为multi-user.target
```

###### 3)centos7鼠标右键复制粘贴

```
一. 命令行执行yum install gpm*

二.运行命令service gpm start
```

###### 4)查看环境变量

```
echo $PATH
1.Linux查看环境变量使用env命令显示所有的环境变量
$ env
2.Linux查看环境变量使用set命令显示所有本地定义的Shell变量
$ set
3.设置与删除
$ export TEST=”Test…” #增加一个环境变量TEST
$ env|grep TEST #此命令有输入，证明环境变量TEST已经存在了
TEST=Test…
$ unset $TEST #删除环境变量TEST
$ env|grep TEST #此命令没有输出，证明环境变量TEST已经存在了
```

###### 5)配置查看ip命令 ifconfig

```
yum search ifconfig

yum install net-tools.x86_64
```

###### 6)centos7 配置语言支持

```
1.查看系统支持的语言有哪些
locale -a
2.查看当前系统正在用语言
echo $LANG
3.设置语言
set env LANG zh
4.查看语言的配置文件
cat /etc/locale.conf
备注:在linux服务器上如果命令行，是不支持中文输入，只能显示中文，在使用图形界面启动模式是可以支持的，可以借助xshell，secureCRT等远程工具，可以轻松复制粘贴等功能以及中英文切换等，在vmWare真实linux服务器环境命令行只能英文输入，复制粘贴也很麻烦不方便。
```

###### 7)重启和关机命令

```
重启命令：
1、reboot
2、shutdown -r now 立刻重启(root用户使用)
3、shutdown -r 10 过10分钟自动重启(root用户使用) 
4、shutdown -r 20:35 在时间为20:35时候重启(root用户使用)
如果是通过shutdown命令设置重启的话，可以用shutdown -c命令取消重启

关机命令：
1、halt   立刻关机
2、poweroff  立刻关机
3、shutdown -h now 立刻关机(root用户使用)
4、shutdown -h 10 10分钟后自动关机
```

###### 8)进程以及端口命令

```
1.查看所有进程
ps -A 显示所有进程  pstree 显示进程的树状图
2.显示在运行所有进程,# ps -ef //显示所有命令，连带命令行,
ps -u root //显示root进程用户信息
ps -aux | more
2.查找某类进程
ps -aux | grep nginx 或者 ps -ef|grep nginx
3.直接杀进程
kill -s 9 pid
4.查找端口谁在用
netstat -anp | grep 80  查看端口
5.显示系统时间
date 
date "+%Y-%m-%d %H:%M:%S" （功能：显示年月日时分秒）
时间同步的方式
1.删除本地时间rm -f /etc/localtime
2.使用命令cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime复制时区文件到localtime
3.使用命令“yum install -y ntpdate”安装“ntpdate”
4.使用命令“ntpdate -u ntp.api.bz”将本地时间与网络时间同步
```

###### 9)查看日志，以及查找日志

```
1.tail 命令可用于查看文件的内容，有一个常用的参数 -f 常用于查阅正在改变的日志文件。
     a.tail -f filename 会把 filename 文件里的最尾部的内容显示在屏幕上，并且不断刷新，只要 filename 更新就可以看到最新的文件内容
tail [参数] [文件] 
-n<行数> 显示文件的尾部 n 行内容
      tail -n +20 notes.log  显示从第 20 行至文件末尾:
      tail -n 20 notes.log  显示filename最后20行。 
      tail -r -n 10 filename  逆序显示filename最后10行。
      head -n 10  test.log   查询日志文件中的头10行日志;
      head -n -10  test.log   查询日志文件除了最后10行的其他所有日志; 
2.从文件内容查找匹配指定字符串的行：
grep "被查找的字符串" 文件名
3.查找时不区分大小写：
 grep –i "被查找的字符串" 文件名
4.从文件内容查找与正则表达式匹配的行：
 grep –e "正则表达式" 文件名
5.查找匹配的行数：
  grep -c "被查找的字符串" 文件名
6从根目录开始查找所有扩展名为 .log 的文本文件，并找出包含 "ERROR" 的行：
find / -type f -name "*.log" | xargs grep "ERROR"
find / -name file
可以用-iname不区分大小写，find / -iname file
7.查找出'eth0'的前后各三行:grep -n -A 3 -B 3 'eth0'或者 grep -n -C 3 'eth0'
8.解释：'[^g]oo'查找oo前面没有g的字符串。第19行，也可以组成oo前面是oo,故而符合要求。
9行首匹配用^,行尾匹配用$：grep -n '^[a-z]' regular_express.txt
```

###### 10) sudo命令

```
Linux sudo命令以系统管理者的身份执行指令，也就是说，经由 sudo 所执行的指令就好像是 root 亲自执行。
使用权限：在 /etc/sudoers 中有出现的使用者。
sudo 规则;
-V 显示版本编号
-l 显示出自己（执行 sudo 的使用者）的权限
```

###### 11)yum常用命令

```
1. 列出所有可更新的软件清单命令：yum check-update
2.  更新所有软件命令：yum update
3. 仅安装指定的软件命令：yum install <package_name>
4. 仅更新指定的软件命令：yum update <package_name>
5. 列出所有可安裝的软件清单命令：yum list
6. 删除软件包命令：yum remove <package_name>
7.  查找软件包命令：yum search <keyword>
8.  yum clean all:清除缓存目录下的软件包及旧的 headers
9.yum clean packages: 清除缓存目录下的软件包,yum clean headers: 清除缓存目录下的 headers,yum clean oldheaders: 清除缓存目录下旧的 headers
```

###### 12)防火墙关闭命令

```
# 关闭防火墙
systemctl stop firewalld.service
# 禁止防火墙开机启动
systemctl disable firewalld.service
# 启动docker
systemctl start docker
# 重启docker
systemctl restart docker
```

###### 13)安装rzsz

```
1. yum install lrzsz
2.使用
从服务端发送文件到客户端：sz filename
从客户端上传文件到服务端：rz

```

###### 14)vi相关

```
一、在Linux中的vi编辑模式中查找关键字的方法：
1、进入vi中，先按下"ESC"跳转成命令输入模式
2、输入斜杠“/”，这时屏幕会跳转到底部，输入栏出现"/"
3、输入你需要查找的关键字，回车
4、如果要继续查找关键字，输入n
5、向前查找，输入N（大写）
6、删除一行就直接在vi的命令模式下按键dd
7、u 撤销上一步的操作，Ctrl+r 恢复上一步被撤销的操作，对等操作
      
```



##### 三、docker容器的安装与使用

###### 1.docker背景

​	在分布式微服务大发展背景下，越来越多因为开发环境和测试环境或者生产环境都需要进行一系列服务配置，中间有开发人员，测试人员，运维成员，在承担不同环境职责，会因为环境配置或者环境影响服务出现一些错误或者未知问题影响工作效率系统稳定等因素，出现docker,开发人员可以将自己开发环境 进行打包成一个镜像, 然后发布到Docker仓库  运维人员 通过Docker运行镜像就可以获得和开发人员相同的软件环境 从而避免环境问题导致的错误异常.

 docker可以一次构建，处处运行

###### 2.docker安装

```
默认安装路径 /var/lib/docker
# 设置yum源
yum install -y yum-utils device-mapper-persistent-data lvm2
 
# 添加yum源(官方)
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 添加yum源(阿里) 推荐使用这一条 官方的太慢了
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 生成缓存 更新yum软件包索引
yum makecache fast
 
# 安装Docker-CE
yum -y install docker-ce
```

###### 3.docker常用命令

```
docker version # 查看版本
systemctl start docker #　启动
systemctl stop docker　# 停止
systemctl status docker #　查看状态
systemctl restart docker　　# 重启
systemctl enable docker  # 设置开机自动启动
# 验证，运行hello-world
docker run hello-world  # 下载hello-world镜像并运行
docker container ls 列表
docker container ls -a ：它用于列出所有正在运行的容器。
```

###### 4.docker常用命令

| 操作       | 命令                             | 说明                                                    |
| ---------- | -------------------------------- | ------------------------------------------------------- |
| 查找       | docker search 关键字             | 可以在Docker Hub网站查看镜像的详细信息，如镜像的tag标签 |
| 拉取       | docker pull 镜像名:tag           | :tag表示软件的版本，如果不指定默认是latest              |
| 列表       | docker images                    | 查看所有本地镜像                                        |
| 获取原信息 | docker inspect 镜像id            | 获取镜像的元信息，详细信息                              |
| 删除       | docker rmi -f 镜像id或镜像名:tag | 删除指定的本地镜像，-f表示强制删除                      |

###### 5.容器常用操作命令

容器的镜像地址：https://hub.docker.com/search 

| 操作         | 命令                                                         | 说明                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 运行         | docker run --name 容器名 -i -t -p 主机端口:容器端口 -d -v 主机目录:容器目录:ro 镜像id或镜像名称:tag | --name 指定容器名，名称自定义，如果不指定会自动命名； -i 以交互模式运行，即以交互模式运行容器；-t 分配一个伪终端，即命令行，通常组合使用-it ；-p 指定端口映射，将主机端口映射到容器内的端口；-d 表示后台运行，即守护式运行容器；-v 指定挂载主机目录到容器目录，默认为rw读写模式，ro表示只读 |
| 列表         | docker ps -a -q                                              | 查看正在运行的容器，-a表示显示所有容器，-q表示只显示容器id   |
| 启动         | docker start 容器id或容器名称                                | 启动容器                                                     |
| 停止         | docker stop 容器id或容器名称                                 | 停止正在运行的容器                                           |
| 删除         | docker rm -f 容器id或容器名称                                | 删除容器，-f表示强制删除                                     |
| 日志         | docker logs 容器id或容器名称                                 | 获取容器的日志                                               |
| 在容器中执行 | docker exec -it 容器id或容器名称 /bin/bash                   | 进入正在运行的容器中并开启一个交互模式的终端，可以在容器中执行操作 |
| 拷贝文件     | docker cp 主机中的文件路径 容器id或容器名称:容器路径；docker cp 容器id或容器名称:容器中的文件路径 主机路径 | 将文件中的文件拷贝到容器中；将容器中的文件拷贝到主机中       |
| 获取元信息   | docker inspect 容器id                                        | 获取容器的元信息                                             |

###### 6.docker安装tomcat

```
1.拉取tomcat:docker pull tomcat  //这表示拉取最新的
2.启动tomcat,容器中默认端口，以容器名称启动
docker run --name mytomcat tomcat:latest //
这种启动方式没有做端口映射 是无法通过ip加端口方式访问
3.docker run -p 8888:8080 -d tomcat(镜像名字) 这种启动方式，可以通过ip：8888访问（-d 是设置该容器后台运行）
或者docker run --name tomcat_v1 -p:8888:8080 -d qsssyh/tomcat:v1
镜像的创建
4.提交镜像，在已有的镜像上自定义镜像，必须先启动
 语法：docker commit -m="描述消息" -a="作者" 容器id或容器名 镜像名:tag
 docker commit -m="自定义Tomcat镜像Demo" -a="liaole" eb3257561d9e  liaole/tomcat:v1
5.测试镜像
docker run --name tomcat_v1 -p:8080:8080 -d liaole/tomcat:v1
6.进入到容器命令交互模式
docker exec -it eb3257561d9e /bin/bash
7.根据Dockerfile文件来自动构建镜像
	 (1) 在根目录创建 Dockerfile 文件 vi Dockerfile
	 (2) 编辑Dockerfile 文件 文件中以#开头为注释
	
# 基础镜像
FROM tomcat
# 作者
MAINTAINER qq2940500
 
# 执行命令
RUN rm -f /usr/local/tomcat/webapps/ROOT/index.jsp
RUN echo "welcome to tomcat!" > /usr/local/tomcat/webapps/ROOT/index.html
	(3) 构建新镜像，语法：docker build -f Dockerfile文件的路径 -t 镜像名:tag 命令执行的上下文
最后的 . 符号是执行上下文的语法 必须要有的

8.docker tag根据镜像id做标签，用于应用的回滚
  docker iamge tag 镜像名src:tag 镜像名targert:tag
9.docker镜像推送和拉取
//登录镜像中心
sudo docker login --username=xxxxx registry.cn-shanghai.aliyuncs.com
//中心中拉取镜像
sudo docker pull registry.cn-shanghai.aliyuncs.com/liaole/tomcat:[镜像版本号]
//将镜像推送到Registry
sudo docker push registry.cn-shanghai.aliyuncs.com/liaole/tomcat:[镜像版本号]
```

##### 四、mysql相关安装

```
1.查找mysql的镜像版本
$ docker search mysql
2.拉取mysql最新版本
docker pull mysql:latest
3.运行容器
docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
参数说明
-p 3306:3306 ：映射容器服务的 3306 端口到宿主机的 3306 端口，外部主机可以直接通过 宿主机ip:3306 访问到 MySQL 的服务。
MYSQL_ROOT_PASSWORD=123456：设置 MySQL 服务 root 用户的密码。
4.查看是否安装成功
docker ps
5.登录mysql
mysql -h localhost -u root -p
6.允许所有ip访问root用户（下面暂时不能用）
alter user 'root'@'localhost' identified with mysql_native_password by '123456';
grant all privileges on *.* to root@'%' ;
7.建立目录映射方式启动至外部访问
1)创建目录
/opt/mysql/data
/opt/mysql/logs
/opt/mysql/conf
data目录将映射为mysql容器配置的数据文件存放路径
logs目录将映射为mysql容器的日志目录
conf目录里的配置文件将映射为mysql容器的配置文件

2)docker run -p 3306:3306 --name mysql -v $PWD/conf:/etc/mysql/conf.d -v $PWD/logs:/logs -v $PWD/data:/var/lib/mysql  --privileged=true  -e MYSQL_ROOT_PASSWORD=123456 -d mysql

-p 3306:3306：将容器的 3306 端口映射到主机的 3306 端口。
-v -v $PWD/conf:/etc/mysql/conf.d：将主机当前目录下的 conf/my.cnf 挂载到容器的 /etc/mysql/my.cnf。
-v $PWD/logs:/logs：将主机当前目录下的 logs 目录挂载到容器的 /logs。
-v $PWD/data:/var/lib/mysql ：将主机当前目录下的data目录挂载到容器的 /var/lib/mysql 。
-e MYSQL_ROOT_PASSWORD=123456：初始化 root 用户的密码。
8.进入容器
docker exec -it  id /bin/bash
mysql -u root -p
ALTER USER 'root'@'%' IDENTIFIED BY '123456' PASSWORD EXPIRE NEVER;
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password by '123456';
flush privileges;

## mysql8解决sql_mode=only_full_group_by
set global sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

set session sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';
--查询时区
show variables like '%time_zone%';
```

##### 五、jdk环境安装

```
1.查看JDK软件包列表
yum search java | grep -i --color jdk
2.安装jdk
yum install -y java-1.8.0-openjdk.x86_64 java-1.8.0-openjdk-devel.x86_64
3.环境变量配置
 1)jdk默认安装路径 /usr/lib/jvm
 2)vi /etc/profile
 # set java environment  
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.181-3.b13.el7_5.x86_64
PATH=$PATH:$JAVA_HOME/bin  
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar  
export JAVA_HOME  CLASSPATH  PATH 
4.保存关闭profile文件，执行如下命令生效
source /etc/profile
5.查看jdk变量
echo $JAVA_HOME
echo $PATH
echo $CLASSPATH
6.查看版本
java -version
7.java11 
home地址
/usr/lib/jvm/java-11-openjdk-11.0.11.0.9-1.el7_9.x86_64
```

##### 六、nacos相关安装

```
1.下载nacos镜像
 docker pull nacos/nacos-server
2.运行
docker run -d -p 8848:8848 -e MODE=standalone - v /opt/nacos/init.d/custom.properties:/home/nacos/init.d/custom.properties -v/opt/nacos/logs:/home/nacos/logs --restart always --name nacos nacos/nacos-server
//这里custom.properties 应该是个文件才对 不应该是目录，如果创建成目录会报错

```

##### 七、nginx相关安装 lvs+keeplived openstry

```
1. 安装依赖库
yum install libtermcap-devel ncurses-devel libevent-devel readline-devel pcre- devel gcc openssl openssl-devel per perl wget

2.下载安装包
wget https://openresty.org/download/openresty-1.11.2.5.tar.gz
//此时会下载到当前目录下
3.tar -xf openresty-1.11.2.5.tar.gz
4.安装
./configure --prefix=/usr/local/openresty --with-luajit --without- http_redis2_module --with-http_stub_status_module --with-http_v2_module --with-http_gzip_static_module --with-http_sub_module --add- module=/usr/local/liaole/ngx_cache_purge-2.3/
5.编译并安装 
make && make install
6.环境变量安装
vi /etc/profile  // 编辑
export PATH=/usr/local/openresty/nginx/sbin:$PATH //添加
source /etc/profile  //使文件生效
7配置开机启动
vi /usr/lib/systemd/system/nginx.service
[Service] 
Type=forking 
PIDFile=/usr/local/openresty/nginx/logs/nginx.pid ExecStartPre=/usr/local/openresty/nginx/sbin/nginx -t ExecStart=/usr/local/openresty/nginx/sbin/nginx
ExecReload=/bin/kill -s HUP $MAINPID 
ExecStop=/bin/kill -s QUIT $MAINPID 
PrivateTmp=true

[Install] 
WantedBy=multi-user.target

8重新加载某个服务的配置文件: systemctl daemon-reload
9开机启动: systemctl enable nginx.service
10启动nginx：systemctl start nginx.service
11nginx自带的命令
nginx -s reload
```

##### 八、redis相关安装

###### 1.使用linux本地方式安装

```
1.获取redis资源
wget http://download.redis.io/releases/redis-6.0.10.tar.gz
2.解压
tar -xzvf redis-6.0.10.tar.gz
3.安装
cd redis-6.0.10
make && make install PREFIX=/usr/local/redis
会报错 高版本redis
4.安装Redis6.0.10需要高版本gcc，所以先临时升级gcc
yum -y install centos-release-scl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
#临时将gcc设置为高版本，会话结束就变回低版本gcc
scl enable devtoolset-9 bash
5.重新make ,然后make install PREFIX=/usr/local/redis
6.在安装目录下创建配置文件
mkdir /usr/local/redis/etc 
mv redis.conf /usr/local/redis/etc
7修改配置文件
#主要修改的内容
port 6379    #端口 默认可以修改
daemonize yes    #后台运行
pidfile /var/run/redis.pid    #pid文件位置，和启动脚本保持一至，代表redis启动时候pid存放的位置
4启动脚本编写
vi /etc/rc.d/init.d/redis
#!/bin/sh
#
# Simple Redis init.d script conceived to work on Linux systems
# as it does use of the /proc filesystem.
source /etc/init.d/functions
REDISPORT=6379
EXEC=/usr/local/redis/bin/redis-server
CLIEXEC=/usr/local/redis/bin/redis-cli
 
PIDFILE=/var/run/redis.pid    //判断进程是否已经开启
CONF=/usr/local/redis/etc/redis.conf
BIND_IP='0.0.0.0' 
 
start(){     
    if [ -f $PIDFILE ]
    then
        echo "$PIDFILE exists, process is already running or crashed"
    else
        echo "Starting Redis server..."
        $EXEC $CONF
    fi
    if [ "$?"="0" ]  
    then  
        echo "Redis is running..." 
    fi 
}
 
stop(){ 
    if [ ! -f $PIDFILE ]
    then
        echo "$PIDFILE does not exist, process is not running"
    else
        PID=$(cat $PIDFILE)
        echo "Stopping ..."       
        $CLIEXEC -h $BIND_IP  -p $REDISPORT  SHUTDOWN 
        sleep 1
        while [ -x /proc/${PID} ]
        do
            echo "Waiting for Redis to shutdown ..."
            sleep 1
        done
            echo "Redis stopped"
    fi
}
 
restart(){
    stop
    start 
}

status(){ 
    ps -ef|grep redis-server|grep -v grep >/dev/null 2>&1 
    if [ $? -eq 0 ];then 
        echo "redis server is running" 
    else
        echo "redis server is stopped" 
    fi
} 
 
case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;         
    restart)
        restart
        ;;         
    status)
        status
        ;;     
    *)
     
     echo "Usage: redis-server {start|stop|status|start}" >&2 
     exit 1 
esac
5.脚本加权限 chmod +x /etc/rc.d/init.d/redis
6.启动 service redis start  --> systemctl status redis
7.将redis-cli,redis-server拷贝到bin下，让redis-cli指令可以在任意目录下直接使用
cp /usr/local/redis/bin/redis-server /usr/local/bin/
cp /usr/local/redis/bin/redis-cli /usr/local/bin/
```

##### 九、cancel相关安装

```
1.docker安装下载镜像
docker pull canal/canal-server
2.执行安装启动
docker run -p 11111:11111 --name canal -d
3.进入执行目录
docker exec -it canal /bin/bash
4.canal默认安装目录 /home/admin/canal-server
5.修改配置文件
修改核心配置canal.properties 和instance.properties，
canal.properties 是canal自身的配置，
instance.properties是需要同步数据的数据库连接配置。
其中canal.properties
canal.id = 2  //增加
example/instance.properties  
# position info canal.instance.master.address=192.168.5.128:3306
# table regex #canal.instance.filter.regex=.*\\..* 
#监听配置 
canal.instance.filter.regex=shop_goods.ad_items
```

##### 十、elasearch+logstash+kibaner相关安装

###### 1.elasearch安装

```
1.docker 下载elasticsearch
docker pull elasticsearch:6.8.12
2.docker run --name elasticsearch -d -e ES_JAVA_OPTS="-Xms512m -Xmx512m" -e "discovery.type=single-node" -p 9200:9200 -p 9300:9300 elasticsearch:6.8.12
docker start elasticsearch
3.进入容器
docker exec -it elasticsearch /bin/bash
4. 跨域配置 修改/usr/share/elasticsearch/config/elasticsearch.yml
cluster.name: "elasticsearch"
http.cors.enabled: true
http.cors.allow-origin: "*"
network.host: 0.0.0.0
discovery.zen.minimum_master_nodes: 1
5.重启容器`docker restart elasticsearch`
6.访问：http://192.168.5.128:9200/
7.如果启动不了，设置max_map_count不能启动es会启动不起来
cat /proc/sys/vm/max_map_count 默认是65530
重新设置max_map_count的值 sysctl -w vm.max_map_count=262144
8日志查看
docker contaier logs elasticsearch
docker logs -f -t --tail 10 elasticsearch
```

###### 2.参数说明

```
cluster.name:集群服务名字
http.cors.enabled:开启跨域
http.cors.allow-origin: 允许跨域域名，*代表所有域名
network.host: 外部访问的IP
discovery.zen.minimum_master_nodes: 最小主节点个数
```

###### 3.ik分词器安装

```
1.分词器下载地址：https://github.com/medcl/elasticsearch-analysis-ik/releases/tag/v6.8.0
2.下载后，将压缩包解压并将加压的文件放到ik目录下，并拷贝到elasticsearch的plugins目录下即可，用如下命令拷贝：
docker cp ik elasticsearch:/usr/share/elasticsearch/plugins/
3.重启es:
docker restart elasticsearch
备注 分词器要和es版本一致
4.测试是否安装成功
查看 elasticsearch-plugin list
head 验证 http://192.168.5.128:9200/
POST _analyze
{
  "analyzer": "ik_smart",
  "text": "我是中国人"
}

```

###### 4.es-head安装

```
docker run -d -p 9100:9100 docker.io/mobz/elasticsearch-head:5
blissful_rhodes
1、进入`head`安装目录；

2、`cd _site/`

3、编辑`vendor.js`  共有两处

①、6886行   `"application/x-www-form-urlencoded`，改成：` "application/json;charset=UTF-8"`

②、7574行 `"application/x-www-form-urlencoded"`改成：` "application/json;charset=UTF-8"`

docker cp blissful_rhodes:/usr/src/app/_site/vendor.js /root
docker cp /root/vendor.js  blissful_rhodes:/usr/src/app/_site/
```

##### 十一、分布式事务seata 安装

```
1.docker下载seata
docker pull seataio/seata-server
2.安装seata,并运行
docker run --name seata-server -p 8091:8091 -d seataio/seata-server

```

##### 十二、安装RockerMq

###### 1.安装namesrv

```
1.docker 安装 拉取镜像
docker pull rocketmqinc/rocketmq

2.创建namesrv数据存储目录
mkdir -p /opt/rocketmq/data/namesrv/logs
mkdir -p /opt/rocketmq/data/namesrv/store

3.安装NameSrv
docker run -d \ 
--restart=always \ 
--name rmqnamesrv \ 
--privileged=true \ 
-p 9876:9876 \ 
-v /opt/rocketmq/data/namesrv/logs:/root/logs \ 
-v /opt/rocketmq/data/namesrv/store:/root/store \ 
-e "MAX_POSSIBLE_HEAP=100000000" \ 
rocketmqinc/rocketmq \ 
sh mqnamesrv
```

###### 2.namesrv参数说明

| 参数                                               | 说明                                                         |
| -------------------------------------------------- | ------------------------------------------------------------ |
| -d                                                 | 以守护进程的方式启动                                         |
| \- -restart=always                                 | docker重启时候容器自动重启                                   |
| \- -name rmqnamesrv                                | 把容器的名字设置为rmqnamesrv                                 |
| -p 9876:9876                                       | 把容器内的端口9876挂载到宿主机9876上面                       |
| -v  /docker/rocketmq/data/namesrv/logs:/root/logs  | 把容器内的/root/logs日志目录挂载到宿主机的 ，/docker/rocketmq/data/namesrv/logs目录 |
| -v /docker/rocketmq/data/namesrv/store:/root/store | 把容器内的/root/store数据存储目录挂载到宿主机的 /docker/rocketmq/data/namesrv目录 |
| rmqnamesrv                                         | 容器的名字                                                   |
| -e “MAX_POSSIBLE_HEAP=100000000”                   | 设置容器的最大堆内存为100000000                              |
| rocketmqinc/rocketmq                               | 使用的镜像名称                                               |
| sh  mqnamesrv                                      | 启动namesrv服务                                              |

###### 3.安装broker

```
1.创建配置文件 vi /docker/rocketmq/conf/broker.conf
# 所属集群名称，如果节点较多可以配置多个 
brokerClusterName = DefaultCluster 
#broker名称，master和slave使用相同的名称，表明他们的主从关系 
brokerName = broker-a 
#0表示Master，大于0表示不同的slave
brokerId = 0 
#表示几点做消息删除动作，默认是凌晨4点 
deleteWhen = 04 
#在磁盘上保留消息的时长，单位是小时 
fileReservedTime = 48 
#有三个值：SYNC_MASTER，ASYNC_MASTER，SLAVE；同步和异步表示Master和Slave之间同步数据的机 制；
brokerRole = ASYNC_MASTER 
#刷盘策略，取值为：ASYNC_FLUSH，SYNC_FLUSH表示同步刷盘和异步刷盘；SYNC_FLUSH消息写入磁盘后 才返回成功状态，ASYNC_FLUSH不需要；
flushDiskType = ASYNC_FLUSH 
# 设置broker节点所在服务器的ip地址 
brokerIP1 = 192.168.5.128 
#剩余磁盘比例 
diskMaxUsedSpaceRatio=99

2.安装broker
docker run -d \ 
--restart=always \ 
--name rmqbroker \ 
--link rmqnamesrv:namesrv \ 
-p 10911:10911 \ 
-p 10909:10909 \ 
--privileged=true \ 
-v /opt/rocketmq/data/broker/logs:/root/logs \ 
-v /opt/rocketmq/data/broker/store:/root/store \ 
-v /opt/rocketmq/conf/broker.conf:/opt/rocketmq-4.4.0/conf/broker.conf \ 
-e "NAMESRV_ADDR=namesrv:9876" \ 
-e "MAX_POSSIBLE_HEAP=200000000" \ 
rocketmqinc/rocketmq \ 
sh mqbroker -c /opt/rocketmq-4.4.0/conf/broker.conf
```

###### 4.broker相关参数说明

| 参数                                                | 说明                                        |
| --------------------------------------------------- | ------------------------------------------- |
| -d                                                  | 以守护进程的方式启动                        |
| –restart=always                                     | docker重启时候镜像自动重启                  |
| -- name rmqbroker                                   | 把容器的名字设置为rmqbroker                 |
| --link rmqnamesrv:namesrv                           | 和rmqnamesrv容器通信                        |
| -p 10911:10911                                      | 把容器的非vip通道端口挂载到宿主机           |
| -p 10909:10909                                      | 把容器的vip通道端口挂载到宿主机             |
| -e “NAMESRV_ADDR=namesrv:9876”                      | 指定namesrv的地址为本机namesrv的ip地址:9876 |
| -e “MAX_POSSIBLE_HEAP=200000000”                    | 指定broker服务的最大堆内存                  |
| rocketmqinc/rocketmq                                | 使用的镜像名称                              |
| sh mqbroker -c /opt/rocketmq-4.4.0/conf/broker.conf | 指定配置文件启动broker节点                  |

###### 5.控制台安装

```
1.拉取镜像 
docker pull pangliang/rocketmq-console-ng

2.安装pangliang/rocketmq-console-ng
docker run -d \ 
--restart=always \
--link rmqnamesrv:192.168.5.128 \
--name rmqadmin \ 
-e "JAVA_OPTS=-Drocketmq.namesrv.addr=192.168.5.128:9876 \ 
-Dcom.rocketmq.sendMessageWithVIPChannel=false" \ 
-p 8080:8080 \ 
pangliang/rocketmq-console-ng （这个10911端口访问会报错）
3.关闭防火墙（或者开放端口）
#关闭防火墙 
systemctl stop firewalld.service 
#禁止开机启动 
systemctl disable firewalld.service
--（下面这个可以成功）
1.拉取镜像 docker pull styletang/rocketmq-console-ng:latest
2.执行安装styletang/rocketmq-console-ng
docker run -d --name rmqadmin -e "JAVA_OPTS=-Drocketmq.namesrv.addr=192.168.5.128:9876 -Dcom.rocketmq.sendMessageWithVIPChannel=false" -p 8080:8080 -t styletang/rocketmq-console-ng
```

##### 十三、zookeeper相关安装

```
1.下载zookeeper-3.4.12.tar.gz

2.创建和解压至安装目录
mkdir /usr/local/zookeeper
tar -zxvf zookeeper-3.4.12.tar.gz -C /usr/local/zookeeper/
3.拷贝样本配置为主配置
cd /usr/local/zookeeper/zookeeper-3.4.12/conf/
cp zoo_sample.cfg zoo.cfg
4.简单修改配置
创建数据存储目录与日志目录
mkdir /usr/local/zookeeper/zookeeper-3.4.12/dataDir
mkdir /usr/local/zookeeper/zookeeper-3.4.12/dataLogDir
修改数据存储和日志目录
vim /usr/local/zookeeper/zookeeper-3.4.12/conf/zoo.cfg
5.配置zookeeper环境变量
vim /etc/profile 在尾部加入或修改以下
ZOOKEEPER_HOME=/usr/local/zookeeper/zookeeper-3.4.12
PATH=$PATH:$ZOOKEEPER_HOME/bin
6.使配置生效
source /etc/profile
7启动测试
/usr/local/zookeeper/zookeeper-3.4.12/bin/zkServer.sh start
8连接
/usr/local/zookeeper/zookeeper-3.4.12/bin/zkCli.sh
9、开机启动
（1）编辑zookeeper.service文件
vim /usr/lib/systemd/system/zookeeper.service 
[Unit]
Description=zookeeper
After=network.target remote-fs.target nss-lookup.target
[Service]
Type=forking
ExecStart=/usr/local/zookeeper/zookeeper-3.4.12/bin/zkServer.sh start
ExecReload=/usr/local/zookeeper/zookeeper-3.4.12/bin/zkServer.sh restart
ExecStop=/usr/local/zookeeper/zookeeper-3.4.12/bin/zkServer.sh stop
[Install]
WantedBy=multi-user.target
（2）生效
systemctl daemon-reload
（3）改变文件权限
chmod 777 /usr/lib/systemd/system/zookeeper.service
（4）systemctl开机启动zookeeper
systemctl enable /usr/lib/systemd/system/zookeeper.service
（5）查看是否开机启动
systemctl is-enabled zookeeper.service
（6）systemctl取消开机启动zookeeper.service
systemctl disable zookeeper.service
```

##### 十四、kafka相关安装

###### 1.kafka的安装

```
1.下载kafka  默认启动端口9092
wget https://mirrors.bfsu.edu.cn/apache/kafka/2.8.0/kafka_2.13-2.8.0.tgz
2.解压
tar -xvf kafka_2.13-2.8.0.tgz
3.创建安装目录
mkdir /usr/local/kafka
mv kafka_2.13-2.8.0 /usr/local/kafka/
4.进入kafka目录中
cd /usr/local/kafka/kafka_2.13-2.8.0/
5. 配置并启动kafka
   执行命令 vi config/server.properties 修改kafka的配置信息
 单节点什么都不用更改。
 集群需要更改zk配置以及advertised.listeners,broker.id,partitions,log.dirs
6.启动,后台启动
 nohup ./bin/kafka-server-start.sh ./config/server.properties &
 ./bin/kafka-server-start.sh ./config/server.properties
 停止kafka服务
  ./bin/kafka-server-stop.sh ./config/server.properties
 7.创建topic ,
 a.创建单分区单副本的topic tes
 ./bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
 删除
 ./bin/kafka-topics.sh --create --zookeeper localhost:2181 --delete --topic test
 最新版
 ./bin/kafka-topics.sh --create --bootstrap-server 192.168.5.128:9092 -- replication-factor 1 --partitions 1 --topic mslogs
 b.查看topic
 ./bin/kafka-topics.sh --list --zookeeper localhost:2181
 查看kafka特定topic的详情，使用--topic与--describe参数
 ./bin/kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic lx_test_topic --describe
 ./kafka-topics.sh --describe --zookeeper localhost:2181 --topic test
 
 8.生产消息kafka-console-producer.sh
 ./bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
（错误使用--broker-list，高版本使用--bootstrap-server）
./bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test
如果主机ip有多个，最好不要用户localhost,用具体ip地址
9.消费消息kafka-console-consumer.sh
 ./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning

./bin/kafka-console-consumer.sh --bootstrap-server 192.168.5.128:9092 --group study1 --topic study --from-beginning


10.通过kafka提供的命令来查看offset消费情况
使用kafka的bin目录下面的kafka-consumer-groups.sh命令可以查看offset消费情况，注意，如果你的offset是存在kafka集群上的，就指定kafka服务器的地址bootstrap-server：
/kafka-consumer-groups.sh --bootstrap-server 172.17.6.10:9092 --describe --group friend-group

11.查看 kafka 消费者组列表：

./bin/kafka-consumer-groups.sh --bootstrap-server 192.168.5.128:9092 --list
./bin/kafka-consumer-groups.sh --bootstrap-server 192.168.5.128:9092 --group study1 --describe
问题测试
1.如何看topic 总消息数据 ，它对应了那些消费组？每个消费组消费到哪里了，
2.topic中分区是如何分配生产的消息，如何在broker中文件查看消费组到那个位置
3.消费者消费一部分消息后不消费？卡住？

./bin/kafka-run-class.sh kafka.tools.DumpLogSegments --files /tmp/kafka-logs/study-0/00000000000000000000.log  --print-data-log
./bin/kafka-run-class.sh kafka.tools.DumpLogSegments --files /tmp/kafka-logs/study-0/00000000000000000000.index  --print-data-log

kafka-run-class.sh kafka.tools.DumpLogSegments --files /tmp/kafka-logs/firsttopic-0/00000000000000000000.log --print-data-log > 00000000000000000000.txt0
11/kafka-run-class.sh kafka.tools.DumpLogSegments --files /data/kafka/kafka-logs/topic-by-majihui-9/00000000000000000000.index --print-data-log > 00000000000000000000index.txt 
 
 
 总结:
 每个partion都会有对应offset，从0开始递增，每个消息都有自己的position,一个topic会有多个分区，消息发送到topic如果不指定具体分区就是轮询每个分区分发消息。每个分区只能被一个消费组的一个消费者实例消费，分区数与消费组中消费者默认也轮询分配，每个消费者消费对应分区都会有记录消费当前消费offset，以及当前分区总共offset。
```

###### 2.kafak web端管理界面Cmak安装配置

```
下载地址：https://github.com/yahoo/CMAK
1.直接下载编译后的zip包，下载完直接解压当前目录中，
unzip cmak-3.0.0.5.zip
2.创建安装的目录
mkdir /usr/local/kafka-manager
mv cmak-3.0.0.5 /usr/local/kafka-manager
2.修改配置文件 
cd /usr/local/kafka-manager/cmak-3.0.0.5/conf
vi application.conf
修改
cmak.zkhosts="192.168.5.128:2181"  //多个逗号隔开
basicAuthentication.enabled=false //可以修改true

3.启动服务
bin/cmak -Dconfig.file=/usr/local/kafka-manager/cmak-3.0.0.5/conf/application.conf -Dhttp.port=9090 -java-home /usr/lib/jvm/java-11-openjdk-11.0.11.0.9-1.el7_9.x86_64

-Dconfig.file ： 指明配置文件路径
-Dhttp.port ： 端口
-java-home ： 指定java路径，也可以不指定。这里由于需要用jdk11，故进行指定启动

4启动成功后直接访问
192.168.5.128:9090
配置 Add cluster
集群名称可以随意
zookeeper地址，按照提示配置
kafka version 超过版本按最高配置
Enable JMX Polling 打勾
后面可选配置选项看情况选择。会需要消耗一定开销
/kafka-manager/mutex报错解决方案
在zookeeper服务端，通过客户端连接创建对应的目录
create /kafka-manager/mutex ""
create /kafka-manager/mutex/locks ""
create /kafka-manager/mutex/leases ""
```

八、redis相关安装

九、kafka相关安装

Cancel相关安装

maven/gradle安装

git安装

zookeeper相关安装

nacos相关安装

elasearch+logstash+kibaner相关安装

nodejs环境

```
#先找Nginx缓存 
rewrite_by_lua_file /usr/local/openresty/nginx/lua/aditem.lua; 
#启用缓存openresty_cache
proxy_cache proxy_cache; 
#针对指定请求缓存 
#proxy_cache_methods GET; 
#设置指定请求会缓存 
proxy_cache_valid 200 304 60s; 
#最少请求1次才会缓存 
proxy_cache_min_uses 1; 
#如果并发请求，只有第1个请求会去服务器获取数据 
#proxy_cache_lock on; 
#唯一的key 
proxy_cache_key $host$uri$is_args$args; 
#动态代理 
proxy_pass http://192.168.5.1:8081;


serverTimezone=Asia/Shanghai

bin/cmak -Dconfig.file=/usr/local/kafka-manager/cmak-3.0.0.5/conf/application.conf -Dhttp.port=8080 -java-home /usr/lib/jvm/java-11-openjdk-11.0.11.0.9-1.el7_9.x86_64
```

