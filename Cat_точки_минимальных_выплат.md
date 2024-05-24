# Cat от Яндекс для определения цены фьючерса на индекс RTS по точке минимальных выплат, построенной по всем опционам этого фьючерса.

Инсталяция CatBoost


```python
#!pip install --user --upgrade catboost
#!pip install --user --upgrade ipywidgets
#!pip install shap
#!pip install sklearn
#!pip install --upgrade numpy
#!jupyter nbextension enable --py widgetsnbextension
```

### Загрузка данных

Загрузка файла с БД по опционам. Формат файла SQLite.


```python
# !curl -o rts_nn.db -L 'https://drive.google.com/uc?export=download&confirm=yes&id=1ML9AKyQexvic_ut2aMstI_7DQ2b6vHrv'  # Закачка файла БД для Colab
```

Загрузка данных из БД в DF.


```python
import pandas as pd
import numpy as np
import sqlite3
import matplotlib.pyplot as plt


# connection = sqlite3.connect('rts_nn.db', check_same_thread=True)  # Создание соединения с БД для Colab
connection = sqlite3.connect(r'c:\Users\Alkor\gd\data_quote_db\RTS_nn.db', check_same_thread=True)  # Создание соединения с БД для Local
with connection:
  df_all = pd.read_sql('SELECT * FROM `All_opt`', connection)  # Загрузка данных из БД по всем опционам
  # df_all = pd.read_sql('SELECT * FROM `Nearest`', connection)  # Загрузка данных из БД опционам с ближайшей экспирацией

print(df_all.to_string(max_rows=6, max_cols=15))  # Проверка того, что загрузилось
```

           TRADEDATE    -22500    -20000    -17500    -15000    -12500    -10000  ...     17500     20000     22500  zero_strike   close     low    high
    0     2015-01-05  0.728363  0.605605  0.600028  0.456538  0.454987  0.258267  ...  0.909248  0.768367  1.000000        72500  2100.0   -30.0  6480.0
    1     2015-01-06  0.676081  0.560392  0.555003  0.416899  0.415333  0.229272  ...  0.916784  0.780905  1.000000        72500   980.0 -1300.0  2110.0
    2     2015-01-08  0.548794  0.450866  0.446081  0.330494  0.329217  0.181043  ...  0.870850  0.757173  1.000000        72500  7480.0 -1500.0  8880.0
    ...          ...       ...       ...       ...       ...       ...       ...  ...       ...       ...       ...          ...     ...     ...     ...
    2009  2023-02-01  0.872061  0.857970  0.842024  0.777315  0.743621  0.669229  ...  0.847321  0.862301  1.000000        97500  2880.0  2370.0  3870.0
    2010  2023-02-02  1.000000  0.906813  0.869751  0.761446  0.734554  0.509508  ...  0.857213  0.873239  0.896630       102500 -1930.0 -2420.0  -810.0
    2011  2023-02-03  1.000000  0.915020  0.880871  0.765629  0.719663  0.501125  ...  0.909755  0.924095  0.945018       102500 -2400.0 -3450.0 -1520.0
    

Из признаков (характеристик данных) и целевой переменной (close) сформируем датафрейм, в качестве названий колонок возьмем названия признаков.


```python
features = '-22500 -20000 -17500 -15000 -12500 -10000 -7500 -5000 -2500 0 2500 5000 7500 10000 12500 15000 17500 20000 22500'.split()  # Создание списка признаков
print(features)
```

    ['-22500', '-20000', '-17500', '-15000', '-12500', '-10000', '-7500', '-5000', '-2500', '0', '2500', '5000', '7500', '10000', '12500', '15000', '17500', '20000', '22500']
    

### Очистка данных.

Создание DF из колонок с фичами и целевой колонки. Проверка полученного DF на наличие ячеек с NaN.  


```python
df = df_all[features + ['close']]
print(f'Всего значений NaN: {df.isnull().sum().sum()}\n')  # Всего NaN
print(df.isnull().sum())  # Проверка на NaN

```

    Всего значений NaN: 12
    
    -22500    0
    -20000    0
    -17500    0
    -15000    0
    -12500    0
    -10000    0
    -7500     0
    -5000     0
    -2500     3
    0         0
    2500      3
    5000      3
    7500      3
    10000     0
    12500     0
    15000     0
    17500     0
    20000     0
    22500     0
    close     0
    dtype: int64
    

Видно, что ячейки с NaN находятся рядом с нормированным к "0" страйком, значит значения NaN можно заменить на ноль.


```python
df = df.fillna(0)  # Замена NaN на "0"
print(f'Всего значений NaN: {df.isnull().sum().sum()}')  # Всего NaN
```

    Всего значений NaN: 0
    

Данные которые анализируются, являются биржевыми, и среди значений присутствуют выбросы. Для качественного обучения, нужно исключить из обучающей выборки данные с выбросами.  
Проверка данных на выбросы графическим способом.


```python
# Box Plot
import seaborn as sns


sns.set(rc={'figure.figsize':(25,3)})
sns.boxplot(x=df['close'])
```




    <AxesSubplot: xlabel='close'>




    
![png](Cat_%D1%82%D0%BE%D1%87%D0%BA%D0%B8_%D0%BC%D0%B8%D0%BD%D0%B8%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85_%D0%B2%D1%8B%D0%BF%D0%BB%D0%B0%D1%82_files/Cat_%D1%82%D0%BE%D1%87%D0%BA%D0%B8_%D0%BC%D0%B8%D0%BD%D0%B8%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85_%D0%B2%D1%8B%D0%BF%D0%BB%D0%B0%D1%82_16_1.png)
    



```python
sns.set(rc={'figure.figsize':(25,3)})
sns.boxenplot(x=df['close'])
```




    <AxesSubplot: xlabel='close'>




    
![png](Cat_%D1%82%D0%BE%D1%87%D0%BA%D0%B8_%D0%BC%D0%B8%D0%BD%D0%B8%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85_%D0%B2%D1%8B%D0%BF%D0%BB%D0%B0%D1%82_files/Cat_%D1%82%D0%BE%D1%87%D0%BA%D0%B8_%D0%BC%D0%B8%D0%BD%D0%B8%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85_%D0%B2%D1%8B%D0%BF%D0%BB%D0%B0%D1%82_17_1.png)
    


Посмотрим числовые значения перцентилей:


```python
a = df['close']
percentile_min = int(np.percentile(a, 25) - (np.percentile(a, 75) - np.percentile(a, 25)) * 1.5)
print(f'Перцентиле min:\t{percentile_min:,}')

percentile_25 = int(np.percentile(a, 25))
print(f'Перцентиле 25:\t{percentile_25:,}')

percentile_median = int(np.median(a))
print(f'Медиана:\t{percentile_median:,}')

percentile_75 = int(np.percentile(a, 75))
print(f'Перцентиле 75:\t{percentile_75:,}')

percentile_max = int(np.percentile(a, 75) + (np.percentile(a, 75) - np.percentile(a, 25)) * 1.5)
print(f'Перцентиле max:\t{percentile_max:,}')
```

    Перцентиле min:	-7,175
    Перцентиле 25:	-972
    Медиана:	790
    Перцентиле 75:	3,162
    Перцентиле max:	9,365
    

Значения ниже ```'Перцентиле min'``` и выше ```'Перцентиле max'``` будем считать экстремальными. В такие моменты на рынке происходят неординарные события и в это время лучше не торговать. Т.к. тоговать при экстримальных значениях не буду, то и прогнозы для таких значений строить не имеет смысла. Удалю из выборки все экстримальные значения.  
Создание массивов индексов с экстремальными значениями.

Удаляем из DF строки с выбросами


```python
old_shape = df.shape
print("Old Shape: ", old_shape)

df = df[(df.close > percentile_min) & (df.close < percentile_max)]  # Осекаем все строки где 'close' ниже Перцентиле min и выше Перцентиле max

new_shape = df.shape
print("New Shape: ", new_shape)
print(f'Всего удалено строк: {old_shape[0]-new_shape[0]} это {100-(new_shape[0]*100/old_shape[0]):.2f}%')
```

    Old Shape:  (2012, 20)
    New Shape:  (1824, 20)
    Всего удалено строк: 188 это 9.34%
    

Построим гистограмму нормального распределения


```python
sns.displot(df['close'], kde=True)
```




    <seaborn.axisgrid.FacetGrid at 0x25ead9d4130>




    
![png](Cat_%D1%82%D0%BE%D1%87%D0%BA%D0%B8_%D0%BC%D0%B8%D0%BD%D0%B8%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85_%D0%B2%D1%8B%D0%BF%D0%BB%D0%B0%D1%82_files/Cat_%D1%82%D0%BE%D1%87%D0%BA%D0%B8_%D0%BC%D0%B8%D0%BD%D0%B8%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85_%D0%B2%D1%8B%D0%BF%D0%BB%D0%B0%D1%82_24_1.png)
    


### Разделение данных на обучающую и тестовую выборку.


```python
from sklearn.model_selection import train_test_split


X_train, X_test, y_train, y_test = train_test_split(
    df[features],
    df['close'],
    test_size=0.2,
    shuffle=True,
    random_state=3
)

X_train.shape, y_train.shape, X_test.shape, y_test.shape
```




    ((1459, 19), (1459,), (365, 19), (365,))



### Обучение CatBoostRegressor

CatBoostRegressor для задачи регрессии и обучим на обучающей выборке (`X_train`) и целевой переменной для обучающих объектов (`y_train`).  
Начнем обучение на параметрах по умолчанию:  


```python
from catboost import CatBoostRegressor

cat = CatBoostRegressor()
# specify the training parameters 
# cat = CatBoostRegressor(
#     iterations=5000, 
#     # depth=2, 
#     learning_rate=0.01, 
#     loss_function='RMSE'
#     )
```


```python
# grid = {'learning_rate': [0.03, 0.1],
#         'depth': [4, 6, 10],
#         'l2_leaf_reg': [7, 9]}  # 1, 3, 5,

# grid_search_result = cat.grid_search(grid, X=X_train, y=y_train, plot=False, verbose=False)
# print(grid_search_result['params'])
```


```python
cat.fit(X_train,
        y_train,
        verbose=False,
        plot=True)
```


    MetricVisualizer(layout=Layout(align_self='stretch', height='500px'))





    <catboost.core.CatBoostRegressor at 0x25ead9d73a0>



Также мы можем выполнять кроссвалидацию и визуализировать процесс:


```python
from catboost import Pool, cv

params = {"iterations": 5000,
          "depth": 2,
          "loss_function": "RMSE",
          "verbose": False}
cv_dataset = Pool(data=X_train,
                  label=y_train)
scores = cv(cv_dataset,
            params,
            fold_count=5, 
            plot="False")
```


    MetricVisualizer(layout=Layout(align_self='stretch', height='500px'))


    Training on fold [0/5]
    
    bestTest = 2127.91857
    bestIteration = 4993
    
    Training on fold [1/5]
    
    bestTest = 2103.847162
    bestIteration = 4729
    
    Training on fold [2/5]
    
    bestTest = 2227.780064
    bestIteration = 4997
    
    Training on fold [3/5]
    
    bestTest = 2152.787418
    bestIteration = 1546
    
    Training on fold [4/5]
    
    bestTest = 2244.23213
    bestIteration = 1049
    
    

Аналогично вы можете выполнить grid search и визуализировать его:


```python
# grid = {'learning_rate': [0.03, 0.1],
#         'depth': [4, 6, 10, 14],
#         'l2_leaf_reg': [1, 3, 5, 7, 9]}

# grid_search_result = cat.grid_search(grid, X=X_train, y=y_train, plot=False)
```

Также мы можем использовать CatBoost для построения дерева. Вот график первого дерева. Как вы видите из дерева, листья разделяются при одном и том же условии, например, 297, значение > 0.5.


```python
# cat.plot_tree(tree_idx=0)
```

CatBoost дает нам словарь со всеми параметрами модели. Мы можем вывести их, как словарь.


```python
# for key,value in cat.get_all_params().items():
#   print(f'{key}, {value}')
```

Метрики, которые показывает модель.


```python
from sklearn.metrics import mean_absolute_error
from sklearn.metrics import r2_score

prediction = cat.predict(X_test)  # Предсказания для тестовой выборки неучаствовавшей в обучении
cb_mae = mean_absolute_error(prediction, y_test)  # Средняя абсолютная ошибка для данных
cb_r2 = r2_score(y_test, prediction)  # Коэффициент детерминации
print(f'Средняя абсолютная ошибка: {cb_mae=:.3f}. Коэффициент детерминации: {cb_r2=:.3f}')
```

    Средняя абсолютная ошибка: cb_mae=1464.174. Коэффициент детерминации: cb_r2=0.488
    

Сравнение реальных цен закрытия с предсказанными.


```python
pred_df = pd.DataFrame({'true': y_test.astype(int),  # Реальные данные
                        'pred': prediction.astype(int)})  # Предсказанные данные
print(pred_df.to_string(max_rows=100, max_cols=2))
```

          true  pred
    151   -230    57
    38    2260  2707
    1426   210   710
    1721  -270  3640
    784   5720  5550
    868     40    74
    913   -310    20
    1338  2710  2745
    867    160  -223
    1505  -210   847
    1089   930   304
    917    480  -233
    1210  5620  4331
    1983 -1850 -2757
    80    -350  -551
    905   -790 -1736
    1991   770  1714
    1914  -870  -879
    885     60    37
    630   -250    50
    181   -930  1342
    1966  -570   282
    1968  -750 -1566
    1378  -770 -1080
    1041   600  1983
    1536  2750  1660
    240   1070 -1081
    177  -1110   935
    953   2090  3030
    428   -880   497
    165    970  4241
    338   -830  -255
    965  -1750 -1589
    303    870  -212
    1381  1080   -90
    279   3140  2161
    1144 -2750 -2002
    1988  -570  1533
    529  -2620  -343
    400   1900  1616
    1902 -1610 -1400
    1925  2220    31
    95   -5450 -3495
    825    160  -542
    1066   -40   650
    534  -2860   585
    766   6920  8137
    157  -6320 -3783
    620   3730   938
    1616   850   257
    ...    ...   ...
    1542  2820  2409
    390   -750   772
    632   -870   750
    337  -1870 -1388
    1129 -1040  -325
    1370 -1350  -666
    1048  2670   602
    1172  1510   777
    1949  3520   -73
    347   -970  -923
    1407  -760   139
    663   7690  6008
    886    710  -120
    1613 -1230  -875
    41    3530  1892
    1611   940   604
    1907 -4630 -1099
    135  -2990 -2162
    1743  4430  4229
    444   2540  2472
    1571  1640  2101
    1996 -1860  -730
    1331  2600  1886
    717   3110  2718
    823  -2370 -2571
    47   -2740  3275
    164   2480  4035
    607   -470  -786
    975     20 -2083
    441   2410  1893
    952    500  1404
    1520  1320   212
    985  -1420 -1203
    380    910   406
    55    2810  4128
    1195  1680  1567
    1637  1340  2898
    1933 -3320 -3123
    1618  1650  1211
    1188  -820    49
    462   -890    28
    1237  3910  2086
    375   2420  2919
    239    640 -1327
    82    2260  2054
    309  -1450  -404
    1232  2420  1197
    180   -200  2225
    1840   350   964
    1740 -1060  -616
    


```python
count_odinakov_znak = 0
count_razn_znak = 0
for row in pred_df.itertuples(index=False):
    if (row[0] < 0 and row[1] < 0) or (row[0] > 0 and row[1] > 0):  # or (row[0] > 0 and row[1] > 0)
        count_odinakov_znak += 1
    else:
        count_razn_znak += 1

print(f'{count_odinakov_znak =},  {count_razn_znak =}. Соотношение={(count_razn_znak/count_odinakov_znak*100):.2f}%')
```

    count_odinakov_znak =274,  count_razn_znak =91. Соотношение=33.21%
    
