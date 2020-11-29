## `with`关键字
[廖雪峰python文档](https://www.liaoxuefeng.com/wiki/1016959663602400/1115615597164000)
[解析](https://www.jianshu.com/p/5b01fb36fd4c)
> **使用场合：**
>
> + **文件/资源操作**
> + **进程线程之间互斥对象**
> + **支持上下文的其他对象**

1. **需要对一个类实现`__enter__()`和`__exit__()`方法，则通过`with`调用**

```python
>>> class Test():
>>> 	def __enter__(self):
>>> 		[...]
>>> 	def __exit__(self, exc_type, exc_val, exc_tb):
>>>			[...]
```

2. **需要对一个函数调用`with`时候使用`@contextlib.contextmanager`进行装饰：**	
	**`@contextmaneger`接收一个generator，即在被装饰函数内使用`yield`语句把`with... as var`输出出去**
```python
from contextlib import contextmanager

class Query(object):

    def __init__(self, name):
        self.name = name

    def query(self):
        print('Query info about %s...' % self.name)

@contextmanager
def create_query(name):
    print('Begin')
    q = Query(name)
    yield q
    print('End')
```
```python
with create_query('Bob') as q:
    q.query()
```
+ **当想在某段代码执行前后自动执行某些操作，也可以用`@contextmanager`实现，例如：**

```python
@contextmanager
def tag(name):
    print("<%s>" % name)
    yield
    print("</%s>" % name)

with tag("h1"):
    print("hello")
    print("world")

```
上述输出为
```python
<h1>
hello
world
</h1>
```

## `yeild`关键字
[解析](https://blog.csdn.net/mieleizhi0522/article/details/82142856)
配合`tf.GradientTape().stop_recording()`食用：
[tf-Advanced Automatic Differentiation高级自动微分](https://www.tensorflow.org/guide/advanced_autodiff)
**example**
```python
>>> x = tf.Variable(2.0)
>>> y = tf.Variable(3.0)

>>> with tf.GradientTape() as t:
>>>     x_sq = x * x
>>>     # 在stop_recording期间所有操作都不会记录进tape
>>>     with t.stop_recording():
>>> 		y_sq = y * y
>>>     z = x_sq + y_sq

>>> grad = t.gradient(z, {'x': x, 'y': y})

>>> print('dz/dx:', grad['x'])  # 2*x => 4
>>> print('dz/dy:', grad['y'])
dz/dx: tf.Tensor(4.0, shape=(), dtype=float32)
dz/dy: None

```
```python
`tf.GradientTape().stop_recording()`

#经过装饰后变为含有__enter__和__exit__方法的函数，可通过with调用
@tf_contextlib.contextmanager
def stop_recording(self):
    if self._tape is None:
        raise RuntimeError(
          "Trying to stop recording a tape which is not recording.")
    self._pop_tape()
    try:
        yield
    finally:
        self._push_tape()
      
```



## 多继承与super详解
[解析](https://blog.csdn.net/qq_26442553/article/details/81775449)
通过一个类的内置属性查看多继承时候的方法，属性的调用路径
​```python
print(cls.__mro__)
```

一个类的圆括号中继承父类的顺序是从左至右，即当子类使用跟父类相同名字的未指定的方法，按照从左至右的继承顺序查找方法进行使用。
+ 单继承时候，`super.()__init__()`和`cls.__init()`使用无差
+ 多继承时候，`super.()__init__()`需要一次传递全部参数
	为了避免继承报错最好在`__init__()`中加上`*args`和`**kwargs`
	
	
## 查看类中包含的函数
​```python
class Test():
    def __init__(self):
        pass

    def check(self):
        pass

    @staticmethod
    def put():
        pass

    @classmethod
    def own(cls):
        pass
```
```python
>>> Print(Test.__dict__)
{'__module__': '__main__', '__init__': <function Test.__init__ at 0x0000000002214CA8>, 'check': <function Test.check at 0x0000000002214C18>, 'put': <staticmethod object at 0x0000000002278648>, 'own': <classmethod object at 0x0000000002278688>, '__dict__': <attribute '__dict__' of 'Test' objects>, '__weakref__': <attribute '__weakref__' of 'Test' objects>, '__doc__': None}
```


## Property
[详解](https://www.liaoxuefeng.com/wiki/1016959663602400/1017502538658208)
```python
class Student():
	@property
	def score(self):
		return self._score
	
	@score.setter
	def score(self, new_value):
		[...]
		self._score = new_value
```
只定义`@property`时候，则只读而不可设值

## raise
[详解](http://c.biancheng.net/view/2360.html)
通过手动抛出异常的方式可以进行异常检测排查等。

```python
try:
    a = input("输入一个数：")
    #判断用户输入的是否为数字
    if(not a.isdigit()):
        raise ValueError("a 必须是数字")
except ValueError as e:
    print("引发异常：",repr(e))
```
输出为
```python
输入一个数：a
引发异常： ValueError('a 必须是数字')
```

## sys.stdin 和 sys.stuout





## 获取命令行输入
```python
# 默认第一个参数为文件名称
# 之后是各个参数与参数值间以空格分开的所组成的列表
import sys
argv_list_command = sys.argv[1:]
```

## `str.strip()`和`str.replace()`对字符串去除字符
[详解](https://www.runoob.com/python/att-string-strip.html)    [详解2](https://blog.csdn.net/jerrygaoling/article/details/81051447)
只能删除开头或结尾的字符，而不对中间匹配的字符删除
在不指定情况下，删除`\r``\t``\n`和空格
[指定一个字符或者字符串的情况下](https://blog.csdn.net/u012671171/article/details/52024874)都是拆成单个字符组成的匹配列表，在按顺序对目标字符串的字符进行匹配直到头尾出现了第一个找不到匹配项。
单独对开头或结尾的指定字符删除则使用`str.lstrip()`或`str.rstrip()`
对中间的字符进行删除则使用`str.replace('cha','')`

>在python中存在继承了 回车符\r 和 换行符\n 两种标记。
>\r 代表回车，也就是打印头归位，回到某一行的开头。
>\n代表换行，就是走纸，下一行。
>linux只用\n换行。
>win下用\r\n表示换行。

因此在windows中读取有空格的文件是需要对每一行进行空格的清除来保证不会有多余的空白符占用



## 控制台重定向输入/输出
[简析](https://blog.csdn.net/qq_25046261/article/details/78186223)
```cmd
>>> # 输入文件到sys.stdin
>>> python test.py < test.txt
>>> # 输出文件到sys.stdout(同名文件下旧内容会被覆盖)
>>> python test.py > test.txt
>>> # 输出文件到sys.stdout(同名文件下不会被覆盖而是追加)
>>> python test.py >> test.txt
```



## `cls.__new__()`保证单例
[单例实现详解](https://blog.csdn.net/qq_33694648/article/details/104357498)
```python
class Test():
	def __init__(self):
		pass
	# 真正的创建对象函数，优于__init__执行
	def __new__(cls, *args, **kwargs):
		# cls指的是本Test类
		if not hasattr(Test, "_instance"):
			Test._instance = object.__new__(cls)
		# 没有返回值则__init__不会执行
		return Test._instance
```



## re模块
[正则匹配式](https://www.cnblogs.com/shenjianping/p/11647473.html)    [RUNOOB](https://www.runoob.com/python/python-reg-expressions.html)
汉字匹配： `[\u4e00-\u9fa5]`
包括日韩汉字匹配：`[\u2E80-\u9FFF]`





## `operator.itemgetter()`
[简析](https://www.jianshu.com/p/28d6e5a5b9ed)
构造一个获取对象的某一些维的数据的函数，参数值为索引和键值
```python
>>> from operator import itemgetter

>>> ll = [1, 2, 3]
>>> dd = {'name': 'mxt', 'age': 18, 'gender': 'female'}

>>> # 定义函数，获取对象第1个域的值
>>> func1 = itemgetter(1)
>>> res1 = func1(ll)  
2 <class 'int'>

>>> # 定义函数，获取对象第0个域和第2个的值
>>> func2 = itemgetter(0, 2)
>>> res2 = func2(ll) 
(1, 3) <class 'tuple'>

>>> func3 = itemgetter('name')
>>> res3 = func3(dd) 
mxt <class 'str'>

>>> func4 = itemgetter('name', 'gender')
>>> res4 = func4(dd)  
('mxt', 'female') <class 'tuple'>
```
**与lambda的作用比较**

```python
>>> data = [('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10)]

>>>data1 = sorted(data, key=lambda x: (x[1], x[2]))  
[('john', 'A', 15), ('dave', 'B', 10), ('jane', 'B', 12)]

>>> data2 = sorted(data, key=itemgetter(1, 2))  
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```


## 获取空迭代器的元素
使用`for * in []/{}:`此时不会进入循环内部，自动跳过

## 空迭代器与None
空迭代器不为None，类型为各自的迭代类型。
```python
>>> a = {}
>>> type(a)
<class dict>
>>> if not a:
... 	print("empty")
...
empty
>>> if a is not None:
...		print(" is not None")
...
is not None
```

## 面向对象高级编程-类详解
[详解-博客](https://www.cnblogs.com/shenjianping/p/11050198.html)




## 装饰器 
[详解](https://www.jianshu.com/p/ee82b941772a)
[RUNOOB](https://www.runoob.com/w3cnote/python-func-decorators.html)
[装饰器接收参数](https://blog.csdn.net/weixin_42948748/article/details/90116904)
[多个装饰器的执行顺序](https://www.cnblogs.com/azeng1990/p/11192349.html)
[多个装饰器的执行顺序2](https://www.jb51.net/article/148431.htm)
## 函数属性
[详解](https://blog.csdn.net/dongfuguo/article/details/105103472)
[详解2](http://c.biancheng.net/view/2256.html)
[详解3](http://www.voidcn.com/article/p-xwfebqbz-bud.html)

```

```