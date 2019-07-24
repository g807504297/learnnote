# numpy快速入门笔记  
Numpy是python语言的一个扩展程序库，支持大量的维度数组与矩阵运算，此外也针对数组运算提供了大量的数学函数库  
Numpy通常与Scipy和Matplotlib(绘图库)一起使用，这种组合广泛用于替代Matlab，是一个强大的科学计算环境，有助于我们通过python学习数据科学或机器学习  

Scipy:包含的模块有：最优化、线性代数、积分、插值、特殊函数、快速傅里叶变换、信号处理、图像处理、常微分方程求解和其他科学与工程重常用的计算

Matplotlib:是一种可视化操作界面。它利用通用的图形用户界面工具包，如Tkinter.wxPython,Qt或GTK+向应用程序嵌入式绘图提供了API

##  Ndarray对象  
N维数组对象ndarray,塔式一系列同型数据的集合，以0下标为开始进行集合中元素的索引
- ndarray对象时用于存放同类型元素的多维数组
- ndarray中的每个元素在内存中有相同储存大小的区域  
`numpy.array(object, dtype = None, copy = True, order = None, subok = False, ndmin = 0)`
```
名称	描述
object	数组或嵌套的数列
dtype	数组元素的数据类型，可选
copy	对象是否需要复制，可选
order	创建数组的样式，C为行方向，F为列方向，A为任意方向（默认）
subok	默认返回一个与基类类型一致的数组
ndmin	指定生成数组的最小维度
```  
## Numpy数据类型  
基本上可以和c语言的数据类型对应上，以下列举常用Numpy基本类型。  
```
名称	描述
bool_	布尔型数据类型（True 或者 False）
int_	默认的整数类型（类似于 C 语言中的 long，int32 或 int64）
intc	与 C 的 int 类型一样，一般是 int32 或 int 64
intp	用于索引的整数类型（类似于 C 的 ssize_t，一般情况下仍然是 int32 或 int64）
int8	字节（-128 to 127）
int16	整数（-32768 to 32767）
int32	整数（-2147483648 to 2147483647）
int64	整数（-9223372036854775808 to 9223372036854775807）
uint8	无符号整数（0 to 255）
uint16	无符号整数（0 to 65535）
uint32	无符号整数（0 to 4294967295）
uint64	无符号整数（0 to 18446744073709551615）
float_	float64 类型的简写
float16	半精度浮点数，包括：1 个符号位，5 个指数位，10 个尾数位
float32	单精度浮点数，包括：1 个符号位，8 个指数位，23 个尾数位
float64	双精度浮点数，包括：1 个符号位，11 个指数位，52 个尾数位
complex_	complex128 类型的简写，即 128 位复数
complex64	复数，表示双 32 位浮点数（实数部分和虚数部分）
complex128	复数，表示双 64 位浮点数（实数部分和虚数部分）
```  
numpy的数值类型实际上是dtype对象的实例，并对应唯一的字符，包括np.bool_,np.int32,np.float32,等等。 
### 数据类型对象(dtype)  
数据类型独享用来描述 数组对应的内存区域如何使用  
`numpy.dtype(object, align, copy)`
```
object - 要转换为的数据类型对象
align - 如果为 true，填充字段使其类似 C 的结构体。
copy - 复制 dtype 对象 ，如果为 false，则是对内置数据类型对象的引用
```  
## Numpy数组属性  
Numpy数组的维数成为秩(rank),一维数组的秩为1，二维数组的秩为2，以此类推  
在Numpy中，每一个线性的数组成为一个轴(axis),也就是维度。  
Numpy的数组中比较重要ndarray对象属性有：
```
属性	说明
ndarray.ndim	秩，即轴的数量或维度的数量
ndarray.shape	数组的维度，对于矩阵，n 行 m 列
ndarray.reshape 重新调整数组的大小
ndarray.size	数组元素的总个数，相当于 .shape 中 n*m 的值
ndarray.dtype	ndarray 对象的元素类型
ndarray.itemsize	ndarray 对象中每个元素的大小，以字节为单位
ndarray.flags	ndarray 对象的内存信息
ndarray.real	ndarray元素的实部  
ndarray.imag	ndarray 元素的虚部
ndarray.data	包含实际数组元素的缓冲区，由于一般通过数组的索引获取元素，所以通常不需要使用这个属性。
```  
## Numpy创建数组  
ndarray数组除了可以使用底层ndarray构造器来创建外，也可以通过以下几种方式来创建。
### numpy.empty  
numpy.empty方法用来创建一个指定形状(shape)、数据类型(dtype)且未初始化的数组：
`numpy.empty(shape, dtype = float, order = 'c')`
```
参数	描述
shape	数组形状
dtype	数据类型，可选
order	有"C"和"F"两个选项,分别代表，行优先和列优先，在计算机内存中的存储元素的顺序
```  
### numpy.zeros  
创建指定大小的数组，数组元素以0来填充  
`numpy.zeros(shape,dtype = float, order = 'c')`
```

参数	描述
shape	数组形状
dtype	数据类型，可选
order	'C' 用于 C 的行数组，或者 'F' 用于 FORTRAN 的列数组  
```  
### numpy.ones  
用来指定形状的数组，数组元素以1来填充    
`numpy.ones(shape,dtype = None, order ='C')   `
```

参数	描述
shape	数组形状
dtype	数据类型，可选
order	'C' 用于 C 的行数组，或者 'F' 用于 FORTRAN 的列数组
```
## Numpy从已有的数组创建数组  
### numpy.asarray   
类似numpy.array  
`numpy.asarray(a,dtype=None,order = None)`
```
参数	描述
a	任意形式的输入参数，可以是，列表, 列表的元组, 元组, 元组的元组, 元组的列表，多维数组
dtype	数据类型，可选
order	可选，有"C"和"F"两个选项,分别代表，行优先和列优先，在计算机内存中的存储元素的顺序。
```
### numpy.frombuffer  
numpy.frombuffer用于实现动态数组。  
numpy.frombuffer接受buffer的输入参数，以流的形式读入转化为ndarray对象  
`numpy.frombuffer(buffer,dtype = float,count = -1,offset = 0)`
```
参数	描述
buffer	可以是任意对象，会以流的形式读入。
dtype	返回数组的数据类型，可选
count	读取的数据数量，默认为-1，读取所有数据。
offset	读取的起始位置，默认为0。
```  
**_注意：buffer是字符串的时候，python3默认str是unicode类型，所以要转成bytestring在原str前加上b。_**
### numpy.fromiter  
numpy.fromiter方法从可迭代对象中建立ndarray对象，返回一维数组。
`numpy.fromiter(iterable,dtype,count = -1)`
```
参数	描述
iterable	可迭代对象
dtype	返回数组的数据类型
count	读取的数据数量，默认为-1，读取所有数据
```  
## Numpy从数值范围创建数组  

### numpy.arange()   
创建数值范围，并返回ndarray对象，函数格式如下：
根据start与stop指定的范围以及step设定的步长，生成一个ndarray
`nmpay.arange(start,stop,step,dtype)`
```
参数	描述
start	起始值，默认为0
stop	终止值（不包含）
step	步长，默认为1
dtype	返回ndarray的数据类型，如果没有提供，则会使用输入数据的类型

```
### numpy.linspace  
numpy.linspace函数用于创建一个一维数组，数组是一个等差数列构成的  
`np.linspace(start,stop,num =50,endpoint =True,retstep = False,dtype = None)`
```
参数	描述
start	序列的起始值
stop	序列的终止值，如果endpoint为true，该值包含于数列中
num	要生成的等步长的样本数量，默认为50
endpoint	该值为 ture 时，数列中中包含stop值，反之不包含，默认是True。
retstep	如果为 True 时，生成的数组中会显示间距，反之不显示。
dtype	ndarray 的数据类型
```
### numpy.logspace  
用于创建一个等比数列，格式如下：  
`np.logspace(start,stop,num =50,endpoint=True,base = 10.0,dtype = None)`
```
参数	描述
start	序列的起始值为：base ** start
stop	序列的终止值为：base ** stop。如果endpoint为true，该值包含于数列中
num	要生成的等步长的样本数量，默认为50
endpoint	该值为 ture 时，数列中中包含stop值，反之不包含，默认是True。
base	对数 log 的底数。
dtype	ndarray 的数据类型
```
## Numpy切片和索引  
ndarray对象的内容可以通过索引或切片来修改，与python中list的切片操作一样。ndarray数组可以基于0-n的下标进行索引，切片对象可以通过内置的slice函数，并设置start，stop及step参数进行，从原数组中切割出一个新数组。  
```py
import  numpy as np

a = np.array([[1,2,3],[3,4,5],[4,5,6]])
print(a[...,1]) #第二列元素
print(a[1,...]) #第二行元素
print(a[...,1:]) #第二列及剩下的所有元素
```  
## Numpy高级索引  
Numpy比一般的python序列提供更多的索引方式。除了之前看到的用整数和切片的索引外，数组可以由整数数组索引、布尔索引及花式索引。  
### 整数数组索引  
```py
import  numpy as np
'''
取(0,0),(1,1),(2,0)位置处的元素,即[1,4,5]
1  2
3  4
5  6
'''
x = np.array([[1,2],[3,4],[5,6]])
y = x[[0,1,2],[0,1,0]]
print(y)

#可借助切片`：`或`...`与索引数组组合。
import numpy as np
a = np.array([[1,2,3], [4,5,6],[7,8,9]])
b = a[1:3, 1:3]
c = a[1:3,[1,2]]
d = a[...,1:]
print(b)
print(c)
print(d)

```  
### 布尔索引  
我们可以通过一个布尔数组来索引目标数组。
布尔索引通过布尔运算(如:比较运算)来获取符合指定条件的元素的数组。
```py
import numpy as np
#
x = np.array([[0,1,2],[3,4,5],[6,7,8],[9,10,11]])
print(x)
#通过比较运算符，来获取符合指定条件的元素的数组
print(x[x > 5])#打印大于5的元素


import numpy as np 
a = np.array([1,  2+6j,  5,  3.5+5j])  
print (a[np.iscomplex(a)])#过滤掉非复数元素
#print(a[~np.iscomplex(a)])#过滤掉复数元素
```

### 花式索引(没看懂是啥)  
花式索引指的是利用整数数组进行索引。  
花式索引根据索引数组的值作为目标数组的某个轴的下标来取值。对于使用一维整形数组作为索引，如果目标是一维数组，那么索引的据俄国就是对应位置的元素；若果目标是二维数组，那么就是对应下标的行

## Numpy广播(Broadcast)  
广播(Broadcast是numpy对不同形状(shape)的数组进行数值计算的方式，对数组的算术运算通常在相应的元素上进行。  
如果两个数组a和b形状相同，即a.shape == b.shape,那么a*b的结果就是a与b数组对应位相乘。这要求维数相同，且各维度的长度相同。
```
广播的规则:
- 让所有输入数组都向其中形状最长的数组看齐，形状中不足的部分都通过在前面加 1 补齐。
- 输出数组的形状是输入数组形状的各个维度上的最大值。
- 如果输入数组的某个维度和输出数组的对应维度的长度相同或者其长度为 1 时，这个数组能够用来计算，否则出错。
- 当输入数组的某个维度的长度为 1 时，沿着此维度运算时都用此维度上的第一组值。

简单理解：对两个数组，分别比较他们的每一个维度（若其中一个数组没有当前维度则忽略），满足：
- 数组拥有相同形状。
- 当前维度的值相等。
- 当前维度的值有一个是 1。
若条件不满足，抛出 "ValueError: frames are not aligned" 异常。
```  
## Numpy迭代数组 
Num
