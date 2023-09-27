# IDE使用

## VisualStudio使用

### VisualStudio 配置库

+ 添加附加包含目录
  
  **项目** --- **属性** --- **属性页** --- **C/C++** --- **常规**--- **附加包含目录**

+ 添加附加库目录
  
  **项目** --- **属性** --- **属性页** --- **链接器** --- **常规** --- **附加库目录**

+ 添加附加依赖项
  
  **项目** --- **属性** --- **属性页** --- **链接器** --- **输入** --- **附加依赖项**

### MSVC中文乱码

- 原因：qt creator文件使用的是UTF-8编码，但MSVC生成的可执行文件的编码是windows本地的字符集如GB2312

- 解决方法：
  
  > 1.使用QStringLiteral()宏封装字符串
  > 
  > 2.强制MSVC使用UTF-编码
  > 
  > ```
  > #pragma execution_character_set("utf-8")
  > ```
  > 
  > 3.全局方式
  > 
  > + 全局使用utf-8编码
  > 
  > + **项目** --- **属性** --- **属性页** --- **C/C++** --- **所有选项**--- **附加选项**`/utf-8`

## CMake

### 方法一

+ 创建工程 .c .h CMakeLists.txt 
  
  ```
  cmake_minimum_required(VERSION 3.0)
  project(CMakeTest)
  add_executable(CMakeTest CMakeTest.c)
  ```

+ 创建build文件夹,运行cmake
  
  ```
  cd build
  cmake ..
  ```

+ 运行make

### 方法二

+ `Ctrl + Shift + p` 输入` cq`进行快速生成CMake项目
