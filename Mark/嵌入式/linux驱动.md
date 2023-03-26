# linux驱动

## 驱动

驱动是对`/dev`目录下的设备文件进行操作实现对硬件的操作

+ 驱动的运行方式
  
  + 编译进内核
  
  + 编译成模块（.ko）

+ 驱动的安装卸载
  
  ```
  insmod xxx.ko
  //该方式会解决模块依赖问题，默认会去/lib/modules/<kernel-version>目录中查找模块
  depmod//第一次加载驱动时使用，分析驱动模块
  依赖
  modprobe xxx.ko//推荐 
  
  rmmod xxx.ko//推荐
  modprobe -r xxx.ko
  ```

+ `cat /proc/devices`查看已经被使用掉的设备号

+ 查看设备节点是否存在`ls /dev/newchrled -l`

+ 使用驱动`./程序 ./dev/设备 操作`

## 字符设备驱动（老版）

### 字符设备模板

```c
static struct file_operations test_fops;
/*驱动入口函数*/
static int __init xxx_int(void)
{
    /* 入口函数具体内容 */
    int retvalue = 0;
    /* 注册字符设备驱动 */
     retvalue = register_chrdev(200, "chrtest", &test_fops);
     if(retvalue < 0){
    /* 字符设备注册失败,自行处理 */
     }
    return 0;
}
/*驱动出口函数*/
static void __exit xxx_exit(void)
{
    /*注销字符设备驱动*/
    unregister_chrdev(200,"chrtest");

}
static int chrtest_open(struct inode *inode, struct file *filp)
{
    return 0;
}
static int chrtest_release(struct inode *inode, struct file *filp)
{
    return 0;
}
static ssize_t chrtest_read(struct file *filp,
char __user *buf,
size_t cnt, loff_t *offt)
{
    return 0;
}
static ssize_t chrtest_write(struct file *filp,
char __user *buf,
size_t cnt, loff_t *offt)
{
    return 0;
}
static struct file_operations test_fops ={
    .owenr = THIS_MODULE,
    .open = chrtest_open,
    .read = chrtest_read,
    .write = chrtest_write,
    .release = chrtest_release,
}
module_init(xxx_int);
module_exit(xxx_exit);
MODULE_LICENSE("GPL");//必须
MODULE_AUTHOUR();
```

### 设备号的分配

1. 静态分配设备号
   
   `cat /proc/devices`查看设备号占用

2. 动态分配设备号

```c
//申请设备号
int alloc_chrdev_region(dev_t *dev, unsigned baseminor, unsigned count, const char *name);
//释放设备号
void unregister_chrdev_region(dev_t from, unsigned count);
```

### 打印消息

+ 消息级别

```c
#define KERN_SOH "\001" /* ASCII Start Of Header */
#define KERN_SOH_ASCII '\001'
#define KERN_EMERG KERN_SOH "0" /* system is unusable */
#define KERN_ALERT KERN_SOH "1" /* action must be taken immediately*/
#define KERN_CRIT KERN_SOH "2" /* critical conditions */
#define KERN_ERR KERN_SOH "3" /* error conditions */
#define KERN_WARNING KERN_SOH "4" /* warning conditions */
#define KERN_NOTICE KERN_SOH "5" /* normal but significant condition */
#define KERN_INFO KERN_SOH "6" /* informational */
#define KERN_DEBUG KERN_SOH "7" /* debug-level messages */
```

+ 设置消息级别

`printk(KERN_EMERG "gsmi: Log Shutdown Reason\n"); `

### 地址映射

+ ioremap将物理地址映射到虚拟地址

```c
#define ioremap(cookie,size) __arm_ioremap((cookie), (size), MT_DEVICE)

void __iomem * __arm_ioremap(phys_addr_t phys_addr, size_t size, unsigned int mtype)
 {
     return arch_ioremap_caller(phys_addr, size, mtype, __builtin_return_address(0));
 }
```

+ iounmap卸载地址映射

`void iounmap (volatile void __iomem *addr)`

### 操作I/O内存（linux内核不建议靠操作指针来操作内存）

1. 读操作函数
   
   ```c
   u8 readb(const volatile void __iomem *addr)
   u16 readw(const volatile void __iomem *addr)
   u32 readl(const volatile void __iomem *addr)
   ```

2. 写操作函数
   
   ```c
   void writeb(u8 value, volatile void __iomem *addr)
   void writew(u16 value, volatile void __iomem *addr)
   void writel(u32 value, volatile void __iomem *addr)
   ```

### 驱动测试

+ 加载驱动

```c
depmod //第一次加载驱动的时候需要运行此命令
modprobe led.ko //加载驱动n
```

+ 创建设备节点

`mknod /dev/led c 200 0`

+ 运行程序

```
./ledApp /dev/led 1 //点亮 LED 灯
./ledApp /dev/led 0 //关闭 LED 灯
```

## 字符设备驱动（新版）

### 申请设备号

+ 没有指定设备号

`int alloc_chrdev_region(dev_t *dev, unsigned baseminor, unsigned count, const char *name)`

+ 指定了设备号

`int register_chrdev_region(dev_t from, unsigned count, const char *name)`

+ 注销设备

`void unregister_chrdev_region(dev_t from, unsigned count)`

### 添加字符设备

```c
struct cdev test_cdev;
static struct file_operations test_fops={
    .owner = THIS_MODULE,
};
testcdev.owner = THIS_MODULE;
//初始化cdev结构体变量
cdev_init(&testcdev, &test_fops);
//向linux添加字符设备
cdev_add(&testcdev, devid, 1); 
//删除cdev
cdev_del(&testcdev);
```

#### 设置文件私有数据

```c
struct test_dev{
    dev_t devid;    //设备号
        struct cdev cdev;
        struct class *class;    
        struct device *device;
        int major;
        int minor;
}
struct test_dev testdev;
static int test_open(struct inode *inode, struct file *filp)
 {
     filp->private_data = &testdev; /* 设置私有数据 */
     return 0;
 }
```

### 自动创建设备节点（创建类，创建设备）

```c
 struct class *class; /* 类 */
 struct device *device; /* 设备 */
 dev_t devid; /* 设备号 */

/* 驱动入口函数 */
 static int __init xxx_init(void)
 {
 /* 创建类 */
 class = class_create(THIS_MODULE, "xxx");
 /* 创建设备 */
 device = device_create(class, NULL, devid, NULL, "xxx");
 return 0;
 }

 /* 驱动出口函数 */
 static void __exit led_exit(void)
 {
 /* 删除设备 */
 device_destroy(newchrled.class, newchrled.devid);
 /* 删除类 */
 class_destroy(newchrled.class);
 }

 module_init(led_init);
 module_exit(led_exit);
```

## 设备树

放置在内核的`arch/arm/boot/dts/`目录下

### of操作函数

#### 查找结点的of函数

1. of_find_node_by_name
   
   ```c
   struct device_node *of_find_node_by_name(struct device_node *from,
   const char *name);
   ```
   
   from：开始查找的节点，如果为 NULL 表示从根节点开始查找整个设备树。  
   
   name：要查找的节点名字。  
   
   返回值： 找到的节点，如果为 NULL 表示查找失败。

2. of_find_node_by_type
   
   ```c
   struct device_node *of_find_node_by_type(struct device_node *from, const char   *type)
   ```
   
    from：开始查找的节点，如果为 NULL 表示从根节点开始查找整个设备树。
    type：要查找的节点对应的 type 字符串，也就是 device_type 属性值。 
   
   返回值： 找到的节点，如果为 NULL 表示查找失败。

3. of_find_compatible_node
   
   ```c
   struct device_node *of_find_compatible_node(struct device_node *from,
   const char *type,
   const char *compatible)
   ```

      from：开始查找的节点，如果为 NULL 表示从根节点开始查找整个设备树。  

      type：要查找的节点对应的 type 字符串，也就是 device_type 属性值，可以为 NULL,表示忽略掉 device_type 属性。  

      compatible： 要查找的节点所对应的 compatible 属性列表。 
      返回值： 找到的节点，如果为 NULL 表示查找失败

4. of_find_matching_node_and_match
   
   ```c
   struct device_node *of_find_matching_node_and_match(struct device_node *from,const struct of_device_id *matches,
   const struct of_device_id **match)
   ```
   
   from：开始查找的节点，如果为 NULL 表示从根节点开始查找整个设备树。 
   
   matches： of_device_id 匹配表，也就是在此匹配表里面查找节点。
   
   match： 找到的匹配的 of_device_id。
   
   返回值： 找到的节点，如果为 NULL 表示查找失败

5. of_find_node_by_path
   
   ```c
   nline struct device_node *of_find_node_by_path(const char *path)
   ```
   
   path：带有全路径的节点名，可以使用节点的别名（用 aliens 节点中定义的别名）。  
   
   返回值： 找到的节点，如果为 NULL 表示查找失败

#### 查找父/子节点的of函数

1. of_get_parent
   
   ```c
   struct device_node *of_get_parent(const struct device_node *node)
   ```
   
   node：要查找的父节点的节点。
   
   返回值： 找到的父节点。

2. of_get_next_child
   
   ```c
   struct device_node *of_get_next_child(const struct device_node *node,
   struct device_node *prev)
   ```
   
   node：父节点。 
   
   prev：前一个子节点，也就是从哪一个子节点开始迭代的查找下一个子节点。可以设置为NULL，表示从第一个子节点开始。
   
   返回值： 找到的下一个子节点。

#### 提取属性值的of函数

1. of_find_property
   
   ```c
   property *of_find_property(const struct device_node *np,
   const char *name,
   int *lenp)
   ```
   
   np：设备节点。
   
   name： 属性名字。
   
   lenp：属性值的字节数  
   
   返回值： 找到的属性。

2. of_property_count_elems_of_size
   
   ```c
   int of_property_count_elems_of_size(const struct device_node *np,
   const char *propname,
   int elem_size)
   ```
   
   np：设备节点。
   
   proname： 需要统计元素数量的属性名字。 
   
   elem_size：元素长度。 
   
   返回值： 得到的属性元素数量。

3. of_property_read_u32_index
   
   ```c
   int of_property_read_u32_index(const struct device_node *np,
   const char *propname,
   u32 index,
   u32 *out_value)
   ```
   
   np：设备节点。  
   
   proname： 要读取的属性名字。
   
   index：要读取的值的下标。 
   
   out_value：读取到的值 
   
   返回值： 0 读取成功，负值，读取失败， -EINVAL 表示属性不存在， -ENODATA 表示没有要  读取的数据， -EOVERFLOW 表示属性值列表太小。

4. of_property_read_u8_array 函数  
   
   of_property_read_u16_array 函数  
   
   of_property_read_u32_array 函数  
   
   of_property_read_u64_array 函数
   
   ```c
   int of_property_read_u8_array(const struct device_node *np,
   const char *propname,
   u8 *out_values,
   size_t sz)
   int of_property_read_u16_array(const struct device_node *np,
   const char *propname,
   u16 *out_values,
   size_t sz)
   int of_property_read_u32_array(const struct device_node *np,
   const char *propname,
   u32 *out_values,
   size_t sz)
   int of_property_read_u64_array(const struct device_node *np,
   const char *propname,
   u64 *out_values,
   size_t sz)
   ```
   
   np：设备节点。  
   
   proname： 要读取的属性名字。 
   
   out_value：读取到的数组值，分别为 u8、 u16、 u32 和 u64。
   
   sz： 要读取的数组元素数量。  
   
   返回值： 0，读取成功，负值，读取失败， -EINVAL 表示属性不存在， -ENODATA 表示没有 要读取的数据， -EOVERFLOW 表示属性值列表太小。

5. of_property_read_u8 函数  
   
   of_property_read_u16 函数 
   
   of_property_read_u32 函数  
   
   of_property_read_u64 函数
   
   ```c
   int of_property_read_u8(const struct device_node *np,
   const char *propname,
   u8 *out_value)
   int of_property_read_u16(const struct device_node *np,
   const char *propname,
   u16 *out_value)
   int of_property_read_u32(const struct device_node *np,
   const char *propname,
   u32 *out_value)
   int of_property_read_u64(const struct device_node *np,
   const char *propname,
   u64 *out_value)
   ```
   
   np：设备节点。 
   
   proname： 要读取的属性名字。  
   
   out_value：读取到的数组值。  
   
   返回值： 0，读取成功，负值，读取失败， -EINVAL 表示属性不存在， -ENODATA 表示没有 要读取的数据， -EOVERFLOW 表示属性值列表太小。

6. of_property_read_string
   
   ```c
   int of_property_read_string(struct device_node *np,
   const char *propname,
   const char **out_string)
   ```
   
   np：设备节点。  
   
   proname： 要读取的属性名字。 
   
   out_string：读取到的字符串值。 
   
   返回值： 0，读取成功，负值，读取失败。

7. of_n_addr_cells
   
   ```c
   int of_n_addr_cells(struct device_node *np)
   ```
   
   np：设备节点。 
   
   返回值： 获取到的#address-cells 属性值。

8. of_n_size_cells
   
   ```c
   int of_n_size_cells(struct device_node *np)
   ```
   
   np：设备节点。
   
   返回值： 获取到的#size-cells 属性值。
