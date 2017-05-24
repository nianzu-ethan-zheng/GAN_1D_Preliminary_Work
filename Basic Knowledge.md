## There are three relevant ops for implementing Theano's dimshuffle in TensorFlow:
```
tf.transpose() is used to permute the dimensions of a tensor. If the pattern specified in the arguments to dimshuffle is a permutation of the input tensor's dimensions (i.e. there is no 'x' or missing dimension) you can use tf.transpose() to implement dimshuffle().
tf.expand_dims() is used to add one or more size-1 dimensions to a tensor. This handles the case where 'x' is specified as part of the dimshuffle() pattern, but does not reorder the existing dimensions.
tf.squeeze() is used to remove one or more size-1 dimensions from a tensor. This handles the case where a dimension is omitted from a dimshuffle() pattern, but it does not reorder the existing dimensions.
Assuming that the input is a vector, your example (dimshuffle(0, 'x')) can be expressed using tf.expand_dims() only:

input = tf.placeholder(tf.float32, [None])  # Defines an arbitrary-sized vector.
result = tf.expand_dims(input, 1)

print result.get_shape()  # ==> TensorShape([Dimension(None), Dimension(1)])
Taking a more complicated example, dimshuffle(1, 'x', 0) applied to a matrix would be:

input = tf.placeholder(tf.float32, [128, 32])  # Defines a matrix.
output = tf.expand_dims(tf.transpose(input, [1, 0]), 1)

print output.get_shape()
# ==> TensorShape([Dimension(32), Dimension(1), Dimension(128)])
```


## pre_processing
Goto : [pre_processing](http://scikit-learn.org/stable/modules/preprocessing.html#preprocessing-scaler)
[And here](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html#examples-using-sklearn-preprocessing-minmaxscaler)<br>
\--------------------------------------------------------------------------------------------------------

# 取整的几种方式
下面介绍几种常用的取整方法，包括向下取整、四舍五入、向上取整
### 向下取整
向下取整很简单，直接使用int()函数即可

### 四舍五入
内置函数round（）

### 向上取整
调用Python的math库，即 ceil 方法，专门用于向上取整
<br>
\--------------------------------------------------------------------------------------------------------

### Four Level

array(None,) -->vector(None,1) or (1,None)  --> maxtrix (None,None)--> tensor(ALL)<br>
\--------------------------------------------------------------------------------------------------------
# numpy数组拼接方法介绍
思路：首先将数组转成列表，然后利用列表的拼接函数append()、extend()等进行拼接处理，最后将列表转成数组。

示例1：
```
>>> import numpy as np
>>> a=np.array([1,2,5])
>>> b=np.array([10,12,15])
>>> a_list=list(a)
>>> b_list=list(b)

>>> a_list.extend(b_list)

>>> a_list
[1, 2, 5, 10, 12, 15]
>>> a=np.array(a_list)
>>> a
array([ 1,  2,  5, 10, 12, 15])
```
该方法只适用于简单的一维数组拼接，由于转换过程很耗时间，对于大量数据的拼接一般不建议使用。

 

数组拼接方法二

思路：numpy提供了numpy.append(arr, values, axis=None)函数。对于参数规定，要么一个数组和一个数值；要么两个数组，不能三个及以上数组直接append拼接。append函数返回的始终是一个一维数组。

示例2：
```
>>> a=np.arange(5)
>>> a
array([0, 1, 2, 3, 4])
>>> np.append(a,10)
array([ 0,  1,  2,  3,  4, 10])
>>> a
array([0, 1, 2, 3, 4])

 

>>> b=np.array([11,22,33])
>>> b
array([11, 22, 33])
>>> np.append(a,b)
array([ 0,  1,  2,  3,  4, 11, 22, 33])

 

>>> a
array([[1, 2, 3],
       [4, 5, 6]])
>>> b=np.array([[7,8,9],[10,11,12]])
>>> b
array([[ 7,  8,  9],
       [10, 11, 12]])
>>> np.append(a,b)
array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12])
```
numpy的数组没有动态改变大小的功能，numpy.append()函数每次都会重新分配整个数组，并把原来的数组复制到新数组中。

 

数组拼接方法三

思路：numpy提供了numpy.concatenate((a1,a2,...), axis=0)函数。能够一次完成多个数组的拼接。其中a1,a2,...是数组类型的参数
```
numpy.concatenate((a1, a2, ...), axis=0)
Join a sequence of arrays along an existing axis.

Parameters:	
a1, a2, ... : sequence of array_like
The arrays must have the same shape, except in the dimension corresponding to axis (the first, by default).
axis : int, optional
The axis along which the arrays will be joined. Default is 0.
Returns:	
res : ndarray
The concatenated array.
```
示例3：
```
>>> a=np.array([1,2,3])
>>> b=np.array([11,22,33])
>>> c=np.array([44,55,66])
>>> np.concatenate((a,b,c),axis=0)  # 默认情况下，axis=0可以不写
array([ 1,  2,  3, 11, 22, 33, 44, 55, 66]) #对于一维数组拼接，axis的值不影响最后的结果

 

>>> a=np.array([[1,2,3],[4,5,6]])
>>> b=np.array([[11,21,31],[7,8,9]])
>>> np.concatenate((a,b),axis=0)
array([[ 1,  2,  3],
       [ 4,  5,  6],
       [11, 21, 31],
       [ 7,  8,  9]])

>>> np.concatenate((a,b),axis=1)  #axis=1表示对应行的数组进行拼接
array([[ 1,  2,  3, 11, 21, 31],
       [ 4,  5,  6,  7,  8,  9]])
```
 

对numpy.append()和numpy.concatenate()两个函数的运行时间进行比较

示例4：
````
>>> from time import clock as now
>>> a=np.arange(9999)
>>> b=np.arange(9999)
>>> time1=now()
>>> c=np.append(a,b)
>>> time2=now()
>>> print time2-time1
28.2316728446
>>> a=np.arange(9999)
>>> b=np.arange(9999)
>>> time1=now()
>>> c=np.concatenate((a,b),axis=0)
>>> time2=now()
>>> print time2-time1
20.3934997107
```

可知，concatenate()效率更高，适合大规模的数据拼接




# Sort排序

python 列表list中内置了一个十分有用的排序函数sort,sorted，它可以用于列表的排序

```
a = [5,2,1,9,6]        
 
>>> sorted(a)                  #将a从小到大排序,不影响a本身结构 
[1, 2, 5, 6, 9] 
 
>>> sorted(a,reverse = True)   #将a从大到小排序,不影响a本身结构 
[9, 6, 5, 2, 1] 
 
>>> a.sort()                   #将a从小到大排序,影响a本身结构 
>>> a 
[1, 2, 5, 6, 9] 
 
>>> a.sort(reverse = True)     #将a从大到小排序,影响a本身结构 
>>> a 
[9, 6, 5, 2, 1]
```
