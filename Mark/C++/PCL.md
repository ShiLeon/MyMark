# PCL

## 点云转换

```cpp
//创建
pcl::PointCloud<pcl::PointXYZ> cloud;//点云对象
pcl::PointCloud<pcl::PointXYZ>::Ptr cloudPtr
        (new pcl::PointCloud<pcl::PointXYZ>);//点云指针对象　　

//转换
cloud = * cloudPtr;
cloudPtr = cloud.makeShared();
```
