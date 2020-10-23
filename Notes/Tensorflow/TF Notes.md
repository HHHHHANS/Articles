# TF Notes
## 使用`tf.function( )`修饰的函数内部再使用`np.append( )`会报错
```python
@tf.function
def augment_data(image_batch, labels):
    import numpy as np
    from tensorflow.image import flip_left_right

    image_batch = np.append(image_batch, flip_left_right(image_batch), axis=0)
    [ ... ]
```
> ValueError：无法连接零维数组

**此时需要将numpy ops包装进 `tf.py_function`**
```python
def append(image_batch, tf_func):
    return np.append(image_batch, tf_func, axis=0)

@tf.function
def augment_data(image_batch):
    image = tf.py_function(append, 
                                  inp=[image_batch,
                                  tf.image.flip_left_right(image_batch)], 
                                  Tout=[tf.float32]
                                  )
    return image
```




## Keras - 自定义Layer & Model

+ ### 自定义Layer
    ##### [tensorflow官方例子](https://tensorflow.google.cn/guide/keras/custom_layers_and_models)
   #####    [GitHub-source](https://github.com/tensorflow/tensorflow/blob/v2.3.1/tensorflow/python/keras/engine/base_layer.py#L104-L3035)
>***We recommend that descendants of `Layer` implement the following methods***
>* `__init__()`: Defines custom layer attributes, and creates layer state  variables that do not depend on input shapes, using `add_weight()`.
>* `build(self, input_shape)`: This method can be used to create weights that  depend on the shape(s) of the input(s), using `add_weight()`. `__call__()`  will automatically build the layer (if it has not been built yet) by  calling `build()`.
>* `call(self, *args, **kwargs)`: Called in `__call__` after making sure  `build()` has been called. `call()` performs the logic of applying the  layer to the input tensors (which should be passed in as argument).  Two reserved keyword arguments you can optionally use in `call()` are:    - `training` (boolean, whether the call is in      inference mode or training mode)    - `mask` (boolean tensor encoding masked timesteps in the input, used      in RNN layers)
>* `get_config(self)`: Returns a dictionary containing the configuration used  to initialize this layer. If the keys differ from the arguments  in `__init__`, then override `from_config(self)` as well.  This method is used when saving  the layer or a model that contains this layer.

**改写`__init__( )`、`call( )`、`build( )`**

**`call( )`和`build( )`都会在父类`Layer`中自动被`__call__`调用，因此修改自定义类中相应方法即可**
**example**

```python
class Linear(keras.layers.Layer):
    def __init__(self, units=32):
        super(Linear, self).__init__()
        self.units = units
    
    def build(self, input_shape):
        self.w = self.add_weight(
            shape=(input_shape[-1], self.units),
            initializer="random_normal",
            trainable=True,
        )
        self.b = self.add_weight(
            shape=(self.units, ),
            initializer="random_normal",
            trainable=True,
        )
    
    def call(self, inputs):
        return tf.matmul(inputs, self.w) + self.b
```

+ ### 自定义Model




##  Dropout层
    [tensorflow-layers.Dropout](https://tensorflow.google.cn/api_docs/python/tf/keras/layers/Dropout)
    
    [tensorflow-tf.nn.dropout](https://tensorflow.google.cn/api_docs/python/tf/nn/dropout)

  ##### \*Dropout层内核使用的是`nn.dropout（）`进行计算
+ `Dropout`层
```python
>>>tf.random.set_seed(0)
>>>layer = tf.keras.layers.Dropout(.2, input_shape=(2,))
>>>data = np.arange(10).reshape(5, 2).astype(np.float32)
>>>print(data)
[[0. 1.]
 [2. 3.]
 [4. 5.]
 [6. 7.]
 [8. 9.]]
 >>>outputs = layer(data, trainning=True)
 >>>print(outputs)
tf.Tensor(
[[ 0.    1.25]
 [ 2.5   3.75]
 [ 5.    6.25]
 [ 7.5   8.75]
 [10.    0.  ]], shape=(5, 2), dtype=float32)
```
+ `tf.nn.dropout()`
```python
>>>tf.random.set_seed(0)
>>>x = tf.ones([3,5])
>>>tf.nn.dropout(x, rate=0.5, seed=1).numpy()
array([[2., 0., 0., 2., 2.],
       [2., 2., 2., 2., 2.],
       [2., 0., 2., 0., 2.]], dtype=float32)
```
> *With probability `rate` elements of `x` are set to `0`.The remaining elements are scaled up by `1.0 / (1 - rate)`, so that the expected value is preserved.*

注意!! **rate**表示每个隐层节点的被屏蔽的概率（起码在tf中是如此），并不表示张量中的数值被屏蔽的比例。（大数原理？）源码中dropout具体算法步骤是先进行整体缩放再按概率对节点进行置零

   **example**
```python
>>>tf.random.set_seed(0)
>>>x = tf.ones([2,5])
>>>#在特定的seed情况下有可能出现输入经过处理后输出的总值增加的情况
>>>tf.nn.dropout(x, rate = 0.2, seed = 3).numpy()
array([[1.25, 1.25, 1.25, 1.25, 1.25],
       [1.25, 1.25, 1.25, 1.25, 1.25]], dtype=float32)
```

## tensorflow.keras.layers.LSTM
[官方文档](https://www.tensorflow.org/api_docs/python/tf/keras/layers/LSTM)

+ `LSTM()`参数中units：表示输出空间的维度

+ **[LSTM参数量的计算](https://blog.csdn.net/manmanxiaowugun/article/details/82966879)**

一层LSTM的参数量就是一个LSTM-cell的参数总量 -> 跟步长无关，只跟输入的特征维度n和输出的特征维度m有关:	
> [(n+m)\*m+m]\*4

+ 使用`model.fit()`样本输入shape为
`(样本数量-多少条时序数据，循环核展开步长-经过多少时间步产出结果， 每个时间步输入的特征数)`
+ 也可以在构建`LSTM()`层的时候指定`input_shape`参数

```python
>>>import tensorflow as tf
>>>from tensorflow.keras.layers import Layer, LSTM
>>>from tensorflow.keras import models, Sequential
>>>model_LSTM = tf.keras.Sequential([
	LSTM(3, input_shape=(2,5), activation='relu',return_sequences=False, name="Hans"),
])
>>>model_LSTM.compile(optimizer=tf.keras.optimizers.Adam(0.01),
			loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=False),
              metrics=['sparse_categorical_accuracy']
              )
>>>model_LSTM.summary()

Model: "sequential_64"
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
Hans (LSTM)                  (None, 3)                 108       
=================================================================
Total params: 108
Trainable params: 108
Non-trainable params: 0
_________________________________________________________________

```



## 时序数据的padding与mask

[官方例子](https://www.tensorflow.org/guide/keras/masking_and_padding)



## `tf.reduce_sum()`
`reduce_sum()` 用于计算张量tensor沿着某一维度的和，可以在求和后降维。[详细解析](https://www.jianshu.com/p/30b40b504bae)
问题来源：[`tf.gradientTape()`](https://www.tensorflow.org/guide/autodiff)计算梯度/导数

关于整数与矩阵操作间的求导以下例子
[自动微分-sum][pic1] [自动微分-mean][pic2]



## tf Advanced Automatic Differentiation 高级自动微分
[官方文档](https://www.tensorflow.org/guide/advanced_autodiff)



## `tf.keras.layers.BatchNormalization()`
[tf文档](https://www.tensorflow.org/guide/keras/transfer_learning#freezing_layers_understanding_the_trainable_attribute)
>
The only built-in layer that has non-trainable weights is the BatchNormalization layer. It uses non-trainable weights to keep track of the mean and variance of its inputs during training.
The `BatchNormalization` layer has 2 trainable weights and 2 non-trainable weights.

```python
>>> layer = keras.layers.BatchNormalization()
>>> layer.build((None, 4))  # Create the weights

>>> print("weights:", len(layer.weights))
>>> print("trainable_weights:", len(layer.trainable_weights))
>>> print("non_trainable_weights:", len(layer.non_trainable_weights))
weights: 4
trainable_weights: 2
non_trainable_weights: 2

```





























[pic1]:
[pic2]:
