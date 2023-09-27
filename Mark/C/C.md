<img title="" src="file:///D:/GitRepository/MyMark/img/2023-09-05-21-41-49-image.png" alt="" width="134">

# 字符串操作

+ 复制字符串

```cpp
strcpy(char *dest, const char *src);
```

+ 字符串比较

```cpp
strcmp(const char *s1, const char *s2);
```

+ 字符串长度

```cpp
size_t strlen(const char *s);
```

+ 字符串拼接

```cpp
strcat(char *dest, const char *src);
```

+ 字符串查找

```cpp
char *strchr(const char *s, int c);
char *strrchr(const char *s, int c);//最后一次出现的位置
char *strstr(const char *s1, const char *s2);
```

+ 字符串分割

```cpp
char *strtok(char *s, const char *delim);
char str[] = "hello,world,!";
char *p = strtok(str, ",");
while(p != NULL) {
    printf("%s\n", p);
    p = strtok(NULL, ",");
}
```
