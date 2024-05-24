# Статистика по индексу RTS на основе размера тела свечи.

Статистика по свечам после свечи с аномальным (превышающим `QUAN` квантиль за `PERIOD` свечей) телом свечи.  
Проверка гипотез.


```python
import pandas as pd
import numpy as np
import sqlite3
```


```python
PERIOD = 30
QUAN = 95
```


```python
# Загрузка данных из БД
connection = sqlite3.connect(fr'c:\Users\Alkor\gd\data_quote_db\RTS_futures_day.db', check_same_thread=True)  # Создание соединения с БД
with connection:
#   df = pd.read_sql('SELECT * FROM Day', connection)  # Загрузка данных из БД
  df = pd.read_sql('SELECT TRADEDATE, OPEN, LOW, HIGH, CLOSE \
                    FROM Day', connection)  # Загрузка данных из БД

df.columns = map(str.lower, df.columns)  # Название колонок в нижний регистр
df
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
      <th>tradedate</th>
      <th>open</th>
      <th>low</th>
      <th>high</th>
      <th>close</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2015-01-05</td>
      <td>76930.0</td>
      <td>72470.0</td>
      <td>78980.0</td>
      <td>74600.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015-01-06</td>
      <td>74470.0</td>
      <td>71200.0</td>
      <td>74610.0</td>
      <td>73480.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2015-01-08</td>
      <td>73490.0</td>
      <td>71000.0</td>
      <td>81380.0</td>
      <td>79980.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2015-01-09</td>
      <td>79950.0</td>
      <td>74450.0</td>
      <td>81050.0</td>
      <td>77650.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2015-01-12</td>
      <td>77210.0</td>
      <td>73180.0</td>
      <td>77550.0</td>
      <td>73900.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2325</th>
      <td>2024-04-15</td>
      <td>115230.0</td>
      <td>114530.0</td>
      <td>115560.0</td>
      <td>115310.0</td>
    </tr>
    <tr>
      <th>2326</th>
      <td>2024-04-16</td>
      <td>115240.0</td>
      <td>113620.0</td>
      <td>115340.0</td>
      <td>113690.0</td>
    </tr>
    <tr>
      <th>2327</th>
      <td>2024-04-17</td>
      <td>113740.0</td>
      <td>113190.0</td>
      <td>114250.0</td>
      <td>113780.0</td>
    </tr>
    <tr>
      <th>2328</th>
      <td>2024-04-18</td>
      <td>113760.0</td>
      <td>113060.0</td>
      <td>114860.0</td>
      <td>114700.0</td>
    </tr>
    <tr>
      <th>2329</th>
      <td>2024-04-19</td>
      <td>114750.0</td>
      <td>114570.0</td>
      <td>116030.0</td>
      <td>115800.0</td>
    </tr>
  </tbody>
</table>
<p>2330 rows × 5 columns</p>
</div>




```python
# Создание дополнительных колонок
df['up'] = df.apply(lambda x: 1 if (x.open < x.close) else 0, axis=1)  # Создание колонки с напрвлением свечи
df['prev_up'] = df.up.shift(1).astype('Int64')  # Создание колонки с напрвлением предыдущей свечи
df['body'] = abs(df.close - df.open)  # Создание колонки с модулем тела свечи
for i in [75, 80, 85, 90, 95]:  # Создание колонок с квантилями
    df[f'q{i}'] = df.body.rolling(window=PERIOD).quantile(i / 100)  # Создание колонки с заданным квантилем
    # Создание колонки с признаком превышения квантиля текущей свечей (выброс на текущей свече)
    df[f'over_q{i}'] = df.apply(lambda x: 1 if (x['body'] > x[f'q{i}']) else 0, axis=1)
    # Создание колонки с признаком превышения квантиля предыдущей свечей (выброс на предыдущей свече)
    df[f'prev_over_q{i}'] = df[f'over_q{i}'].shift(1).astype('Int64')
df.dropna(inplace=True)  # Стереть строки с NaN

df
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
      <th>tradedate</th>
      <th>open</th>
      <th>low</th>
      <th>high</th>
      <th>close</th>
      <th>up</th>
      <th>prev_up</th>
      <th>body</th>
      <th>q75</th>
      <th>over_q75</th>
      <th>...</th>
      <th>prev_over_q80</th>
      <th>q85</th>
      <th>over_q85</th>
      <th>prev_over_q85</th>
      <th>q90</th>
      <th>over_q90</th>
      <th>prev_over_q90</th>
      <th>q95</th>
      <th>over_q95</th>
      <th>prev_over_q95</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>29</th>
      <td>2015-02-16</td>
      <td>90980.0</td>
      <td>88920.0</td>
      <td>92140.0</td>
      <td>89320.0</td>
      <td>0</td>
      <td>1</td>
      <td>1660.0</td>
      <td>2982.5</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>4154.5</td>
      <td>0</td>
      <td>0</td>
      <td>4246.0</td>
      <td>0</td>
      <td>0</td>
      <td>4823.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>2015-02-17</td>
      <td>89400.0</td>
      <td>88060.0</td>
      <td>93250.0</td>
      <td>88410.0</td>
      <td>0</td>
      <td>0</td>
      <td>990.0</td>
      <td>2982.5</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>4154.5</td>
      <td>0</td>
      <td>0</td>
      <td>4246.0</td>
      <td>0</td>
      <td>0</td>
      <td>4823.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2015-02-18</td>
      <td>88440.0</td>
      <td>88430.0</td>
      <td>93610.0</td>
      <td>92450.0</td>
      <td>1</td>
      <td>0</td>
      <td>4010.0</td>
      <td>3250.0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>4154.5</td>
      <td>0</td>
      <td>0</td>
      <td>4246.0</td>
      <td>0</td>
      <td>0</td>
      <td>4823.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2015-02-19</td>
      <td>92500.0</td>
      <td>88280.0</td>
      <td>93190.0</td>
      <td>90420.0</td>
      <td>0</td>
      <td>1</td>
      <td>2080.0</td>
      <td>2982.5</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>4049.0</td>
      <td>0</td>
      <td>0</td>
      <td>4201.0</td>
      <td>0</td>
      <td>0</td>
      <td>4408.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2015-02-20</td>
      <td>90430.0</td>
      <td>89750.0</td>
      <td>92630.0</td>
      <td>90790.0</td>
      <td>1</td>
      <td>0</td>
      <td>360.0</td>
      <td>2982.5</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>4049.0</td>
      <td>0</td>
      <td>0</td>
      <td>4201.0</td>
      <td>0</td>
      <td>0</td>
      <td>4408.0</td>
      <td>0</td>
      <td>0</td>
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
      <th>2325</th>
      <td>2024-04-15</td>
      <td>115230.0</td>
      <td>114530.0</td>
      <td>115560.0</td>
      <td>115310.0</td>
      <td>1</td>
      <td>1</td>
      <td>80.0</td>
      <td>867.5</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>1150.5</td>
      <td>0</td>
      <td>0</td>
      <td>1359.0</td>
      <td>0</td>
      <td>0</td>
      <td>1643.5</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2326</th>
      <td>2024-04-16</td>
      <td>115240.0</td>
      <td>113620.0</td>
      <td>115340.0</td>
      <td>113690.0</td>
      <td>0</td>
      <td>1</td>
      <td>1550.0</td>
      <td>867.5</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>1150.5</td>
      <td>1</td>
      <td>0</td>
      <td>1451.0</td>
      <td>1</td>
      <td>0</td>
      <td>1693.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2327</th>
      <td>2024-04-17</td>
      <td>113740.0</td>
      <td>113190.0</td>
      <td>114250.0</td>
      <td>113780.0</td>
      <td>1</td>
      <td>0</td>
      <td>40.0</td>
      <td>867.5</td>
      <td>0</td>
      <td>...</td>
      <td>1</td>
      <td>1150.5</td>
      <td>0</td>
      <td>1</td>
      <td>1451.0</td>
      <td>0</td>
      <td>1</td>
      <td>1693.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2328</th>
      <td>2024-04-18</td>
      <td>113760.0</td>
      <td>113060.0</td>
      <td>114860.0</td>
      <td>114700.0</td>
      <td>1</td>
      <td>1</td>
      <td>940.0</td>
      <td>925.0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>1150.5</td>
      <td>0</td>
      <td>0</td>
      <td>1451.0</td>
      <td>0</td>
      <td>0</td>
      <td>1693.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2329</th>
      <td>2024-04-19</td>
      <td>114750.0</td>
      <td>114570.0</td>
      <td>116030.0</td>
      <td>115800.0</td>
      <td>1</td>
      <td>1</td>
      <td>1050.0</td>
      <td>955.0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>1154.0</td>
      <td>0</td>
      <td>0</td>
      <td>1451.0</td>
      <td>0</td>
      <td>0</td>
      <td>1693.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>2301 rows × 23 columns</p>
</div>



## Проверка гипотезы, что после свечи с аномальным превышением размера тела, следует свеча в том же направлении.

H0 - нулевая гипотеза: После свечи с аномальным размером тела с такой же вероятностью идет свеча в том же направлении, что и после обычной свечи.  
H1 - гипотеза один: После свечи с аномальным размером тела с большей вероятностью идет свеча в том же направлении.


```python
from scipy import stats

for i in [75, 80, 85, 90, 95]:
    print(f'Квантиль {i}')
    # Выборка обычных свечей, где предыдущей свечей была свеча на повышение
    df_up = df.loc[(df.prev_up == 1) & (df[f'prev_over_q{i}'] == 0)]  # (df.prev_up == 1) & 
    mean_up = df_up.up.mean()
    print(f'{mean_up=}')

    # Выборка свечей, где предыдущей свечей была свеча на повышение с аномальным размером тела
    df_up_prev_over = df.loc[(df.prev_up == 1) & (df[f'prev_over_q{i}'] == 1)]
    mean_up_prev_over = df_up_prev_over.up.mean()
    print(f'{mean_up_prev_over=}')

    print(stats.ttest_ind(df_up.up, df_up_prev_over.up), '\n')

```

    Квантиль 75
    mean_up=0.5266075388026608
    mean_up_prev_over=0.5254777070063694
    TtestResult(statistic=0.034505650024474796, pvalue=0.9724796093410544, df=1214.0) 
    
    Квантиль 80
    mean_up=0.5308008213552361
    mean_up_prev_over=0.5082644628099173
    TtestResult(statistic=0.6279854015302962, pvalue=0.5301316140622725, df=1214.0) 
    
    Квантиль 85
    mean_up=0.5278048780487805
    mean_up_prev_over=0.518324607329843
    TtestResult(statistic=0.24072313740033027, pvalue=0.8098103507510077, df=1214.0) 
    
    Квантиль 90
    mean_up=0.5266968325791855
    mean_up_prev_over=0.5225225225225225
    TtestResult(statistic=0.08389512127598522, pvalue=0.9331536500922779, df=1214.0) 
    
    Квантиль 95
    mean_up=0.5204525674499565
    mean_up_prev_over=0.6268656716417911
    TtestResult(statistic=-1.6963472009686649, pvalue=0.09007651332392198, df=1214.0) 
    
    


```python
for i in [75, 80, 85, 90, 95]:
    print(f'Квантиль {i}')
    df_dw = df.loc[(df.prev_up == 0) & (df[f'prev_over_q{i}'] == 0)]  # (df.prev_up == 0) & 
    mean_dw = df_dw.up.mean()
    print(f'{mean_dw=}')

    df_dw_prev_over = df.loc[(df.prev_up == 0) & (df[f'prev_over_q{i}'] == 1)]
    mean_dw_prev_over = df_dw_prev_over.up.mean()
    print(f'{mean_dw_prev_over=}')

    print(stats.ttest_ind(df_dw.up, df_dw_prev_over.up), '\n')
```

    Квантиль 75
    mean_dw=0.5325
    mean_dw_prev_over=0.5263157894736842
    TtestResult(statistic=0.17947428959992798, pvalue=0.8575988754154767, df=1083.0) 
    
    Квантиль 80
    mean_dw=0.5355504587155964
    mean_dw_prev_over=0.5117370892018779
    TtestResult(statistic=0.6238655650294528, pvalue=0.5328472550171643, df=1083.0) 
    
    Квантиль 85
    mean_dw=0.5337016574585636
    mean_dw_prev_over=0.5166666666666667
    TtestResult(statistic=0.41790883795775885, pvalue=0.6760966140378768, df=1083.0) 
    
    Квантиль 90
    mean_dw=0.5307377049180327
    mean_dw_prev_over=0.5321100917431193
    TtestResult(statistic=-0.027205639679231293, pvalue=0.9783007296728976, df=1083.0) 
    
    Квантиль 95
    mean_dw=0.5297619047619048
    mean_dw_prev_over=0.5454545454545454
    TtestResult(statistic=-0.26572346814930925, pvalue=0.790502761948757, df=1083.0) 
    
    

По результатам видно, что ни при каком из условий превышения квантилей `pvalue` не опускался ниже 0.05, значит какими бы нибыли различия, они не являются статистически значимыми и верна гипотеза 0. __После свечи с аномальным размером тела с такой же вероятностью идет свеча в том же направлении, что и после обычной свечи.__

## Проверка гипотезы, что после свечи с аномальным превышением размера тела, следует свеча тоже с большим размером тела.

H0 - нулевая гипотеза: После свечи с аномальным размером тела идет свеча со средним размером тела.  
H1 - гипотеза один: После свечи с аномальным размером тела идет свеча с большим размером тела.


```python
for i in [75, 80, 85, 90, 95]:
    print(f'Квантиль {i}')
    df_norn = df.loc[(df[f'prev_over_q{i}'] == 0)]  # (df.prev_up == 0) & 
    mean_norm = df_norn.body.mean()
    print(f'{mean_norm=}')

    df_over = df.loc[(df[f'prev_over_q{i}'] == 1)]
    mean_over = df_over.body.mean()
    print(f'{mean_over=}')

    print(stats.ttest_ind(df_norn.body, df_over.body), '\n')
```

    Квантиль 75
    mean_norm=1435.0881316098707
    mean_over=1416.9782971619366
    TtestResult(statistic=0.22501963645406622, pvalue=0.8219839965964333, df=2299.0) 
    
    Квантиль 80
    mean_norm=1438.813651137595
    mean_over=1396.131868131868
    TtestResult(statistic=0.4813865522656529, pvalue=0.6302876069731498, df=2299.0) 
    
    Квантиль 85
    mean_norm=1431.7512953367875
    mean_over=1423.2075471698113
    TtestResult(statistic=0.08896573326258382, pvalue=0.9291168919869948, df=2299.0) 
    
    Квантиль 90
    mean_norm=1451.792407496396
    mean_over=1227.7727272727273
    TtestResult(statistic=1.8666822277674138, pvalue=0.06207318470273619, df=2299.0) 
    
    Квантиль 95
    mean_norm=1437.2647195178488
    mean_over=1327.1527777777778
    TtestResult(statistic=0.7552705994253834, pvalue=0.45016400287576674, df=2299.0) 
    
    

H0 - нулевая гипотеза: После свечи с аномальным размером тела идет свеча со средним размером тела - верна.
