# Тестовая загрузка данных с Yahoo Finance


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
# Занрузка данных в датафрейм
spy_df = yf.download('SPY', 
                      start='1994-01-01', 
                      end='2021-12-31', 
                      progress=False,
)
spy_df
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
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Adj Close</th>
      <th>Volume</th>
    </tr>
    <tr>
      <th>Date</th>
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
      <th>1994-01-03</th>
      <td>46.593750</td>
      <td>46.656250</td>
      <td>46.406250</td>
      <td>46.468750</td>
      <td>27.340992</td>
      <td>960900</td>
    </tr>
    <tr>
      <th>1994-01-04</th>
      <td>46.531250</td>
      <td>46.656250</td>
      <td>46.468750</td>
      <td>46.656250</td>
      <td>27.451292</td>
      <td>164300</td>
    </tr>
    <tr>
      <th>1994-01-05</th>
      <td>46.718750</td>
      <td>46.781250</td>
      <td>46.531250</td>
      <td>46.750000</td>
      <td>27.506447</td>
      <td>710900</td>
    </tr>
    <tr>
      <th>1994-01-06</th>
      <td>46.812500</td>
      <td>46.843750</td>
      <td>46.687500</td>
      <td>46.750000</td>
      <td>27.506447</td>
      <td>201000</td>
    </tr>
    <tr>
      <th>1994-01-07</th>
      <td>46.843750</td>
      <td>47.062500</td>
      <td>46.718750</td>
      <td>47.031250</td>
      <td>27.671942</td>
      <td>775500</td>
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
      <th>2021-12-23</th>
      <td>468.750000</td>
      <td>472.190002</td>
      <td>468.640015</td>
      <td>470.600006</td>
      <td>463.087219</td>
      <td>56439700</td>
    </tr>
    <tr>
      <th>2021-12-27</th>
      <td>472.059998</td>
      <td>477.309998</td>
      <td>472.010010</td>
      <td>477.260010</td>
      <td>469.640900</td>
      <td>56808600</td>
    </tr>
    <tr>
      <th>2021-12-28</th>
      <td>477.720001</td>
      <td>478.809998</td>
      <td>476.059998</td>
      <td>476.869995</td>
      <td>469.257050</td>
      <td>47274600</td>
    </tr>
    <tr>
      <th>2021-12-29</th>
      <td>476.980011</td>
      <td>478.559998</td>
      <td>475.920013</td>
      <td>477.480011</td>
      <td>469.857361</td>
      <td>54503000</td>
    </tr>
    <tr>
      <th>2021-12-30</th>
      <td>477.929993</td>
      <td>479.000000</td>
      <td>475.670013</td>
      <td>476.160004</td>
      <td>468.558472</td>
      <td>55329000</td>
    </tr>
  </tbody>
</table>
<p>7050 rows × 6 columns</p>
</div>


