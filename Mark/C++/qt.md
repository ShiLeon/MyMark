# QT

## QPixmap、QBitmap、QImage和 QPicture区别

- `QPixmap`专门为图像在屏幕上的显示做了优化；（绘制性能最好）
- `QBitmap`是 QPixmap 的一个子类，它的色深限定为 1，你可以使用 QPixmap 的 isQBitmap() 函数来确定这个 QPixmap 是不是一个 QBitmap；
- `QImage`专门为图像的像素级访问做了优化；（像素级访问，可在子线程绘制）
- `QPicture`可以记录和重现 QPainter 的各条命令；（QPicture只能用于作绘图设备时保存图片状态，要显示必须重写绘图事件，将状态图片加载到QPicture，然后在窗口显示）

### 转换(建议先QImage加载，转成QPixmap绘制)

```cpp
QImage image = pixmap.toImage()；
QPixmap pixmap = QPixmap::fromImage(image);
```

## Visual Studio + Qt槽的使用

+ 方法一：拖拽自定义槽
  
  <img title="" src="../img/9374305026978839b7020058e9a5f5e75d0f7908.png" alt="" width="357">
  
  ![](../img/170fda00d8a868cd1e11485c30f878b84b89e52c.png)

+ 方法二：代码方式
  
  ```
  connect(ui.btnReportEliminate,&QPushButton::clicked,this,&ReportSoft::connectDB);
  ```

## 对象树的用处

最主要作用为new开辟到堆中的对象不用手动delete，只需要对象继承父对象，当父对象析构时即可自动delete。

## 多线程

### 打印当前线程

```cpp
qDebug() << QThread::currentThreadId() << QThread::currentThread();
```

## 动态库和静态库

### 动态库

+ 显示调用
  
  显示调用不需要修改.pro文件，也不需要加头文件，主要是通过 QLibrary 方法
  
  ```
  1. #include xxx
  
  2. QLibrary mylib(“/xxx/xxx/xxx.dll”);
  
  3. 然后 if(mylib.load()) 通过load()函数去加载动态库 (加载后，库将保留在内存中
  ，直到应用程序终止。我们可以尝试使用 unload( ) 来卸载库)
  
  4. 声明函数指针，通过resolve得到库中函数地址
    typedef void (*Fun)();
    Fun method = (Fun)mylib.resolve(“method-name”);
    if(!method) //判断是否获取成功
  
  5. 最后再调用即可
  ```

+ 隐式调用（需要仔细再查查）
  
  放在环境变量路径
  
  程序路径

+ 不同编译器产生的后缀区别
  
  > MinGW xxx.a 
  
  > MSVC xxx.dll

### qmake拷贝文件文件夹到输出目录

```qmake
#配置中增加文件拷贝选项
CONFIG += file_copies

#定义需要增加的文件
# $$PWD表示工程源代码所在目录
# $$OUT_PWD表示发布软件坐在目录
# .files规定文件名或文件夹名
# .path规定发布目标地址
# addfile和adddir为自定义变量名称

addfile.files = $$PWD/filename
addfile.path = $$OUT_PWD/

adddir.files = $$PWD/dirname
adddir.path = $$OUT_PWD/

# COPIES增加需要拷贝的文件或文件夹
COPIES += addfile
COPIES += adddir
```

## .pro文件的说明

+ INCLUDEPATH的作用：
  
   1.该目录下的所有.H文件都被认为是工程中.CPP文件的依赖项，因此，任一.H文件的变更都将引起所有.CPP的重新编译；
  
  2.在#include的时候，不需要填写完整路径，会去每个目录中检查。

+ DEPENDPATH的作用：
  
  该选项的意义等同于INCLUDEPATH的第一个含义。

+ LIBS的作用
  该选项的意义是将指定依赖库的路径，以及依赖库的名称。（动态库/静态库）

+ PRE_TARGETDEPS 的作用
  
  该选项的意义是针对静态库来说，每次都会重新连接静态库，如果没有指定该选项， 程序不会重新连接新的静态库。因此，对于静态库来说，必须指定该选项，避免静态库过时。

+ 操作系统选项标志：
  
  Pro工程文件中只能识别Windows、Unix、Mac三种操作系统的大类：
  
  Windows：win32
  
  Unix：unix
  
  Mac：macx
