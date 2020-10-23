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
​```python
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
```
​```python 
dz/dx: tf.Tensor(4.0, shape=(), dtype=float32)
dz/dy: None
```
`tf.GradientTape().stop_recording()`
```python
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
```python
print(cls.__mro__)
```

一个类的圆括号中继承父类的顺序是从左至右，即当子类使用跟父类相同名字的未指定的方法，按照从左至右的继承顺序查找方法进行使用。
+ 单继承时候，`super.()__init__()`和`cls.__init()`使用无差
+ 多继承时候，`super.()__init__()`需要一次传递全部参数
	为了避免继承报错最好在`__init__()`中加上`*args`和`**kwargs`
	
	
## 查看类中包含的函数
```python
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