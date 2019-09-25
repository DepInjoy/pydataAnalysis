
## 自省

在变量后面添加？，可以显示对象信息。


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

对象的自省，如果对象会实例定义过文档字符串，也可以显示信息。


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

函数返回多个数值


```python
def ops(a, b):
    return a+b, a-b, a*b, a/b
r0, r1, r2, r3 = ops(1,2)
print(r0, r1, r2, r3)
```

    3 -1 2 0.5
    

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
    

应用：实现可以接受任意类型的序列（list、tuple、ndarray）或迭代器


```python
# 检查对象是否是列表
if not isinstance(x, list) and isiterable(x):
    # 转变为列表
    x=list(x)
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
    

## 数据结构

### 元组

元组是一个固定长度，不可改变的Python序列对象。

最简单创建元组的方式，通过逗号将数值分开


```python
tup = 1,2,3,4,5,6
tup
```




    (1, 2, 3, 4, 5, 6)



将元组放在()内定义


```python
nested_tup = (1,2,3,4),(5,6)
print(nested_tup)
```

    ((1, 2, 3, 4), (5, 6))
    

用tuple将任意序列或迭代器转换成元组


```python
tup=tuple([1,2,3,4])
print(tup)

tup=tuple("Hello")
print(tup)
```

    (1, 2, 3, 4)
    ('H', 'e', 'l', 'l', 'o')
    

如果元组中某个对象是可变的，如列表，可以在原位进行修改


```python
tup=tuple(['Hello', [1,2], True])
tup[1].append(3)
print(tup)
```

    ('Hello', [1, 2, 3], True)
    

+可以实现元组的串联


```python
tup=(1,2,3)+(5,6,7,8)
print(tup)
```

    (1, 2, 3, 5, 6, 7, 8)
    

乘号 可以实现几个元组的复制串联


```python
tup=("重要的事情说三遍！！ ")*3
print(tup)
```

    重要的事情说三遍！！ 重要的事情说三遍！！ 重要的事情说三遍！！ 
    

#### 拆分元组


```python
tup=(6,7,8)
a,b,c=tup
print(a,b,c)
```

    6 7 8
    

含有元组的元组的拆分


```python
tup=4,5,(6,8)
a,b,(c, d)=tup
print(a,b,c,d)
```

    4 5 6 8
    

变量拆分用于迭代元组或者列表序列


```python
seq=[(1,2,3),(4,5,6),(7,8,9)]
for a, b, c in seq:
    print('a={0}, b={1}, c={2}'.format(a,b,c))
```

    a=1, b=2, c=3
    a=4, b=5, c=6
    a=7, b=8, c=9
    

从元组开头“摘取”多个元素,*rest用法也用在函数签名中抓取任意长度列表的位置参数


```python
tup = (1,2,3,4,5,6)
a, b,*rest = tup
print(a,b,rest)
```

    1 2 [3, 4, 5, 6]
    

舍弃部分数据


```python
tup = (1,2,3,4,5,6)
a, b, *_= tup
print(a,b)
```

    1 2
    

#### 元组方法

count方法统计某个值出现的次数


```python
tup=(1,2,2,2,3,4,5,2,2)
tup.count(2)
```




    5



index获得特定值的位置,如果找不到特定值会抛出ValueError的异常


```python
tup=(1,2,2,2,3,4,5,2,2)
print(tup.index(2))
try:
    tup.index(6)
except ValueError:
    print("Can not find this value")
```

    1
    Can not find this value
    

## 列表

内容可修改，长度可变。


```python
lst=[6,7,8,None]
print(lst)

tup=(6,7,8)
lst=list(tup)
print(lst)

gen=range(0,10)
print(gen)
# list函数实体化迭代器或生成器
list(gen)
```

    [6, 7, 8, None]
    [6, 7, 8]
    range(0, 10)
    




    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



#### 添加和删除元素

append在列表尾部追加元素


```python
lst=[1,2,3,4,5,6,7,8,9]
lst.append(10)
print(lst)
```

    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    

insert在指定位置插入元素,相较于append消耗的计算量更大。


```python
lst.insert(0,0)
print(lst)
```

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    

pop删除并返回指定位置的元素


```python
lst.pop(0)
print(lst)
```

    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    

remove删除某个指定的值，remove会先寻找第一个值并删除


```python
lst.remove(10)
print(lst)
```

    [1, 2, 3, 4, 5, 6, 7, 8, 9]
    

in判断列表包含某个值，添加not in判断列表不含某个值


```python
print(6 in lst)
print(6 not in lst)
```

    True
    False
    

对于已经定义的列表，extend追加多个元素


```python
lst=[1,2,3,4,5,6,7,8,9]
lst.extend([12,13,16])
print(lst)
```

    [1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 13, 16]
    

+将两个列表串联,加号串联计算量更大，因为需要创建一个列表并复制对象。extend方法更为可取。


```python
lst=[1,2,3,4,5,6,7,8,9]+[12,13,16]
print(lst)
```

    [1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 13, 16]
    

#### 排序

sort可以将列表原地排序


```python
a=[1,7,3,6,9]
a.sort()
print(a)
```

    [1, 3, 6, 7, 9]
    


```python
lst=["saw", "small", "He", "foxes", "six"]
lst.sort(key=len)
print(lst)
```

    ['He', 'saw', 'six', 'small', 'foxes']
    

sorted函数将任意序列元素返回一个新的排好序的列表


```python
l=[7,3,5,4,2,9,1]
l = sorted(l)
print(l)

sorted("horse race")
```

    [1, 2, 3, 4, 5, 7, 9]
    




    [' ', 'a', 'c', 'e', 'e', 'h', 'o', 'r', 'r', 's']



bisect.bisect可以找到插入值后依然保持正确排序的位置，bisect.insert表示向上述位置插入数值。bisect不会检查列表是否已经排序好，未排序的列表使用bisect不会产生错误。


```python
import bisect
import random
random.seed(1)
l=[]  
for i in range(1,10):
  r=random.randint(1,100)
  position=bisect.bisect(l,r)
  bisect.insort(l,r)
  print('%3d %3d'%(r,position),l)
```

     18   0 [18]
     73   1 [18, 73]
     98   2 [18, 73, 98]
      9   0 [9, 18, 73, 98]
     33   2 [9, 18, 33, 73, 98]
     16   1 [9, 16, 18, 33, 73, 98]
     64   4 [9, 16, 18, 33, 64, 73, 98]
     98   7 [9, 16, 18, 33, 64, 73, 98, 98]
     58   4 [9, 16, 18, 33, 58, 64, 73, 98, 98]
    

#### 序列函数

##### enumerate函数

迭代序列，同时跟踪当前项的序列号。


```python
l=["Hello", ",", "nice" "to", "meet", "you"]
for i ,v in enumerate(l):
    print("{0} : {1}".format(i, v))
```

    0 : Hello
    1 : ,
    2 : niceto
    3 : meet
    4 : you
    

##### zip函数

zip可以将多个列表、元组或其它序列成对组合成一个元组列表,它可以处理任意多的序列，元素个数取决于最短的序列


```python
seq1 = ['foo', 'bar', 'baz']
seq2 = ['one', 'two', 'three']
zipped=zip(seq1, seq2)
print(list(zipped))

seq3=[False, True]
print(list(zip(seq1, seq2, seq3)))
```

    [('foo', 'one'), ('bar', 'two'), ('baz', 'three')]
    [('foo', 'one', False), ('bar', 'two', True)]
    

zip的常见用法之，同时迭代多个序列，可以结合enumerate一起使用


```python
for i,(a, b) in enumerate(zip(seq1, seq2)):
    print("{0}: {1} {2}".format(i, a, b))
```

    0: foo one
    1: bar two
    2: baz three
    

zip可以被用来解压序列，也可以当作把行的列表转换为列的列表


```python
pitchers = [('Nolan', 'Ryan'), ('Roger', 'Clemens'),('Schilling', 'Curt')]
first_names, last_names = zip(*pitchers)
print(first_names)
print(last_names)
```

    ('Nolan', 'Roger', 'Schilling')
    ('Ryan', 'Clemens', 'Curt')
    

##### reversed函数

reversed可以从后向前迭代一个序列


```python
list(reversed(range(10)))
```




    [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]



### 字典

也称为哈希映射或关联数组，字典的值可以是任意Python对象，而键通常是不可变的标量类型（整数、浮点型、字符串）或元组（元组中的对象必须是不可变的）。这被称为“可哈希性”。可以用hash函数检测一个对象是否是可哈希的。

#### 创建字典


```python
d={'a' : 'some value', 'b' : [1, 2, 3, 4]}
print(d)
```

    {'a': 'some value', 'b': 'some value'}
    

用序列创建字典


```python
mapping = {}
for key, value in zip(range(0,5), reversed(range(5))):
    mapping[key] = value
print(mapping)
```

    {0: 4, 1: 3, 2: 2, 3: 1, 4: 0}
    


```python
mapping = dict(zip(range(5), reversed(range(5))))
print(mapping)
```

    {0: 4, 1: 3, 2: 2, 3: 1, 4: 0}
    

#### 删除和更新元素


```python
d['b'] = 'some value'
del d['b']
print(d)

d['b'] = 'some value'
d.pop('b')
```

    {'a': 'some value'}
    




    'some value'




```python
d['b'] = 'some value'
d.update({'b' : 'foo', 'c' : 12})
print(d)
```

    {'a': 'some value', 'b': 'foo', 'c': 12}
    

#### 默认值

get默认会返回None,也可以设置默认值返回。


```python
words={'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}
word=words.get('c')
print(word)

word=words.get('c', "NOT FOUND")
print(word)
```

    None
    NOT FOUND
    


```python
from collections import defaultdict

words = ['apple', 'bat', 'bar', 'atom', 'book']
by_letter = defaultdict(list)
for word in words:
    by_letter[word[0]].append(word)
print(by_letter)
```

    defaultdict(<class 'list'>, {'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']})
    


```python
words = ['apple', 'bat', 'bar', 'atom', 'book']
by_letter={}
for word in words:
    letter = word[0]
    by_letter.setdefault(letter, []).append(word)
print(by_letter)
```

    {'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}
    

### 集合

无序的不可重复的元素的集合

#### 集合运算

合并运算,提取两个集合中不重复的元素，采用union方法或者|运算符。


```python
a = {1, 2, 3, 4, 5}
b = {3, 4, 5, 6, 7, 8}

a.union(b)
print(a|b)
```

    {1, 2, 3, 4, 5, 6, 7, 8}
    

交集运算，提取同时包含在两个集合中的元素，可以用intersection或&运算符。


```python
a.intersection(b)
print(a&b)
```

    {3, 4, 5}
    

检测一个集合是否是另外一个集合的子集


```python
{1,2,3}.issubset(a)
```




    True



检测一个集合是否是另外一个集合的父集


```python
a.issuperset({1, 2, 3})
```




    True



判断两个集合是否相等


```python
{1,2,3} == {3,2,1}
```




    True



### 列表、集合和字典推导式

#### filter条件


```python
strings = ['a', 'as', 'bat', 'car', 'dove', 'python']
[x.upper() for x in strings if len(x) > 2]
```




    ['BAT', 'CAR', 'DOVE', 'PYTHON']



#### map函数


```python
set(map(len, strings))
```




    {1, 2, 3, 4, 6}



字典推导


```python
mapping = {val : index for index, val in enumerate(strings)}
mapping
```




    {'a': 0, 'as': 1, 'bat': 2, 'car': 3, 'dove': 4, 'python': 5}




```python
all_data = [['John', 'Emily', 'Michael', 'Mary', 'Steven'],['Maria', 'Juan', 'Jevier', 'Natalia', 'Pilar']]
tmp = [name for names in all_data for name in names if name.count('e') >= 2]
print(tmp)
```

    ['Steven', 'Jevier']
    


```python
import re
states = ['   Alabama ', 'Georgia!', 'Georgia', 'georgia', 'FlOrIda','south   carolina##', 'West virginia?']
def clean_strings(strings):
    result = []
    for value in strings:
        value = value.strip()
        value = re.sub('[!#?]', '', value)
        value = value.title()
        result.append(value)
    return result
print(clean_strings(states))
```

    ['Alabama', 'Georgia', 'Georgia', 'Georgia', 'Florida', 'South   Carolina', 'West Virginia']
    


```python
def remove_punctuation(value):
    return re.sub('[!#?]', '', value)

clean_ops = [str.strip, remove_punctuation, str.title]

def clean_strings(strings, ops):
    result = []
    for value in strings:
        for function in ops:
            value = function(value)
        result.append(value)
    return result
print(clean_strings(states, clean_ops))
```

    ['Alabama', 'Georgia', 'Georgia', 'Georgia', 'Florida', 'South   Carolina', 'West Virginia']
    


```python
def remove_punctuation(value):
    value = value.strip()
    value = re.sub('[!#?]', '', value)
    value = value.title()
    return value

for x in map(remove_punctuation, states):
    print(x)
```

    Alabama
    Georgia
    Georgia
    Georgia
    Florida
    South   Carolina
    West Virginia
    

## 函数

### 匿名函数(lambda函数)


```python
my_add = lambda x, y : x+y
my_add(8,8)
```




    16



### 函数传参

修改不可更改对象


```python
a = 1
print("Before id(a)={0}", id(a))
def fun(a):
    print("Afetr 1 id(a)={0}", id(a))
    a = 2
    print("Afetr 2 id(a)={0}", id(a))
fun(a)
print(a)
```

    Before id(a)={0} 1614241952
    Afetr 1 id(a)={0} 1614241952
    Afetr 2 id(a)={0} 1614241984
    1
    


```python
更改可更改对象
```


```python
b = []
print("Before id(b)={0}", id(b))
def fun1(b):
    b.append(2)
    print("After id(b)={0}", id(b))
fun1(b)
print(b)
```

    Before id(b)={0} 94518152
    After id(b)={0} 94518152
    [2]
    

### 柯里化：部分参数应用

柯里化（currying）是一个有趣的计算机科学术语，它指的是通过“部分参数应用”从现有函数派生出新函数的技术。


```python
def add_numbers(x, y):
    return x + y
```

add_numbers的第二个参数被称为“柯里化”的。


```python
add_five = lambda y: add_numbers(5, y)
add_five(6)
```




    11



内置的functools模块可以用partial函数将此过程简化。


```python
from functools import partial
add_five = partial(add_numbers, 5)
add_five(6)
```




    11



### 迭代器

以一种一致的方式对序列进行迭代（比如列表中的对象或文件中的行）是Python的一个重要特点。这是通过一种叫做迭代器协议（iterator protocol，它是一种使对象可迭代的通用方式）的方式实现的，一个原生的使对象可迭代的方法。

### 生成器构造可迭代对象


```python
# 生成器
def squares(n=10):
    print('Generating squares from 1 to {0}'.format(n))
    for i in range(1, n + 1):
        yield i ** 2

# 调用生成器，此时没有代码被执行
gen = squares()

# 从生成器中请求元素时，开始执行代码
for item in gen:
    print(item, end=" ")
```

    Generating squares from 1 to 10
    1 4 9 16 25 36 49 64 81 100 

### 生成器表达式


```python
gen = (x ** 2 for x in range(11))
for item in gen:
    print(item, end=" ")
```

    0 1 4 9 16 25 36 49 64 81 100 

生成器表达式也可以取代列表推导式，作为函数参数


```python
sum(x**2 for x in range(11))
```




    385




```python
dict((x,x**2) for x in range(11))
```




    {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81, 10: 100}



### itertools模块

标准库itertools模块中有一组用于许多常见数据算法的生成器。

groupby可以接受任何序列和一个函数，根据函数的返回值对序列中的连续元素进行分组。


```python
import itertools
first_letter = lambda x : x[0]
names = ['Alan', 'Adam', 'Wes', 'Will', 'Albert', 'Steven']
for letter, names in itertools.groupby(names, first_letter):
    print(letter, list(names))
```

    A ['Alan', 'Adam']
    W ['Wes', 'Will']
    A ['Albert']
    S ['Steven']
    

创建一个迭代器，返回迭代器中所有长度为K的子序列，返回的子序列中的项按输入迭代器中的顺序排序 (不带重复)。K指定生成排列的元素的长度，如果不指定，则默认为可迭代对象的元素长度。


```python
import itertools
list(itertools.combinations('ABC', 2))
```




    [('A', 'B'), ('A', 'C'), ('B', 'C')]



创建一个迭代器，返回迭代器中所有长度为K的子序列,考虑顺序


```python
import itertools
list(itertools.permutations('ABC', 2))
```




    [('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'C'), ('C', 'A'), ('C', 'B')]



生成输入迭代器的笛卡尔积，结果为元组。


```python
import itertools
l1 = [1, 2]
l2 = [3, 4]
l3 = [5, 6]
list(itertools.product(l1, l2, l3))
```




    [(1, 3, 5),
     (1, 3, 6),
     (1, 4, 5),
     (1, 4, 6),
     (2, 3, 5),
     (2, 3, 6),
     (2, 4, 5),
     (2, 4, 6)]



### 错误和异常处理

#### 异常

查看异常


```python
dir(__builtins__)
```




    ['ArithmeticError',
     'AssertionError',
     'AttributeError',
     'BaseException',
     'BlockingIOError',
     'BrokenPipeError',
     'BufferError',
     'BytesWarning',
     'ChildProcessError',
     'ConnectionAbortedError',
     'ConnectionError',
     'ConnectionRefusedError',
     'ConnectionResetError',
     'DeprecationWarning',
     'EOFError',
     'Ellipsis',
     'EnvironmentError',
     'Exception',
     'False',
     'FileExistsError',
     'FileNotFoundError',
     'FloatingPointError',
     'FutureWarning',
     'GeneratorExit',
     'IOError',
     'ImportError',
     'ImportWarning',
     'IndentationError',
     'IndexError',
     'InterruptedError',
     'IsADirectoryError',
     'KeyError',
     'KeyboardInterrupt',
     'LookupError',
     'MemoryError',
     'ModuleNotFoundError',
     'NameError',
     'None',
     'NotADirectoryError',
     'NotImplemented',
     'NotImplementedError',
     'OSError',
     'OverflowError',
     'PendingDeprecationWarning',
     'PermissionError',
     'ProcessLookupError',
     'RecursionError',
     'ReferenceError',
     'ResourceWarning',
     'RuntimeError',
     'RuntimeWarning',
     'StopAsyncIteration',
     'StopIteration',
     'SyntaxError',
     'SyntaxWarning',
     'SystemError',
     'SystemExit',
     'TabError',
     'TimeoutError',
     'True',
     'TypeError',
     'UnboundLocalError',
     'UnicodeDecodeError',
     'UnicodeEncodeError',
     'UnicodeError',
     'UnicodeTranslateError',
     'UnicodeWarning',
     'UserWarning',
     'ValueError',
     'Warning',
     'WindowsError',
     'ZeroDivisionError',
     '__IPYTHON__',
     '__build_class__',
     '__debug__',
     '__doc__',
     '__import__',
     '__loader__',
     '__name__',
     '__package__',
     '__spec__',
     'abs',
     'all',
     'any',
     'ascii',
     'bin',
     'bool',
     'bytearray',
     'bytes',
     'callable',
     'chr',
     'classmethod',
     'compile',
     'complex',
     'copyright',
     'credits',
     'delattr',
     'dict',
     'dir',
     'display',
     'divmod',
     'enumerate',
     'eval',
     'exec',
     'filter',
     'float',
     'format',
     'frozenset',
     'get_ipython',
     'getattr',
     'globals',
     'hasattr',
     'hash',
     'help',
     'hex',
     'id',
     'input',
     'int',
     'isinstance',
     'issubclass',
     'iter',
     'len',
     'license',
     'list',
     'locals',
     'map',
     'max',
     'memoryview',
     'min',
     'next',
     'object',
     'oct',
     'open',
     'ord',
     'pow',
     'print',
     'property',
     'range',
     'repr',
     'reversed',
     'round',
     'set',
     'setattr',
     'slice',
     'sorted',
     'staticmethod',
     'str',
     'sum',
     'super',
     'tuple',
     'type',
     'vars',
     'zip']



#### 异常捕获和处理

捕获所有的异常


```python
try:
    num1 = int(input('Enter thefirstnumber: '))
    num2 = int(input('Enter thesecondnumber: '))
    print(num1 / num2)
except Exception as err: 
    print('Invalidinput!', err)
```

    Enter thefirstnumber: 3
    Enter thesecondnumber: 0
    Invalidinput! division by zero
    


```python
try:
    num1 = int(input('Enter thefirstnumber: '))
    num2 = int(input('Enter thesecondnumber: '))
    print(num1 / num2)
except(ValueError, ZeroDivisionError): 
    print('Invalidinput!')
```

    Enter thefirstnumber: d3
    Invalidinput!
    


```python
try:
    num1 = int(input('Enter thefirstnumber: '))
    num2 = int(input('Enter thesecondnumber: '))
    print(num1 / num2)
except ValueError as err: 
    print('Please input a digit!', err)
except ZeroDivisionError: 
    print('The secondnumbercannotbezero!')
else:
    print("Everything is OK!")
finally:
    # 无论是否发生异常，finally语句都会执行
    print("Alawys execute")
```

    Enter thefirstnumber: 4
    Enter thesecondnumber: 2
    2.0
    Everything is OK!
    Alawys execute
    

#### 上下文管理器（Context Manager）和with语句

with语句使用于对资源进行访问的场合。
确保使用过程中不管是否发生异常，都会执行必要的“清理”操作，并释放资源。比如文件使用后自动关闭，线程中锁的自动获取和释放。


```python
try:
    with open('./data.txt') as f:
        for line in f:
            print(line, end='')
except IOError as err:
    print("Read failed, ", err)
```

    Read failed,  [Errno 2] No such file or directory: './data.txt'
    


```python
def my_power(x, n = 2):
    s = 1
    while n > 0:
        n -= 1
        s = s * x
    return s
print(my_power(-3))
print(my_power(3,3))
```

    9
    27
    

## 文件操作

rb:二进制方式打开；r+:读写模式；w:只写模式，创建新的文件，如果存在同名文件则删除；r:只读模式


```python
with open("jupyters/segismundo.txt",mode="r", encoding='utf-8') as f:
    for line in f:
        # 去除末尾的空格
        line = line.rstrip()
        print(line)
```

    Sueña el rico en su riqueza,
    que más cuidados le ofrece;
    
    sueña el pobre que padece,
    su miseria y su pobreza;
    
    sueña el que a medrar empieza,
    sueña el que afana y pretende,
    sueña el que agravia y ofende,
    
    y en el mundo, en conclusión,
    todos sueñan lo que son,
    aunque ninguno lo entiende.
    


```python

```
