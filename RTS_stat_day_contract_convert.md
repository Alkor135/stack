# Статистика по индексу RTS на основе дней контракта.

Не работает т.к. в БД нет SECID контракта.

Подключение гугл диска


```python
from google.colab import drive
drive.mount('/content/drive')
```

    Mounted at /content/drive
    

Подключение к БД.


```python
import pandas as pd
import numpy as np
import sqlite3
import datetime
```


```python
connection = sqlite3.connect(r'/content/drive/MyDrive/data_quote_db/RTS_futures_minute_to_day_converter.db', check_same_thread=True)  # Создание соединения с БД
```

Загрузка данных в таблицу pandas.


```python
with connection:
  df = pd.read_sql('SELECT * FROM Day', connection)  # Загрузка данных из БД

print(df.to_string(max_rows=6, max_cols=25))  # Проверка того, что загрузилось
```

           TRADEDATE      OPEN       LOW      HIGH     CLOSE  VOLUME
    0     2015-01-05   78450.0   72470.0   78450.0   73100.0  375214
    1     2015-01-06   73230.0   71000.0   74900.0   72760.0  331659
    2     2015-01-08   72800.0   72800.0   81380.0   80190.0  520335
    ...          ...       ...       ...       ...       ...     ...
    2239  2023-12-12  105020.0  104240.0  106540.0  105710.0   89329
    2240  2023-12-13  105720.0  105150.0  107100.0  106960.0   73140
    2241  2023-12-14  106990.0  103910.0  107490.0  104100.0   95264
    

Подготовка DF


```python
# df = df.drop(['OPENPOSITION', 'SHORTNAME', 'LSTTRADE'], axis=1)
df['TRADEDATE'] = pd.to_datetime(df['TRADEDATE'])  # Смена типа
df = df.dropna().reset_index(drop=True)  # Удаление NaN
df = df.sort_values(by='TRADEDATE', ascending=False)  # Сортировка по убыванию
```

Создание и заполнение колонки с торговым днем месяца по убыванию.


```python
df['Work_day'] = np.nan  # Создание колонки и заполнение NaN
contr_cur = 0
n = 1
for i in range(0, len(df)):
  if i == 0:
    contr_prev = df.iloc[0, df.columns.get_loc('SECID')]
  contr_cur = df.iloc[i, df.columns.get_loc('SECID')]
  if contr_cur != contr_prev:
    n = 1

  df.iloc[i, df.columns.get_loc('Work_day')] = n
  n += 1
  contr_prev = contr_cur

df[['Work_day']] = df[['Work_day']].astype(int)
df = df.sort_values(by='TRADEDATE', ascending=True)  # Сортировка по возрастанию
print(df.to_string(max_rows=6, max_cols=25))  # Проверка
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    /usr/local/lib/python3.10/dist-packages/pandas/core/indexes/base.py in get_loc(self, key, method, tolerance)
       3801             try:
    -> 3802                 return self._engine.get_loc(casted_key)
       3803             except KeyError as err:
    

    /usr/local/lib/python3.10/dist-packages/pandas/_libs/index.pyx in pandas._libs.index.IndexEngine.get_loc()
    

    /usr/local/lib/python3.10/dist-packages/pandas/_libs/index.pyx in pandas._libs.index.IndexEngine.get_loc()
    

    pandas/_libs/hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()
    

    pandas/_libs/hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()
    

    KeyError: 'SECID'

    
    The above exception was the direct cause of the following exception:
    

    KeyError                                  Traceback (most recent call last)

    <ipython-input-6-97b8c48cfd62> in <cell line: 4>()
          4 for i in range(0, len(df)):
          5   if i == 0:
    ----> 6     contr_prev = df.iloc[0, df.columns.get_loc('SECID')]
          7   contr_cur = df.iloc[i, df.columns.get_loc('SECID')]
          8   if contr_cur != contr_prev:
    

    /usr/local/lib/python3.10/dist-packages/pandas/core/indexes/base.py in get_loc(self, key, method, tolerance)
       3802                 return self._engine.get_loc(casted_key)
       3803             except KeyError as err:
    -> 3804                 raise KeyError(key) from err
       3805             except TypeError:
       3806                 # If we have a listlike key, _check_indexing_error will raise
    

    KeyError: 'SECID'


Отбрасываем первый и последний контракт.


```python
start_contr = df.iloc[0, df.columns.get_loc('SECID')]
end_contr = df.iloc[-1, df.columns.get_loc('SECID')]
df = df[(df['SECID'] != end_contr) & (df['SECID'] != start_contr)]
print(end_contr)
print(df.to_string(max_rows=12, max_cols=25))  # Проверка
```

Добавление колонки направления бара.


```python
# df['Up/Down'] = df.apply(lambda x: 1 if (x['OPEN'] < x['CLOSE']) else 0, axis=1)
df['Up'] = df.apply(lambda x: 1 if (x['OPEN'] < x['CLOSE']) else np.nan, axis=1)
df['Down'] = df.apply(lambda x: 1 if (x['OPEN'] >= x['CLOSE']) else np.nan, axis=1)
df['Body'] = df.apply(lambda x: -x['OPEN'] + x['CLOSE'], axis=1)
print(df.to_string(max_rows=6, max_cols=25))  # Проверка
# print('\n', df.value_counts("Up/Down"))
```


```python
agg_func_count = {'Up': ['count'],
                  'Down': ['count'],
                  'Body': ['sum']}
df_count = df.groupby(['Work_day']).agg(agg_func_count) # статистика
df_count
```


```python
df_count['Up-Down'] = df_count.apply(lambda x: x['Up'] - x['Down'], axis=1)
df_count[['Up-Down']] = df_count[['Up-Down']].astype(int)
print(df_count.to_string(max_rows=70, max_cols=25))  # Проверка
```

Находим порог выбросов в положительную сторону.


```python
perc75 = df['per75'] = np.percentile(df_count['Body'], 75)
print(perc75)
```


```python
df_count = df_count.loc[df_count['Body', 'sum'] > perc75]
print(df_count.to_string(max_rows=70, max_cols=25))  # Проверка
print('\n', len(df_count))
day_lst = list(df_count.index)
print(day_lst)
```


```python
df = df.loc[df['Work_day'].isin(day_lst)]

df["Body_cum"] = df["Body"].cumsum()
df
```


```python
from matplotlib import pyplot as plt
import seaborn as sns
def _plot_series(series, series_name, series_index=0):
  from matplotlib import pyplot as plt
  import seaborn as sns
  palette = list(sns.palettes.mpl_palette('Dark2'))
  xs = series['TRADEDATE']
  ys = series['Body_cum']

  plt.plot(xs, ys, label=series_name, color=palette[series_index % len(palette)])

fig, ax = plt.subplots(figsize=(10, 5.2), layout='constrained')
df_sorted = df.sort_values('TRADEDATE', ascending=True)
_plot_series(df_sorted, '')
sns.despine(fig=fig, ax=ax)
plt.xlabel('TRADEDATE')
# plt.grid(axis='x')  # , color='0.95'
plt.grid()
_ = plt.ylabel('OPEN')
```
