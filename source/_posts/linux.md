---
title: linux操作笔记
date: 2018-11-19 13:49:40
categories: linux
tags: [linux]
---

# linux常见命令

**linux客户端工具：强力推荐[mobaXterm](https://mobaxterm.mobatek.net/download-home-edition.html)**

ctrl+c强行终止当前程序

ctrl+d退出终端

ctrl+s暂停当前程序，按任意键继续


---

touch test.txt  创建文件

touch love_{1..10}_linux.txt  创建多个文件

---

sudo adduser lilei 创建一个名为lilei的用户

su -l lilei 切换用户

ctrl+d  退出当前用户

sudo deluser lilei --remove-home  删除用户

sudo chown shiyanlou iphone6 将iphone6的文件所有者更换为shiyanlourf 

ls -l  使用较长格式列出文件

ls -lh  使用较长格式列出文件

ls -A  显示所有文件（包括隐藏文件）

ll iphone6  查看iphone6文件信息

chmod go-rwx iphone6  g、o、u、分别表示group、others、user,+、-分别表示增加和去掉相应的权限（修改文件的读写权限）

---

pwd 查看当前所在路径

cd ..进入上级目录

cd ~进入home目录

mkdir mkdir  创建新目录

mkdir -p father/son/grandson  同时创建父级目录

cp test father/son/grandson  将test文件复制到相应文件夹中

cp -r father father family 将father文件夹复制到family文件夹中

rm test 删除test文件

rm -f test  强制删除test文件

rm -r family  删除目录

mv file1 Documents  将file1文件移动到Documents目录下

mv file1 myfile  将file1重命名为myfile

---

cat file  查看file文件内容

cat -n file 查看file文件内容（带行号）

nl file  带行号的文件内容打印

file file  查看file文件类型

---

nohup xeyes & 召唤眼睛

sudo apt-get update;sudo apt-get install cmatrix  安装Ubuntu

cmatrix 召唤数字雨

sudo apt-get update;sudo apt-get install libaa-bin

aafire  召唤火焰

./file  执行file文件

---

PATH=$PATH:/home/shiyanlou/mybin  添加自定义路径到环境变量（当前shell有效）

echo "PATH=$PATH:home/shiyanlou/mybin" >> .zshrc 添加自定义路径到环境变量（全局）

echo $path  查看变量path的值

path=${path%/home/shiyanlou/mybin}  删除/home/shiyanlou/mybin

temp=${temp//world/123}  将world字符串替换为123

unset temp  删除变量

set 列出所有变量

---

whereis who  搜索who相关的文件（简单快速）

locate /etc/sh  查找/tec下所有以sh开头的文件（快而全）

locate /user/share/\*.jpg  查找/user/share/下所有jpg文件

sudo find /etc/ -name interfaces  在/etc/目录下搜索名字叫做interfaces的文件或者目录

---

zip -r -q -o shiyanlou.zip /home/shiyanlou  将实验楼打包成一个文件，-r（目录）表示递归但因包含子目录的全部内容，-q表示安静模式，不向屏幕输出信息，-o表示输出文件

du -h shiyanlou.zip  查看打包后的文件大小

unzip shiyanlou.zip  解压到当前目录

unzip -q shiyanlou.zip -d ziptest  解压到ziptest文件夹，若无则自动新建文件夹

unzip -l shiyanlou.zip  不解压，直接查看压缩文件内容

unzip -O GBK  shiyanlou.zip  指定编码类型

---

sudo apt-get update;sudo apt-get install rar unrar  安装rar和unrar工具

tar -cf shiyanlou.tar ~  创建一个tar包，-c：创建一个tar包文件，-f指定创建的文件名

tar -zcvf something.tar something 打包

tar -zxvf something.tar  解包

tar -xf shiyanlou.tar -C tardir  解包一个文件（-x参数）到指定路径的已存在目录（-c参数）

tar -tf shiyanlou.tar  只查看不解包文件-t参数

tar -czf shiyanlou.tar.gz  ~创建一个tar.gz压缩文件

tar -cJf shiyanlou.tar.xz  ~创建一个tar.xz压缩文件

tar -cjf shiyanlou.tar.bz2  ~创建一个tar.bz2压缩文件

---

ps aux|grep java 查看java进程

netstat -anp|grep 80 查看80端口的进程

nohup ./start.sh & 启动脚本后台运行并将日志输出到nohup

---

docker ps 列出所有容器信息

docker restart 重启某个镜像

docker exec -ti 进入某个镜像

./zkCli.sh -server 192.168.0.1:42201 启动服务

ls /dubbo/com.xiaoyu.service/providers/dubbo 查看服务提供者

---

**数据库备份脚本：**


```
#!/bin/sh

time=$(date '+%Y%m%d%H%M%S')
mysql_user=sys
mysql_password=123456

mysqldump -u${mysql_user} -p${mysql_password} maoyao | gzip > /home/mysqlbackup/maoyao${time}.sql.gz

cd /home/mysqlbackup

rm -rffind . -name'*.sql.gz' -mtime10 #删除10天之前的备份文件

#end
```

---
**设置定时器：**

crontab -e 

00 0 * * * /home/mysqlbackup/dbbackup.sh #每天的0:00执行一下sh脚本

---
**nginx：**

重启：./nginx -s reload





