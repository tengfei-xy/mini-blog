# GNU对C的拓展

## 柔性数组成员（零长数组、变长数组）

动态创建结构体

```c
struct s {int n;log d[0];}
int m = 数值;
struct s *p = malloc(sizeof (struct s) + sizeof (long [m]))
```



## case关键词

case支持范围匹配

```c
case 'a'...'z'
break;
```



## typeof 关键词

typeof(x)能够获取x的数据类型，经常用在宏定义

## 可变参数宏

```c
#define pr_debug(fmt,arg...) \
printk(fmt,##arg)
```

可变参数arg被忽略或空时，printk函数的##操作将预处理器去掉前面的逗号。

## 元素编号

```c
unsigned char data[MAX] = 
{
  [0] = 10,
  [10 ... 50],
  [55]=55,
};
```



## 当前函数名

`__PRETTY_FUNCTION__`和`__FUNCTION__`表示当前的函数名

## 特殊属性声明

详见《一个64位操作系统的设计与实现》P24

