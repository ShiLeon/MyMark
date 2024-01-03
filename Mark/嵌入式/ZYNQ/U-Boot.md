## U-Boot命令

### 帮助help或者？

### 信息查询

+ bdinfo

+ printenv

+ version

#### 环境变量

+ setenv

+ saveenv

```
设置bootargs
setenv bootargs = "earlycon console=ttyPS0,115200 clk_ignore_unused root=/dev/mmcblk1p2 rw rootwait"

setenv bootargs = "console=ttyPS0,115200 root=/dev/nfs nfsroot=192.168.31.44:/home/leon/workspace/nfs/rootfs1,nfsvers=3 proto=tcp rw ip=dhcp"
```
