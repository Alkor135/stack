TaLib библиотека.
Установка и импорт в Colab


```python
# Установка библиотеки TaLib
!wget http://prdownloads.sourceforge.net/ta-lib/ta-lib-0.4.0-src.tar.gz
!tar -xzvf ta-lib-0.4.0-src.tar.gz
%cd ta-lib
!./configure --prefix=/usr
!make
!make install
!pip install Ta-Lib
```


```python
# Импорт TaLib
import talib
```


```python
# Просмотр групп индикаторов
# list(talib.get_function_groups().keys())
```


```python
# Просмотр самих индикаторов
# talib.get_function_groups()
```


```python
# Установка необходимых пакетов
!pip install yfinance
!pip install yahoofinancials
```


```python
# Импорт необходимых библиотек
import pandas as pd
import yfinance as yf
from yahoofinancials import YahooFinancials
```


```python
# Загрузка данных в датафрейм
spy_df = yf.download('SPY')
spy_df.pop('Adj Close')  # Удаляем столбец
spy_df.pop('Volume')
spy_df
```

    [*********************100%***********************]  1 of 1 completed
    





  <div id="df-b49f5849-c8af-4951-869c-880175e87f9a">
    <div class="colab-df-container">
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
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1993-01-29</th>
      <td>43.968750</td>
      <td>43.968750</td>
      <td>43.750000</td>
      <td>43.937500</td>
    </tr>
    <tr>
      <th>1993-02-01</th>
      <td>43.968750</td>
      <td>44.250000</td>
      <td>43.968750</td>
      <td>44.250000</td>
    </tr>
    <tr>
      <th>1993-02-02</th>
      <td>44.218750</td>
      <td>44.375000</td>
      <td>44.125000</td>
      <td>44.343750</td>
    </tr>
    <tr>
      <th>1993-02-03</th>
      <td>44.406250</td>
      <td>44.843750</td>
      <td>44.375000</td>
      <td>44.812500</td>
    </tr>
    <tr>
      <th>1993-02-04</th>
      <td>44.968750</td>
      <td>45.093750</td>
      <td>44.468750</td>
      <td>45.000000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2022-01-14</th>
      <td>461.190002</td>
      <td>465.089996</td>
      <td>459.899994</td>
      <td>464.720001</td>
    </tr>
    <tr>
      <th>2022-01-18</th>
      <td>459.739990</td>
      <td>459.959991</td>
      <td>455.309998</td>
      <td>456.489990</td>
    </tr>
    <tr>
      <th>2022-01-19</th>
      <td>458.130005</td>
      <td>459.609985</td>
      <td>451.459991</td>
      <td>451.750000</td>
    </tr>
    <tr>
      <th>2022-01-20</th>
      <td>453.750000</td>
      <td>458.739990</td>
      <td>444.500000</td>
      <td>446.750000</td>
    </tr>
    <tr>
      <th>2022-01-21</th>
      <td>445.559998</td>
      <td>448.059998</td>
      <td>437.950012</td>
      <td>437.980011</td>
    </tr>
  </tbody>
</table>
<p>7299 rows × 4 columns</p>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-b49f5849-c8af-4951-869c-880175e87f9a')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
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
          document.querySelector('#df-b49f5849-c8af-4951-869c-880175e87f9a button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-b49f5849-c8af-4951-869c-880175e87f9a');
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
  </div>





```python
# Просмотр индикаторов группы Статистика
# talib.get_function_groups()['Statistic Functions']
```


```python
# Вывод справки по индикатору стандартного отклонения
# ?talib.STDDEV()
```


```python
# Вывод справки по индикатору SMA
# ?talib.LINEARREG()
```


```python
# Создание series индикатора STDDEV по данным из dataframe
stddev1 = talib.STDDEV(spy_df['Close'], timeperiod=200, nbdev=1)
stddev2 = talib.STDDEV(spy_df['Close'], timeperiod=200, nbdev=2)
stddev3 = talib.STDDEV(spy_df['Close'], timeperiod=200, nbdev=3)
# Создание series индикатора LINEARREG по данным из dataframe
lr200 = talib.LINEARREG(spy_df['Close'], timeperiod=200)
```


```python
# Преобразование series в dtaframe
# stddev1.to_frame(name='stddev1')
```


```python
# Создание series индикатора STDDEV по данным из dataframe
stddev1 = talib.STDDEV(spy_df['Close'], timeperiod=200, nbdev=1)
stddev2 = talib.STDDEV(spy_df['Close'], timeperiod=200, nbdev=2)
stddev3 = talib.STDDEV(spy_df['Close'], timeperiod=200, nbdev=3)
spy_df
```





  <div id="df-a1359b7c-00e7-4223-9239-2b52dc924734">
    <div class="colab-df-container">
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
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>lr200</th>
      <th>stddev1</th>
      <th>stddev2</th>
      <th>stddev3</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1993-01-29</th>
      <td>43.968750</td>
      <td>43.968750</td>
      <td>43.750000</td>
      <td>43.937500</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1993-02-01</th>
      <td>43.968750</td>
      <td>44.250000</td>
      <td>43.968750</td>
      <td>44.250000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1993-02-02</th>
      <td>44.218750</td>
      <td>44.375000</td>
      <td>44.125000</td>
      <td>44.343750</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1993-02-03</th>
      <td>44.406250</td>
      <td>44.843750</td>
      <td>44.375000</td>
      <td>44.812500</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1993-02-04</th>
      <td>44.968750</td>
      <td>45.093750</td>
      <td>44.468750</td>
      <td>45.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
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
    </tr>
    <tr>
      <th>2022-01-14</th>
      <td>461.190002</td>
      <td>465.089996</td>
      <td>459.899994</td>
      <td>464.720001</td>
      <td>472.946246</td>
      <td>19.709324</td>
      <td>39.418648</td>
      <td>59.127972</td>
    </tr>
    <tr>
      <th>2022-01-18</th>
      <td>459.739990</td>
      <td>459.959991</td>
      <td>455.309998</td>
      <td>456.489990</td>
      <td>472.907191</td>
      <td>19.585512</td>
      <td>39.171024</td>
      <td>58.756535</td>
    </tr>
    <tr>
      <th>2022-01-19</th>
      <td>458.130005</td>
      <td>459.609985</td>
      <td>451.459991</td>
      <td>451.750000</td>
      <td>472.764161</td>
      <td>19.440123</td>
      <td>38.880247</td>
      <td>58.320370</td>
    </tr>
    <tr>
      <th>2022-01-20</th>
      <td>453.750000</td>
      <td>458.739990</td>
      <td>444.500000</td>
      <td>446.750000</td>
      <td>472.519767</td>
      <td>19.285459</td>
      <td>38.570918</td>
      <td>57.856377</td>
    </tr>
    <tr>
      <th>2022-01-21</th>
      <td>445.559998</td>
      <td>448.059998</td>
      <td>437.950012</td>
      <td>437.980011</td>
      <td>472.114588</td>
      <td>19.143575</td>
      <td>38.287150</td>
      <td>57.430724</td>
    </tr>
  </tbody>
</table>
<p>7299 rows × 8 columns</p>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-a1359b7c-00e7-4223-9239-2b52dc924734')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
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
          document.querySelector('#df-a1359b7c-00e7-4223-9239-2b52dc924734 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-a1359b7c-00e7-4223-9239-2b52dc924734');
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
  </div>





```python
# Убираем данные с пустыми значениями
spy_df = spy_df.dropna()
spy_df
```





  <div id="df-f205400f-2445-4432-ab93-fac9569cf716">
    <div class="colab-df-container">
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
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>lr200</th>
      <th>stddev1</th>
      <th>stddev2</th>
      <th>stddev3</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1993-11-11</th>
      <td>46.500000</td>
      <td>46.625000</td>
      <td>46.343750</td>
      <td>46.375000</td>
      <td>46.298988</td>
      <td>0.833193</td>
      <td>1.666386</td>
      <td>2.499579</td>
    </tr>
    <tr>
      <th>1993-11-12</th>
      <td>46.468750</td>
      <td>46.750000</td>
      <td>46.437500</td>
      <td>46.593750</td>
      <td>46.313853</td>
      <td>0.834038</td>
      <td>1.668077</td>
      <td>2.502115</td>
    </tr>
    <tr>
      <th>1993-11-15</th>
      <td>46.687500</td>
      <td>46.687500</td>
      <td>46.437500</td>
      <td>46.562500</td>
      <td>46.330826</td>
      <td>0.836547</td>
      <td>1.673095</td>
      <td>2.509642</td>
    </tr>
    <tr>
      <th>1993-11-16</th>
      <td>46.656250</td>
      <td>46.812500</td>
      <td>46.468750</td>
      <td>46.781250</td>
      <td>46.352733</td>
      <td>0.841278</td>
      <td>1.682556</td>
      <td>2.523834</td>
    </tr>
    <tr>
      <th>1993-11-17</th>
      <td>46.812500</td>
      <td>46.812500</td>
      <td>46.406250</td>
      <td>46.531250</td>
      <td>46.374002</td>
      <td>0.845604</td>
      <td>1.691208</td>
      <td>2.536812</td>
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
    </tr>
    <tr>
      <th>2022-01-14</th>
      <td>461.190002</td>
      <td>465.089996</td>
      <td>459.899994</td>
      <td>464.720001</td>
      <td>472.946246</td>
      <td>19.709324</td>
      <td>39.418648</td>
      <td>59.127972</td>
    </tr>
    <tr>
      <th>2022-01-18</th>
      <td>459.739990</td>
      <td>459.959991</td>
      <td>455.309998</td>
      <td>456.489990</td>
      <td>472.907191</td>
      <td>19.585512</td>
      <td>39.171024</td>
      <td>58.756535</td>
    </tr>
    <tr>
      <th>2022-01-19</th>
      <td>458.130005</td>
      <td>459.609985</td>
      <td>451.459991</td>
      <td>451.750000</td>
      <td>472.764161</td>
      <td>19.440123</td>
      <td>38.880247</td>
      <td>58.320370</td>
    </tr>
    <tr>
      <th>2022-01-20</th>
      <td>453.750000</td>
      <td>458.739990</td>
      <td>444.500000</td>
      <td>446.750000</td>
      <td>472.519767</td>
      <td>19.285459</td>
      <td>38.570918</td>
      <td>57.856377</td>
    </tr>
    <tr>
      <th>2022-01-21</th>
      <td>445.559998</td>
      <td>448.059998</td>
      <td>437.950012</td>
      <td>437.980011</td>
      <td>472.114588</td>
      <td>19.143575</td>
      <td>38.287150</td>
      <td>57.430724</td>
    </tr>
  </tbody>
</table>
<p>7100 rows × 8 columns</p>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-f205400f-2445-4432-ab93-fac9569cf716')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
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
          document.querySelector('#df-f205400f-2445-4432-ab93-fac9569cf716 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-f205400f-2445-4432-ab93-fac9569cf716');
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
  </div>





```python
# Создаем колонки со значениями -1/-2/-3 STD
spy_df = spy_df.assign(STD1=lambda x: x['lr200'] - x['stddev1'],
                       STD2=lambda x: x['lr200'] - x['stddev2'],
                       STD3=lambda x: x['lr200'] - x['stddev3'])
spy_df
```





  <div id="df-f3a6e484-be4f-480b-a397-a6e8cd5cbc16">
    <div class="colab-df-container">
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
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>lr200</th>
      <th>stddev1</th>
      <th>stddev2</th>
      <th>stddev3</th>
      <th>STD1</th>
      <th>STD2</th>
      <th>STD3</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1993-11-11</th>
      <td>46.500000</td>
      <td>46.625000</td>
      <td>46.343750</td>
      <td>46.375000</td>
      <td>46.298988</td>
      <td>0.833193</td>
      <td>1.666386</td>
      <td>2.499579</td>
      <td>45.465795</td>
      <td>44.632602</td>
      <td>43.799408</td>
    </tr>
    <tr>
      <th>1993-11-12</th>
      <td>46.468750</td>
      <td>46.750000</td>
      <td>46.437500</td>
      <td>46.593750</td>
      <td>46.313853</td>
      <td>0.834038</td>
      <td>1.668077</td>
      <td>2.502115</td>
      <td>45.479814</td>
      <td>44.645776</td>
      <td>43.811738</td>
    </tr>
    <tr>
      <th>1993-11-15</th>
      <td>46.687500</td>
      <td>46.687500</td>
      <td>46.437500</td>
      <td>46.562500</td>
      <td>46.330826</td>
      <td>0.836547</td>
      <td>1.673095</td>
      <td>2.509642</td>
      <td>45.494278</td>
      <td>44.657731</td>
      <td>43.821184</td>
    </tr>
    <tr>
      <th>1993-11-16</th>
      <td>46.656250</td>
      <td>46.812500</td>
      <td>46.468750</td>
      <td>46.781250</td>
      <td>46.352733</td>
      <td>0.841278</td>
      <td>1.682556</td>
      <td>2.523834</td>
      <td>45.511455</td>
      <td>44.670178</td>
      <td>43.828900</td>
    </tr>
    <tr>
      <th>1993-11-17</th>
      <td>46.812500</td>
      <td>46.812500</td>
      <td>46.406250</td>
      <td>46.531250</td>
      <td>46.374002</td>
      <td>0.845604</td>
      <td>1.691208</td>
      <td>2.536812</td>
      <td>45.528398</td>
      <td>44.682794</td>
      <td>43.837189</td>
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
      <th>2022-01-14</th>
      <td>461.190002</td>
      <td>465.089996</td>
      <td>459.899994</td>
      <td>464.720001</td>
      <td>472.946246</td>
      <td>19.709324</td>
      <td>39.418648</td>
      <td>59.127972</td>
      <td>453.236922</td>
      <td>433.527598</td>
      <td>413.818274</td>
    </tr>
    <tr>
      <th>2022-01-18</th>
      <td>459.739990</td>
      <td>459.959991</td>
      <td>455.309998</td>
      <td>456.489990</td>
      <td>472.907191</td>
      <td>19.585512</td>
      <td>39.171024</td>
      <td>58.756535</td>
      <td>453.321679</td>
      <td>433.736167</td>
      <td>414.150655</td>
    </tr>
    <tr>
      <th>2022-01-19</th>
      <td>458.130005</td>
      <td>459.609985</td>
      <td>451.459991</td>
      <td>451.750000</td>
      <td>472.764161</td>
      <td>19.440123</td>
      <td>38.880247</td>
      <td>58.320370</td>
      <td>453.324038</td>
      <td>433.883914</td>
      <td>414.443791</td>
    </tr>
    <tr>
      <th>2022-01-20</th>
      <td>453.750000</td>
      <td>458.739990</td>
      <td>444.500000</td>
      <td>446.750000</td>
      <td>472.519767</td>
      <td>19.285459</td>
      <td>38.570918</td>
      <td>57.856377</td>
      <td>453.234308</td>
      <td>433.948849</td>
      <td>414.663390</td>
    </tr>
    <tr>
      <th>2022-01-21</th>
      <td>445.559998</td>
      <td>448.059998</td>
      <td>437.950012</td>
      <td>437.980011</td>
      <td>472.114588</td>
      <td>19.143575</td>
      <td>38.287150</td>
      <td>57.430724</td>
      <td>452.971013</td>
      <td>433.827438</td>
      <td>414.683863</td>
    </tr>
  </tbody>
</table>
<p>7100 rows × 11 columns</p>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-f3a6e484-be4f-480b-a397-a6e8cd5cbc16')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
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
          document.querySelector('#df-f3a6e484-be4f-480b-a397-a6e8cd5cbc16 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-f3a6e484-be4f-480b-a397-a6e8cd5cbc16');
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
  </div>





```python
import datetime
```


```python
increment_is_completed = False  # Приращение депо в этом месяце (False-не было, True-было)
dete_increment: int = 15  # Дата увеличения депозита
prev_bar_month: int = 0  # Месяц предыдущего бара

end_index = None
# Перебор строк DF, как приход нового бара
for row in spy_df.tail().itertuples():  # Перебор строк DF
    cur_bar_day: int = datetime.date.timetuple(row[0])[2]
    cur_bar_month: int = datetime.date.timetuple(row[0])[1]

    # Если месяц предыдущего бара не равен месяцу текущего бара
    if cur_bar_month != prev_bar_month:
        increment_is_completed = False

    print(f'Месяц: {cur_bar_month}, '
          f'день: {cur_bar_day}, '
          f'даты: {row[0]}')
    
    end_index = row[0]
    prev_bar_month = cur_bar_month

df2 = spy_df[:end_index].tail(2).head(1)
print(df2)

date = df2.index.date  # Получение даты из индекса
print(date)
# print(datetime.datetime.strftime(date))

# print(df2.iloc[0, 0])
```

    Месяц: 1, день: 14, даты: 2022-01-14 00:00:00
    Месяц: 1, день: 18, даты: 2022-01-18 00:00:00
    Месяц: 1, день: 19, даты: 2022-01-19 00:00:00
    Месяц: 1, день: 20, даты: 2022-01-20 00:00:00
    Месяц: 1, день: 21, даты: 2022-01-21 00:00:00
                  Open       High    Low  ...        STD1        STD2       STD3
    Date                                  ...                                   
    2022-01-20  453.75  458.73999  444.5  ...  453.234308  433.948849  414.66339
    
    [1 rows x 11 columns]
    [datetime.date(2022, 1, 20)]
    
