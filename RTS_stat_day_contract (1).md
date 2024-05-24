# Статистика по индексу RTS на основе дней контракта.

Статистика по направлениям свечей в зависимости от оставшихся торговых дней контракта.

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
connection = sqlite3.connect(r'/content/drive/MyDrive/data_quote_db/RTS_futures_day.db', check_same_thread=True)  # Создание соединения с БД
```

Загрузка данных в таблицу pandas.


```python
with connection:
  df = pd.read_sql('SELECT * FROM Day', connection)  # Загрузка данных из БД

print(df.to_string(max_rows=6, max_cols=25))  # Проверка того, что загрузилось
```

           TRADEDATE      SECID      OPEN       LOW      HIGH     CLOSE  VOLUME  OPENPOSITION SHORTNAME    LSTTRADE
    0     2015-01-05  RIH5_2015   76930.0   72470.0   78980.0   74600.0  372848        751996  RTS-3.15  2015-03-16
    1     2015-01-06  RIH5_2015   74470.0   71200.0   74610.0   73480.0  319307        731236  RTS-3.15  2015-03-16
    2     2015-01-08  RIH5_2015   73490.0   71000.0   81380.0   79980.0  537469        751010  RTS-3.15  2015-03-16
    ...          ...        ...       ...       ...       ...       ...     ...           ...       ...         ...
    2253  2023-12-29       RIH4  110400.0  109690.0  110720.0  109890.0   44823         41050  RTS-3.24  2024-03-21
    2254  2024-01-03       RIH4  109710.0  109030.0  109910.0  109500.0   31522         40786  RTS-3.24  2024-03-21
    2255  2024-01-04       RIH4  109510.0  109210.0  110310.0  110230.0   33235         44376  RTS-3.24  2024-03-21
    

Подготовка DF


```python
df = df.drop(['OPENPOSITION', 'SHORTNAME', 'LSTTRADE'], axis=1)
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

          TRADEDATE      SECID      OPEN       LOW      HIGH     CLOSE  VOLUME  Work_day
    0    2015-01-05  RIH5_2015   76930.0   72470.0   78980.0   74600.0  372848        48
    1    2015-01-06  RIH5_2015   74470.0   71200.0   74610.0   73480.0  319307        47
    2    2015-01-08  RIH5_2015   73490.0   71000.0   81380.0   79980.0  537469        46
    ...         ...        ...       ...       ...       ...       ...     ...       ...
    2253 2023-12-29       RIH4  110400.0  109690.0  110720.0  109890.0   44823         3
    2254 2024-01-03       RIH4  109710.0  109030.0  109910.0  109500.0   31522         2
    2255 2024-01-04       RIH4  109510.0  109210.0  110310.0  110230.0   33235         1
    

Отбрасываем первый и последний контракт.


```python
start_contr = df.iloc[0, df.columns.get_loc('SECID')]
end_contr = df.iloc[-1, df.columns.get_loc('SECID')]
df = df[(df['SECID'] != end_contr) & (df['SECID'] != start_contr)]
print(end_contr)
print(df.to_string(max_rows=12, max_cols=25))  # Проверка
```

    RIH4
          TRADEDATE      SECID      OPEN       LOW      HIGH     CLOSE  VOLUME  Work_day
    48   2015-03-17  RIM5_2015   79930.0   79730.0   81440.0   80820.0  552443        61
    49   2015-03-18  RIM5_2015   80800.0   80520.0   83050.0   82330.0  491316        60
    50   2015-03-19  RIM5_2015   82420.0   82420.0   86470.0   83260.0  741351        59
    51   2015-03-20  RIM5_2015   83150.0   81930.0   85160.0   84930.0  670325        58
    52   2015-03-23  RIM5_2015   85080.0   83720.0   86000.0   84270.0  594733        57
    53   2015-03-24  RIM5_2015   84310.0   83650.0   87020.0   86260.0  585397        56
    ...         ...        ...       ...       ...       ...       ...     ...       ...
    2242 2023-12-14       RIZ3  106100.0  104540.0  107490.0  104800.0   86708         6
    2243 2023-12-15       RIZ3  104920.0  103910.0  106660.0  106510.0  109741         5
    2244 2023-12-18       RIZ3  106530.0  106310.0  108540.0  108260.0   73624         4
    2245 2023-12-19       RIZ3  108320.0  107090.0  108760.0  107530.0   68482         3
    2246 2023-12-20       RIZ3  107690.0  107380.0  108700.0  107520.0   50000         2
    2247 2023-12-21       RIZ3  107510.0  104800.0  107790.0  104810.0   39222         1
    

Добавление колонки направления бара.


```python
# df['Up/Down'] = df.apply(lambda x: 1 if (x['OPEN'] < x['CLOSE']) else 0, axis=1)
df['Up'] = df.apply(lambda x: 1 if (x['OPEN'] < x['CLOSE']) else np.nan, axis=1)
df['Down'] = df.apply(lambda x: 1 if (x['OPEN'] >= x['CLOSE']) else np.nan, axis=1)
df['Body'] = df.apply(lambda x: -x['OPEN'] + x['CLOSE'], axis=1)
print(df.to_string(max_rows=6, max_cols=25))  # Проверка
# print('\n', df.value_counts("Up/Down"))
```

          TRADEDATE      SECID      OPEN       LOW      HIGH     CLOSE  VOLUME  Work_day   Up  Down    Body
    48   2015-03-17  RIM5_2015   79930.0   79730.0   81440.0   80820.0  552443        61  1.0   NaN   890.0
    49   2015-03-18  RIM5_2015   80800.0   80520.0   83050.0   82330.0  491316        60  1.0   NaN  1530.0
    50   2015-03-19  RIM5_2015   82420.0   82420.0   86470.0   83260.0  741351        59  1.0   NaN   840.0
    ...         ...        ...       ...       ...       ...       ...     ...       ...  ...   ...     ...
    2245 2023-12-19       RIZ3  108320.0  107090.0  108760.0  107530.0   68482         3  NaN   1.0  -790.0
    2246 2023-12-20       RIZ3  107690.0  107380.0  108700.0  107520.0   50000         2  NaN   1.0  -170.0
    2247 2023-12-21       RIZ3  107510.0  104800.0  107790.0  104810.0   39222         1  NaN   1.0 -2700.0
    


```python
agg_func_count = {'Up': ['count'],
                  'Down': ['count'],
                  'Body': ['sum']}
df_count = df.groupby(['Work_day']).agg(agg_func_count) # статистика
df_count
```





  <div id="df-99ef616c-aa0d-4f70-b316-283facab75c6" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>Up</th>
      <th>Down</th>
      <th>Body</th>
    </tr>
    <tr>
      <th></th>
      <th>count</th>
      <th>count</th>
      <th>sum</th>
    </tr>
    <tr>
      <th>Work_day</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>18</td>
      <td>18</td>
      <td>21570.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>19</td>
      <td>17</td>
      <td>-20190.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>18</td>
      <td>18</td>
      <td>-5050.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>14</td>
      <td>26430.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>21</td>
      <td>15</td>
      <td>1830.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>66</th>
      <td>3</td>
      <td>2</td>
      <td>2020.0</td>
    </tr>
    <tr>
      <th>67</th>
      <td>2</td>
      <td>1</td>
      <td>3100.0</td>
    </tr>
    <tr>
      <th>68</th>
      <td>0</td>
      <td>3</td>
      <td>-4550.0</td>
    </tr>
    <tr>
      <th>69</th>
      <td>3</td>
      <td>0</td>
      <td>1340.0</td>
    </tr>
    <tr>
      <th>70</th>
      <td>0</td>
      <td>2</td>
      <td>-640.0</td>
    </tr>
  </tbody>
</table>
<p>70 rows × 3 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-99ef616c-aa0d-4f70-b316-283facab75c6')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-99ef616c-aa0d-4f70-b316-283facab75c6 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-99ef616c-aa0d-4f70-b316-283facab75c6');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-e5995b9d-2582-4d90-a8b3-a0cd1cfa9c4e">
  <button class="colab-df-quickchart" onclick="quickchart('df-e5995b9d-2582-4d90-a8b3-a0cd1cfa9c4e')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-e5995b9d-2582-4d90-a8b3-a0cd1cfa9c4e button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>
    </div>
  </div>





```python
df_count['Up-Down'] = df_count.apply(lambda x: x['Up'] - x['Down'], axis=1)
df_count[['Up-Down']] = df_count[['Up-Down']].astype(int)
print(df_count.to_string(max_rows=70, max_cols=25))  # Проверка
```

                Up  Down     Body Up-Down
             count count      sum        
    Work_day                             
    1           18    18  21570.0       0
    2           19    17 -20190.0       2
    3           18    18  -5050.0       0
    4           22    14  26430.0       8
    5           21    15   1830.0       6
    6           20    16  23000.0       4
    7           17    18 -43620.0      -1
    8           17    18 -22480.0      -1
    9           22    13   4610.0       9
    10          15    20 -34940.0      -5
    11          18    17  -6770.0       1
    12          15    20  14680.0      -5
    13          22    13  25380.0       9
    14          15    20  -6250.0      -5
    15          19    16  -9550.0       3
    16          21    14  -6940.0       7
    17          16    19  -4570.0      -3
    18          21    14  11400.0       7
    19          19    16  -4950.0       3
    20          15    20 -12160.0      -5
    21          21    14   2970.0       7
    22          17    18   2720.0      -1
    23          23    12  23130.0      11
    24          19    16  10540.0       3
    25          16    19  -5660.0      -3
    26          19    16  14580.0       3
    27          13    22  -7680.0      -9
    28          20    15  12370.0       5
    29          19    16   6970.0       3
    30          16    19 -15750.0      -3
    31          19    16  13720.0       3
    32          20    15  11800.0       5
    33          20    15  14890.0       5
    34          18    17  -3830.0       1
    35          14    21  -6750.0      -7
    36          16    19 -12490.0      -3
    37          19    16  -2180.0       3
    38          17    18   2030.0      -1
    39          18    17  -5400.0       1
    40          16    19     40.0      -3
    41          19    16  -1050.0       3
    42          19    16   4320.0       3
    43          16    19   1840.0      -3
    44          24    11   -340.0      13
    45          17    18   4150.0      -1
    46          15    20  -2530.0      -5
    47          18    17  12700.0       1
    48          20    15   9120.0       5
    49          22    13  20350.0       9
    50          22    12  19920.0      10
    51          21    13   9100.0       8
    52          22    12  17030.0      10
    53          15    18 -10710.0      -3
    54          17    16   2020.0       1
    55          19    14   1620.0       5
    56          26     6  20120.0      20
    57          11    21 -26510.0     -10
    58          19    13   3510.0       6
    59          15    17    660.0      -2
    60          19    12  10160.0       7
    61          13    17   -560.0      -4
    62          14    11  -2140.0       3
    63           9    10  -9850.0      -1
    64          11     8   3990.0       3
    65           4     6  -5050.0      -2
    66           3     2   2020.0       1
    67           2     1   3100.0       1
    68           0     3  -4550.0      -3
    69           3     0   1340.0       3
    70           0     2   -640.0      -2
    

Находим порог выбросов в положительную сторону.


```python
perc75 = np.percentile(df_count['Body'], 75)
# perc75 = df['per75'] = np.percentile(df_count['Body'], 75)
print(perc75)
```

    10445.0
    


```python
df_count = df_count.loc[df_count['Body', 'sum'] > perc75]
print(df_count.to_string(max_rows=70, max_cols=25))  # Проверка
print('\n', len(df_count))
day_lst = list(df_count.index)
print(day_lst)
```

                Up  Down     Body Up-Down
             count count      sum        
    Work_day                             
    1           18    18  21570.0       0
    4           22    14  26430.0       8
    6           20    16  23000.0       4
    12          15    20  14680.0      -5
    13          22    13  25380.0       9
    18          21    14  11400.0       7
    23          23    12  23130.0      11
    24          19    16  10540.0       3
    26          19    16  14580.0       3
    28          20    15  12370.0       5
    31          19    16  13720.0       3
    32          20    15  11800.0       5
    33          20    15  14890.0       5
    47          18    17  12700.0       1
    49          22    13  20350.0       9
    50          22    12  19920.0      10
    52          22    12  17030.0      10
    56          26     6  20120.0      20
    
     18
    [1, 4, 6, 12, 13, 18, 23, 24, 26, 28, 31, 32, 33, 47, 49, 50, 52, 56]
    


```python
df = df.loc[df['Work_day'].isin(day_lst)]

df["Body_cum"] = df["Body"].cumsum()
df
```

    <ipython-input-13-e0b30971629c>:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df["Body_cum"] = df["Body"].cumsum()
    





  <div id="df-90099498-a401-4d2f-80ef-f7eaf0393c74" class="colab-df-container">
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
      <th>TRADEDATE</th>
      <th>SECID</th>
      <th>OPEN</th>
      <th>LOW</th>
      <th>HIGH</th>
      <th>CLOSE</th>
      <th>VOLUME</th>
      <th>Work_day</th>
      <th>Up</th>
      <th>Down</th>
      <th>Body</th>
      <th>Body_cum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>53</th>
      <td>2015-03-24</td>
      <td>RIM5_2015</td>
      <td>84310.0</td>
      <td>83650.0</td>
      <td>87020.0</td>
      <td>86260.0</td>
      <td>585397</td>
      <td>56</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1950.0</td>
      <td>1950.0</td>
    </tr>
    <tr>
      <th>57</th>
      <td>2015-03-30</td>
      <td>RIM5_2015</td>
      <td>84680.0</td>
      <td>83460.0</td>
      <td>86610.0</td>
      <td>86480.0</td>
      <td>487779</td>
      <td>52</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1800.0</td>
      <td>3750.0</td>
    </tr>
    <tr>
      <th>59</th>
      <td>2015-04-01</td>
      <td>RIM5_2015</td>
      <td>86430.0</td>
      <td>85610.0</td>
      <td>89670.0</td>
      <td>89650.0</td>
      <td>657389</td>
      <td>50</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>3220.0</td>
      <td>6970.0</td>
    </tr>
    <tr>
      <th>60</th>
      <td>2015-04-02</td>
      <td>RIM5_2015</td>
      <td>89550.0</td>
      <td>89190.0</td>
      <td>92440.0</td>
      <td>92340.0</td>
      <td>637462</td>
      <td>49</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>2790.0</td>
      <td>9760.0</td>
    </tr>
    <tr>
      <th>62</th>
      <td>2015-04-06</td>
      <td>RIM5_2015</td>
      <td>91740.0</td>
      <td>91640.0</td>
      <td>96090.0</td>
      <td>95940.0</td>
      <td>498450</td>
      <td>47</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>4200.0</td>
      <td>13960.0</td>
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
    </tr>
    <tr>
      <th>2235</th>
      <td>2023-12-05</td>
      <td>RIZ3</td>
      <td>106960.0</td>
      <td>106010.0</td>
      <td>107360.0</td>
      <td>106480.0</td>
      <td>80964</td>
      <td>13</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>-480.0</td>
      <td>318130.0</td>
    </tr>
    <tr>
      <th>2236</th>
      <td>2023-12-06</td>
      <td>RIZ3</td>
      <td>106470.0</td>
      <td>103620.0</td>
      <td>106780.0</td>
      <td>104220.0</td>
      <td>117661</td>
      <td>12</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>-2250.0</td>
      <td>315880.0</td>
    </tr>
    <tr>
      <th>2242</th>
      <td>2023-12-14</td>
      <td>RIZ3</td>
      <td>106100.0</td>
      <td>104540.0</td>
      <td>107490.0</td>
      <td>104800.0</td>
      <td>86708</td>
      <td>6</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>-1300.0</td>
      <td>314580.0</td>
    </tr>
    <tr>
      <th>2244</th>
      <td>2023-12-18</td>
      <td>RIZ3</td>
      <td>106530.0</td>
      <td>106310.0</td>
      <td>108540.0</td>
      <td>108260.0</td>
      <td>73624</td>
      <td>4</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1730.0</td>
      <td>316310.0</td>
    </tr>
    <tr>
      <th>2247</th>
      <td>2023-12-21</td>
      <td>RIZ3</td>
      <td>107510.0</td>
      <td>104800.0</td>
      <td>107790.0</td>
      <td>104810.0</td>
      <td>39222</td>
      <td>1</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>-2700.0</td>
      <td>313610.0</td>
    </tr>
  </tbody>
</table>
<p>628 rows × 12 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-90099498-a401-4d2f-80ef-f7eaf0393c74')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-90099498-a401-4d2f-80ef-f7eaf0393c74 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-90099498-a401-4d2f-80ef-f7eaf0393c74');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-8779d1f4-e1db-4141-84d0-812089322332">
  <button class="colab-df-quickchart" onclick="quickchart('df-8779d1f4-e1db-4141-84d0-812089322332')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-8779d1f4-e1db-4141-84d0-812089322332 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>
    </div>
  </div>





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


    
![png](RTS_stat_day_contract%20%281%29_files/RTS_stat_day_contract%20%281%29_23_0.png)
    

