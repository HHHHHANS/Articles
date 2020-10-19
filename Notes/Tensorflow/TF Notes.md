# TF Notes
+ ## 使用`tf.function( )`修饰的函数内部再使用`np.append( )`会报错
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

+ ##  Keras - 自定义Layer & Model
    ##### [tensorflow官方例子](https://tensorflow.google.cn/guide/keras/custom_layers_and_models)
   #####    [GitHub-source](https://github.com/tensorflow/tensorflow/blob/v2.3.1/tensorflow/python/keras/engine/base_layer.py#L104-L3035)
>***We recommend that descendants of `Layer` implement the following methods***
>* `__init__()`: Defines custom layer attributes, and creates layer state  variables that do not depend on input shapes, using `add_weight()`.
>* `build(self, input_shape)`: This method can be used to create weights that  depend on the shape(s) of the input(s), using `add_weight()`. `__call__()`  will automatically build the layer (if it has not been built yet) by  calling `build()`.
>* `call(self, *args, **kwargs)`: Called in `__call__` after making sure  `build()` has been called. `call()` performs the logic of applying the  layer to the input tensors (which should be passed in as argument).  Two reserved keyword arguments you can optionally use in `call()` are:    - `training` (boolean, whether the call is in      inference mode or training mode)    - `mask` (boolean tensor encoding masked timesteps in the input, used      in RNN layers)
>* `get_config(self)`: Returns a dictionary containing the configuration used  to initialize this layer. If the keys differ from the arguments  in `__init__`, then override `from_config(self)` as well.  This method is used when saving  the layer or a model that contains this layer.

**改写`__init__( )`、`call( )`、`build( )`**

**`call( )`和`build( )`都会在父类`Layer`自动被__call__引用，因此修改自定义类中相应方法即可**
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

+ ##  Dropout层
    [tensorflow-layers.Dropout](https://tensorflow.google.cn/api_docs/python/tf/keras/layers/Dropout)
    
    [tensorflow-tf.nn.dropout](https://tensorflow.google.cn/api_docs/python/tf/nn/dropout)

  ##### \*Dropout层内核使用的是`nn.dropout（）`进行计算
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
```python
>>>tf.random.set_seed(0)
>>>x = tf.ones([3,5])
>>>tf.nn.dropout(x, rate=0.5, seed=1).numpy()
array([[2., 0., 0., 2., 2.],
       [2., 2., 2., 2., 2.],
       [2., 0., 2., 0., 2.]], dtype=float32)
```
>***With probability `rate` elements of `x` are set to `0`.The remaining elements are scaled up by `1.0 / (1 - rate)`, so that the expected value is preserved.***

注意!! **rate**表示每个隐层节点的被屏蔽的概率（起码在tf中是如此），并不表示张量中的数值被屏蔽的比例。（大数原理？）
>**源码中dropout具体算法步骤是先进行整体缩放再按概率对节点进行置零**

   **example**
```python
>>>tf.random.set_seed(0)
>>>x = tf.ones([2,5])
>>>#在特定的seed情况下有可能出现数值增加的情况
>>>tf.nn.dropout(x, rate = 0.2, seed = 3).numpy()
array([[1.25, 1.25, 1.25, 1.25, 1.25],
       [1.25, 1.25, 1.25, 1.25, 1.25]], dtype=float32)
```

