# Статистика по фьючерсу Si на основе дней в месяце.

Статистика по направлениям свечей в зависимости от дня в месяце.

Подключение гугл диска


```python
from google.colab import drive
drive.mount('/content/drive')
```

    Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount("/content/drive", force_remount=True).
    

Подключение к БД.


```python
import pandas as pd
import numpy as np
import sqlite3
import datetime
```


```python
connection = sqlite3.connect(r'/content/drive/MyDrive/data_quote_db/Si_futures_day.db', check_same_thread=True)  # Создание соединения с БД
```

Загрузка данных в таблицу pandas.


```python
with connection:
  df = pd.read_sql('SELECT * FROM Day', connection)  # Загрузка данных из БД

print(df.to_string(max_rows=6, max_cols=25))  # Проверка того, что загрузилось
```

           TRADEDATE      SECID     OPEN      LOW     HIGH    CLOSE   VOLUME  OPENPOSITION SHORTNAME    LSTTRADE
    0     2015-01-05  SiH5_2015  57860.0  56424.0  61483.0  61101.0   837896       2052722   Si-3.15  2015-03-16
    1     2015-01-06  SiH5_2015  61260.0  61076.0  65850.0  64464.0   924763       2041056   Si-3.15  2015-03-16
    2     2015-01-08  SiH5_2015  64424.0  61000.0  65708.0  61380.0  1328024       2069926   Si-3.15  2015-03-16
    ...          ...        ...      ...      ...      ...      ...      ...           ...       ...         ...
    2246  2023-11-30       SiZ3  89242.0  88899.0  90305.0  90138.0  1063131       5604960  Si-12.23  2023-12-21
    2247  2023-12-01       SiZ3  90150.0  89741.0  91019.0  90969.0   865188       5564512  Si-12.23  2023-12-21
    2248  2023-12-04       SiZ3  91000.0  90661.0  91695.0  91610.0   841721       5446546  Si-12.23  2023-12-21
    

Подготовка DF


```python
df = df.drop(['SECID', 'OPENPOSITION', 'SHORTNAME', 'LSTTRADE'], axis=1)
df['TRADEDATE'] = pd.to_datetime(df['TRADEDATE'])  # Смена типа
df = df.dropna().reset_index(drop=True)  # Удаление NaN
df = df.sort_values(by='TRADEDATE', ascending=False)  # Сортировка по убыванию
```

Создание и заполнение колонки с торговым днем месяца по убыванию.


```python
df['Work_day'] = np.nan  # Создание колонки и заполнение NaN
month_cur = 0
n = 1
for i in range(0, len(df)):
  if i == 0:
    month_prev = df.iloc[0, 0].month
  month_cur = df.iloc[i, 0].month
  if month_cur != month_prev:
    n = 1

  df.iloc[i, df.columns.get_loc('Work_day')] = n
  n += 1
  month_prev = month_cur

df[['Work_day']] = df[['Work_day']].astype(int)
df = df.sort_values(by='TRADEDATE', ascending=True)  # Сортировка по возрастанию
print(df.to_string(max_rows=6, max_cols=25))  # Проверка
```

          TRADEDATE     OPEN      LOW     HIGH    CLOSE   VOLUME  Work_day
    0    2015-01-05  57860.0  56424.0  61483.0  61101.0   837896        19
    1    2015-01-06  61260.0  61076.0  65850.0  64464.0   924763        18
    2    2015-01-08  64424.0  61000.0  65708.0  61380.0  1328024        17
    ...         ...      ...      ...      ...      ...      ...       ...
    2246 2023-11-30  89242.0  88899.0  90305.0  90138.0  1063131         1
    2247 2023-12-01  90150.0  89741.0  91019.0  90969.0   865188         2
    2248 2023-12-04  91000.0  90661.0  91695.0  91610.0   841721         1
    

Отбрасываем последний (неполный) месяц.


```python
year = df.iloc[-1, 0].year
month = df.iloc[-1, 0].month
end_date = datetime.datetime(year, month, 1)
df = df[df['TRADEDATE'] < end_date]
print(f'{year}-{month} отбрасываем этот месяц.')
print(df.to_string(max_rows=6, max_cols=25))  # Проверка
```

    2023-12 отбрасываем этот месяц.
          TRADEDATE     OPEN      LOW     HIGH    CLOSE   VOLUME  Work_day
    0    2015-01-05  57860.0  56424.0  61483.0  61101.0   837896        19
    1    2015-01-06  61260.0  61076.0  65850.0  64464.0   924763        18
    2    2015-01-08  64424.0  61000.0  65708.0  61380.0  1328024        17
    ...         ...      ...      ...      ...      ...      ...       ...
    2244 2023-11-28  89628.0  89230.0  90262.0  90067.0   997493         3
    2245 2023-11-29  90068.0  89057.0  90239.0  89245.0  1116565         2
    2246 2023-11-30  89242.0  88899.0  90305.0  90138.0  1063131         1
    

Добавление колонки направления бара.


```python
# df['Up/Down'] = df.apply(lambda x: 1 if (x['OPEN'] < x['CLOSE']) else 0, axis=1)
df['Up'] = df.apply(lambda x: 1 if (x['OPEN'] < x['CLOSE']) else np.nan, axis=1)
df['Down'] = df.apply(lambda x: 1 if (x['OPEN'] >= x['CLOSE']) else np.nan, axis=1)
df['Body'] = df.apply(lambda x: -x['OPEN'] + x['CLOSE'], axis=1)
print(df.to_string(max_rows=6, max_cols=25))  # Проверка
# print('\n', df.value_counts("Up/Down"))
```

          TRADEDATE     OPEN      LOW     HIGH    CLOSE   VOLUME  Work_day   Up  Down    Body
    0    2015-01-05  57860.0  56424.0  61483.0  61101.0   837896        19  1.0   NaN  3241.0
    1    2015-01-06  61260.0  61076.0  65850.0  64464.0   924763        18  1.0   NaN  3204.0
    2    2015-01-08  64424.0  61000.0  65708.0  61380.0  1328024        17  NaN   1.0 -3044.0
    ...         ...      ...      ...      ...      ...      ...       ...  ...   ...     ...
    2244 2023-11-28  89628.0  89230.0  90262.0  90067.0   997493         3  1.0   NaN   439.0
    2245 2023-11-29  90068.0  89057.0  90239.0  89245.0  1116565         2  NaN   1.0  -823.0
    2246 2023-11-30  89242.0  88899.0  90305.0  90138.0  1063131         1  1.0   NaN   896.0
    

Агрегация данных: подсчет количества баров на повышение и понижение по рабочену дню в месяце, суммирование размера тела свечи.


```python
agg_func_count = {'Up': ['count'],
                  'Down': ['count'],
                  'Body': ['sum']}
df_count = df.groupby(['Work_day']).agg(agg_func_count)  # статистика по Up / Down
df_count
```





  <div id="df-20c2e880-0d7d-45e1-97bf-e0805d87e2b7" class="colab-df-container">
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
      <td>58</td>
      <td>49</td>
      <td>4895.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>53</td>
      <td>54</td>
      <td>-15720.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>63</td>
      <td>44</td>
      <td>10507.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>65</td>
      <td>42</td>
      <td>14409.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>55</td>
      <td>52</td>
      <td>2124.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>49</td>
      <td>58</td>
      <td>-5481.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>46</td>
      <td>61</td>
      <td>-11855.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>51</td>
      <td>56</td>
      <td>1133.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>49</td>
      <td>58</td>
      <td>-8344.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>46</td>
      <td>61</td>
      <td>1360.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>46</td>
      <td>61</td>
      <td>-10044.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>45</td>
      <td>62</td>
      <td>-13533.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>50</td>
      <td>57</td>
      <td>-990.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>54</td>
      <td>53</td>
      <td>-9595.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>52</td>
      <td>55</td>
      <td>-5115.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>50</td>
      <td>57</td>
      <td>-5091.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>48</td>
      <td>59</td>
      <td>-663.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>50</td>
      <td>57</td>
      <td>-8669.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>46</td>
      <td>58</td>
      <td>9472.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>50</td>
      <td>44</td>
      <td>8302.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>37</td>
      <td>39</td>
      <td>9098.0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>23</td>
      <td>15</td>
      <td>4027.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>6</td>
      <td>3</td>
      <td>464.0</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-20c2e880-0d7d-45e1-97bf-e0805d87e2b7')"
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
        document.querySelector('#df-20c2e880-0d7d-45e1-97bf-e0805d87e2b7 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-20c2e880-0d7d-45e1-97bf-e0805d87e2b7');
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


<div id="df-18c34fe3-7ab6-4a11-b140-c5f3f0194107">
  <button class="colab-df-quickchart" onclick="quickchart('df-18c34fe3-7ab6-4a11-b140-c5f3f0194107')"
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
        document.querySelector('#df-18c34fe3-7ab6-4a11-b140-c5f3f0194107 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>
    </div>
  </div>




Создание колонки соотношения повышающихся и понижающихся баров.


```python
df_count['Up-Down'] = df_count.apply(lambda x: x['Up'] - x['Down'], axis=1)
df_count[['Up-Down']] = df_count[['Up-Down']].astype(int)
df_count
```





  <div id="df-e47bfd25-4fa8-42c4-9e36-80f7fecef25f" class="colab-df-container">
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
      <th>Up-Down</th>
    </tr>
    <tr>
      <th></th>
      <th>count</th>
      <th>count</th>
      <th>sum</th>
      <th></th>
    </tr>
    <tr>
      <th>Work_day</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>58</td>
      <td>49</td>
      <td>4895.0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>53</td>
      <td>54</td>
      <td>-15720.0</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>63</td>
      <td>44</td>
      <td>10507.0</td>
      <td>19</td>
    </tr>
    <tr>
      <th>4</th>
      <td>65</td>
      <td>42</td>
      <td>14409.0</td>
      <td>23</td>
    </tr>
    <tr>
      <th>5</th>
      <td>55</td>
      <td>52</td>
      <td>2124.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>49</td>
      <td>58</td>
      <td>-5481.0</td>
      <td>-9</td>
    </tr>
    <tr>
      <th>7</th>
      <td>46</td>
      <td>61</td>
      <td>-11855.0</td>
      <td>-15</td>
    </tr>
    <tr>
      <th>8</th>
      <td>51</td>
      <td>56</td>
      <td>1133.0</td>
      <td>-5</td>
    </tr>
    <tr>
      <th>9</th>
      <td>49</td>
      <td>58</td>
      <td>-8344.0</td>
      <td>-9</td>
    </tr>
    <tr>
      <th>10</th>
      <td>46</td>
      <td>61</td>
      <td>1360.0</td>
      <td>-15</td>
    </tr>
    <tr>
      <th>11</th>
      <td>46</td>
      <td>61</td>
      <td>-10044.0</td>
      <td>-15</td>
    </tr>
    <tr>
      <th>12</th>
      <td>45</td>
      <td>62</td>
      <td>-13533.0</td>
      <td>-17</td>
    </tr>
    <tr>
      <th>13</th>
      <td>50</td>
      <td>57</td>
      <td>-990.0</td>
      <td>-7</td>
    </tr>
    <tr>
      <th>14</th>
      <td>54</td>
      <td>53</td>
      <td>-9595.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>15</th>
      <td>52</td>
      <td>55</td>
      <td>-5115.0</td>
      <td>-3</td>
    </tr>
    <tr>
      <th>16</th>
      <td>50</td>
      <td>57</td>
      <td>-5091.0</td>
      <td>-7</td>
    </tr>
    <tr>
      <th>17</th>
      <td>48</td>
      <td>59</td>
      <td>-663.0</td>
      <td>-11</td>
    </tr>
    <tr>
      <th>18</th>
      <td>50</td>
      <td>57</td>
      <td>-8669.0</td>
      <td>-7</td>
    </tr>
    <tr>
      <th>19</th>
      <td>46</td>
      <td>58</td>
      <td>9472.0</td>
      <td>-12</td>
    </tr>
    <tr>
      <th>20</th>
      <td>50</td>
      <td>44</td>
      <td>8302.0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>21</th>
      <td>37</td>
      <td>39</td>
      <td>9098.0</td>
      <td>-2</td>
    </tr>
    <tr>
      <th>22</th>
      <td>23</td>
      <td>15</td>
      <td>4027.0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>23</th>
      <td>6</td>
      <td>3</td>
      <td>464.0</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-e47bfd25-4fa8-42c4-9e36-80f7fecef25f')"
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
        document.querySelector('#df-e47bfd25-4fa8-42c4-9e36-80f7fecef25f button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-e47bfd25-4fa8-42c4-9e36-80f7fecef25f');
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


<div id="df-1b54133c-e0b7-417c-a68c-ce6b2d8860e7">
  <button class="colab-df-quickchart" onclick="quickchart('df-1b54133c-e0b7-417c-a68c-ce6b2d8860e7')"
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
        document.querySelector('#df-1b54133c-e0b7-417c-a68c-ce6b2d8860e7 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>
    </div>
  </div>




Расчет 75 перцентиля для определения выбросов вверх по сумме.  
Создание списка рабочих дней, когда произошли выбросы по сумме


```python
perc75 = np.percentile(df_count['Body', 'sum'], 75)
print(perc75)
df_count = df_count.loc[df_count['Body', 'sum'] > perc75]
day_lst = list(df_count.index)
print(day_lst)
df_count
```

    4461.0
    [1, 3, 4, 19, 20, 21]
    





  <div id="df-58bb2c5e-9384-4f03-b2d0-1f61e39443bd" class="colab-df-container">
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
      <th>Up-Down</th>
    </tr>
    <tr>
      <th></th>
      <th>count</th>
      <th>count</th>
      <th>sum</th>
      <th></th>
    </tr>
    <tr>
      <th>Work_day</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>58</td>
      <td>49</td>
      <td>4895.0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>63</td>
      <td>44</td>
      <td>10507.0</td>
      <td>19</td>
    </tr>
    <tr>
      <th>4</th>
      <td>65</td>
      <td>42</td>
      <td>14409.0</td>
      <td>23</td>
    </tr>
    <tr>
      <th>19</th>
      <td>46</td>
      <td>58</td>
      <td>9472.0</td>
      <td>-12</td>
    </tr>
    <tr>
      <th>20</th>
      <td>50</td>
      <td>44</td>
      <td>8302.0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>21</th>
      <td>37</td>
      <td>39</td>
      <td>9098.0</td>
      <td>-2</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-58bb2c5e-9384-4f03-b2d0-1f61e39443bd')"
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
        document.querySelector('#df-58bb2c5e-9384-4f03-b2d0-1f61e39443bd button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-58bb2c5e-9384-4f03-b2d0-1f61e39443bd');
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


<div id="df-bbbef9cf-9db3-4e7c-bd9a-ba0182ccd968">
  <button class="colab-df-quickchart" onclick="quickchart('df-bbbef9cf-9db3-4e7c-bd9a-ba0182ccd968')"
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
        document.querySelector('#df-bbbef9cf-9db3-4e7c-bd9a-ba0182ccd968 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>
    </div>
  </div>





```python
# df = df.loc[(df['Work_day'] >= 14) | (df['Work_day'] == 2)]  # Выборка по
# df = df.loc[df['Work_day'] >= 14]
# df = df.loc[df['Work_day'] == 2]
df = df.loc[df['Work_day'].isin(day_lst)]
# df = df.loc[(df['Work_day'] == 20) | (df['Work_day'] == 21)]  # Выборка по

df["Body_cum"] = df["Body"].cumsum()
df
```

    <ipython-input-25-921b1204bdfc>:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df["Body_cum"] = df["Body"].cumsum()
    





  <div id="df-e4aa3737-a1a5-4be2-b33d-525f4414f4a2" class="colab-df-container">
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
      <th>0</th>
      <td>2015-01-05</td>
      <td>57860.0</td>
      <td>56424.0</td>
      <td>61483.0</td>
      <td>61101.0</td>
      <td>837896</td>
      <td>19</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>3241.0</td>
      <td>3241.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2015-01-27</td>
      <td>67940.0</td>
      <td>67883.0</td>
      <td>71116.0</td>
      <td>68602.0</td>
      <td>2632638</td>
      <td>4</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>662.0</td>
      <td>3903.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2015-01-28</td>
      <td>68694.0</td>
      <td>67691.0</td>
      <td>69736.0</td>
      <td>69250.0</td>
      <td>2150927</td>
      <td>3</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>556.0</td>
      <td>4459.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2015-01-30</td>
      <td>70497.0</td>
      <td>69153.0</td>
      <td>72900.0</td>
      <td>71819.0</td>
      <td>2909011</td>
      <td>1</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1322.0</td>
      <td>5781.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2015-02-02</td>
      <td>71795.0</td>
      <td>69308.0</td>
      <td>71970.0</td>
      <td>69784.0</td>
      <td>3677420</td>
      <td>19</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>-2011.0</td>
      <td>3770.0</td>
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
      <th>2227</th>
      <td>2023-11-03</td>
      <td>94650.0</td>
      <td>93467.0</td>
      <td>94950.0</td>
      <td>93615.0</td>
      <td>1119360</td>
      <td>20</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>-1035.0</td>
      <td>55244.0</td>
    </tr>
    <tr>
      <th>2228</th>
      <td>2023-11-06</td>
      <td>93620.0</td>
      <td>93162.0</td>
      <td>93865.0</td>
      <td>93545.0</td>
      <td>402120</td>
      <td>19</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>-75.0</td>
      <td>55169.0</td>
    </tr>
    <tr>
      <th>2243</th>
      <td>2023-11-27</td>
      <td>89450.0</td>
      <td>89005.0</td>
      <td>89719.0</td>
      <td>89629.0</td>
      <td>776992</td>
      <td>4</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>179.0</td>
      <td>55348.0</td>
    </tr>
    <tr>
      <th>2244</th>
      <td>2023-11-28</td>
      <td>89628.0</td>
      <td>89230.0</td>
      <td>90262.0</td>
      <td>90067.0</td>
      <td>997493</td>
      <td>3</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>439.0</td>
      <td>55787.0</td>
    </tr>
    <tr>
      <th>2246</th>
      <td>2023-11-30</td>
      <td>89242.0</td>
      <td>88899.0</td>
      <td>90305.0</td>
      <td>90138.0</td>
      <td>1063131</td>
      <td>1</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>896.0</td>
      <td>56683.0</td>
    </tr>
  </tbody>
</table>
<p>595 rows × 11 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-e4aa3737-a1a5-4be2-b33d-525f4414f4a2')"
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
        document.querySelector('#df-e4aa3737-a1a5-4be2-b33d-525f4414f4a2 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-e4aa3737-a1a5-4be2-b33d-525f4414f4a2');
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


<div id="df-a77eb720-27df-4248-893c-4dcc66f28975">
  <button class="colab-df-quickchart" onclick="quickchart('df-a77eb720-27df-4248-893c-4dcc66f28975')"
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
        document.querySelector('#df-a77eb720-27df-4248-893c-4dcc66f28975 button');
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
_ = plt.ylabel('Body_cum')
```


    
![png](Si_stat_day_month_files/Si_stat_day_month_24_0.png)
    

