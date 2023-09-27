## 变量与接口的关联

![](../../../img/2022-07-24-20-05-12-image.png)

## pipeline和dataflow的区别

+ Pipeline是针对函数体内部的处理过程进行约束

+ Dataflow则是针对函数体与函数体之间的处理过程

## HLS2020.1与opencv仿真设置

### 仿真

+ cflags

`-ID:\opencv\build2\install\include -ID:/HLS/Vitis_Libraries/vision/L1/include -std=c++0x`

+ linker flags

`-L D:/opencv/build2/install/x64/mingw/lib -lopencv_imgcodecs460 -lopencv_imgproc460 -lopencv_core460 -lopencv_highgui460 -lopencv_flann460 -lopencv_features2d460`

### 综合

+ cflags

`-ID:/HLS/Vitis_Libraries/vision/L1/include -std=c++0x`

## 数组

+ ap_fix需要使用static才能将数组初始化为0

+ float可以使用`float[] = {0}`初始化 
