# Прогноз цены акций  
По материал видео  
ИСПОЛЬЗУЕМ ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ (нейронные сети) ДЛЯ ПРЕДСКАЗАНИЯ ЦЕНЫ АКЦИЙ!  
Alexander Ershov  
https://www.youtube.com/watch?v=LI94ZkjE_w4

## Загрузка данных


```python
# Current stable release for CPU and GPU
#!pip install tensorflow

# Установка Yfinance
# !pip install yfinance
```


```python
import pandas as pd
from datetime import timedelta
import numpy as np
import tensorflow as tf
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import mean_absolute_error
from matplotlib import pyplot as plt

import yfinance as yf
```


```python
df: pd = yf.download('AAPL', '2014-09-01')  # Загрузка данных с YFinance
df = df.rename_axis('Date').reset_index()  # Индекс "Date" преобразуем в колонку, а сам индекс переиндексируем
print(type(df))
print(df.to_string(max_rows=6, max_cols=20))  # Просмотр DF
```

    [*********************100%***********************]  1 of 1 completed
    <class 'pandas.core.frame.DataFrame'>
               Date        Open        High         Low       Close   Adj Close     Volume
    0    2014-09-02   25.764999   25.934999   25.680000   25.825001   23.145872  214256000
    1    2014-09-03   25.775000   25.799999   24.645000   24.735001   22.168955  501684000
    2    2014-09-04   24.712500   25.022499   24.447500   24.530001   21.985214  342872000
    ...         ...         ...         ...         ...         ...         ...        ...
    2100 2023-01-04  126.889999  128.660004  125.080002  126.360001  126.360001   89113600
    2101 2023-01-05  127.129997  127.769997  124.760002  125.019997  125.019997   80962700
    2102 2023-01-06  126.010002  130.289993  124.889999  129.619995  129.619995   87686600
    


```python
df.shape  # Показывает количество строк и столбцов
```




    (2103, 7)




```python
# Период за который есть данные
df["Date"].min(), df["Date"].max()
```




    (Timestamp('2014-09-02 00:00:00'), Timestamp('2023-01-06 00:00:00'))




```python
fig, ax = plt.subplots(1, 2, figsize=(15, 5))

# Строим график изменения цены акций
ax[0].plot(df['Date'], df['Open'])
ax[0].set_title('График по Open')

# Проверяем есть ли похожесть графиков
ax[1].plot(df['Date'], df['High'])
ax[1].set_title('График по High')
```




    Text(0.5, 1.0, 'График по High')




    
![png](NN_%D0%9F%D1%80%D0%BE%D0%B3%D0%BD%D0%BE%D0%B7_%D1%86%D0%B5%D0%BD%D1%8B_%D0%B0%D0%BA%D1%86%D0%B8%D0%B9_files/NN_%D0%9F%D1%80%D0%BE%D0%B3%D0%BD%D0%BE%D0%B7_%D1%86%D0%B5%D0%BD%D1%8B_%D0%B0%D0%BA%D1%86%D0%B8%D0%B9_7_1.png)
    



```python
# Проверяем корреляцию (зачем, не понятно)
df[["Open", "Close", "Low", "High"]].corr()
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
      <th>Open</th>
      <th>Close</th>
      <th>Low</th>
      <th>High</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Open</th>
      <td>1.000000</td>
      <td>0.999581</td>
      <td>0.999802</td>
      <td>0.999844</td>
    </tr>
    <tr>
      <th>Close</th>
      <td>0.999581</td>
      <td>1.000000</td>
      <td>0.999811</td>
      <td>0.999803</td>
    </tr>
    <tr>
      <th>Low</th>
      <td>0.999802</td>
      <td>0.999811</td>
      <td>1.000000</td>
      <td>0.999774</td>
    </tr>
    <tr>
      <th>High</th>
      <td>0.999844</td>
      <td>0.999803</td>
      <td>0.999774</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



## Подготовка данных


```python
# Для анализа возьмем исторические данные за 6 лет
df_6_yr = df[df["Date"] > df["Date"].max() - timedelta(days=365 * 6)]
```


```python
df_6_yr.shape  # Строки, колонки
```




    (1510, 7)




```python
# Минимальная и максимальная дата
df_6_yr["Date"].min(), df_6_yr["Date"].max()
```




    (Timestamp('2017-01-09 00:00:00'), Timestamp('2023-01-06 00:00:00'))




```python
# Разбиение данных на тренировочную часть и валидационную
train_size = int(df_6_yr.shape[0] * 0.8)
train_df = df_6_yr.iloc[:train_size]
val_df = df_6_yr.iloc[train_size:]
```


```python
# Размеры тренировочного и валидационного DF
train_df.shape, val_df.shape
```




    ((1208, 7), (302, 7))




```python
# Проверка дат в каждом из DF
train_df["Date"].min(), train_df["Date"].max(), val_df["Date"].min(), val_df["Date"].max()
```




    (Timestamp('2017-01-09 00:00:00'),
     Timestamp('2021-10-25 00:00:00'),
     Timestamp('2021-10-26 00:00:00'),
     Timestamp('2023-01-06 00:00:00'))



### Создание датасета для временного ряда


```python
# scaler используется для нормализации фичей
scaler = StandardScaler()
scaler.fit(train_df[["Close"]])

def make_dataset(
    df,  # DF данные будут использоваться для создания датасета
    window_size, # Количество элементов временного ряда, которые будут использоваться для предсказания следующего элемента
    batch_size,  # Количество элементов в батче для обучения
    use_scaler=True,  # Использовать scaler для нормализации
    shuffle=True  # Делать ли смешивание элементов в датасете
    ):
  features = df[["Close"]].iloc[:-window_size]  # Создание DF фичей

  if use_scaler:
    features = scaler.transform(features)  # Нормализация фичей

  data = np.array(features, dtype=np.float32)  # Приведение данных к нужному типу

  # Создание датасета с временным рядом на TensorFlow
  ds = tf.keras.preprocessing.timeseries_dataset_from_array(
      data=data,  # Фичи в виде многомерного массива
      targets=df["Close"].iloc[window_size:],  # Лейблы
      sequence_length=window_size,  # Длина последовательности
      sequence_stride=1,  # Параметр на сколько нужно сдвигать сэмпл
      shuffle=shuffle,
      batch_size=batch_size)
  return ds
```


```python
# Создание датасета для примера
example_ds = make_dataset(df=train_df, window_size=3, batch_size=2, use_scaler=False, shuffle=False)
```


```python
# Элементы датасета можно получать по одному, если вызвать next
example_feature, example_label = next(example_ds.as_numpy_iterator())
```


```python
example_feature.shape
```




    (2, 3, 1)




```python
example_label.shape
```




    (2,)




```python
train_df["Close"].iloc[:6]
```




    593    29.747499
    594    29.777500
    595    29.937500
    596    29.812500
    597    29.760000
    598    30.000000
    Name: Close, dtype: float64




```python
# Посмотрим первый сэмпл из бача
print(example_feature[0])
print(example_label[0])
```

    [[29.7475]
     [29.7775]
     [29.9375]]
    29.8125
    


```python
# Посмотрим второй сэмпл из бача
print(example_feature[1])
print(example_label[1])
```

    [[29.7775]
     [29.9375]
     [29.8125]]
    29.760000228881836
    

### Создание настоящего датасета для тренировочного и валидационного дата


```python
window_size = 10
batch_size = 8
train_ds = make_dataset(df=train_df, window_size=window_size, batch_size=batch_size, use_scaler=True, shuffle=True)
val_ds = make_dataset(df=val_df, window_size=window_size, batch_size=batch_size, use_scaler=True, shuffle=True)
```

## Создание и обучение модели


```python
lstm_model = tf.keras.models.Sequential([
    tf.keras.layers.LSTM(32, return_sequences=False),
    tf.keras.layers.Dense(1)
])
```


```python
# Компиляция и обучение модели
def compile_and_fit(model, train_ds, val_ds, num_epochs: int = 20):
  model.compile(
        loss=tf.losses.MeanSquaredError(),
        optimizer=tf.optimizers.Adam(),
        metrics=[tf.metrics.MeanAbsoluteError()]
      )
  history = model.fit(
      train_ds, 
      epochs=num_epochs,
      validation_data=val_ds,
      verbose=0
      )
  return history
```


```python
history =  compile_and_fit(lstm_model, train_ds, val_ds, num_epochs=100)
```


```python
fig, ax = plt.subplots(1, 2, figsize=(15, 5))

ax[0].plot(history.history['mean_absolute_error'])
ax[0].set_title('mean_absolute_error')

ax[1].plot(history.history['val_mean_absolute_error'])
ax[1].set_title('val_mean_absolute_error')
```




    Text(0.5, 1.0, 'val_mean_absolute_error')




    
![png](NN_%D0%9F%D1%80%D0%BE%D0%B3%D0%BD%D0%BE%D0%B7_%D1%86%D0%B5%D0%BD%D1%8B_%D0%B0%D0%BA%D1%86%D0%B8%D0%B9_files/NN_%D0%9F%D1%80%D0%BE%D0%B3%D0%BD%D0%BE%D0%B7_%D1%86%D0%B5%D0%BD%D1%8B_%D0%B0%D0%BA%D1%86%D0%B8%D0%B9_31_1.png)
    



```python
lstm_model.evaluate(train_ds)
```

    149/149 [==============================] - 0s 1ms/step - loss: 2.9821 - mean_absolute_error: 1.1069
    




    [2.9821152687072754, 1.1068565845489502]




```python
lstm_model.evaluate(val_ds)
```

    36/36 [==============================] - 0s 2ms/step - loss: 139.2479 - mean_absolute_error: 9.0816
    




    [139.24789428710938, 9.08163833618164]




```python
lstm_model = tf.keras.models.Sequential([
    tf.keras.layers.LSTM(32, return_sequences=False),
    tf.keras.layers.Dense(1)
])

history =  compile_and_fit(lstm_model, train_ds, val_ds, num_epochs=500)
```


```python
lstm_model.evaluate(train_ds)
```

    149/149 [==============================] - 0s 1ms/step - loss: 2.4417 - mean_absolute_error: 1.0877
    




    [2.441659688949585, 1.0876860618591309]




```python
lstm_model.evaluate(val_ds)
```

    36/36 [==============================] - 0s 2ms/step - loss: 114.6875 - mean_absolute_error: 7.6499
    




    [114.68746185302734, 7.6499223709106445]




```python
fig, ax = plt.subplots(1, 2, figsize=(15, 5))

ax[0].plot(history.history['mean_absolute_error'])
ax[0].set_title('mean_absolute_error')

ax[1].plot(history.history['val_mean_absolute_error'])
ax[1].set_title('val_mean_absolute_error')
```




    Text(0.5, 1.0, 'val_mean_absolute_error')




    
![png](NN_%D0%9F%D1%80%D0%BE%D0%B3%D0%BD%D0%BE%D0%B7_%D1%86%D0%B5%D0%BD%D1%8B_%D0%B0%D0%BA%D1%86%D0%B8%D0%B9_files/NN_%D0%9F%D1%80%D0%BE%D0%B3%D0%BD%D0%BE%D0%B7_%D1%86%D0%B5%D0%BD%D1%8B_%D0%B0%D0%BA%D1%86%D0%B8%D0%B9_37_1.png)
    


## Обучение модели с Dropout от переобучения


```python
lstm_model = tf.keras.models.Sequential([
    tf.keras.layers.LSTM(32, return_sequences=False),
    tf.keras.layers.Dropout(0.2),
    tf.keras.layers.Dense(1)
])

history =  compile_and_fit(lstm_model, train_ds, val_ds, num_epochs=500)
```


```python
lstm_model.evaluate(train_ds)
```

    149/149 [==============================] - 0s 1ms/step - loss: 3.3754 - mean_absolute_error: 1.2123
    




    [3.3754022121429443, 1.2123301029205322]




```python
lstm_model.evaluate(val_ds)
```

    36/36 [==============================] - 0s 2ms/step - loss: 48.9928 - mean_absolute_error: 5.4315
    




    [48.9927864074707, 5.431506156921387]




```python
fig, ax = plt.subplots(1, 2, figsize=(15, 5))

ax[0].plot(history.history['mean_absolute_error'])
ax[0].set_title('mean_absolute_error')

ax[1].plot(history.history['val_mean_absolute_error'])
ax[1].set_title('val_mean_absolute_error')
```




    Text(0.5, 1.0, 'val_mean_absolute_error')




    
![png](NN_%D0%9F%D1%80%D0%BE%D0%B3%D0%BD%D0%BE%D0%B7_%D1%86%D0%B5%D0%BD%D1%8B_%D0%B0%D0%BA%D1%86%D0%B8%D0%B9_files/NN_%D0%9F%D1%80%D0%BE%D0%B3%D0%BD%D0%BE%D0%B7_%D1%86%D0%B5%D0%BD%D1%8B_%D0%B0%D0%BA%D1%86%D0%B8%D0%B9_42_1.png)
    



```python

```
