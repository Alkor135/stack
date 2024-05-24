# Оптимизация гиперпараметров нейросети с помощью [Keras Tuner](https://github.com/keras-team/keras-tuner)

Учебный курс "[Программирование нейросетей на Python](https://www.asozykin.ru/courses/nnpython)".

Чтобы запускать и редактировать код, сохраните копию этого ноутбука себе (File->Save a copy in Drive...). Свою копию вы сможете изменять и запускать.

Не забудьте подключить GPU, чтобы сеть обучалась быстрее (Runtime -> Change Runtime Type -> Hardware Accelerator -> GPU).



## Гиперпараметры обучения нейронной сети

- Количество слоев нейронной сети
- Количество нейронов в каждом слое
- Функции активации, которые используются в слоях
- Тип оптимизатора при обучении нейронной сети
- Количество эпох обучения

## Установка Keras Tuner


```python
pip install -U keras-tuner
```

    Requirement already satisfied: keras-tuner in c:\anaconda3\envs\catboost_keras\lib\site-packages (1.2.0)
    Requirement already satisfied: requests in c:\anaconda3\envs\catboost_keras\lib\site-packages (from keras-tuner) (2.28.1)
    Requirement already satisfied: tensorflow>=2.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from keras-tuner) (2.10.0)
    Requirement already satisfied: packaging in c:\users\alkor\appdata\roaming\python\python310\site-packages (from keras-tuner) (22.0)
    Requirement already satisfied: kt-legacy in c:\anaconda3\envs\catboost_keras\lib\site-packages (from keras-tuner) (1.0.4)
    Requirement already satisfied: ipython in c:\users\alkor\appdata\roaming\python\python310\site-packages (from keras-tuner) (8.7.0)
    Requirement already satisfied: keras-preprocessing>=1.1.1 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (1.1.2)
    Requirement already satisfied: setuptools in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (65.6.3)
    Requirement already satisfied: tensorflow-io-gcs-filesystem>=0.23.1 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (0.30.0)
    Requirement already satisfied: libclang>=13.0.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (15.0.6.1)
    Requirement already satisfied: flatbuffers>=2.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (2.0)
    Requirement already satisfied: astunparse>=1.6.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (1.6.3)
    Requirement already satisfied: gast<=0.4.0,>=0.2.1 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (0.4.0)
    Requirement already satisfied: tensorboard<2.11,>=2.10 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (2.10.0)
    Requirement already satisfied: typing-extensions>=3.6.6 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (4.4.0)
    Requirement already satisfied: keras<2.11,>=2.10.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (2.10.0)
    Requirement already satisfied: protobuf<3.20,>=3.9.2 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (3.19.6)
    Requirement already satisfied: absl-py>=1.0.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (1.3.0)
    Requirement already satisfied: numpy>=1.20 in c:\users\alkor\appdata\roaming\python\python310\site-packages (from tensorflow>=2.0->keras-tuner) (1.24.1)
    Requirement already satisfied: h5py>=2.9.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (3.7.0)
    Requirement already satisfied: wrapt>=1.11.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (1.14.1)
    Requirement already satisfied: termcolor>=1.1.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (2.1.0)
    Requirement already satisfied: google-pasta>=0.1.1 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (0.2.0)
    Requirement already satisfied: tensorflow-estimator<2.11,>=2.10.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (2.10.0)
    Requirement already satisfied: six>=1.12.0 in c:\users\alkor\appdata\roaming\python\python310\site-packages (from tensorflow>=2.0->keras-tuner) (1.16.0)
    Requirement already satisfied: grpcio<2.0,>=1.24.3 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (1.42.0)
    Requirement already satisfied: opt-einsum>=2.3.2 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorflow>=2.0->keras-tuner) (3.3.0)
    Requirement already satisfied: pickleshare in c:\users\alkor\appdata\roaming\python\python310\site-packages (from ipython->keras-tuner) (0.7.5)
    Requirement already satisfied: prompt-toolkit<3.1.0,>=3.0.11 in c:\users\alkor\appdata\roaming\python\python310\site-packages (from ipython->keras-tuner) (3.0.36)
    Requirement already satisfied: backcall in c:\users\alkor\appdata\roaming\python\python310\site-packages (from ipython->keras-tuner) (0.2.0)
    Requirement already satisfied: jedi>=0.16 in c:\users\alkor\appdata\roaming\python\python310\site-packages (from ipython->keras-tuner) (0.18.2)
    Requirement already satisfied: decorator in c:\users\alkor\appdata\roaming\python\python310\site-packages (from ipython->keras-tuner) (5.1.1)
    Requirement already satisfied: stack-data in c:\users\alkor\appdata\roaming\python\python310\site-packages (from ipython->keras-tuner) (0.6.2)
    Requirement already satisfied: traitlets>=5 in c:\users\alkor\appdata\roaming\python\python310\site-packages (from ipython->keras-tuner) (5.8.0)
    Requirement already satisfied: pygments>=2.4.0 in c:\users\alkor\appdata\roaming\python\python310\site-packages (from ipython->keras-tuner) (2.13.0)
    Requirement already satisfied: colorama in c:\users\alkor\appdata\roaming\python\python310\site-packages (from ipython->keras-tuner) (0.4.6)
    Requirement already satisfied: matplotlib-inline in c:\users\alkor\appdata\roaming\python\python310\site-packages (from ipython->keras-tuner) (0.1.6)
    Requirement already satisfied: charset-normalizer<3,>=2 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from requests->keras-tuner) (2.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from requests->keras-tuner) (2022.12.7)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from requests->keras-tuner) (1.26.13)
    Requirement already satisfied: idna<4,>=2.5 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from requests->keras-tuner) (3.4)
    Requirement already satisfied: wheel<1.0,>=0.23.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from astunparse>=1.6.0->tensorflow>=2.0->keras-tuner) (0.37.1)
    Requirement already satisfied: parso<0.9.0,>=0.8.0 in c:\users\alkor\appdata\roaming\python\python310\site-packages (from jedi>=0.16->ipython->keras-tuner) (0.8.3)
    Requirement already satisfied: wcwidth in c:\users\alkor\appdata\roaming\python\python310\site-packages (from prompt-toolkit<3.1.0,>=3.0.11->ipython->keras-tuner) (0.2.5)
    Requirement already satisfied: google-auth-oauthlib<0.5,>=0.4.1 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (0.4.4)
    Requirement already satisfied: markdown>=2.6.8 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (3.4.1)
    Requirement already satisfied: google-auth<3,>=1.6.3 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (2.6.0)
    Requirement already satisfied: tensorboard-plugin-wit>=1.6.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (1.8.1)
    Requirement already satisfied: werkzeug>=1.0.1 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (2.2.2)
    Requirement already satisfied: tensorboard-data-server<0.7.0,>=0.6.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (0.6.1)
    Requirement already satisfied: pure-eval in c:\users\alkor\appdata\roaming\python\python310\site-packages (from stack-data->ipython->keras-tuner) (0.2.2)
    Requirement already satisfied: asttokens>=2.1.0 in c:\users\alkor\appdata\roaming\python\python310\site-packages (from stack-data->ipython->keras-tuner) (2.2.1)
    Requirement already satisfied: executing>=1.2.0 in c:\users\alkor\appdata\roaming\python\python310\site-packages (from stack-data->ipython->keras-tuner) (1.2.0)
    Requirement already satisfied: pyasn1-modules>=0.2.1 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from google-auth<3,>=1.6.3->tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (0.2.8)
    Requirement already satisfied: rsa<5,>=3.1.4 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from google-auth<3,>=1.6.3->tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (4.7.2)
    Requirement already satisfied: cachetools<6.0,>=2.0.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from google-auth<3,>=1.6.3->tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (4.2.2)
    Requirement already satisfied: requests-oauthlib>=0.7.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from google-auth-oauthlib<0.5,>=0.4.1->tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (1.3.0)
    Requirement already satisfied: MarkupSafe>=2.1.1 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from werkzeug>=1.0.1->tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (2.1.1)
    Requirement already satisfied: pyasn1<0.5.0,>=0.4.6 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from pyasn1-modules>=0.2.1->google-auth<3,>=1.6.3->tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (0.4.8)
    Requirement already satisfied: oauthlib>=3.0.0 in c:\anaconda3\envs\catboost_keras\lib\site-packages (from requests-oauthlib>=0.7.0->google-auth-oauthlib<0.5,>=0.4.1->tensorboard<2.11,>=2.10->tensorflow>=2.0->keras-tuner) (3.2.1)
    Note: you may need to restart the kernel to use updated packages.
    

## Подключаем нужные пакеты


```python
%tensorflow_version 2.x
from keras.datasets import fashion_mnist
from keras.models import Sequential
from keras.layers import Dense
from keras import utils
from google.colab import files
from kerastuner.tuners import RandomSearch, Hyperband, BayesianOptimization
import numpy as np
```

    UsageError: Line magic function `%tensorflow_version` not found.
    

## Подготовка данных для обучения сети


```python
(x_train, y_train), (x_test, y_test) = fashion_mnist.load_data()
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Cell In[8], line 1
    ----> 1 (x_train, y_train), (x_test, y_test) = fashion_mnist.load_data()
    

    NameError: name 'fashion_mnist' is not defined



```python
x_train = x_train.reshape(60000, 784)
x_test = x_test.reshape(10000, 784)
x_train = x_train / 255 
x_test = x_test / 255 
y_train = utils.to_categorical(y_train, 10)
y_test = utils.to_categorical(y_test, 10)
```

## Задаем функцию создания нейронной сети


```python
def build_model(hp):
    model = Sequential()
    activation_choice = hp.Choice('activation', values=['relu', 'sigmoid', 'tanh', 'elu', 'selu'])    
    model.add(Dense(units=hp.Int('units_input',    # Полносвязный слой с разным количеством нейронов
                                   min_value=512,    # минимальное количество нейронов - 128
                                   max_value=1024,   # максимальное количество - 1024
                                   step=32),
                    input_dim=784,
                    activation=activation_choice))
    model.add(Dense(units=hp.Int('units_hidden',        
                                   min_value=128,   
                                   max_value=600,   
                                   step=32),
                    activation=activation_choice))   
    model.add(Dense(10, activation='softmax'))
    model.compile(
        optimizer=hp.Choice('optimizer', values=['adam','rmsprop','SGD']),
        loss='categorical_crossentropy',
        metrics=['accuracy'])
    return model
```

## Создаем tuner

Доступные типы тюнеров: 
- RandomSearch - случайный поиск.
- Hyperband - алгоритм оптимизации на основе многорукого бандита, Li, Lisha, and Kevin Jamieson. ["Hyperband: A Novel Bandit-Based Approach to Hyperparameter Optimization."Journal of Machine Learning Research 18 (2018): 1-52](http://jmlr.org/papers/v18/16-558.html).
- BayesianOptimization - [байесовская оптимизация](https://en.wikipedia.org/wiki/Bayesian_optimization).


```python
tuner = RandomSearch(
    build_model,                 # функция создания модели
    objective='val_accuracy',    # метрика, которую нужно оптимизировать - 
                                 # доля правильных ответов на проверочном наборе данных
    max_trials=80,               # максимальное количество запусков обучения 
    directory='test_directory'   # каталог, куда сохраняются обученные сети  
    )
```

Пространство поиска


```python
tuner.search_space_summary()
```

    Search space summary
    Default search space size: 4
    activation (Choice)
    {'default': 'relu', 'conditions': [], 'values': ['relu', 'sigmoid', 'tanh', 'elu', 'selu'], 'ordered': False}
    units_input (Int)
    {'default': None, 'conditions': [], 'min_value': 512, 'max_value': 1024, 'step': 32, 'sampling': 'linear'}
    units_hidden (Int)
    {'default': None, 'conditions': [], 'min_value': 128, 'max_value': 600, 'step': 32, 'sampling': 'linear'}
    optimizer (Choice)
    {'default': 'adam', 'conditions': [], 'values': ['adam', 'rmsprop', 'SGD'], 'ordered': False}
    

Подбор гиперпараметров


```python
tuner.search(x_train,                  # Данные для обучения
             y_train,                  # Правильные ответы
             batch_size=256,           # Размер мини-выборки
             epochs=20,                # Количество эпох обучения 
             validation_split=0.2,     # Часть данных, которая будет использоваться для проверки
             )
```

    Trial 80 Complete [00h 00m 16s]
    val_accuracy: 0.8482499718666077
    
    Best val_accuracy So Far: 0.9000833630561829
    Total elapsed time: 00h 26m 18s
    

## Выбираем лучшую модель


```python
tuner.results_summary()
```

    Results summary
    Results in test_directory/untitled_project
    Showing 10 best trials
    <keras_tuner.engine.objective.Objective object at 0x7fa5c7956280>
    Trial summary
    Hyperparameters:
    activation: relu
    units_input: 896
    units_hidden: 448
    optimizer: adam
    Score: 0.9000833630561829
    Trial summary
    Hyperparameters:
    activation: tanh
    units_input: 864
    units_hidden: 256
    optimizer: adam
    Score: 0.8993333578109741
    Trial summary
    Hyperparameters:
    activation: relu
    units_input: 512
    units_hidden: 384
    optimizer: adam
    Score: 0.8985000252723694
    Trial summary
    Hyperparameters:
    activation: relu
    units_input: 896
    units_hidden: 160
    optimizer: adam
    Score: 0.8985000252723694
    Trial summary
    Hyperparameters:
    activation: tanh
    units_input: 576
    units_hidden: 448
    optimizer: adam
    Score: 0.8984166383743286
    Trial summary
    Hyperparameters:
    activation: relu
    units_input: 704
    units_hidden: 576
    optimizer: adam
    Score: 0.8983333110809326
    Trial summary
    Hyperparameters:
    activation: relu
    units_input: 640
    units_hidden: 512
    optimizer: adam
    Score: 0.8982499837875366
    Trial summary
    Hyperparameters:
    activation: tanh
    units_input: 704
    units_hidden: 416
    optimizer: adam
    Score: 0.8980000019073486
    Trial summary
    Hyperparameters:
    activation: relu
    units_input: 704
    units_hidden: 192
    optimizer: adam
    Score: 0.8977500200271606
    Trial summary
    Hyperparameters:
    activation: relu
    units_input: 960
    units_hidden: 288
    optimizer: adam
    Score: 0.8975833058357239
    

Получаем три лучших модели


```python
models = tuner.get_best_models(num_models=3)
```

Оцениваем качество модели на тестовых данных


```python
for model in models:
  model.summary()
  model.evaluate(x_test, y_test)
  print() 
```

    Model: "sequential"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     dense (Dense)               (None, 896)               703360    
                                                                     
     dense_1 (Dense)             (None, 448)               401856    
                                                                     
     dense_2 (Dense)             (None, 10)                4490      
                                                                     
    =================================================================
    Total params: 1,109,706
    Trainable params: 1,109,706
    Non-trainable params: 0
    _________________________________________________________________
    313/313 [==============================] - 1s 2ms/step - loss: 0.3435 - accuracy: 0.8917
    
    Model: "sequential"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     dense (Dense)               (None, 864)               678240    
                                                                     
     dense_1 (Dense)             (None, 256)               221440    
                                                                     
     dense_2 (Dense)             (None, 10)                2570      
                                                                     
    =================================================================
    Total params: 902,250
    Trainable params: 902,250
    Non-trainable params: 0
    _________________________________________________________________
    313/313 [==============================] - 1s 2ms/step - loss: 0.3201 - accuracy: 0.8890
    
    Model: "sequential"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     dense (Dense)               (None, 512)               401920    
                                                                     
     dense_1 (Dense)             (None, 384)               196992    
                                                                     
     dense_2 (Dense)             (None, 10)                3850      
                                                                     
    =================================================================
    Total params: 602,762
    Trainable params: 602,762
    Non-trainable params: 0
    _________________________________________________________________
    313/313 [==============================] - 1s 2ms/step - loss: 0.3359 - accuracy: 0.8890
    
    
