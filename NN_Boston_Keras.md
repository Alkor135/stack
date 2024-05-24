# Boston Housing Dataset | Задача регрессии | Детали метода fit | НЕЙРОННЫЕ СЕТИ 6

https://www.youtube.com/watch?v=PLlic60dgS4  
Юля  
machine learrrning

## Детали метода fit в Keras

Сегодня обсудим:
1. Как обучаться по батчам (batch_size)
2. Как при обучении считать метрики и на тесте (validation_data)
3. Как обучаться на генераторах
4. И как визуализировать процесс обучения


## Поддержка
Платформа boosty https://boosty.to/machine_learrrning


<img src='https://drive.google.com/uc?id=1WX1sZjw2nHwpifsOQnCiE-SmyoSZ1ChJ'>



Получение данных

Описание датасета:

- CRIM     per capita crime rate by town
- ZN       proportion of residential land zoned for lots over 25,000 sq.ft.
- INDUS    proportion of non-retail business acres per town
- CHAS     Charles River dummy variable (= 1 if tract bounds river; 0 otherwise)
- NOX      nitric oxides concentration (parts per 10 million)
- RM       average number of rooms per dwelling
- AGE      proportion of owner-occupied units built prior to 1940
- DIS      weighted distances to five Boston employment centres
- RAD      index of accessibility to radial highways
- TAX      full-value property-tax rate per 10,000
- PTRATIO  pupil-teacher ratio by town
- B        1000(Bk - 0.63)^2 where Bk is the proportion of blacks by town
- LSTAT    % lower status of the population
- MEDV     Median value of owner-occupied homes in $1000's


```python
from keras.datasets import boston_housing

(X_train, y_train), (X_test, y_test) = boston_housing.load_data()

X_train.shape, X_test.shape
```




    ((404, 13), (102, 13))




```python
X_train[:1]
```




    array([[  1.23247,   0.     ,   8.14   ,   0.     ,   0.538  ,   6.142  ,
             91.7    ,   3.9769 ,   4.     , 307.     ,  21.     , 396.9    ,
             18.72   ]])



Масштабирование данных


```python
mean = X_train.mean(axis=0)
std = X_train.std(axis=0)

mean, std
```




    (array([3.74511057e+00, 1.14801980e+01, 1.11044307e+01, 6.18811881e-02,
            5.57355941e-01, 6.26708168e+00, 6.90106436e+01, 3.74027079e+00,
            9.44059406e+00, 4.05898515e+02, 1.84759901e+01, 3.54783168e+02,
            1.27408168e+01]),
     array([9.22929073e+00, 2.37382770e+01, 6.80287253e+00, 2.40939633e-01,
            1.17147847e-01, 7.08908627e-01, 2.79060634e+01, 2.02770050e+00,
            8.68758849e+00, 1.66168506e+02, 2.19765689e+00, 9.39946015e+01,
            7.24556085e+00]))




```python
X_train -= mean
X_train /= std

X_test -= mean
X_test /= std
```


```python
X_train.mean(axis=0)
```




    array([-1.01541438e-16,  1.09923072e-17,  1.74337992e-15, -1.26686340e-16,
           -5.25377321e-15,  6.41414864e-15,  2.98441140e-16,  4.94653823e-16,
            1.12671149e-17, -1.98136337e-16,  2.36686358e-14,  5.95679996e-15,
            6.13920356e-16])




```python
X_train.std(axis=0)
```




    array([1., 1., 1., 1., 1., 1., 1., 1., 1., 1., 1., 1., 1.])



### Архитектура сети


Определение сети через класс Sequential и добавление слоев в него через add


```python
from keras.models import Sequential
from keras.layers import Dense
import tensorflow as tf
tf.random.set_seed(9)


model = Sequential()
model.add(Dense(32, activation='relu', input_shape=(X_train.shape[1], )))
model.add(Dense(16, activation='relu'))
model.add(Dense(1))

model.summary()
```

    Model: "sequential_2"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     dense_6 (Dense)             (None, 32)                448       
                                                                     
     dense_7 (Dense)             (None, 16)                528       
                                                                     
     dense_8 (Dense)             (None, 1)                 17        
                                                                     
    =================================================================
    Total params: 993
    Trainable params: 993
    Non-trainable params: 0
    _________________________________________________________________
    


```python
model.compile(optimizer='adam', loss='mse', metrics=['mae'])
```

### Обучение сети


```python
%%time

num_epochs = 10

model.fit(X_train, y_train,
          epochs=num_epochs)
```

    Epoch 1/10
    13/13 [==============================] - 0s 2ms/step - loss: 568.1458 - mae: 21.9958
    Epoch 2/10
    13/13 [==============================] - 0s 2ms/step - loss: 548.3544 - mae: 21.5271
    Epoch 3/10
    13/13 [==============================] - 0s 2ms/step - loss: 526.5145 - mae: 20.9990
    Epoch 4/10
    13/13 [==============================] - 0s 2ms/step - loss: 500.7500 - mae: 20.3578
    Epoch 5/10
    13/13 [==============================] - 0s 2ms/step - loss: 470.4189 - mae: 19.5854
    Epoch 6/10
    13/13 [==============================] - 0s 2ms/step - loss: 434.6254 - mae: 18.6555
    Epoch 7/10
    13/13 [==============================] - 0s 2ms/step - loss: 394.5962 - mae: 17.5325
    Epoch 8/10
    13/13 [==============================] - 0s 2ms/step - loss: 349.9172 - mae: 16.2734
    Epoch 9/10
    13/13 [==============================] - 0s 3ms/step - loss: 299.5508 - mae: 14.7792
    Epoch 10/10
    13/13 [==============================] - 0s 2ms/step - loss: 246.4727 - mae: 13.1105
    CPU times: user 717 ms, sys: 29.9 ms, total: 747 ms
    Wall time: 730 ms
    

Сейчас прошло 13 батчей через сеть

#### **batch_size**
> batch_size: Integer или None.<br>
Количество сэмплов за один шаг градиентного спуска<br>
Если None, то batch_size примет дефолтное значение 32<br>
Не указывайте batch_size, если ваши данные представлены в виде генератора или keras.utils.Sequence (из-за того, что они сами генерируют данные батчами)


```python
X_train.shape
```




    (404, 13)




```python
404 / 32
```




    12.625



<img src='https://drive.google.com/uc?export=view&id=1j8SxKYEi12jzJXi_bPO28q5SV9emuu0Y'>


```python
%%time
num_epochs = 10

model.fit(X_train, y_train,
          epochs=num_epochs,
          batch_size=64)
```

    Epoch 1/10
    7/7 [==============================] - 0s 2ms/step - loss: 205.0507 - mae: 11.6536
    Epoch 2/10
    7/7 [==============================] - 0s 2ms/step - loss: 177.2639 - mae: 10.6083
    Epoch 3/10
    7/7 [==============================] - 0s 2ms/step - loss: 152.6436 - mae: 9.6686
    Epoch 4/10
    7/7 [==============================] - 0s 2ms/step - loss: 132.8702 - mae: 8.8585
    Epoch 5/10
    7/7 [==============================] - 0s 2ms/step - loss: 116.7651 - mae: 8.2183
    Epoch 6/10
    7/7 [==============================] - 0s 2ms/step - loss: 104.0974 - mae: 7.6807
    Epoch 7/10
    7/7 [==============================] - 0s 2ms/step - loss: 94.2699 - mae: 7.2536
    Epoch 8/10
    7/7 [==============================] - 0s 2ms/step - loss: 85.9139 - mae: 6.8800
    Epoch 9/10
    7/7 [==============================] - 0s 2ms/step - loss: 79.1750 - mae: 6.5551
    Epoch 10/10
    7/7 [==============================] - 0s 2ms/step - loss: 72.6714 - mae: 6.2447
    CPU times: user 262 ms, sys: 26.9 ms, total: 289 ms
    Wall time: 353 ms
    


```python
404 / 64
```




    6.3125




```python
%%time
num_epochs = 10

model.fit(X_train, y_train,
          epochs=num_epochs,
          batch_size=404)
```

    Epoch 1/10
    1/1 [==============================] - 0s 183ms/step - loss: 68.9849 - mae: 6.0634
    Epoch 2/10
    1/1 [==============================] - 0s 14ms/step - loss: 68.2099 - mae: 6.0216
    Epoch 3/10
    1/1 [==============================] - 0s 11ms/step - loss: 67.4433 - mae: 5.9807
    Epoch 4/10
    1/1 [==============================] - 0s 11ms/step - loss: 66.6846 - mae: 5.9404
    Epoch 5/10
    1/1 [==============================] - 0s 9ms/step - loss: 65.9333 - mae: 5.9007
    Epoch 6/10
    1/1 [==============================] - 0s 9ms/step - loss: 65.1889 - mae: 5.8613
    Epoch 7/10
    1/1 [==============================] - 0s 9ms/step - loss: 64.4519 - mae: 5.8223
    Epoch 8/10
    1/1 [==============================] - 0s 6ms/step - loss: 63.7225 - mae: 5.7836
    Epoch 9/10
    1/1 [==============================] - 0s 6ms/step - loss: 63.0009 - mae: 5.7454
    Epoch 10/10
    1/1 [==============================] - 0s 6ms/step - loss: 62.2875 - mae: 5.7079
    CPU times: user 331 ms, sys: 8.28 ms, total: 339 ms
    Wall time: 342 ms
    

#### **steps_per_epoch**

>  steps_per_epoch: Integer or `None`.<br>
        Общее количество шагов (батчей сэмплов) на одной эпохе. <br>
        Если стоит None, то steps_per_epoch равняется количеству сэмплов в датасете деленное на batch size


```python
404 / 10
```




    40.4




```python
%%time
num_epochs = 10

model.fit(X_train, y_train,
          epochs=num_epochs, 
          steps_per_epoch=10)
```

    Epoch 1/10
    10/10 [==============================] - 0s 2ms/step - loss: 58.5746 - mae: 5.5218
    Epoch 2/10
    10/10 [==============================] - 0s 2ms/step - loss: 52.8691 - mae: 5.2081
    Epoch 3/10
    10/10 [==============================] - 0s 2ms/step - loss: 47.0966 - mae: 4.9088
    Epoch 4/10
    10/10 [==============================] - 0s 2ms/step - loss: 43.0253 - mae: 4.6953
    Epoch 5/10
    10/10 [==============================] - 0s 2ms/step - loss: 39.3765 - mae: 4.4933
    Epoch 6/10
    10/10 [==============================] - 0s 2ms/step - loss: 36.2800 - mae: 4.3142
    Epoch 7/10
    10/10 [==============================] - 0s 2ms/step - loss: 33.6417 - mae: 4.1737
    Epoch 8/10
    10/10 [==============================] - 0s 2ms/step - loss: 31.5410 - mae: 4.0545
    Epoch 9/10
    10/10 [==============================] - 0s 2ms/step - loss: 29.8779 - mae: 3.9553
    Epoch 10/10
    10/10 [==============================] - 0s 2ms/step - loss: 28.2445 - mae: 3.8496
    CPU times: user 327 ms, sys: 24.5 ms, total: 351 ms
    Wall time: 330 ms
    

#### **validation_split**

> validation_split: Float между 0 и 1.<br>
        Доля обучающих данных, которая будет использоваться как валидационная часть<br>
        Модель возьмет эту долю и не будет на ней обучаться, а будет подсчитывать метрики и функцию потерь в конце каждой эпохи<br>
        Данные для валидации берутся из данных с конца в `x` и `y` **до перемешивания**.


```python
404 - 81
```




    323




```python
%%time
num_epochs = 10

model.fit(X_train, y_train,
          epochs=num_epochs, 
          batch_size=1,
          validation_split=0.2)
```

    Epoch 1/10
    323/323 [==============================] - 1s 3ms/step - loss: 23.7887 - mae: 3.4618 - val_loss: 22.3951 - val_mae: 3.4461
    Epoch 2/10
    323/323 [==============================] - 1s 2ms/step - loss: 18.3039 - mae: 3.0675 - val_loss: 18.8717 - val_mae: 3.1509
    Epoch 3/10
    323/323 [==============================] - 1s 3ms/step - loss: 15.8008 - mae: 2.7966 - val_loss: 19.4543 - val_mae: 3.3434
    Epoch 4/10
    323/323 [==============================] - 1s 3ms/step - loss: 13.8707 - mae: 2.6303 - val_loss: 16.4949 - val_mae: 3.0323
    Epoch 5/10
    323/323 [==============================] - 1s 3ms/step - loss: 12.9570 - mae: 2.4865 - val_loss: 15.5677 - val_mae: 2.8850
    Epoch 6/10
    323/323 [==============================] - 1s 3ms/step - loss: 11.7685 - mae: 2.3840 - val_loss: 16.9575 - val_mae: 2.7447
    Epoch 7/10
    323/323 [==============================] - 1s 2ms/step - loss: 11.1454 - mae: 2.3930 - val_loss: 14.1790 - val_mae: 2.6787
    Epoch 8/10
    323/323 [==============================] - 1s 2ms/step - loss: 10.9632 - mae: 2.2944 - val_loss: 14.8609 - val_mae: 2.7368
    Epoch 9/10
    323/323 [==============================] - 1s 2ms/step - loss: 10.2214 - mae: 2.1759 - val_loss: 18.4467 - val_mae: 3.0628
    Epoch 10/10
    323/323 [==============================] - 1s 2ms/step - loss: 10.0996 - mae: 2.2170 - val_loss: 17.3058 - val_mae: 2.7520
    CPU times: user 8.91 s, sys: 491 ms, total: 9.4 s
    Wall time: 10.5 s
    

#### **validation_data**

> validation_data: Данные, на которых считается функция потерь и метрики в конце каждой эпохи<br>
Модель не будет обучаться на этих данных



```python
%%time
num_epochs = 10

model.fit(X_train, y_train,
          epochs=num_epochs, 
          batch_size=1,
          validation_data=(X_test, y_test))
```

    Epoch 1/10
    404/404 [==============================] - 1s 2ms/step - loss: 10.6234 - mae: 2.2634 - val_loss: 22.9307 - val_mae: 2.9804
    Epoch 2/10
    404/404 [==============================] - 1s 2ms/step - loss: 10.3111 - mae: 2.2410 - val_loss: 22.5022 - val_mae: 3.0728
    Epoch 3/10
    404/404 [==============================] - 1s 2ms/step - loss: 9.8253 - mae: 2.2548 - val_loss: 24.5678 - val_mae: 2.9912
    Epoch 4/10
    404/404 [==============================] - 1s 2ms/step - loss: 9.7436 - mae: 2.2245 - val_loss: 26.1471 - val_mae: 3.1539
    Epoch 5/10
    404/404 [==============================] - 1s 2ms/step - loss: 9.3499 - mae: 2.1764 - val_loss: 22.1797 - val_mae: 2.9892
    Epoch 6/10
    404/404 [==============================] - 1s 2ms/step - loss: 8.9368 - mae: 2.1157 - val_loss: 22.1921 - val_mae: 2.8944
    Epoch 7/10
    404/404 [==============================] - 1s 2ms/step - loss: 8.7185 - mae: 2.1304 - val_loss: 23.7712 - val_mae: 2.9776
    Epoch 8/10
    404/404 [==============================] - 1s 2ms/step - loss: 8.8730 - mae: 2.1336 - val_loss: 22.6418 - val_mae: 3.0593
    Epoch 9/10
    404/404 [==============================] - 1s 2ms/step - loss: 8.4429 - mae: 2.0925 - val_loss: 25.5204 - val_mae: 3.1512
    Epoch 10/10
    404/404 [==============================] - 1s 2ms/step - loss: 8.5598 - mae: 2.1031 - val_loss: 20.9785 - val_mae: 2.9012
    CPU times: user 10.4 s, sys: 618 ms, total: 11.1 s
    Wall time: 9.67 s
    

#### **validation_batch_size**

>validation_batch_size: Integer или `None`.<br>
        Количество сэмплов в один валидационный батч<br>
        Если None, то по дефолту равняется `batch_size`.<br>
        Не указывайте `validation_batch_size`, если ваши данные представлены в виде генератора или keras.utils.Sequence (из-за того, что они сами генерируют данные батчами)


```python
X_test.shape
```




    (102, 13)




```python
%%time
num_epochs = 10

model.fit(X_train, y_train,
          epochs=num_epochs, 
          batch_size=1,
          validation_data=(X_test, y_test),
          validation_batch_size=X_test.shape[0])
```

    Epoch 1/10
    404/404 [==============================] - 1s 2ms/step - loss: 7.2319 - mae: 1.9065 - val_loss: 18.4802 - val_mae: 2.6468
    Epoch 2/10
    404/404 [==============================] - 1s 2ms/step - loss: 7.3097 - mae: 1.9392 - val_loss: 19.0097 - val_mae: 2.6813
    Epoch 3/10
    404/404 [==============================] - 1s 2ms/step - loss: 6.9716 - mae: 1.9379 - val_loss: 20.7965 - val_mae: 2.7282
    Epoch 4/10
    404/404 [==============================] - 1s 2ms/step - loss: 7.2285 - mae: 1.9399 - val_loss: 22.9594 - val_mae: 2.9067
    Epoch 5/10
    404/404 [==============================] - 1s 2ms/step - loss: 7.0538 - mae: 1.9161 - val_loss: 18.5269 - val_mae: 2.7269
    Epoch 6/10
    404/404 [==============================] - 2s 4ms/step - loss: 6.9260 - mae: 1.9017 - val_loss: 18.7155 - val_mae: 2.6446
    Epoch 7/10
    404/404 [==============================] - 1s 3ms/step - loss: 6.5769 - mae: 1.8571 - val_loss: 21.4642 - val_mae: 2.8613
    Epoch 8/10
    404/404 [==============================] - 1s 2ms/step - loss: 6.9314 - mae: 1.9247 - val_loss: 20.0303 - val_mae: 2.8935
    Epoch 9/10
    404/404 [==============================] - 1s 2ms/step - loss: 6.5798 - mae: 1.8599 - val_loss: 22.4113 - val_mae: 2.9370
    Epoch 10/10
    404/404 [==============================] - 1s 2ms/step - loss: 6.6844 - mae: 1.8754 - val_loss: 18.5403 - val_mae: 2.7512
    CPU times: user 10.2 s, sys: 628 ms, total: 10.8 s
    Wall time: 20.5 s
    

#### **validation_freq**
>validation_freq: Только нужен, если предоставлены validation data. Integer
        или `collections.abc.Container` (например, list, tuple, и др.).<br>
        Если integer, то столько эпох с обучением пройдет, до одной валидации, например, `validation_freq=2` будет запускать валидацию каждые 2 эпохи.<br>
        Если это Container, то валидация будет запущена в эти эпохи, к примеру,`validation_freq=[1, 2, 10]` запускает валидацию на 1, 2 и 10 эпохах.


```python
%%time
num_epochs = 10

model.fit(X_train, y_train,
          epochs=num_epochs, 
          batch_size=1,
          validation_data=(X_test, y_test),
          validation_batch_size=X_test.shape[0],
          validation_freq=2)
```

    Epoch 1/10
    404/404 [==============================] - 1s 2ms/step - loss: 6.3856 - mae: 1.8088
    Epoch 2/10
    404/404 [==============================] - 1s 2ms/step - loss: 6.5494 - mae: 1.8484 - val_loss: 17.8738 - val_mae: 2.5859
    Epoch 3/10
    404/404 [==============================] - 1s 2ms/step - loss: 6.2152 - mae: 1.8297
    Epoch 4/10
    404/404 [==============================] - 1s 2ms/step - loss: 6.4376 - mae: 1.8509 - val_loss: 21.5753 - val_mae: 2.8450
    Epoch 5/10
    404/404 [==============================] - 1s 2ms/step - loss: 6.2807 - mae: 1.8280
    Epoch 6/10
    404/404 [==============================] - 1s 2ms/step - loss: 6.1780 - mae: 1.8152 - val_loss: 17.3600 - val_mae: 2.5641
    Epoch 7/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.8426 - mae: 1.7742
    Epoch 8/10
    404/404 [==============================] - 1s 2ms/step - loss: 6.1193 - mae: 1.8214 - val_loss: 18.8972 - val_mae: 2.8485
    Epoch 9/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.8522 - mae: 1.7629
    Epoch 10/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.9085 - mae: 1.7709 - val_loss: 17.7636 - val_mae: 2.7245
    CPU times: user 8.87 s, sys: 541 ms, total: 9.41 s
    Wall time: 8.17 s
    


```python
%%time
num_epochs = 10

model.fit(X_train, y_train,
          epochs=num_epochs, 
          batch_size=1,
          validation_data=(X_test, y_test),
          validation_batch_size=X_test.shape[0],
          validation_freq=[1, 5, 10])
```

    Epoch 1/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.6817 - mae: 1.7411 - val_loss: 16.2072 - val_mae: 2.5336
    Epoch 2/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.8160 - mae: 1.7626
    Epoch 3/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.4958 - mae: 1.7213
    Epoch 4/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.7843 - mae: 1.7613
    Epoch 5/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.5535 - mae: 1.7405 - val_loss: 16.0309 - val_mae: 2.5557
    Epoch 6/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.5476 - mae: 1.7391
    Epoch 7/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.2158 - mae: 1.6938
    Epoch 8/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.5027 - mae: 1.7281
    Epoch 9/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.2354 - mae: 1.6793
    Epoch 10/10
    404/404 [==============================] - 1s 2ms/step - loss: 5.2701 - mae: 1.6782 - val_loss: 16.0769 - val_mae: 2.6569
    CPU times: user 8.83 s, sys: 548 ms, total: 9.37 s
    Wall time: 8.09 s
    

### Обучение нейросети на генераторах

Про Sequence больше [тут](https://www.tensorflow.org/api_docs/python/tf/keras/utils/Sequence)


```python
import numpy as np
from tensorflow.keras.utils import Sequence


class DataGenerator(Sequence):
    def __init__(self, data, labels, batch_size=32):
        self.batch_size = batch_size
        self.data = data
        self.labels = labels
        
    def __len__(self):
        return int(np.round(len(self.data) / self.batch_size))

    def __getitem__(self, index):
        X = self.data[index * self.batch_size : (index+1) * self.batch_size]
        y = self.labels[index * self.batch_size : (index+1) * self.batch_size]

        return X, y
```


```python
train_datagen = DataGenerator(X_train, y_train)
test_datagen = DataGenerator(X_test, y_test)
```


```python
len(train_datagen)
```




    13




```python
len(test_datagen)
```




    3




```python
for X, y in train_datagen:
    print(X.shape)
    print(y.shape)
    break
```

    (32, 13)
    (32,)
    


```python
404 / 32
```




    12.625




```python
%%time
num_epochs = 10

model.fit(train_datagen,
          epochs=num_epochs, 
          validation_data=test_datagen)
```

    Epoch 1/10
    13/13 [==============================] - 0s 12ms/step - loss: 4.3028 - mae: 1.5017 - val_loss: 10.6313 - val_mae: 2.3160
    Epoch 2/10
    13/13 [==============================] - 0s 6ms/step - loss: 4.2798 - mae: 1.5001 - val_loss: 10.5893 - val_mae: 2.3111
    Epoch 3/10
    13/13 [==============================] - 0s 6ms/step - loss: 4.2795 - mae: 1.5009 - val_loss: 10.5323 - val_mae: 2.3027
    Epoch 4/10
    13/13 [==============================] - 0s 6ms/step - loss: 4.2758 - mae: 1.5027 - val_loss: 10.5573 - val_mae: 2.3057
    Epoch 5/10
    13/13 [==============================] - 0s 6ms/step - loss: 4.2753 - mae: 1.5022 - val_loss: 10.6443 - val_mae: 2.3170
    Epoch 6/10
    13/13 [==============================] - 0s 6ms/step - loss: 4.2678 - mae: 1.4992 - val_loss: 10.6279 - val_mae: 2.3156
    Epoch 7/10
    13/13 [==============================] - 0s 6ms/step - loss: 4.2676 - mae: 1.4984 - val_loss: 10.5898 - val_mae: 2.3081
    Epoch 8/10
    13/13 [==============================] - 0s 6ms/step - loss: 4.2700 - mae: 1.5015 - val_loss: 10.4984 - val_mae: 2.2943
    Epoch 9/10
    13/13 [==============================] - 0s 6ms/step - loss: 4.2610 - mae: 1.4988 - val_loss: 10.5547 - val_mae: 2.3004
    Epoch 10/10
    13/13 [==============================] - 0s 6ms/step - loss: 4.2553 - mae: 1.4952 - val_loss: 10.6195 - val_mae: 2.3104
    CPU times: user 1.21 s, sys: 51.5 ms, total: 1.26 s
    Wall time: 1.41 s
    

### History

Больше [тут](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/History) 


```python
%%time
num_epochs = 10

history = model.fit(train_datagen,
                    epochs=num_epochs, 
                    validation_data=test_datagen)
```

    Epoch 1/10
    13/13 [==============================] - 1s 29ms/step - loss: 567.5922 - mae: 21.9870 - val_loss: 560.5504 - val_mae: 21.9396
    Epoch 2/10
    13/13 [==============================] - 0s 5ms/step - loss: 547.6655 - mae: 21.5061 - val_loss: 539.8265 - val_mae: 21.4437
    Epoch 3/10
    13/13 [==============================] - 0s 6ms/step - loss: 525.2613 - mae: 20.9650 - val_loss: 516.1148 - val_mae: 20.8650
    Epoch 4/10
    13/13 [==============================] - 0s 6ms/step - loss: 499.9608 - mae: 20.3316 - val_loss: 488.0100 - val_mae: 20.1525
    Epoch 5/10
    13/13 [==============================] - 0s 5ms/step - loss: 469.9943 - mae: 19.5615 - val_loss: 454.7536 - val_mae: 19.2809
    Epoch 6/10
    13/13 [==============================] - 0s 5ms/step - loss: 435.0141 - mae: 18.6295 - val_loss: 416.6180 - val_mae: 18.2459
    Epoch 7/10
    13/13 [==============================] - 0s 6ms/step - loss: 394.4474 - mae: 17.5131 - val_loss: 374.2877 - val_mae: 17.1353
    Epoch 8/10
    13/13 [==============================] - 0s 5ms/step - loss: 350.4708 - mae: 16.2909 - val_loss: 326.7027 - val_mae: 15.9008
    Epoch 9/10
    13/13 [==============================] - 0s 5ms/step - loss: 301.7717 - mae: 14.8538 - val_loss: 276.3856 - val_mae: 14.5170
    Epoch 10/10
    13/13 [==============================] - 0s 5ms/step - loss: 250.4051 - mae: 13.2169 - val_loss: 222.0869 - val_mae: 12.8515
    CPU times: user 1.63 s, sys: 59.5 ms, total: 1.69 s
    Wall time: 1.93 s
    


```python
history
```




    <keras.callbacks.History at 0x7f0fd4e5e710>




```python
history.history
```




    {'loss': [567.5922241210938,
      547.6654663085938,
      525.2612915039062,
      499.96075439453125,
      469.99432373046875,
      435.0141296386719,
      394.4473571777344,
      350.4708251953125,
      301.77166748046875,
      250.4050750732422],
     'mae': [21.987014770507812,
      21.506057739257812,
      20.965007781982422,
      20.331594467163086,
      19.561542510986328,
      18.629533767700195,
      17.5130615234375,
      16.290931701660156,
      14.853785514831543,
      13.216885566711426],
     'val_loss': [560.5504150390625,
      539.8264770507812,
      516.1148071289062,
      488.010009765625,
      454.7536315917969,
      416.6179504394531,
      374.2876892089844,
      326.7027282714844,
      276.3855895996094,
      222.0868682861328],
     'val_mae': [21.939577102661133,
      21.443740844726562,
      20.864990234375,
      20.152509689331055,
      19.28089714050293,
      18.245872497558594,
      17.135271072387695,
      15.900837898254395,
      14.517024993896484,
      12.851500511169434]}




```python
import matplotlib.pyplot as plt

plt.plot(history.history['loss'], label='train loss')
plt.plot(history.history['val_loss'], label='test loss')
plt.legend();
```


    
![png](NN_Boston_Keras_files/NN_Boston_Keras_53_0.png)
    



```python
plt.plot(history.history['mae'], label='train mae')
plt.plot(history.history['val_mae'], label='test mae')
plt.legend();
```


    
![png](NN_Boston_Keras_files/NN_Boston_Keras_54_0.png)
    


History - Callback, про них поговорим в следующих занятиях

## Summary


Вот мы и разобрались в деталях, что можно передавать в метод fit в Keras.

**batch size**

<img src='https://miro.medium.com/max/1400/1*jig1e2BDLN7KPmgOcirflQ.jpeg' width=500>

___________________

**steps_per_epoch**

<img src='https://media4.giphy.com/media/FMAUcALBqjajXV0qej/giphy.gif?cid=ecf05e47xa7bnffmbiq333qb36btdm8a262ww7xbgyu6ti2t&rid=giphy.gif&ct=g'>

___________________

**validation_split**

<img src='https://media2.giphy.com/media/D2cY6SySBjyBuOzsYq/giphy.gif?cid=ecf05e47triw0wubv3aubehd4enhcus9dlxr996xnss8uffg&rid=giphy.gif&ct=g'>

___________________

**validation_data** и **validation_batch_size**

<img src='https://media0.giphy.com/media/yOhgNO9YsmdsQ/giphy.gif?cid=ecf05e473qx1a618m4t3o922udc1mcfmqtfwdyi4mrqg2gcf&rid=giphy.gif&ct=g'>

___________________

**validation_freq**

<img src='http://memecrunch.com/meme/BH6DD/whats-the-frequency/image.jpg'>

___________________

**Обучение нейросети на генераторах**

<img src='https://realprodom.com/images/cat-proger.gif'>

___________________

**Callback History**

<img src='https://drive.google.com/uc?id=1BxyC2qbNFgavqzCDQPj_sSuHYrjQHfNy' width=400>


**Муррр** ♥
