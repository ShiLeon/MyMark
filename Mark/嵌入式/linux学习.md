# Linux

## 锁

+ 自旋锁

+ 读写锁

+ RCU（read-copy-update）

+ 信号量（semaphore）

+ 互斥量（mutex）

## petalinux定制系统

+ 导入hdf,编译生成fsbl,u-boot,合成BOOT.BIN

+ 编译设备树

+ 编译内核

## 压缩知识

### 打包并压缩

+ c: 创建压缩文件

+ z: 使用 gzip 的方式进行文件压缩

+ j: 使用 bzip2 的方式进行文件压缩

+ v: 压缩过程中显示压缩信息，可以省略不写

+ f: 指定压缩包的名字

```
tar -czvf all.tar.gz *.c test.txt app monitor
```

### 解压缩

- x: 释放压缩文件内容
- z: 使用 gzip 的方式进行文件压缩，压缩包后缀为.tar.gz
- j: 使用 bzip2 的方式进行文件压缩，压缩包后缀为.tar.bz2
- v: 解压缩过程中显示解压缩信息
- f: 指定压缩包的名字

```
tar -xzvf all.tar.gz -C xxx
```
