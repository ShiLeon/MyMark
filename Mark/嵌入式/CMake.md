# CMake

## 方法一

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

## 方法二

+ `Ctrl + Shift + p` 输入` cq`进行快速生成CMake项目
