## platform驱动框架

## misc设备驱动框架

+ 注册misc设备

```c
int misc_register(struct miscdevice * misc)
```

相当于

```c
1 alloc_chrdev_region(); /* 申请设备号 */
2 cdev_init(); /* 初始化 cdev */
3 cdev_add(); /* 添加 cdev */
4 class_create(); /* 创建类 */
5 device_create(); /* 创建设备 */
```

+ 卸载misc设备

```c
int misc_deregister(struct miscdevice *misc)
```

相当于

```c
1 cdev_del(); /* 删除 cdev */
2 unregister_chrdev_region(); /* 注销设备号 */
3 device_destroy(); /* 删除设备 */
4 class_destroy(); /* 删除类 */
```
