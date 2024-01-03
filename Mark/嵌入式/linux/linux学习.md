# Linux

## shell命令

### 文件目录

```shell
man         # 获得命令帮助信息
help        # 获得shell内置命令的帮助信息
pwd         # 显示当前工作目录的绝对路径
ls          # 列出目录内容
ls -a       # 列出全部文件，包括隐藏文件
ls -l       # 列出全部文件，包括文件属性和权限等
```

### 压缩

#### 打包并压缩

+ c: 创建压缩文件

+ z: 使用 gzip 的方式进行文件压缩

+ j: 使用 bzip2 的方式进行文件压缩

+ v: 压缩过程中显示压缩信息，可以省略不写

+ f: 指定压缩包的名字dd

```
tar -czvf all.tar.gz *.c test.txt app monitor
```

#### 解压缩

- x: 释放压缩文件内容
- z: 使用 gzip 的方式进行文件压缩，压缩包后缀为.tar.gz
- j: 使用 bzip2 的方式进行文件压缩，压缩包后缀为.tar.bz2
- v: 解压缩过程中显示解压缩信息
- f: 指定压缩包的名字

```
tar -xzvf all.tar.gz -C xxx 
```

### 权限（通过ls -l查看）

+ r可读权限

+ w可写权限

+ x可执行权限

#### 修改权限chmod

```cpp
chmod +x xxx    #增加可执行权限
chmod {u/g/o/a}{+/-/=}{r/w/x} 文件或目录 # 变更权限的语法
```

#### 修改归属者chown

```
chown 最终用户 文件    # 修改文件的所有者
chown -R 最终用户:最终用户组 文件或目录   # 递归改变文件所有者和所有组
```

### 使用`&`如`./hello &`使程序运行于后台，使用`kill -9 xxx`结束进程，`ps`查看pid号是多少

### 关机

```shell
shutdown -h now
poweroff
init 0
halt -p
```

### 文件操作

```shell
echo        # 输出内容到控制台
echo -e     # 输出内容到控制台，支持反斜线字符转换\n\t，输出内容中有`\\`为输出\本身cat         # 查看文件内容，从第一行开始显示，适合查看小文件
cat -n      # 查看文件内容同时显示所有的行号，包括空行more        # 分屏查看文件内容。空格向下翻一页，回车向下翻一行，Ctrl+F向下滚一屏，Ctrl+B返回上一屏，=输出当前行号，:f输出文件名和当前行号，q离开more不再显示
less        # 分屏显示文件内容，根据显示需要加载内容，适合查看大型文件。空格向下翻一页，pgup向下翻一页，pgdn向上翻一页，/字串向下搜寻字串n向下找N向上找，?字串向上搜寻字串n向上找N向下找，q离开
head        # 查看文件头部内容，默认显示头10行内容
head -n     # 查看文件头数行内容
tail        # 查看文件尾部内容，默认显示后10行内容
tail -n     # 查看文件尾部数
ll>文件名    # 列表的内容写入(覆盖写入)文件中
ll>>文件名   # 列表的内容追加到文件的末尾行内容
ln -s 原文件或目录 软链接名 # 给原文件创建一个软链接(若软链接和原文件不在同一目录下，则写原文件时不能直接写文件名，应该写路径加文件名)
history     # 查看已经执行过的历史命令
```

### 进程线程

```shell
ps -aux | grep XXX  # 查看系统中所有进程
ps -ef | grep XXX   # 可以查看子父进程之间的关系
ps                  # 查看系统中所有进程
top -d 数           # 查看系统健康状态，每隔数秒更新
top -i              # 使top不显示任何闲置或僵死进程
top -p              # 通过指定进程ID来监控某个进程的状态，P：按CPU使用率排序。M：按内存使用率排序。N：按PID排序
kill -9 进程号       # 通过进程号杀死进程
killall 进程名称     # 通过进程名称杀死进程
netstat -anp | grep 指定的进程号 # 查看指定进程号的网络信息
netstat -nlp | grep 指定的端口号 # 查看指定端口号的占用情况
```

### 搜索查找

```bash
find 搜索范围 -name 指定的文件名 # 根据名称查找搜索范围下指定格式的文件
find 搜索范围 -user 用户名       # 根据用户名查找搜索范围下用户名为指定用户名的文件
find 搜索范围 -size +n   # 根据文件大小查找搜索范围下文件大小大于n的文件(+n:大于,-n:小于,n:等于)
grep                     # 过滤查找性和权限等
which 命令               # 查找命令在哪个目录下
```

### 磁盘分区

```shell
df                    # 列出文件系统的整体磁盘使用量，检查磁盘空间占用情况
df -h                 # 以较易阅读的格式显示磁盘使用量和占用情况
fdisk -l              # 显示所有磁盘的分区列表
mount/umount 挂载/卸载 # 很少用
```

## 查看版本

```shell
g++ --version
gcc --version
```

## nfs服务使用

+ 安装服务`sudo apt install nfs-kernel-server`

+ 配置nfs目录和权限

```
sudo vi /etc/exports
/**/** *(rw,sync,no_root_squash)
```

+ 重启nfs服务`sudo service nfs-kernel-server restart`

+ 显示共享目录`showmount -e`

+ 挂载

`mount -t nfs -o nolock 192.168.31.245:/home/zynq7020/linux/nfs /mnt`

`mount -t nfs -o nolock 192.168.31.44:/home/leon/workspace/nfs /mnt`

+ 取消挂载`umount.nfs /mnt`如果显示busy使用`fuser -m /mnt`查看占用的pid然后`kill -9 xxxx` 

## 修复崩溃后的系统

出现`FAT-fs (mmcblk1p1): Volume was not properly unmounted. Some data may be corrupt. Please run fsck.`时

```shell
先umount /dev/sdb
sudo fsck -y /dev/sdb1 //一般使用-y即可
 -a 自动修复文件系统，不询问任何问题。
    -A 依照/etc/fstab配置文件的内容，检查文件内所列的全部文件系统。
    -N 不执行指令，仅列出实际执行会进行的动作。
    -P 当搭配"-A"参数使用时，则会同时检查所有的文件系统。
    -r 采用互动模式，在执行修复时询问问题，让用户得以确认并决定处理方式。
    -R 当搭配"-A"参数使用时，则会略过/目录的文件系统不予检查。
    -s 依序执行检查作业，而非同时执行。
    -t<文件系统类型> 指定要检查的文件系统类型。
    -T 执行fsck指令时，不显示标题信息。
    -V 显示指令执行过程。
```

## U-Boot

```
petalinux-config -c u-boot
petalinux-build -c u-boot
petalinux-package --boot --fsbl --fpga --u-boot --force
```

## 应用

### 字体

存放在`/usr/share/fonts`

强制重建所有字体缓存文件`sudo fc-cache -fv`

#### qt中文显示乱码问题

复制一个中文字体到`/usr/share/fonts`重建缓存

### MatchBox

```
 export DISPLAY=:0.0
 export DISPLAY=192.168.31.30:0.0(使用MobaXterm显示于其他计算机上)
 matchbox-window-manager
 matchbox-desktopu
```

## 
