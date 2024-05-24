# Статистика по ТП СЛ для одного файла с сортировкой по часам

Монтирование гугл диска


```python
from google.colab import drive
drive.mount('/content/drive')
```

    Mounted at /content/drive
    

Импортирование библиотек


```python
import pandas as pd
from pathlib import *
import numpy as np
import matplotlib.pyplot as plt

# plt.style.use('fivethirteight')
```

Загрузка файла в DataFrame


```python
source_file: Path = Path(fr'/content/drive/MyDrive/data_quote/data_prepare_RTS_range_mvc_tpsl_sec/SPFB.RTS_00_range250_splice_2021.txt')

df = pd.read_csv(source_file, delimiter=',')  # Считываем данные в DF
df
```





  <div id="df-3e1066a0-01fa-47eb-8ee6-3041fa7dc468">
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
      <th>&lt;DATE&gt;</th>
      <th>&lt;TIME&gt;</th>
      <th>&lt;OPEN&gt;</th>
      <th>&lt;HIGH&gt;</th>
      <th>&lt;LOW&gt;</th>
      <th>&lt;CLOSE&gt;</th>
      <th>&lt;VOL&gt;</th>
      <th>&lt;MAX_VOLUME_PRICE&gt;</th>
      <th>&lt;MAX_VOLUME_CLUSTER&gt;</th>
      <th>&lt;TP_SL&gt;</th>
      <th>&lt;SEC&gt;</th>
      <th>&lt;PER&gt;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20210104</td>
      <td>100000</td>
      <td>139240.0</td>
      <td>139490.0</td>
      <td>139240.0</td>
      <td>139490.0</td>
      <td>413</td>
      <td>140050.0</td>
      <td>143</td>
      <td>3</td>
      <td>1</td>
      <td>3.24</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20210104</td>
      <td>100001</td>
      <td>140130.0</td>
      <td>140000.0</td>
      <td>139750.0</td>
      <td>140000.0</td>
      <td>884</td>
      <td>140060.0</td>
      <td>128</td>
      <td>3</td>
      <td>1</td>
      <td>1.24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20210104</td>
      <td>100002</td>
      <td>140390.0</td>
      <td>140600.0</td>
      <td>140350.0</td>
      <td>140600.0</td>
      <td>984</td>
      <td>140450.0</td>
      <td>119</td>
      <td>3</td>
      <td>3</td>
      <td>0.40</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20210104</td>
      <td>100005</td>
      <td>140610.0</td>
      <td>140780.0</td>
      <td>140530.0</td>
      <td>140780.0</td>
      <td>336</td>
      <td>140560.0</td>
      <td>52</td>
      <td>3</td>
      <td>2</td>
      <td>0.12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20210104</td>
      <td>100007</td>
      <td>140790.0</td>
      <td>141040.0</td>
      <td>140790.0</td>
      <td>141040.0</td>
      <td>853</td>
      <td>141000.0</td>
      <td>424</td>
      <td>3</td>
      <td>1</td>
      <td>0.84</td>
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
      <th>32310</th>
      <td>20211230</td>
      <td>210209</td>
      <td>160000.0</td>
      <td>160060.0</td>
      <td>159810.0</td>
      <td>160060.0</td>
      <td>6291</td>
      <td>159900.0</td>
      <td>562</td>
      <td>-1</td>
      <td>2648</td>
      <td>0.36</td>
    </tr>
    <tr>
      <th>32311</th>
      <td>20211230</td>
      <td>214617</td>
      <td>160070.0</td>
      <td>160080.0</td>
      <td>159830.0</td>
      <td>159830.0</td>
      <td>1883</td>
      <td>159990.0</td>
      <td>198</td>
      <td>0</td>
      <td>751</td>
      <td>0.36</td>
    </tr>
    <tr>
      <th>32312</th>
      <td>20211230</td>
      <td>215848</td>
      <td>159820.0</td>
      <td>159990.0</td>
      <td>159740.0</td>
      <td>159990.0</td>
      <td>5732</td>
      <td>159860.0</td>
      <td>682</td>
      <td>-1</td>
      <td>3645</td>
      <td>0.48</td>
    </tr>
    <tr>
      <th>32313</th>
      <td>20211230</td>
      <td>225933</td>
      <td>160000.0</td>
      <td>160030.0</td>
      <td>159780.0</td>
      <td>159780.0</td>
      <td>6881</td>
      <td>159930.0</td>
      <td>856</td>
      <td>0</td>
      <td>2552</td>
      <td>0.40</td>
    </tr>
    <tr>
      <th>32314</th>
      <td>20211230</td>
      <td>234205</td>
      <td>159770.0</td>
      <td>159950.0</td>
      <td>159710.0</td>
      <td>159760.0</td>
      <td>3365</td>
      <td>159750.0</td>
      <td>273</td>
      <td>0</td>
      <td>900</td>
      <td>2.00</td>
    </tr>
  </tbody>
</table>
<p>32315 rows × 12 columns</p>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-3e1066a0-01fa-47eb-8ee6-3041fa7dc468')"
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
          document.querySelector('#df-3e1066a0-01fa-47eb-8ee6-3041fa7dc468 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-3e1066a0-01fa-47eb-8ee6-3041fa7dc468');
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




График


```python
plt.figure(figsize = (13,5))
plt.plot(df['<CLOSE>'], label = source_file.name)
plt.xlabel('2021')
plt.ylabel('Price')
plt.legend(loc = 'upper left')
plt.show()
```


    
![png](%D0%A1%D1%82%D0%B0%D1%82%D0%B8%D1%81%D1%82%D0%B8%D0%BA%D0%B0%20%D0%BF%D0%BE%20%D0%A2%D0%9F%20%D0%A1%D0%9B%20%D0%B4%D0%BB%D1%8F%20%D0%BE%D0%B4%D0%BD%D0%BE%D0%B3%D0%BE%20%D1%80%D0%B5%D0%BD%D0%B4%D0%B6%20%D1%84%D0%B0%D0%B9%D0%BB%D0%B0%20%D1%81%20EMA18%20CCI50%20_files/%D0%A1%D1%82%D0%B0%D1%82%D0%B8%D1%81%D1%82%D0%B8%D0%BA%D0%B0%20%D0%BF%D0%BE%20%D0%A2%D0%9F%20%D0%A1%D0%9B%20%D0%B4%D0%BB%D1%8F%20%D0%BE%D0%B4%D0%BD%D0%BE%D0%B3%D0%BE%20%D1%80%D0%B5%D0%BD%D0%B4%D0%B6%20%D1%84%D0%B0%D0%B9%D0%BB%D0%B0%20%D1%81%20EMA18%20CCI50%20_8_0.png)
    


Установка и инсталирование библиотеки TaLib


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
# Просмотр индикаторов группы Overlap Studies
# talib.get_function_groups()['Overlap Studies']
```


```python
# Просмотр индикаторов группы Momentum Indicators
# talib.get_function_groups()['Momentum Indicators']
```


```python
# Вывод справки по индикатору EMA
# ?talib.EMA()
```

Создание колонки со значениями от индикатора EMA с периодом 18


```python
df['<EMA18>'] = talib.EMA(df['<CLOSE>'], timeperiod=18)
# df
```

График


```python
plt.figure(figsize = (13,5))
plt.plot(df['<CLOSE>'], label = source_file.name)
plt.plot(df['<EMA18>'], label = 'EMA18')
plt.xlabel('2021')
plt.ylabel('Price')
plt.legend(loc = 'upper left')
plt.show()
```


    
![png](%D0%A1%D1%82%D0%B0%D1%82%D0%B8%D1%81%D1%82%D0%B8%D0%BA%D0%B0%20%D0%BF%D0%BE%20%D0%A2%D0%9F%20%D0%A1%D0%9B%20%D0%B4%D0%BB%D1%8F%20%D0%BE%D0%B4%D0%BD%D0%BE%D0%B3%D0%BE%20%D1%80%D0%B5%D0%BD%D0%B4%D0%B6%20%D1%84%D0%B0%D0%B9%D0%BB%D0%B0%20%D1%81%20EMA18%20CCI50%20_files/%D0%A1%D1%82%D0%B0%D1%82%D0%B8%D1%81%D1%82%D0%B8%D0%BA%D0%B0%20%D0%BF%D0%BE%20%D0%A2%D0%9F%20%D0%A1%D0%9B%20%D0%B4%D0%BB%D1%8F%20%D0%BE%D0%B4%D0%BD%D0%BE%D0%B3%D0%BE%20%D1%80%D0%B5%D0%BD%D0%B4%D0%B6%20%D1%84%D0%B0%D0%B9%D0%BB%D0%B0%20%D1%81%20EMA18%20CCI50%20_20_0.png)
    


Создание маркера бара относительно индикатора EMA18.  
>Если Low выше EMA18, записывается 1.  
>Если High ниже EMA18, записывается -1.  
>В остальных случаях, записывается 0.


```python
from typing import Any
def ema_m(ema18: Any, high: Any, low: Any) -> str:
    """ Функция преобразует EMA18 в маркер для бара"""
    if low > ema18:
      return 1
    elif high < ema18:
      return -1
    else:
      return 0

df['<EMA18_M>'] = df.apply(lambda x: ema_m(x['<EMA18>'], x['<HIGH>'], x['<LOW>']), axis=1)  # 
# df
```

Создание колонки CCI периода 50


```python
df['<CCI50>'] = talib.CCI(df['<HIGH>'], df['<LOW>'], df['<CLOSE>'], timeperiod=50)
# df
```

Создание маркера бара относительно индикатора CCI с периодом 50.  
>Если CCI50 больше 50, записывается 1.  
>Если CCI50 меньше 50, записывается -1.


```python
def cci_m(cci50: Any) -> str:
    """ Функция преобразует CCI с периодом 50 в маркер для бара"""
    if cci50 >= 0:
      return 1
    else:
      return -1

df['<CCI50_M>'] = df.apply(lambda x: cci_m(x['<CCI50>']), axis=1)  # 
df
```





  <div id="df-16da1023-305c-4677-8aa2-22d1465d828c">
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
      <th>&lt;DATE&gt;</th>
      <th>&lt;TIME&gt;</th>
      <th>&lt;OPEN&gt;</th>
      <th>&lt;HIGH&gt;</th>
      <th>&lt;LOW&gt;</th>
      <th>&lt;CLOSE&gt;</th>
      <th>&lt;VOL&gt;</th>
      <th>&lt;MAX_VOLUME_PRICE&gt;</th>
      <th>&lt;MAX_VOLUME_CLUSTER&gt;</th>
      <th>&lt;TP_SL&gt;</th>
      <th>&lt;SEC&gt;</th>
      <th>&lt;PER&gt;</th>
      <th>&lt;EMA18&gt;</th>
      <th>&lt;EMA18_M&gt;</th>
      <th>&lt;CCI50&gt;</th>
      <th>&lt;CCI50_M&gt;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20210104</td>
      <td>100000</td>
      <td>139240.0</td>
      <td>139490.0</td>
      <td>139240.0</td>
      <td>139490.0</td>
      <td>413</td>
      <td>140050.0</td>
      <td>143</td>
      <td>3</td>
      <td>1</td>
      <td>3.24</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20210104</td>
      <td>100001</td>
      <td>140130.0</td>
      <td>140000.0</td>
      <td>139750.0</td>
      <td>140000.0</td>
      <td>884</td>
      <td>140060.0</td>
      <td>128</td>
      <td>3</td>
      <td>1</td>
      <td>1.24</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20210104</td>
      <td>100002</td>
      <td>140390.0</td>
      <td>140600.0</td>
      <td>140350.0</td>
      <td>140600.0</td>
      <td>984</td>
      <td>140450.0</td>
      <td>119</td>
      <td>3</td>
      <td>3</td>
      <td>0.40</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20210104</td>
      <td>100005</td>
      <td>140610.0</td>
      <td>140780.0</td>
      <td>140530.0</td>
      <td>140780.0</td>
      <td>336</td>
      <td>140560.0</td>
      <td>52</td>
      <td>3</td>
      <td>2</td>
      <td>0.12</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20210104</td>
      <td>100007</td>
      <td>140790.0</td>
      <td>141040.0</td>
      <td>140790.0</td>
      <td>141040.0</td>
      <td>853</td>
      <td>141000.0</td>
      <td>424</td>
      <td>3</td>
      <td>1</td>
      <td>0.84</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>-1</td>
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
    </tr>
    <tr>
      <th>32310</th>
      <td>20211230</td>
      <td>210209</td>
      <td>160000.0</td>
      <td>160060.0</td>
      <td>159810.0</td>
      <td>160060.0</td>
      <td>6291</td>
      <td>159900.0</td>
      <td>562</td>
      <td>-1</td>
      <td>2648</td>
      <td>0.36</td>
      <td>159501.030362</td>
      <td>1</td>
      <td>168.609608</td>
      <td>1</td>
    </tr>
    <tr>
      <th>32311</th>
      <td>20211230</td>
      <td>214617</td>
      <td>160070.0</td>
      <td>160080.0</td>
      <td>159830.0</td>
      <td>159830.0</td>
      <td>1883</td>
      <td>159990.0</td>
      <td>198</td>
      <td>0</td>
      <td>751</td>
      <td>0.36</td>
      <td>159535.658745</td>
      <td>1</td>
      <td>148.158640</td>
      <td>1</td>
    </tr>
    <tr>
      <th>32312</th>
      <td>20211230</td>
      <td>215848</td>
      <td>159820.0</td>
      <td>159990.0</td>
      <td>159740.0</td>
      <td>159990.0</td>
      <td>5732</td>
      <td>159860.0</td>
      <td>682</td>
      <td>-1</td>
      <td>3645</td>
      <td>0.48</td>
      <td>159583.484141</td>
      <td>1</td>
      <td>137.842837</td>
      <td>1</td>
    </tr>
    <tr>
      <th>32313</th>
      <td>20211230</td>
      <td>225933</td>
      <td>160000.0</td>
      <td>160030.0</td>
      <td>159780.0</td>
      <td>159780.0</td>
      <td>6881</td>
      <td>159930.0</td>
      <td>856</td>
      <td>0</td>
      <td>2552</td>
      <td>0.40</td>
      <td>159604.170020</td>
      <td>1</td>
      <td>124.014714</td>
      <td>1</td>
    </tr>
    <tr>
      <th>32314</th>
      <td>20211230</td>
      <td>234205</td>
      <td>159770.0</td>
      <td>159950.0</td>
      <td>159710.0</td>
      <td>159760.0</td>
      <td>3365</td>
      <td>159750.0</td>
      <td>273</td>
      <td>0</td>
      <td>900</td>
      <td>2.00</td>
      <td>159620.573176</td>
      <td>1</td>
      <td>109.894751</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>32315 rows × 16 columns</p>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-16da1023-305c-4677-8aa2-22d1465d828c')"
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
          document.querySelector('#df-16da1023-305c-4677-8aa2-22d1465d828c button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-16da1023-305c-4677-8aa2-22d1465d828c');
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




График


```python
plt.figure(figsize = (13,9))
plt.subplot(2, 1, 1)
plt.plot(df['<CLOSE>'], label = 'Price')  # построение графика
plt.title(source_file.name)  # заголовок
plt.ylabel("Price", fontsize=14)  # ось ординат
plt.grid(True)  # включение отображение сетки
plt.subplot(2, 1, 2)
plt.plot(df['<CCI50>'], label = 'CCI 50')  # построение графика
plt.xlabel("2021", fontsize=14)  # ось абсцисс
plt.ylabel("CCI", fontsize=14)  # ось ординат
plt.grid(True)  # включение отображение сетки
plt.show()
```


    
![png](%D0%A1%D1%82%D0%B0%D1%82%D0%B8%D1%81%D1%82%D0%B8%D0%BA%D0%B0%20%D0%BF%D0%BE%20%D0%A2%D0%9F%20%D0%A1%D0%9B%20%D0%B4%D0%BB%D1%8F%20%D0%BE%D0%B4%D0%BD%D0%BE%D0%B3%D0%BE%20%D1%80%D0%B5%D0%BD%D0%B4%D0%B6%20%D1%84%D0%B0%D0%B9%D0%BB%D0%B0%20%D1%81%20EMA18%20CCI50%20_files/%D0%A1%D1%82%D0%B0%D1%82%D0%B8%D1%81%D1%82%D0%B8%D0%BA%D0%B0%20%D0%BF%D0%BE%20%D0%A2%D0%9F%20%D0%A1%D0%9B%20%D0%B4%D0%BB%D1%8F%20%D0%BE%D0%B4%D0%BD%D0%BE%D0%B3%D0%BE%20%D1%80%D0%B5%D0%BD%D0%B4%D0%B6%20%D1%84%D0%B0%D0%B9%D0%BB%D0%B0%20%D1%81%20EMA18%20CCI50%20_28_0.png)
    


Очистка DF


```python
df = df.loc[df['<TP_SL>'] != 0]  # Убираем строки где <TP_SL> равен 0 (конец дня)

# Убираем строки где кластер с макс объемом вне бара (быстрое движение рынка)
df = df.loc[df['<HIGH>'] > df['<MAX_VOLUME_PRICE>']]
df = df.loc[df['<MAX_VOLUME_PRICE>'] > df['<LOW>']]

df = df.dropna()  # Убираем строки где есть NaN

df
```





  <div id="df-c15c27cb-cc14-4af3-a07b-f4a1e03abdd8">
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
      <th>&lt;DATE&gt;</th>
      <th>&lt;TIME&gt;</th>
      <th>&lt;OPEN&gt;</th>
      <th>&lt;HIGH&gt;</th>
      <th>&lt;LOW&gt;</th>
      <th>&lt;CLOSE&gt;</th>
      <th>&lt;VOL&gt;</th>
      <th>&lt;MAX_VOLUME_PRICE&gt;</th>
      <th>&lt;MAX_VOLUME_CLUSTER&gt;</th>
      <th>&lt;TP_SL&gt;</th>
      <th>&lt;SEC&gt;</th>
      <th>&lt;PER&gt;</th>
      <th>&lt;EMA18&gt;</th>
      <th>&lt;EMA18_M&gt;</th>
      <th>&lt;CCI50&gt;</th>
      <th>&lt;CCI50_M&gt;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>20210104</td>
      <td>150042</td>
      <td>143080.0</td>
      <td>143160.0</td>
      <td>142910.0</td>
      <td>142910.0</td>
      <td>5766</td>
      <td>143060.0</td>
      <td>440</td>
      <td>-1</td>
      <td>531</td>
      <td>0.40</td>
      <td>143281.486171</td>
      <td>-1</td>
      <td>39.880022</td>
      <td>1</td>
    </tr>
    <tr>
      <th>50</th>
      <td>20210104</td>
      <td>150933</td>
      <td>142900.0</td>
      <td>143110.0</td>
      <td>142860.0</td>
      <td>142860.0</td>
      <td>3897</td>
      <td>142920.0</td>
      <td>446</td>
      <td>-1</td>
      <td>454</td>
      <td>0.76</td>
      <td>143237.119205</td>
      <td>-1</td>
      <td>33.638523</td>
      <td>1</td>
    </tr>
    <tr>
      <th>51</th>
      <td>20210104</td>
      <td>151707</td>
      <td>142850.0</td>
      <td>143070.0</td>
      <td>142820.0</td>
      <td>143070.0</td>
      <td>6361</td>
      <td>143020.0</td>
      <td>499</td>
      <td>2</td>
      <td>905</td>
      <td>0.80</td>
      <td>143219.527710</td>
      <td>-1</td>
      <td>34.411960</td>
      <td>1</td>
    </tr>
    <tr>
      <th>52</th>
      <td>20210104</td>
      <td>153212</td>
      <td>143080.0</td>
      <td>143150.0</td>
      <td>142900.0</td>
      <td>142900.0</td>
      <td>4180</td>
      <td>143080.0</td>
      <td>352</td>
      <td>-1</td>
      <td>922</td>
      <td>0.28</td>
      <td>143185.893214</td>
      <td>-1</td>
      <td>31.668145</td>
      <td>1</td>
    </tr>
    <tr>
      <th>53</th>
      <td>20210104</td>
      <td>154734</td>
      <td>142890.0</td>
      <td>143070.0</td>
      <td>142820.0</td>
      <td>143070.0</td>
      <td>4448</td>
      <td>142950.0</td>
      <td>427</td>
      <td>2</td>
      <td>882</td>
      <td>0.52</td>
      <td>143173.693929</td>
      <td>-1</td>
      <td>29.505819</td>
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
      <th>32305</th>
      <td>20211230</td>
      <td>193140</td>
      <td>159590.0</td>
      <td>159640.0</td>
      <td>159390.0</td>
      <td>159390.0</td>
      <td>1944</td>
      <td>159550.0</td>
      <td>412</td>
      <td>-1</td>
      <td>638</td>
      <td>0.36</td>
      <td>159209.997260</td>
      <td>1</td>
      <td>137.569629</td>
      <td>1</td>
    </tr>
    <tr>
      <th>32306</th>
      <td>20211230</td>
      <td>194218</td>
      <td>159380.0</td>
      <td>159550.0</td>
      <td>159300.0</td>
      <td>159550.0</td>
      <td>6736</td>
      <td>159400.0</td>
      <td>861</td>
      <td>1</td>
      <td>3287</td>
      <td>0.40</td>
      <td>159245.787022</td>
      <td>1</td>
      <td>129.534127</td>
      <td>1</td>
    </tr>
    <tr>
      <th>32309</th>
      <td>20211230</td>
      <td>204130</td>
      <td>159940.0</td>
      <td>159990.0</td>
      <td>159740.0</td>
      <td>159990.0</td>
      <td>4529</td>
      <td>159820.0</td>
      <td>422</td>
      <td>-1</td>
      <td>1239</td>
      <td>0.32</td>
      <td>159435.269229</td>
      <td>1</td>
      <td>172.136341</td>
      <td>1</td>
    </tr>
    <tr>
      <th>32310</th>
      <td>20211230</td>
      <td>210209</td>
      <td>160000.0</td>
      <td>160060.0</td>
      <td>159810.0</td>
      <td>160060.0</td>
      <td>6291</td>
      <td>159900.0</td>
      <td>562</td>
      <td>-1</td>
      <td>2648</td>
      <td>0.36</td>
      <td>159501.030362</td>
      <td>1</td>
      <td>168.609608</td>
      <td>1</td>
    </tr>
    <tr>
      <th>32312</th>
      <td>20211230</td>
      <td>215848</td>
      <td>159820.0</td>
      <td>159990.0</td>
      <td>159740.0</td>
      <td>159990.0</td>
      <td>5732</td>
      <td>159860.0</td>
      <td>682</td>
      <td>-1</td>
      <td>3645</td>
      <td>0.48</td>
      <td>159583.484141</td>
      <td>1</td>
      <td>137.842837</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>30953 rows × 16 columns</p>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-c15c27cb-cc14-4af3-a07b-f4a1e03abdd8')"
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
          document.querySelector('#df-c15c27cb-cc14-4af3-a07b-f4a1e03abdd8 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-c15c27cb-cc14-4af3-a07b-f4a1e03abdd8');
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




Статистика группировкой по EMA 18


```python
df_ema18 = df.groupby('<EMA18_M>')
df_ema18.first()
```





  <div id="df-b80ee7e5-e253-405d-9bfd-fc511b9389f7">
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
      <th>&lt;DATE&gt;</th>
      <th>&lt;TIME&gt;</th>
      <th>&lt;OPEN&gt;</th>
      <th>&lt;HIGH&gt;</th>
      <th>&lt;LOW&gt;</th>
      <th>&lt;CLOSE&gt;</th>
      <th>&lt;VOL&gt;</th>
      <th>&lt;MAX_VOLUME_PRICE&gt;</th>
      <th>&lt;MAX_VOLUME_CLUSTER&gt;</th>
      <th>&lt;TP_SL&gt;</th>
      <th>&lt;SEC&gt;</th>
      <th>&lt;PER&gt;</th>
      <th>&lt;EMA18&gt;</th>
      <th>&lt;CCI50&gt;</th>
      <th>&lt;CCI50_M&gt;</th>
    </tr>
    <tr>
      <th>&lt;EMA18_M&gt;</th>
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
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>-1</th>
      <td>20210104</td>
      <td>150042</td>
      <td>143080.0</td>
      <td>143160.0</td>
      <td>142910.0</td>
      <td>142910.0</td>
      <td>5766</td>
      <td>143060.0</td>
      <td>440</td>
      <td>-1</td>
      <td>531</td>
      <td>0.40</td>
      <td>143281.486171</td>
      <td>39.880022</td>
      <td>1</td>
    </tr>
    <tr>
      <th>0</th>
      <td>20210104</td>
      <td>160216</td>
      <td>143080.0</td>
      <td>143220.0</td>
      <td>142970.0</td>
      <td>143220.0</td>
      <td>4112</td>
      <td>143060.0</td>
      <td>402</td>
      <td>-1</td>
      <td>565</td>
      <td>0.36</td>
      <td>143178.568252</td>
      <td>41.378152</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20210104</td>
      <td>164538</td>
      <td>143400.0</td>
      <td>143560.0</td>
      <td>143310.0</td>
      <td>143560.0</td>
      <td>5569</td>
      <td>143430.0</td>
      <td>548</td>
      <td>-1</td>
      <td>872</td>
      <td>0.48</td>
      <td>143222.849725</td>
      <td>71.406564</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-b80ee7e5-e253-405d-9bfd-fc511b9389f7')"
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
          document.querySelector('#df-b80ee7e5-e253-405d-9bfd-fc511b9389f7 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-b80ee7e5-e253-405d-9bfd-fc511b9389f7');
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
df_ema18_grp = df.groupby('<EMA18_M>').groups
print(f'Количество баров выше ЕМА 18: {len(df_ema18_grp[1])}')
print(f'Количество баров ниже ЕМА 18: {len(df_ema18_grp[-1])}')
print(f'Количество баров на ЕМА 18: {len(df_ema18_grp[0])}')
```

    Количество баров выше ЕМА 18: 11074
    Количество баров ниже ЕМА 18: 10973
    Количество баров на ЕМА 18: 8906
    

Создание DataFreme для баров на повышение


```python
df_up = df.loc[df['<HIGH>'] == df['<CLOSE>']]  # DF только для баров на повышение
# df_up = df_up.loc[df['<EMA18_M>'] == 1]  # которые выше ЕМА 18
df_up = df_up.loc[df['<CCI50_M>'] == 1]  # CCI 50 положительный
print(f'Относительная статистика ТП/СЛ для баров на повышение, которые выше EMA 18 при положительном CCI 50')
df_up['<TP_SL>'].value_counts(normalize=True)
```

    Относительная статистика ТП/СЛ для баров на повышение, которые выше EMA 18 при положительном CCI 50
    




    -1    0.518514
     3    0.206199
     1    0.179463
     2    0.095824
    Name: <TP_SL>, dtype: float64




```python
print(f'Количественная статистика ТП/СЛ для баров на повышение, которые выше EMA 18 при положительном CCI 50')
df_up['<TP_SL>'].value_counts()
```

    Количественная статистика ТП/СЛ для баров на повышение, которые выше EMA 18 при положительном CCI 50
    




    -1    4383
     3    1743
     1    1517
     2     810
    Name: <TP_SL>, dtype: int64



Статистика ТП/СЛ для файла


```python
print(f'Количественная статистика ТП/СЛ для файла {source_file.name}')
df['<TP_SL>'].value_counts()
```

    Количественная статистика ТП/СЛ для файла SPFB.RTS_00_range250_splice_2021.txt
    




    -1    15921
     3     6934
     1     5331
     2     2767
    Name: <TP_SL>, dtype: int64




```python
print(f'Относительная статистика ТП/СЛ для файла {source_file.name}')
df['<TP_SL>'].value_counts(normalize=True)
```

    Относительная статистика ТП/СЛ для файла SPFB.RTS_00_range250_splice_2021.txt
    




    -1    0.514360
     3    0.224017
     1    0.172229
     2    0.089394
    Name: <TP_SL>, dtype: float64




```python
df_result: pd = pd.DataFrame()

```
