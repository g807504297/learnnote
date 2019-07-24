# 面向对象高级编程

## 使用_slots_  
使用__slots__可以限制实例的属性，例如，只允许对Student实例添加name和age属性。
为了达到限制的目的，python允许在定义class的时候，定义一个特殊的__slots__变量，来限制该class的实例能添加的属性：
```py
class Student(object):
    __slots__ = ('name','age')#用tuple定义允许绑定的属性名称
```
注意：使用__slots__定义的属性，仅对当前类实例起作用，对继承的子类是不起作用的。除非在子类也定义__slots__,这样，子类实例允许定义的属性就是自身的__slots__加上父类的__slots__。
## 使用@property  
python内置的@property装饰器就是负责把一个方法当做一个属性来调用
```py
class Student(object):
    @property
    def score(self):  #s.get_score()
        return self._score

    @score.setter
    def score(self,value): #s.set_score()
        if not isinstance(value,int):
            raise ValueError("Score must be an integer!")
        if value <0 or value >100:
            raise ValueError("socre must between 1~100")
        self._score = value

#call
s = Student()
s.score = 50 # 实际转化为s.set_score(60)
print(s.score)# 实际转换s.get_score()
s.score = 101
```

## 多重继承  
继承是面向对象编程的一个重要方式，应为通过继承，子类就可以扩展父类的功能。  
### MixIn  
在设计类的继承关系时，通常，主线都是单一继承下来的，例如，Ostrich继承自Bird。但是，如果需要“混入”额外的功能，通过多重继承就可以实现，比如，让Ostrich除了继承自Bird外，再同时继承Runnable。这种设计通常称之为MixIn。  
为了更好地看出继承关系，我们把Runnable和Flyable改为RunnableMixIn和FlyableMixIn。类似的，你还可以定义出肉食动物CarnivorousMixIn和植食动物HerbivoresMixIn，让某个动物同时拥有好几个MixIn：  
MixIn的目的就是给一个类增加多个功能，这样，在设计类的时候，我们优先考虑通过多重继承来组合多个MixIn的功能，而不是设计多层次的复杂的继承关系。

## 定制类 
看到形如__xxx__的变量或者函数名就要注意了，这些在python中是有特殊用途的，例如__slots__限制实例属性，__len__()为了让class作用于len()函数。python中还有许多这种有特殊用途的函数，可以帮助我们定制类。
###__str__  
__str__()返回用户看到的字符串，__repr__()返回程序开发者看到的字符串
```py
class Student(object):
    def __init__(self,name):
        self.name = name

    def __str__(self):
        return "student object (name: %s)"%self.name
    __repr__ = __str__

print(Student("Machael"))

``` 
### __iter__
如果一个类想被用于`for .... in`循环，类似list或者tuple那样，就必须实现一个`__iter__()`方法，该方法返回一个迭代对象，然后，python的for循环就会不断调用该对象的`__next__()`方法拿到循环的下一个值，直到遇到`StopIteration`错误时退出循环。
```py
class Fib(object):
    def __init__(self):
        self.a,self.b = 0,1 #初始化两个计数器a,b

    def __iter__(self):
        return self  #实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a,self.b  = self.b,self.a+self.b #计算下一个值
        if self.a > 100000:  #退出循环的条件
            raise StopIteration()
        return self.a  #返回下一个值

for n in Fib():
    print(n)
```  
### __getitem__  
#### 下标取元素
要想像list那样按照下标取出元素，需要实现`__getitem__()`方法:
```py
class Fib(object):
    def __getitem__(self, n):
        a, b =1,1
        for x in range(n):
            a,b = b, a+b
        return a
f  = Fib()
for i in range(20):
    print(f[i])
```
#### 切片方法
`__getitem__()`传入的参数可能是一个int，也可能是一个切片对象`slice`,所以要进行判断：
```py
class Fib(object):
    def __getitem__(self, n):
        if isinstance(n ,int): # n是索引
            a,b =1,1
            for x in range(n):
                a, b = b, a+b
            return a

        if isinstance(n,slice): # n是切片
            start = n.start
            stop = n.stop
            if start is None:
                start = 0
            a, b = 1,1
            L = []
            for x in range(stop):
                if x >= start:
                    L.append(a)
                a,b = b,a+b
            return L
f  = Fib()
print(f[0:5])
print(f[:5])
print(f[5:10])
```  
`__getitem__()`的参数也可能是一个可以作为key的object，例如str，与之对应的是`__setitem__()`方法，把对象视作list或dict来对集合进行赋值。最后，还有一个`__delitem__()`方法用于删除某个元素。  

### __getattr__  
当调用不存在的属性时，比如`score`,python解释器会试图调用`__getattr__(self,'score')`来尝试获得属性，这样我们就有机会返回`score`的值
```py
class Student(object):
    def __init__(self):
        self.name = 'Michael'

    def __getattr__(self,attr):
        if attr =='score': #可以返回score值
            return 99
        if attr == 'age':#可以返回函数，调用方式要改变,加一个()
            return lambda:24

s = Student()
print(s.score)
print(s.age())
```
### __call__  
任何类，只需要定义一个`__call__()`方法，就可以对实例进行调用。
```py
class Student(object):
    def __init__(self,name):
        self.name = name

    def __call__(self):
        print("my name is %s"% self.name)


s = Student("Lisi")
s()
```  
## 枚举类
```
#待补充

```

## 使用元类  
### type()  
动态语言和静态语言的最大不同个，就是函数和类的定义，不是编译时定义的，而是运行时动态创建的。
