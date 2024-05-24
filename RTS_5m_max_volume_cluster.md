### Для файла с 5 мин котировками находим кластер с максимальным объемом и добавляем эти данные

Для расчата взяты файлы за одну дату с 5 мин котировками и журналом сделок(тики)  
SPFB.RTS_200901_200901.csv - файл с 5 мин котировками  
SPFB.RTS_200901_200901_ticks.csv - файл с тиками


```python
import pandas as pd
pd.set_option('max_rows', 5)
```


```python
# Создание df из csv файлов
df_5m = pd.read_csv('data/SPFB.RTS_200901_200901.csv', delimiter=','); df_5m  # Создание df для 5 мин баров
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
      <th>&lt;TICKER&gt;</th>
      <th>&lt;PER&gt;</th>
      <th>&lt;DATE&gt;</th>
      <th>&lt;TIME&gt;</th>
      <th>&lt;OPEN&gt;</th>
      <th>&lt;HIGH&gt;</th>
      <th>&lt;LOW&gt;</th>
      <th>&lt;CLOSE&gt;</th>
      <th>&lt;VOL&gt;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>SPFB.RTS</td>
      <td>5</td>
      <td>20200901</td>
      <td>100000</td>
      <td>126200.0</td>
      <td>127210.0</td>
      <td>126200.0</td>
      <td>127030.0</td>
      <td>24238</td>
    </tr>
    <tr>
      <th>1</th>
      <td>SPFB.RTS</td>
      <td>5</td>
      <td>20200901</td>
      <td>100500</td>
      <td>127030.0</td>
      <td>127320.0</td>
      <td>127000.0</td>
      <td>127120.0</td>
      <td>16623</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>160</th>
      <td>SPFB.RTS</td>
      <td>5</td>
      <td>20200901</td>
      <td>234000</td>
      <td>127790.0</td>
      <td>127820.0</td>
      <td>127730.0</td>
      <td>127780.0</td>
      <td>912</td>
    </tr>
    <tr>
      <th>161</th>
      <td>SPFB.RTS</td>
      <td>5</td>
      <td>20200901</td>
      <td>234500</td>
      <td>127780.0</td>
      <td>127870.0</td>
      <td>127740.0</td>
      <td>127860.0</td>
      <td>1931</td>
    </tr>
  </tbody>
</table>
<p>162 rows × 9 columns</p>
</div>




```python
df_ticks = pd.read_csv('data/SPFB.RTS_200901_200901_ticks.csv', delimiter=','); df_ticks  # Создание df из тиков
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
      <th>&lt;TICKER&gt;</th>
      <th>&lt;PER&gt;</th>
      <th>&lt;DATE&gt;</th>
      <th>&lt;TIME&gt;</th>
      <th>&lt;LAST&gt;</th>
      <th>&lt;VOL&gt;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>SPFB.RTS</td>
      <td>0</td>
      <td>20200901</td>
      <td>100000</td>
      <td>126200.0</td>
      <td>22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>SPFB.RTS</td>
      <td>0</td>
      <td>20200901</td>
      <td>100000</td>
      <td>126240.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>230562</th>
      <td>SPFB.RTS</td>
      <td>0</td>
      <td>20200901</td>
      <td>234959</td>
      <td>127860.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>230563</th>
      <td>SPFB.RTS</td>
      <td>0</td>
      <td>20200901</td>
      <td>234959</td>
      <td>127860.0</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>230564 rows × 6 columns</p>
</div>




```python
# Обработка df
add_dic = {'<MAX_V_PR>': [], '<MAX_V>': []}  # Словарь в который будем записывать цена/макс объем для выборок по 5 мин
for idx in df_5m.index:  # Перебираем индексы df_5m (для определения времени начала свечи)
    time_candle = df_5m.iloc[idx]['<TIME>']  # Время начала выбранной свечи
    
    # Формирование df тиковой выборки под свечу с временем time_candle
#     df_candle = df_ticks[df_ticks['<TIME>'] >= time_candle]  # Отсекаем предшествующие
#     df_candle = df_candle[df_candle['<TIME>'] < time_candle + 500]  # Отсекаем последующие
    df_candle = df_ticks[(df_ticks['<TIME>'] >= time_candle) & (df_ticks['<TIME>'] < time_candle + 500)]
    
    # Группировка по ценам в свече
    s_rez = df_candle.groupby('<LAST>')['<VOL>'].sum()  # Series (в индексе цена, в значениях суммы объемов)
    max_clu = s_rez.max()  # Нахождение максимального значения в Series (макс сумма объема в кластере)
    max_idx = s_rez.idxmax()  # Нахождение индекса для максимального значения в Series (соответствует цене)
    
    # Добавление полученных значений в словарь
    add_dic['<MAX_V_PR>'].append(max_idx)
    add_dic['<MAX_V>'].append(max_clu)
df_rez = pd.DataFrame(add_dic); df_rez  # Делаем результирующий df из списка
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
      <th>&lt;MAX_V_PR&gt;</th>
      <th>&lt;MAX_V&gt;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>127100.0</td>
      <td>1831</td>
    </tr>
    <tr>
      <th>1</th>
      <td>127180.0</td>
      <td>1305</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>160</th>
      <td>127750.0</td>
      <td>287</td>
    </tr>
    <tr>
      <th>161</th>
      <td>127800.0</td>
      <td>341</td>
    </tr>
  </tbody>
</table>
<p>162 rows × 2 columns</p>
</div>




```python
# Объединяем dataframe
df_merge = df_5m.join(df_rez); df_merge  # Объединяем полученный df с 5 мин
# col = list(df_merge)  # Создание списка столбцов
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
      <th>&lt;TICKER&gt;</th>
      <th>&lt;PER&gt;</th>
      <th>&lt;DATE&gt;</th>
      <th>&lt;TIME&gt;</th>
      <th>&lt;OPEN&gt;</th>
      <th>&lt;HIGH&gt;</th>
      <th>&lt;LOW&gt;</th>
      <th>&lt;CLOSE&gt;</th>
      <th>&lt;VOL&gt;</th>
      <th>&lt;MAX_V_PR&gt;</th>
      <th>&lt;MAX_V&gt;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>SPFB.RTS</td>
      <td>5</td>
      <td>20200901</td>
      <td>100000</td>
      <td>126200.0</td>
      <td>127210.0</td>
      <td>126200.0</td>
      <td>127030.0</td>
      <td>24238</td>
      <td>127100.0</td>
      <td>1831</td>
    </tr>
    <tr>
      <th>1</th>
      <td>SPFB.RTS</td>
      <td>5</td>
      <td>20200901</td>
      <td>100500</td>
      <td>127030.0</td>
      <td>127320.0</td>
      <td>127000.0</td>
      <td>127120.0</td>
      <td>16623</td>
      <td>127180.0</td>
      <td>1305</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>160</th>
      <td>SPFB.RTS</td>
      <td>5</td>
      <td>20200901</td>
      <td>234000</td>
      <td>127790.0</td>
      <td>127820.0</td>
      <td>127730.0</td>
      <td>127780.0</td>
      <td>912</td>
      <td>127750.0</td>
      <td>287</td>
    </tr>
    <tr>
      <th>161</th>
      <td>SPFB.RTS</td>
      <td>5</td>
      <td>20200901</td>
      <td>234500</td>
      <td>127780.0</td>
      <td>127870.0</td>
      <td>127740.0</td>
      <td>127860.0</td>
      <td>1931</td>
      <td>127800.0</td>
      <td>341</td>
    </tr>
  </tbody>
</table>
<p>162 rows × 11 columns</p>
</div>




```python
df_merge.to_csv('data/SPFB.RTS_200901.csv', index=False)  # Пишем в файл результат (без индекса)
```
