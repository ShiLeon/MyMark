## ini格式

```
# filename: config.ini
[user]
name=admin
password=123456
is_admin=true

[mysql]
host=10.10.10.10
port=3306
db=apitest
user=root
password=123456
```

## 初始化

```python
from configparser import ConfigParser
conf = ConfigParser()  # 需要实例化一个ConfigParser对象
conf.read('config.ini')  # 需要添加上config.ini的路径，不需要open打开，直接给文件路径就读取，也可以指定encoding='utf-8'
```

## 使用方法

```python
conf.sections(): #获取所有的section名，结果[‘user’, ‘mysql’, ‘log’]
conf['mysql']['port']: #获取section端port变量的值，字符串格式
conf['mysql'].get('port'): #同上，字符串格式
conf.get('mysql', 'port'): #同上，字符串格式
conf['mysql'].getint('port'): #获取对应变量的整型值
conf['mysql'].getfloat('port'): #获取对应变量的浮点型值
conf['user'].getboolean('is_admin'): #获取对应变量的布尔值，支持配置为yes/no, on/off, true/false 和 1/0，都可以转化为Python中的True/False
conf.has_section(section)：#检查是否有该section
conf.options(section)：#输出section中所有的变量名
conf.has_option(section, option)：#检查指定section下是否有该变量值
```

遍历

```python
for key, value in conf['mysql'].items():
    print(key, value)

```



