## 基本概念

- **Bootloader**：提供了加载应用程序和执行固件更新的基础能力。
- **IAP (In-Application Programming)**：是一种**技术或过程**，指的是在设备运行状态下（通常是在Bootloader的引导下，或者有时甚至是应用程序自己调用特定代码）对自身的程序存储器进行擦写，以达到更新固件的目的。
- **OTA (Over-The-Air)**：是一种**固件交付方式**，特指通过**无线通信**（Wi-Fi, Bluetooth, 蜂窝网络等）将新的固件包发送到设备。设备接收到OTA包后，通常会利用其IAP能力来完成实际的烧录更新。

## 报文格式

### bin格式

### Motorola格式

![image-20251014102809822](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014102809844.png)

![image-20251014102224810](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014102231991.png)

### Intel格式

![image-20251014102253007](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014102253094.png)

![image-20251014102402860](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014102402985.png)

## 通用需求

<img src="https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014102624681.png" alt="image-20251014102624629" style="zoom: 33%;" />![image-20251014102648764](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014102648855.png)

![image-20251014103012544](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014103012611.png)

## 刷写流程

![image-20251014103230289](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014103230330.png)

![image-20251014103311875](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014103311960.png)

![image-20251014103818108](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014103818196.png)

![image-20251014104103253](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014104103328.png)

### 使用到的服务

![image-20251014104337483](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014104337537.png)

![image-20251014104504460](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014104504519.png)

+ 一类

![image-20251014104924854](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014104924908.png)

+ 二类

![image-20251014105019020](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014105019071.png)

### 刷写流程

![image-20251014105222370](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014105222461.png)

![image-20251014105408513](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014105408606.png)

![image-20251014105958451](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014105958527.png)

![image-20251014110453800](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014110453874.png)

### 一二类区别

![image-20251014110615653](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/20251014110615729.png)