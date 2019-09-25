

```python
import pandas as pd
import numpy as np
```

## 数据结构

### Series

Series是一种类似于一维数组的对象，它由一组数据以及一组相关的数据标签（即索引）组成。Series的字符串表现形式为：索引在左边，值在右边。如果没有手动创建索引，pandas会自动创建一个0到N-1（其中，N为数据的长度）的整数型索引。你可以通过Series 的values和index属性获取其数组表示形式和索引对象：


```python
obj=pd.Series([3,5,7,9])
print(obj)
```

    0    3
    1    5
    2    7
    3    9
    dtype: int64
    

获取数组对象


```python
obj.values
```




    array([3, 5, 7, 9], dtype=int64)



获取索引对象，索引对象是不可变的，索引的不可变性使得Index对象在多个数据结构之间可以安全地共享。


```python
obj.index
```




    RangeIndex(start=0, stop=4, step=1)



可以通过赋值方式修改索引


```python
obj.index=['Bob', 'Steve', 'Jeff', 'Ryan']
obj
```




    Bob      3
    Steve    5
    Jeff     7
    Ryan     9
    dtype: int64



创建Series时，手动添加索引


```python
obj2=pd.Series([3,5,7,9], index=['d', 'b', 'a', 'c'])
print(obj2)
```

    d    3
    b    5
    a    7
    c    9
    dtype: int64
    

可以通过索引选取series中的单个或一组值


```python
print(obj2['a'])
print(obj2[['a', 'c']])
```

    7
    a    7
    c    9
    dtype: int64
    

使用Numpy函数或运算，依然会保留索引


```python
obj2[obj2>5]
```




    a    7
    c    9
    dtype: int64




```python
obj2*2
```




    d     6
    b    10
    a    14
    c    18
    dtype: int64




```python
np.exp(obj2)
```




    d      20.085537
    b     148.413159
    a    1096.633158
    c    8103.083928
    dtype: float64



也可以将Series看做定长的有序字典，当作索引值到数值之间的一个映射。


```python
print(
'b' in obj2,
'e' in obj2)
```

    True False
    

也可以通过python字典来创建Series,其中索引就是原字典的键。


```python
sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
obj3 = pd.Series(sdata)
obj3
```




    Ohio      35000
    Texas     71000
    Oregon    16000
    Utah       5000
    dtype: int64



states中的California找不到对应的数值，因此被匹配为NAN，其他的三个被匹配到并赋予相应的数值。


```python
states = ['California', 'Ohio', 'Oregon', 'Texas']
obj4 = pd.Series(sdata, index=states)
obj4
```




    California        NaN
    Ohio          35000.0
    Oregon        16000.0
    Texas         71000.0
    dtype: float64



isnull和notnull来检测缺失值


```python
pd.isnull(obj4)
obj4.isnull()
```




    California     True
    Ohio          False
    Oregon        False
    Texas         False
    dtype: bool




```python
pd.notnull(obj4)
```




    California    False
    Ohio           True
    Oregon         True
    Texas          True
    dtype: bool



根据运算的索引标签自动对齐数据


```python
obj3+obj4
```




    California         NaN
    Ohio           70000.0
    Oregon         32000.0
    Texas         142000.0
    Utah               NaN
    dtype: float64



Series对象本身也有name属性


```python
obj4.name = "population"
obj4
```




    state
    California        NaN
    Ohio          35000.0
    Oregon        16000.0
    Texas         71000.0
    Name: population, dtype: float64



Series索引也有name属性


```python
obj4.index.name = "state"
obj4
```




    state
    California        NaN
    Ohio          35000.0
    Oregon        16000.0
    Texas         71000.0
    Name: population, dtype: float64



### DataFrame

DataFrame是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔值等）。DataFrame既有行索引也有列索引，它可以被看做由Series组成的字典（共用同一个索引）。DataFrame中的数据是以一个或多个二维块存放的（而不是列表、字典或别的一维数据结构）。


```python
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002, 2003],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]
       }
frame = pd.DataFrame(data)
frame
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>state</th>
      <th>year</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Ohio</td>
      <td>2000</td>
      <td>1.5</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Ohio</td>
      <td>2001</td>
      <td>1.7</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Ohio</td>
      <td>2002</td>
      <td>3.6</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Nevada</td>
      <td>2001</td>
      <td>2.4</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Nevada</td>
      <td>2002</td>
      <td>2.9</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Nevada</td>
      <td>2003</td>
      <td>3.2</td>
    </tr>
  </tbody>
</table>
</div>



对于数据量比较大的，head方法会选取前5行


```python
frame.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>state</th>
      <th>year</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Ohio</td>
      <td>2000</td>
      <td>1.5</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Ohio</td>
      <td>2001</td>
      <td>1.7</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Ohio</td>
      <td>2002</td>
      <td>3.6</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Nevada</td>
      <td>2001</td>
      <td>2.4</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Nevada</td>
      <td>2002</td>
      <td>2.9</td>
    </tr>
  </tbody>
</table>
</div>



选取指定列，按照指定列进行排序，如果传入的列不存在，则结果显示缺失值


```python
frame2 = pd.DataFrame(data, columns=["year", "state", "pop", "debt"],
            index=["one", "two", "three", "four", "five", "six"])
frame2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>two</td>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>three</td>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>six</td>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



可以通过字典标记方式获取一个Series


```python
frame2["year"]
```




    one      2000
    two      2001
    three    2002
    four     2001
    five     2002
    six      2003
    Name: year, dtype: int64




```python
frame2.year
```




    one      2000
    two      2001
    three    2002
    four     2001
    five     2002
    six      2003
    Name: year, dtype: int64



行也可以通过位置或者名称去获取，比如loc


```python
frame2.loc["three"]
```




    year     2002
    state    Ohio
    pop       3.6
    debt      NaN
    Name: three, dtype: object




```python
frame2.loc["three"].year
```




    2002



列也可以通过赋值方式来修改


```python
frame2["debt"]=16.5
frame2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>16.5</td>
    </tr>
    <tr>
      <td>two</td>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>16.5</td>
    </tr>
    <tr>
      <td>three</td>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>16.5</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>16.5</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>16.5</td>
    </tr>
    <tr>
      <td>six</td>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
      <td>16.5</td>
    </tr>
  </tbody>
</table>
</div>




```python
frame2["debt"]=np.arange(6.)
frame2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>two</td>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>three</td>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>4.0</td>
    </tr>
    <tr>
      <td>six</td>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
      <td>5.0</td>
    </tr>
  </tbody>
</table>
</div>



将列表或数组赋值给某个列时，其长度必须跟DataFrame的长度相匹配。如果赋值的是一个Series，就会精确匹配DataFrame的索引，所有的空位都将被填上缺失值


```python
val = pd.Series([-1.2, -1.5, -1.7], index=['two', 'four', 'five'])
frame2['debt'] = val
frame2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>two</td>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>-1.2</td>
    </tr>
    <tr>
      <td>three</td>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>-1.5</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>-1.7</td>
    </tr>
    <tr>
      <td>six</td>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



添加一列


```python
frame2['eastern'] = frame2.state == 'Ohio'
frame2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
      <th>eastern</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>NaN</td>
      <td>True</td>
    </tr>
    <tr>
      <td>two</td>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>-1.2</td>
      <td>True</td>
    </tr>
    <tr>
      <td>three</td>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>NaN</td>
      <td>True</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>-1.5</td>
      <td>False</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>-1.7</td>
      <td>False</td>
    </tr>
    <tr>
      <td>six</td>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
      <td>NaN</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



删除列


```python
if 'eastern' in frame2:
    del frame2['eastern']
frame2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>two</td>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>-1.2</td>
    </tr>
    <tr>
      <td>three</td>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>-1.5</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>-1.7</td>
    </tr>
    <tr>
      <td>six</td>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



如果嵌套字典传给DataFrame，pandas就会被解释为：外层字典的键作为列，内层键则作为行索引.


```python
pop = { 'Nevada': {2001: 2.4, 2002: 2.9},
        'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}
frame3 = pd.DataFrame(pop)
frame3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Nevada</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2001</td>
      <td>2.4</td>
      <td>1.7</td>
    </tr>
    <tr>
      <td>2002</td>
      <td>2.9</td>
      <td>3.6</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>NaN</td>
      <td>1.5</td>
    </tr>
  </tbody>
</table>
</div>



转置运算


```python
frame3.T
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>2001</th>
      <th>2002</th>
      <th>2000</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Nevada</td>
      <td>2.4</td>
      <td>2.9</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>Ohio</td>
      <td>1.7</td>
      <td>3.6</td>
      <td>1.5</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.DataFrame(pop, index=[2001, 2002, 2003])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Nevada</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2001</td>
      <td>2.4</td>
      <td>1.7</td>
    </tr>
    <tr>
      <td>2002</td>
      <td>2.9</td>
      <td>3.6</td>
    </tr>
    <tr>
      <td>2003</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



可以设置DataFrame的index和columns的男属性，并显示


```python
frame3.index.name = 'year'
frame3.columns.name = 'state'
frame3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>state</th>
      <th>Nevada</th>
      <th>Ohio</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2001</td>
      <td>2.4</td>
      <td>1.7</td>
    </tr>
    <tr>
      <td>2002</td>
      <td>2.9</td>
      <td>3.6</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>NaN</td>
      <td>1.5</td>
    </tr>
  </tbody>
</table>
</div>



获取DataFrame中的数据，如果DataFrame各列的数据类型不同，则值数组的dtype就会选用能兼容所有列的数据类型


```python
frame3.values
```




    array([[2.4, 1.7],
           [2.9, 3.6],
           [nan, 1.5]])




```python
frame2.values
```




    array([[2000, 'Ohio', 1.5, nan],
           [2001, 'Ohio', 1.7, -1.2],
           [2002, 'Ohio', 3.6, nan],
           [2001, 'Nevada', 2.4, -1.5],
           [2002, 'Nevada', 2.9, -1.7],
           [2003, 'Nevada', 3.2, nan]], dtype=object)



### 索引对象

pandas的索引对象负责管理轴标签和其他元数据（比如轴名称等），Index对象是不可变的，用户不能对其进行修改，不可变可以使Index对象在多个数据结构之间安全共享


```python
labels = pd.Index(np.arange(3))
obj2 = pd.Series([1.5, -2.5, 0], index=labels)
obj2
```




    0    1.5
    1   -2.5
    2    0.0
    dtype: float64



Index类似一个固定大小的集合,Index也可以包含重复的标签，重复的标签会显示相同的值。


```python
frame3.columns
```




    Index(['Nevada', 'Ohio'], dtype='object', name='state')




```python
'Ohio' in frame3.columns
```




    True




```python
2003 in frame3.index
```




    False



## 基本功能

### 重新索引

reindex函数创建一个新对象，它的数据符合新的索引，某个索引值当前不存在，就引入缺失值。


```python
obj = pd.Series([4.5, 7.2, -5.3, 3.6], index=['d', 'b', 'a', 'c'])
obj2 = obj.reindex(['a', 'b', 'c', 'd', 'e'])
obj2
```




    a   -5.3
    b    7.2
    c    3.6
    d    4.5
    e    NaN
    dtype: float64



对于时间序列这样的有序数据，重新索引时可能需要做一些插值处理,method选项可以辅助实现，如使用ffill可以实现前向值填充。


```python
obj3 = pd.Series(['blue', 'purple', 'yellow'], index=[0, 2, 4])
obj3=obj3.reindex(range(6), method='ffill')
obj3
```




    0      blue
    1      blue
    2    purple
    3    purple
    4    yellow
    5    yellow
    dtype: object



借助DataFrame，reindex可以修改（行）索引和列,只传递一个序列时，会重新索引结果的行.


```python
frame = pd.DataFrame(np.arange(9).reshape((3, 3)),
            index=['a', 'c', 'd'],
            columns=['Ohio', 'Texas', 'California'])
frame2 = frame.reindex(['a',"b", 'c', 'd'])
frame2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Ohio</th>
      <th>Texas</th>
      <th>California</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>a</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>b</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>c</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <td>d</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>8.0</td>
    </tr>
  </tbody>
</table>
</div>



列可以用columns关键字重新索引


```python
frame.reindex(columns=['Texas', 'Utah', 'California'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Texas</th>
      <th>Utah</th>
      <th>California</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>a</td>
      <td>1</td>
      <td>NaN</td>
      <td>2</td>
    </tr>
    <tr>
      <td>c</td>
      <td>4</td>
      <td>NaN</td>
      <td>5</td>
    </tr>
    <tr>
      <td>d</td>
      <td>7</td>
      <td>NaN</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



### 丢弃指定轴上的项


```python
obj = pd.Series(np.arange(5.), index=['a', 'b', 'c', 'd', 'e'])
new_obj=obj.drop('a')
new_obj
```




    b    1.0
    c    2.0
    d    3.0
    e    4.0
    dtype: float64




```python
obj.drop(['b', 'c'])
```




    a    0.0
    d    3.0
    e    4.0
    dtype: float64



对于DataFrame，可以删除任意轴上的索引值.


```python
data = pd.DataFrame(np.arange(16).reshape((4, 4)),
            index=['Ohio', 'Colorado', 'Utah', 'New York'],
            columns=['one', 'two', 'three', 'four'])
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
      <th>three</th>
      <th>four</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ohio</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>Colorado</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <td>Utah</td>
      <td>8</td>
      <td>9</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>New York</td>
      <td>12</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>



用标签序列调用drop会从行标签（axis 0）删除值.


```python
data.drop(['Ohio'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
      <th>three</th>
      <th>four</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Colorado</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <td>Utah</td>
      <td>8</td>
      <td>9</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>New York</td>
      <td>12</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>



通过传递axis=1或axis='columns'可以删除列的值.


```python
data.drop('two', axis=1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>three</th>
      <th>four</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ohio</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>Colorado</td>
      <td>4</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <td>Utah</td>
      <td>8</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>New York</td>
      <td>12</td>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.drop(['two', 'three'], axis='columns')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>four</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ohio</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <td>Colorado</td>
      <td>4</td>
      <td>7</td>
    </tr>
    <tr>
      <td>Utah</td>
      <td>8</td>
      <td>11</td>
    </tr>
    <tr>
      <td>New York</td>
      <td>12</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>



修改Series或DataFrame的大小或形状，不会返回新的对象，此种方式会直接销毁所有被删除的数据。


```python
data.drop(['two'], axis='columns', inplace=True)
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>three</th>
      <th>four</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ohio</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>Colorado</td>
      <td>4</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <td>Utah</td>
      <td>8</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>New York</td>
      <td>12</td>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>



### 索引、选取和过滤


```python
obj = pd.Series(np.arange(4.), index=['a', 'b', 'c', 'd'])
```


```python
obj['b']
```




    1.0




```python
obj[1]
```




    1.0




```python
obj[1:4]
obj[[ 'b', 'c', 'd']]
```




    b    1.0
    c    2.0
    d    3.0
    dtype: float64




```python
obj[[1,3]]
obj[['b', 'd']]
```




    b    1.0
    d    3.0
    dtype: float64




```python
obj[obj>1]
```




    c    2.0
    d    3.0
    dtype: float64




```python
obj["b":"d"] = 5
obj
```




    a    0.0
    b    5.0
    c    5.0
    d    5.0
    dtype: float64




```python
data = pd.DataFrame(np.arange(16).reshape((4, 4)),
             index=['Ohio', 'Colorado', 'Utah', 'New York'],
            columns=['one', 'two', 'three', 'four'])
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
      <th>three</th>
      <th>four</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ohio</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>Colorado</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <td>Utah</td>
      <td>8</td>
      <td>9</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>New York</td>
      <td>12</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>




```python
data[["three", "one"]]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>three</th>
      <th>one</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ohio</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <td>Colorado</td>
      <td>6</td>
      <td>4</td>
    </tr>
    <tr>
      <td>Utah</td>
      <td>10</td>
      <td>8</td>
    </tr>
    <tr>
      <td>New York</td>
      <td>14</td>
      <td>12</td>
    </tr>
  </tbody>
</table>
</div>




```python
data[:2]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
      <th>three</th>
      <th>four</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ohio</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>Colorado</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>




```python
data[data["three"]>6]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
      <th>three</th>
      <th>four</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Utah</td>
      <td>8</td>
      <td>9</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>New York</td>
      <td>12</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>



布尔型DataFrame索引


```python
data<5
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
      <th>three</th>
      <th>four</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ohio</td>
      <td>True</td>
      <td>True</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <td>Colorado</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <td>Utah</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <td>New York</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
data[data<5]=0
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
      <th>three</th>
      <th>four</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ohio</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>Colorado</td>
      <td>0</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <td>Utah</td>
      <td>8</td>
      <td>9</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>New York</td>
      <td>12</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>



### 用loc和iloc进行选取

使用轴标签（loc）或整数索引（iloc），从DataFrame选择行和列的子集。


```python
data.loc['Colorado', ['two', 'three']]
```




    two      5
    three    6
    Name: Colorado, dtype: int32




```python
data.iloc[1,[1,2]]
```




    two      5
    three    6
    Name: Colorado, dtype: int32




```python
data.iloc[1]
```




    one      0
    two      5
    three    6
    four     7
    Name: Colorado, dtype: int32




```python
data.iloc[:,:3][data.three > 5]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
      <th>three</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Colorado</td>
      <td>0</td>
      <td>5</td>
      <td>6</td>
    </tr>
    <tr>
      <td>Utah</td>
      <td>8</td>
      <td>9</td>
      <td>10</td>
    </tr>
    <tr>
      <td>New York</td>
      <td>12</td>
      <td>13</td>
      <td>14</td>
    </tr>
  </tbody>
</table>
</div>



### 算术运算和数据对齐

pandas最重要的一个功能是，它可以对不同索引的对象进行算术运算。在将对象相加时，如果存在不同的索引对，则结果的索引就是该索引对的并集,这就像数据库在索引标签上进行自动外连接。自动的数据对齐操作在不重叠的索引处引入了NAN值，缺失值会在算术运算过程中传播。


```python
s1 = pd.Series([7.3, -2.5, 3.4, 1.5], index=['a', 'c', 'd', 'e'])
s2 = pd.Series([-2.1, 3.6, -1.5, 4, 3.1], index=['a', 'c', 'e', 'f', 'g'])
s1+s2
```




    a    5.2
    c    1.1
    d    NaN
    e    0.0
    f    NaN
    g    NaN
    dtype: float64




```python
df1 = pd.DataFrame(np.arange(9.).reshape((3, 3)), columns=list('bcd'),
            index=['Ohio', 'Texas', 'Colorado'])
df2 = pd.DataFrame(np.arange(12.).reshape((4, 3)), columns=list('bde'),
            index=['Utah', 'Ohio', 'Texas', 'Oregon'])
df1+df2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Colorado</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>Ohio</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>6.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>Oregon</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>Texas</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>Utah</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### 在算术方法中填充值


```python
df1 = pd.DataFrame(np.arange(12.).reshape((3, 4)),columns=list('abcd'))
df1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>8.0</td>
      <td>9.0</td>
      <td>10.0</td>
      <td>11.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2 = pd.DataFrame(np.arange(20.).reshape((4, 5)),columns=list('abcde'))
df2.loc[1, 'b'] = np.nan
df2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>7.0</td>
      <td>8.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>10.0</td>
      <td>11.0</td>
      <td>12.0</td>
      <td>13.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>15.0</td>
      <td>16.0</td>
      <td>17.0</td>
      <td>18.0</td>
      <td>19.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1+df2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>1</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>13.0</td>
      <td>15.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2</td>
      <td>18.0</td>
      <td>20.0</td>
      <td>22.0</td>
      <td>24.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



使用df1的add方法，传入df2以及一个fill_value参数


```python
df1.add(df2, fill_value=0)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>13.0</td>
      <td>15.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>18.0</td>
      <td>20.0</td>
      <td>22.0</td>
      <td>24.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>15.0</td>
      <td>16.0</td>
      <td>17.0</td>
      <td>18.0</td>
      <td>19.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
1/df1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>inf</td>
      <td>1.000000</td>
      <td>0.500000</td>
      <td>0.333333</td>
    </tr>
    <tr>
      <td>1</td>
      <td>0.250</td>
      <td>0.200000</td>
      <td>0.166667</td>
      <td>0.142857</td>
    </tr>
    <tr>
      <td>2</td>
      <td>0.125</td>
      <td>0.111111</td>
      <td>0.100000</td>
      <td>0.090909</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1.rdiv(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>8.0</td>
      <td>9.0</td>
      <td>10.0</td>
      <td>11.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1.radd(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>9.0</td>
      <td>10.0</td>
      <td>11.0</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>



重新索引并指定填充值


```python
df1.reindex(columns=df2.columns, fill_value=0)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>8.0</td>
      <td>9.0</td>
      <td>10.0</td>
      <td>11.0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### DataFrame和Series之间的运算

从arr减去arr[0]，每一行都会执行这个操作，这叫广播（broadcasting）


```python
arr = np.arange(12.).reshape((3, 4))
arr - arr[0]
```




    array([[0., 0., 0., 0.],
           [4., 4., 4., 4.],
           [8., 8., 8., 8.]])




```python
frame = pd.DataFrame(np.arange(12.).reshape((4, 3)),
            columns=list('bde'),
            index=['Utah', 'Ohio', 'Texas', 'Oregon'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Utah</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>Ohio</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <td>Texas</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <td>Oregon</td>
      <td>9.0</td>
      <td>10.0</td>
      <td>11.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
frame - frame.iloc[0]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Utah</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>Ohio</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>Texas</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <td>Oregon</td>
      <td>9.0</td>
      <td>9.0</td>
      <td>9.0</td>
    </tr>
  </tbody>
</table>
</div>



如果某个索引值在DataFrame的列或Series的索引中找不到，则参与运算的两个对象就会被重新索引以形成并集


```python
series2 = pd.Series(range(3), index=['b', 'e', 'f'])
```


```python
frame + series2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
      <th>f</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Utah</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>Ohio</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>6.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>Texas</td>
      <td>6.0</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>Oregon</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



必须使用算术运算方法才可以在行上广播，DataFrame的行索引（axis='index' or axis=0）并进行广播。


```python
series3 = frame['d']
frame.sub(series3, axis='index')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Utah</td>
      <td>-1.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>Ohio</td>
      <td>-1.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>Texas</td>
      <td>-1.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>Oregon</td>
      <td>-1.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>



### 函数应用和映射


```python
frame = pd.DataFrame(np.random.randn(4, 3), columns=list('bde'),index=['Utah', 'Ohio', 'Texas', 'Oregon'])
frame
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Utah</td>
      <td>-1.328524</td>
      <td>-0.180159</td>
      <td>-0.687256</td>
    </tr>
    <tr>
      <td>Ohio</td>
      <td>-0.182982</td>
      <td>-0.053937</td>
      <td>0.370892</td>
    </tr>
    <tr>
      <td>Texas</td>
      <td>-0.195018</td>
      <td>0.635394</td>
      <td>0.876238</td>
    </tr>
    <tr>
      <td>Oregon</td>
      <td>0.313373</td>
      <td>-1.317549</td>
      <td>0.743823</td>
    </tr>
  </tbody>
</table>
</div>




```python
np.abs(frame)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Utah</td>
      <td>1.328524</td>
      <td>0.180159</td>
      <td>0.687256</td>
    </tr>
    <tr>
      <td>Ohio</td>
      <td>0.182982</td>
      <td>0.053937</td>
      <td>0.370892</td>
    </tr>
    <tr>
      <td>Texas</td>
      <td>0.195018</td>
      <td>0.635394</td>
      <td>0.876238</td>
    </tr>
    <tr>
      <td>Oregon</td>
      <td>0.313373</td>
      <td>1.317549</td>
      <td>0.743823</td>
    </tr>
  </tbody>
</table>
</div>




```python
f = lambda x: x.max() - x.min()
frame.apply(f)
```




    b    1.641897
    d    1.952943
    e    1.563494
    dtype: float64



传递axis='columns'到apply,传入的函数在每行上执行。


```python
frame.apply(f, axis='columns')
```




    Utah      1.148365
    Ohio      0.553873
    Texas     1.071256
    Oregon    2.061372
    dtype: float64



传递到apply的函数不是必须返回一个标量，还可以返回由多个值组成的Series。


```python
def f(x):
    return pd.Series([x.min(), x.max()], index=['min', 'max'])
frame.apply(f)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>min</td>
      <td>-1.328524</td>
      <td>-1.317549</td>
      <td>-0.687256</td>
    </tr>
    <tr>
      <td>max</td>
      <td>0.313373</td>
      <td>0.635394</td>
      <td>0.876238</td>
    </tr>
  </tbody>
</table>
</div>



元素级的Python函数也是可以用的。假如你想得到frame中各个浮点值的格式化字符串，使用applymap即可


```python
format = lambda x: '%.2f' % x
frame.applymap(format)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Utah</td>
      <td>-1.33</td>
      <td>-0.18</td>
      <td>-0.69</td>
    </tr>
    <tr>
      <td>Ohio</td>
      <td>-0.18</td>
      <td>-0.05</td>
      <td>0.37</td>
    </tr>
    <tr>
      <td>Texas</td>
      <td>-0.20</td>
      <td>0.64</td>
      <td>0.88</td>
    </tr>
    <tr>
      <td>Oregon</td>
      <td>0.31</td>
      <td>-1.32</td>
      <td>0.74</td>
    </tr>
  </tbody>
</table>
</div>



Series有一个用于应用元素级函数的map方法


```python
frame['e'].map(format)
```




    Utah      -0.69
    Ohio       0.37
    Texas      0.88
    Oregon     0.74
    Name: e, dtype: object



### 排序和排名

对行或列索引进行按字典顺序


```python
obj = pd.Series(range(4), index=['d', 'a', 'b', 'c'])
obj.sort_index()
```




    a    1
    b    2
    c    3
    d    0
    dtype: int64




```python
frame = pd.DataFrame(np.arange(8).reshape((2, 4)),
                     index=['three', 'one'],
                     columns=['d', 'a', 'b', 'c'])
frame.sort_index()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>d</th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <td>three</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
frame.sort_index(axis=1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>three</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <td>one</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



降序排序


```python
frame.sort_index(axis=1, ascending=False)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>d</th>
      <th>c</th>
      <th>b</th>
      <th>a</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>three</td>
      <td>0</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <td>one</td>
      <td>4</td>
      <td>7</td>
      <td>6</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



值排序，任何缺失值默认都会放到Series的尾部


```python
obj = pd.Series([4, np.nan, 7, np.nan, -3, 2])
obj.sort_values()
```




    4   -3.0
    5    2.0
    0    4.0
    2    7.0
    1    NaN
    3    NaN
    dtype: float64



当排序一个DataFrame时，你可能希望根据一个或多个列中的值进行排序。将一个或多个列的名字传递给sort_values的by选项即可


```python
frame = pd.DataFrame({'b': [4, 7, -3, 2], 'a': [0, 1, 0, 1]})
frame.sort_values(by="b")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>a</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2</td>
      <td>-3</td>
      <td>0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <td>0</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>7</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
frame.sort_values(by=["a", 'b'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>a</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2</td>
      <td>-3</td>
      <td>0</td>
    </tr>
    <tr>
      <td>0</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <td>1</td>
      <td>7</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



默认情况下，rank是通过“为各组分配一个平均排名”的方式破坏平级关系.


```python
        # 排名   6,  1, 7, 4, 3, 2, 5
obj = pd.Series([7, -5, 7, 4, 2, 0, 4])
obj.rank()
```




    0    6.5
    1    1.0
    2    6.5
    3    4.5
    4    3.0
    5    2.0
    6    4.5
    dtype: float64



根据值在原数据中出现的顺序给出排名，对于相同的值按照出现的顺序排名


```python
obj.rank(method='first')
```




    0    6.0
    1    1.0
    2    7.0
    3    4.0
    4    3.0
    5    2.0
    6    5.0
    dtype: float64



降序排序，max:对于相同的值都取大的排名


```python
obj.rank(ascending=False, method='max')
```




    0    2.0
    1    7.0
    2    2.0
    3    4.0
    4    5.0
    5    6.0
    6    4.0
    dtype: float64




```python
frame = pd.DataFrame({'b': [4.3, 7, -3, 2], 'a': [0, 1, 0, 1], 'c': [-2, 5, 8, -2.5]})
frame.rank(axis='columns')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>a</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>



### 带有重复标签的轴索引

判断索引是否唯一


```python
obj = pd.Series(range(5), index=['a', 'a', 'b', 'b', 'c'])
obj.index.is_unique
```




    False




```python
obj["b"]
```




    b    2
    b    3
    dtype: int64




```python
df = pd.DataFrame(np.random.randn(4, 3), index=['a', 'a', 'b', 'b'])
df.loc["b"]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>b</td>
      <td>-0.326046</td>
      <td>0.887334</td>
      <td>1.943780</td>
    </tr>
    <tr>
      <td>b</td>
      <td>-0.312123</td>
      <td>-1.071505</td>
      <td>-2.228214</td>
    </tr>
  </tbody>
</table>
</div>



## 汇总和计算描述统计

NA值会自动被排除，除非整个切片（行或列）都是NA，通过skipna选项可以禁用该功能。

按列求和


```python
df = pd.DataFrame([[1.4, np.nan], [7.1, -4.5],[np.nan, np.nan], [0.75, -1.3]],
        index=['a', 'b', 'c', 'd'],columns=['one', 'two'])
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>a</td>
      <td>1.40</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>b</td>
      <td>7.10</td>
      <td>-4.5</td>
    </tr>
    <tr>
      <td>c</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>d</td>
      <td>0.75</td>
      <td>-1.3</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.sum()
```




    one    9.25
    two   -5.80
    dtype: float64



传入axis='columns'或axis=1将会按行进行求和运算


```python
df.sum(axis=1)
```




    a    1.40
    b    2.60
    c    0.00
    d   -0.55
    dtype: float64




```python
df.mean(axis='columns')
```




    a    1.400
    b    1.300
    c      NaN
    d   -0.275
    dtype: float64




```python
df.mean(axis='columns', skipna=False)
```




    a      NaN
    b    1.300
    c      NaN
    d   -0.275
    dtype: float64



idxmin和idxmax间接统计最小值或最大值的索引。


```python
df.idxmax()
```




    one    b
    two    d
    dtype: object




```python
df.idxmin()
```




    one    d
    two    b
    dtype: object



累计


```python
df.cumsum()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>a</td>
      <td>1.40</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>b</td>
      <td>8.50</td>
      <td>-4.5</td>
    </tr>
    <tr>
      <td>c</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>d</td>
      <td>9.25</td>
      <td>-5.8</td>
    </tr>
  </tbody>
</table>
</div>



多种汇总统计结果


```python
df.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>count</td>
      <td>3.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <td>mean</td>
      <td>3.083333</td>
      <td>-2.900000</td>
    </tr>
    <tr>
      <td>std</td>
      <td>3.493685</td>
      <td>2.262742</td>
    </tr>
    <tr>
      <td>min</td>
      <td>0.750000</td>
      <td>-4.500000</td>
    </tr>
    <tr>
      <td>25%</td>
      <td>1.075000</td>
      <td>-3.700000</td>
    </tr>
    <tr>
      <td>50%</td>
      <td>1.400000</td>
      <td>-2.900000</td>
    </tr>
    <tr>
      <td>75%</td>
      <td>4.250000</td>
      <td>-2.100000</td>
    </tr>
    <tr>
      <td>max</td>
      <td>7.100000</td>
      <td>-1.300000</td>
    </tr>
  </tbody>
</table>
</div>



非数值型汇总统计


```python
obj = pd.Series(['a', 'a', 'b', 'c'] * 4)
obj.describe()
```




    count     16
    unique     3
    top        a
    freq       8
    dtype: object



### 相关系数与协方差


```python
all_data = {ticker: web.get_data_yahoo(ticker)
            for ticker in ['AAPL', 'IBM', 'MSFT', 'GOOG']}

price = pd.DataFrame({ticker: data['Adj Close']
                     for ticker, data in all_data.items()})
volume = pd.DataFrame({ticker: data['Volume']
                      for ticker, data in all_data.items()})
```

查看百分数变化


```python
returns = price.pct_change()
returns.tail()
```

Series的corr方法用于计算两个Series中重叠的、非NA的、按索引对齐的值的相关系数。


```python
returns['MSFT'].corr(returns['IBM'])
returns.MSFT.corr(returns.IBM)
```

cov计算协方差

返回完整的相关系数


```python
returns.corr()
```

返回完整的协方差矩阵


```python
returns.cov()
```

利用DataFrame的corrwith方法，你可以计算其列或行跟另一个Series或DataFrame之间的相关系数。


```python
returns.corrwith(returns.IBM)
```

传入一个DataFrame则会计算按列名配对的相关系数


```python
returns.corrwith(volume)
```

### 唯一值、值计数以及成员资格


```python
obj = pd.Series(['c', 'a', 'd', 'a', 'a', 'b', 'b', 'c', 'c'])
obj.unique()
```




    array(['c', 'a', 'd', 'b'], dtype=object)



计算各值出现的频次


```python
pd.value_counts(obj.values, sort=True)
```




    a    3
    c    3
    b    2
    d    1
    dtype: int64



isin用于判断矢量化集合的成员资格，可用于过滤Series中或DataFrame列中数据的子集


```python
mask = obj.isin(['b', 'c'])
mask
```




    0     True
    1    False
    2    False
    3    False
    4    False
    5     True
    6     True
    7     True
    8     True
    dtype: bool




```python
obj[mask]
```




    0    c
    5    b
    6    b
    7    c
    8    c
    dtype: object




```python
to_match = pd.Series(['c', 'a', 'b', 'b', 'c', 'a'])
unique_vals = pd.Series(['c', 'b', 'a'])
pd.Index(unique_vals).get_indexer(to_match)
```




    array([0, 2, 1, 1, 0, 2], dtype=int64)



计算DataFrame数值出现频次


```python
data = pd.DataFrame({'Qu1': [1, 3, 4, 3, 4],
                     'Qu2': [2, 3, 1, 2, 3],
                     'Qu3': [1, 5, 2, 4, 4]})
data.apply(pd.value_counts).fillna(0)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Qu1</th>
      <th>Qu2</th>
      <th>Qu3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
