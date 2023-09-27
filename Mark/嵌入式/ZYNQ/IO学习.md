# 操作流程

```mermaid
graph
A("request_mem_region()申请内存")-->B("ioremap()地址映射")
B-->C(读写函数)
C-->D("iounmap()释放地址")
D-->E("release_mem_region()释放内存") 
```
