## 数据库配置

+ 初始化data数据目录

```
//cd到bin目录下执行下面的指令，../data指的是postgresql的数据库目录，我们执行初始化
initdb.exe -D ../data
```

+ 初始化PostgreSQL服务

```
//执行下面指令注册服务！（cd到PostgreSQL中的bin目录） 
pg_ctl.exe register -N "服务名" -D PostgreSQL中的data（数据库）目录的绝对路径 
pg_ctl.exe register -N "pgsql" -D D:\PostgreSQL\12\data
```

+ cmd连接数据库

```
 //cd到PostgreSQL中的bin目录
  psql.exe -U postgres -p 5432
```

## C++使用linpgxx连接PostgreSQL

+ 
