# 系统定制相关

## 使用petalinux

```
***创建项目
sptl
petalinux-create -t project --template zynqMP -n xxxx
cd xxxx
petalinux-config --get-hw-description <xsa 文件路径>
如果需要重新生成设备树需要编译一下

***编译生成BOOT.BIN（fsbl和uboot）以后基本不再改动
petalinux-build -c bootloader
petalinux-build -c u-boot
petalinux-build -c pmufw //编译平台管理文件
petalinux-build -c arm-trusted-firmware //编译安全固件
petalinux-package --boot --fsbl --u-boot --force

***编译生成内核
petalinux-config -c kernel
petalinux-build -c kernel

***编译生成设备树
petalinux-build -c device-tree -x cleansstate
petalinux-build -c device-tree

***编译rootfs
petalinux-config -c rootfs
petalinux-build -c rootfs


#清理以前编译的结果
petalinux-build -x mrproper -f 
```

## 不使用petalinux

```
设置sdk工作环境(交叉编译时用)
. /opt/petalinux/2019.2/environment-setup-aarch64-xilinx-linux

***编译内核
make distclean
make xilinx_zynqmp_defconfig
make -j8
make dtbs //编译设备树
```

# 系统挂载

```
mount -t nfs -o nolock 192.168.31.44:/home/leon/workspace/nfs /mnt
cd /lib/modules/4.19.0
modprobe axidma.ko
cd /mnt/xilinx_axidma/outputs
cd /mnt/Zeta_Test/Zeta_Test/outputs
mount -t nfs -o nolock 192.168.31.245:/home/zynq7020/work/nfs /mnt 

fuser -m /mnt
kill -9 xxxx
umount.nfs /mnt
```

# 环境安装

## 交叉编译opencv

```shell
#opencv目录下
mkdir build
cd build
```

新建aarch64-linux-gnu.cmake

```cmake
 SET(CMAKE_SYSTEM_NAME Linux) 
 SET(CMAKE_SYSTEM_PROCESSOR aarch64) 
 SET(CMAKE_C_COMPILER   "aarch64-linux-gnu-gcc") 
 SET(CMAKE_CXX_COMPILER "aarch64-linux-gnu-g++") 
 SET(CMAKE_FIND_ROOT_PATH /usr/aarch64-linux-gnu/bin)  
```

```shell
cmake .. -DCMAKE_TOOLCHAIN_FILE=./aarch64-linux-gnu.cmake -DCMAKE_INSTALL_PREFIX=xxx -DCMAKE_BUILD_TYPE=Release -DBUILD_JPEG=ON  
make -j6
make install -j6
```

# 定制ubuntu根文件系统

+ 拷贝qemu

```
移植aarch64，拷贝qemu-arm-static 
sudo cp /usr/bin/qemu-aarch64-static ./usr/bin
```

+ 拷贝dns配置文件

```
sudo cp /etc/resolv.conf ./etc/resolv.conf //拷贝主机的 resolv.conf 文件到 ubuntu-base 根文件系统中
```

+ 换源

```
ubuntu16.04
./etc/apt/sources.list
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-updates main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-updates main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-backports main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-backports main restricted universe multiverse

deb http://ports.ubuntu.com/ubuntu-ports/ xenial-security main restricted universe multiverse
# deb-src http://ports.ubuntu.com/ubuntu-ports/ xenial-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-proposed main restricted universe multiverse
# # deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-proposed main restricted universe multiverse
```

+ 安装常用软件

```
apt update
apt install sudo
apt install vim
apt install kmod net-tools ethtool ifupdown language-pack-en-base rsyslog htop iputils-ping
```

+ 设置root密码

```
passwd root
```

+ 创建新用户

```
useradd -s '/bin/bash' -m -G adm,sudo leon
passwd leon
```

+ 设置主机名和IP

```
echo "zeta" > /etc/hostname
echo "127.0.0.1 localhost" >> /etc/hosts
echo "127.0.0.1 zeta" >> /etc/hosts
```

+ 配置串口

```
ln -s /lib/systemd/system/getty@.service /etc/systemd/system/getty.target.wants/getty@ttyPS0.service
```

+ 设置dhcp

```
打开/etc/network/interfaces 配置文件，注释掉“ source-directory /etc/network/interfaces.d”
添加
```

+ 设置nfs挂载

```
setenv bootargs = "earlycon console=ttyPS0,115200 clk_ignore_unused root=/dev/mmcblk1p2 rw rootwait"
setenv bootargs = "console=ttyPS0,115200 root=/dev/nfs nfsroot=192.168.31.44:/home/leon/workspace/nfs/rootfs/ubuntu16,nfsvers=3 proto=tcp rw ip=dhcp"
saveenv //保存环境变量
```

## ubuntu系统升级

```
sudo apt update
sudo apt upgrade
sudo apt dist-upgrade
```
