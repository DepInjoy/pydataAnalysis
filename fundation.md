
## 自省

### 在变量后面添加？，可以显示对象信息。


```python
b=[1,2,3]
b?
```

Type:        list
String form: [1, 2, 3]
Length:      3
Docstring:  
Built-in mutable sequence.

If no argument is given, the constructor creates a new empty list.
The argument must be an iterable if specified.

### 对象的自省，如果对象会实例定义过文档字符串，也可以显示信息。


```python
def add_number(a, b):
    '''
    Add two numbers together
    Return
    ---------
    the_sum:type of arguments
    '''
    return a+b
```

显示文档字符串


```python
add_number?
```

显示函数源码


```python
add_number??
```

## 魔术命令

timeit测试语句的执行时间


```python
import numpy as np
a=np.random.randn(100, 100)
%timeit np.dot(a, a)
```

    54 µs ± 1.05 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)
    

## 函数

### 对象方法调用

### 属性和方法

### 鸭子类型
不关心对象的类型，只关心对象是否有某种用途或者方法，这通常被称为“鸭子类型”。

验证一个对象是否遵循迭代协议，可迭代的意味着它有__iter__魔术方法，判断函数时iter函数


```python
def isiterable(obj):
    try:
        iter(obj)
        return True
    except TypeError:
        return False
print(isiterable("a string"))
print(isiterable(5))
```

    True
    False
    

应用：实现可以接受惹你类型的序列（list、tuple、ndarray）或迭代器


```python
# 检查对象是否是列表
if not isinstance(x, list) and isiterable(x):
    # 转变为列表
    x=list(x)
```


```python

```

## 数据类型

### 字符串
字符串是不可被改变的，不能修改字符串

#### 字符串格式化

在字符串前面添加r，代表字符就是其自身。


```python
s=r"This\has\no\special\characters\n"
print(s)
```

    This\has\no\special\characters\n
    

{0:.2f}:格式化第一个参数带有两个小数的浮点数
{1:s}: 格式化第二个参数为字符串
{2:d}: 格式化第三个参数为整数


```python
tmp='{0:.2f} {1:s} are worth US ${2:d}.'
tmp=tmp.format(7.092, "RMB", 1)
print(tmp)
```

    7.09 RMB are worth US $1.
    


```python
n = '---{:,d}----'.format(10000000)
n1 = '---{:.2f} {:,d}----'.format(1.234, 100000005)
print(n)
print(n1)
```

    ---10,000,000----
    ---1.23 100,000,005----
    


```python

```


```python

```
