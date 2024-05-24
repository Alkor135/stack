# MOEX ISS загрузка данных по биожевым котировкам.

## 1. Фьючерсы

### 1.1. Получение информации по торгуемым фьючерсам на заданную дату


```python
pip install apimoex  # Инсталяция библиотеки для запросов к серверу MOEX
```

    Collecting apimoex
      Downloading apimoex-1.3.0-py3-none-any.whl (11 kB)
    Requirement already satisfied: requests in /usr/local/lib/python3.10/dist-packages (from apimoex) (2.31.0)
    Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.10/dist-packages (from requests->apimoex) (3.3.2)
    Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.10/dist-packages (from requests->apimoex) (3.6)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.10/dist-packages (from requests->apimoex) (2.0.7)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.10/dist-packages (from requests->apimoex) (2023.11.17)
    Installing collected packages: apimoex
    Successfully installed apimoex-1.3.0
    


```python
import apimoex
import pandas as pd
import requests
```

Возьмем дату для загрузки информации по торгуемым фьючерсам RTS.


```python
trade_date: str = '2023-11-08'  # Дата за которую будем получать информацию
tiker: str = 'RTS'

# trade_date: str = '08.11.2023'  # Дата за которую будем получать информацию
# trade_date = datetime.strptime(trade_date, "%d.%m.%Y").date()  # Меняем тип у даты
```

В arguments выберем все поля которые нам может прислать в ответе сервер.
Сформируем строку запроса к серверу.


```python
arguments = {'securities.columns': ("BOARDID, TRADEDATE, SECID, OPEN, LOW, "
             "HIGH, CLOSE, OPENPOSITIONVALUE, VALUE, VOLUME, OPENPOSITION, "
             "SETTLEPRICE")}
request_url = (f'http://iss.moex.com/iss/history/engines/futures/markets/forts/securities.json?'
               f'date={trade_date}&assetcode={tiker}')
print(f'{request_url=}')
```

    request_url='http://iss.moex.com/iss/history/engines/futures/markets/forts/securities.json?date=2023-11-08&assetcode=RTS'
    

Делаем запрос к серверу и выводим ответ.


```python
with requests.Session() as session:
  iss = apimoex.ISSClient(session, request_url, arguments)
  data = iss.get()
  df = pd.DataFrame(data['history'])
df
```





  <div id="df-984a94d1-a21a-47a9-b699-5cb3de6cdbf4" class="colab-df-container">
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
      <th>BOARDID</th>
      <th>TRADEDATE</th>
      <th>SECID</th>
      <th>OPEN</th>
      <th>LOW</th>
      <th>HIGH</th>
      <th>CLOSE</th>
      <th>OPENPOSITIONVALUE</th>
      <th>VALUE</th>
      <th>VOLUME</th>
      <th>OPENPOSITION</th>
      <th>SETTLEPRICE</th>
      <th>SWAPRATE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIH4</td>
      <td>108530.0</td>
      <td>107760.0</td>
      <td>109500.0</td>
      <td>109500.0</td>
      <td>4.586878e+08</td>
      <td>7.419192e+07</td>
      <td>371.0</td>
      <td>2280</td>
      <td>109500.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIH4RIM4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000e+00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>0.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIH5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.900099e+06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14</td>
      <td>112750.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIM4</td>
      <td>109300.0</td>
      <td>109300.0</td>
      <td>109500.0</td>
      <td>109500.0</td>
      <td>2.373911e+07</td>
      <td>1.807854e+06</td>
      <td>9.0</td>
      <td>118</td>
      <td>109500.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIM4RIU4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000e+00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>100.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIM5</td>
      <td>110300.0</td>
      <td>110300.0</td>
      <td>110300.0</td>
      <td>110300.0</td>
      <td>2.674963e+07</td>
      <td>2.026487e+05</td>
      <td>1.0</td>
      <td>132</td>
      <td>110300.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIU4</td>
      <td>109600.0</td>
      <td>109600.0</td>
      <td>109600.0</td>
      <td>109600.0</td>
      <td>1.047086e+07</td>
      <td>2.013626e+05</td>
      <td>1.0</td>
      <td>52</td>
      <td>109600.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIU4RIZ4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000e+00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>400.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIU5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000e+00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>116000.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIZ3</td>
      <td>109450.0</td>
      <td>108530.0</td>
      <td>110400.0</td>
      <td>110330.0</td>
      <td>1.784279e+10</td>
      <td>1.160192e+10</td>
      <td>57614.0</td>
      <td>88016</td>
      <td>110340.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIZ3RIH4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000e+00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>-840.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIZ4</td>
      <td>108660.0</td>
      <td>108660.0</td>
      <td>110000.0</td>
      <td>110000.0</td>
      <td>1.576360e+07</td>
      <td>4.017331e+05</td>
      <td>2.0</td>
      <td>78</td>
      <td>110000.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-984a94d1-a21a-47a9-b699-5cb3de6cdbf4')"
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
        document.querySelector('#df-984a94d1-a21a-47a9-b699-5cb3de6cdbf4 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-984a94d1-a21a-47a9-b699-5cb3de6cdbf4');
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


<div id="df-9726d1fa-1f5e-4d92-b8d6-2085a12c7548">
  <button class="colab-df-quickchart" onclick="quickchart('df-9726d1fa-1f5e-4d92-b8d6-2085a12c7548')"
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
        document.querySelector('#df-9726d1fa-1f5e-4d92-b8d6-2085a12c7548 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>
    </div>
  </div>




Получили таблицу со всеми торгуемыми фьючерсами по RTS на выбранную дату.  
Цель получить тикер фьючерса с ближайшей датой истечения, но в татлице данных по дате истечения нет.  
Меняем запрос для создания поля с датой истечения.


```python
def get_info_future(session, security):
    """
    Запрашивает у MOEX информацию по инструменту
    :param session: Подключение к MOEX
    :param security: Тикер инструмента
    :return: Дата последних торгов (если её нет то возвращает 2130.01.01), короткое имя
    """
    security_info = apimoex.find_security_description(session, security)
    df = pd.DataFrame(security_info)

    if len(df) != 0:
        # Удаляем строку с 'DELIVERYTYPE', слишком длинная
        df.drop(df[(df['name'] == 'DELIVERYTYPE')].index, inplace=True)
        name_lst = list(df['name'])  # Поле 'name' в список
        if all(x in name_lst for x in ['SHORTNAME', 'LSTTRADE']):
            df = df[df['name'].isin(['SHORTNAME', 'LSTTRADE'])]  # Выборка необходимых строк
            rez_lst: list = list(df.value)  # Колонку 'value' в список
            return pd.Series(rez_lst)
        elif all(x in name_lst for x in ['SHORTNAME', 'LSTDELDATE']):
            df = df[df['name'].isin(['SHORTNAME', 'LSTDELDATE'])]  # Выборка необходимых строк
            rez_lst: list = list(df.value)  # Колонку 'value' в список
            return pd.Series(rez_lst)
        else:
            return pd.Series([float('nan'), '2130-01-01'])
    else:
        return pd.Series([float('nan'), '2130-01-01'])

with requests.Session() as session:
  iss = apimoex.ISSClient(session, request_url, arguments)
  data = iss.get()
  df = pd.DataFrame(data['history'])
  if len(df) != 0:  # Если полученный ответ не нулевой, чтобы исключить выходные дни
    # Создаем новые колонки 'SHORTNAME', 'LSTTRADE' и заполняем
    df[['SHORTNAME', 'LSTTRADE']] = df.apply(lambda x: get_info_future(session, x['SECID']), axis=1)
    df["LSTTRADE"] = pd.to_datetime(df["LSTTRADE"]).dt.date
df
```





  <div id="df-bd56dae9-c0aa-4225-bd0e-2981149fc124" class="colab-df-container">
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
      <th>BOARDID</th>
      <th>TRADEDATE</th>
      <th>SECID</th>
      <th>OPEN</th>
      <th>LOW</th>
      <th>HIGH</th>
      <th>CLOSE</th>
      <th>OPENPOSITIONVALUE</th>
      <th>VALUE</th>
      <th>VOLUME</th>
      <th>OPENPOSITION</th>
      <th>SETTLEPRICE</th>
      <th>SWAPRATE</th>
      <th>SHORTNAME</th>
      <th>LSTTRADE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIH4</td>
      <td>108530.0</td>
      <td>107760.0</td>
      <td>109500.0</td>
      <td>109500.0</td>
      <td>4.586878e+08</td>
      <td>7.419192e+07</td>
      <td>371.0</td>
      <td>2280</td>
      <td>109500.0</td>
      <td>0.0</td>
      <td>RTS-3.24</td>
      <td>2024-03-21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIH4RIM4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000e+00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2130-01-01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIH5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.900099e+06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14</td>
      <td>112750.0</td>
      <td>0.0</td>
      <td>RTS-3.25</td>
      <td>2025-03-20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIM4</td>
      <td>109300.0</td>
      <td>109300.0</td>
      <td>109500.0</td>
      <td>109500.0</td>
      <td>2.373911e+07</td>
      <td>1.807854e+06</td>
      <td>9.0</td>
      <td>118</td>
      <td>109500.0</td>
      <td>0.0</td>
      <td>RTS-6.24</td>
      <td>2024-06-20</td>
    </tr>
    <tr>
      <th>4</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIM4RIU4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000e+00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2130-01-01</td>
    </tr>
    <tr>
      <th>5</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIM5</td>
      <td>110300.0</td>
      <td>110300.0</td>
      <td>110300.0</td>
      <td>110300.0</td>
      <td>2.674963e+07</td>
      <td>2.026487e+05</td>
      <td>1.0</td>
      <td>132</td>
      <td>110300.0</td>
      <td>0.0</td>
      <td>RTS-6.25</td>
      <td>2025-06-19</td>
    </tr>
    <tr>
      <th>6</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIU4</td>
      <td>109600.0</td>
      <td>109600.0</td>
      <td>109600.0</td>
      <td>109600.0</td>
      <td>1.047086e+07</td>
      <td>2.013626e+05</td>
      <td>1.0</td>
      <td>52</td>
      <td>109600.0</td>
      <td>0.0</td>
      <td>RTS-9.24</td>
      <td>2024-09-19</td>
    </tr>
    <tr>
      <th>7</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIU4RIZ4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000e+00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>400.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2130-01-01</td>
    </tr>
    <tr>
      <th>8</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIU5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000e+00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>116000.0</td>
      <td>0.0</td>
      <td>RTS-9.25</td>
      <td>2025-09-18</td>
    </tr>
    <tr>
      <th>9</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIZ3</td>
      <td>109450.0</td>
      <td>108530.0</td>
      <td>110400.0</td>
      <td>110330.0</td>
      <td>1.784279e+10</td>
      <td>1.160192e+10</td>
      <td>57614.0</td>
      <td>88016</td>
      <td>110340.0</td>
      <td>0.0</td>
      <td>RTS-12.23</td>
      <td>2023-12-21</td>
    </tr>
    <tr>
      <th>10</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIZ3RIH4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000e+00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>-840.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2130-01-01</td>
    </tr>
    <tr>
      <th>11</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIZ4</td>
      <td>108660.0</td>
      <td>108660.0</td>
      <td>110000.0</td>
      <td>110000.0</td>
      <td>1.576360e+07</td>
      <td>4.017331e+05</td>
      <td>2.0</td>
      <td>78</td>
      <td>110000.0</td>
      <td>0.0</td>
      <td>RTS-12.24</td>
      <td>2024-12-19</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-bd56dae9-c0aa-4225-bd0e-2981149fc124')"
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
        document.querySelector('#df-bd56dae9-c0aa-4225-bd0e-2981149fc124 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-bd56dae9-c0aa-4225-bd0e-2981149fc124');
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


<div id="df-84325ce5-51ad-4710-84a9-55ef40c10459">
  <button class="colab-df-quickchart" onclick="quickchart('df-84325ce5-51ad-4710-84a9-55ef40c10459')"
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
        document.querySelector('#df-84325ce5-51ad-4710-84a9-55ef40c10459 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>
    </div>
  </div>




Теперь когда в таблице есть поле с датой последних торгов, фильтруем таблицу по минимальной дате в этом поле.


```python
df = (df[df['LSTTRADE'] == df['LSTTRADE'].min()]).reset_index(drop=True)
df
```





  <div id="df-6b490148-7c66-4605-9911-9211b0dc2bf6" class="colab-df-container">
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
      <th>BOARDID</th>
      <th>TRADEDATE</th>
      <th>SECID</th>
      <th>OPEN</th>
      <th>LOW</th>
      <th>HIGH</th>
      <th>CLOSE</th>
      <th>OPENPOSITIONVALUE</th>
      <th>VALUE</th>
      <th>VOLUME</th>
      <th>OPENPOSITION</th>
      <th>SETTLEPRICE</th>
      <th>SWAPRATE</th>
      <th>SHORTNAME</th>
      <th>LSTTRADE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RFUD</td>
      <td>2023-11-08</td>
      <td>RIZ3</td>
      <td>109450.0</td>
      <td>108530.0</td>
      <td>110400.0</td>
      <td>110330.0</td>
      <td>1.784279e+10</td>
      <td>1.160192e+10</td>
      <td>57614.0</td>
      <td>88016</td>
      <td>110340.0</td>
      <td>0.0</td>
      <td>RTS-12.23</td>
      <td>2023-12-21</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-6b490148-7c66-4605-9911-9211b0dc2bf6')"
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
        document.querySelector('#df-6b490148-7c66-4605-9911-9211b0dc2bf6 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-6b490148-7c66-4605-9911-9211b0dc2bf6');
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
  </div>




### 1.2. Получить свечи заданного интервала в минутах.

https://iss.moex.com/iss/engines/futures/markets/forts/securities/riz3/candles?from=2023-11-08&till=2023-11-10&interval=1  # Свечи по фьючерсу


```python
with requests.Session() as session:
    data = apimoex.get_market_candles(
        session=session,
        security="RIZ3",
        market="forts",
        engine="futures",
        interval=1,
        start = trade_date,
        end = trade_date,
        columns = ('begin', 'open', 'close', 'high', 'low', 'volume',),
    )
    df = pd.DataFrame(data)
    # df.set_index("begin", inplace=True)
    # print(df.head(), "\n")
    # print(df.tail(), "\n")
    # df.info()
    print(df)
```

                       begin    open   close    high     low  volume
    0    2023-11-08 08:59:00  108860  108860  108860  108860       2
    1    2023-11-08 09:00:00  108890  108970  109000  108860     240
    2    2023-11-08 09:01:00  108970  109050  109060  108960     139
    3    2023-11-08 09:02:00  109060  109010  109060  109000      76
    4    2023-11-08 09:03:00  109010  109060  109060  108990     103
    ..                   ...     ...     ...     ...     ...     ...
    846  2023-11-08 23:45:00  110020  110010  110020  110010       4
    847  2023-11-08 23:46:00  110030  110010  110030  110010       6
    848  2023-11-08 23:47:00  110000  110010  110010  110000       7
    849  2023-11-08 23:48:00  110000  109990  110000  109950      54
    850  2023-11-08 23:49:00  109980  109960  109980  109940      80
    
    [851 rows x 6 columns]
    


```python
df = df.loc[(df['open'] == 109450) | (df['close'] == 110330)]
print(df)
```

                       begin    open   close    high     low  volume
    585  2023-11-08 18:49:00  110320  110330  110330  110270     208
    

Предыдущий день.


```python
from datetime import datetime, timedelta

with requests.Session() as session:
    data = apimoex.get_market_candles(
        session=session,
        security="RIZ3",
        market="forts",
        engine="futures",
        interval=1,
        start = (datetime.strptime(f'{trade_date}', "%Y-%m-%d") - timedelta(days=1)).strftime("%Y-%m-%d"),
        end = (datetime.strptime(f'{trade_date}', "%Y-%m-%d") - timedelta(days=1)).strftime("%Y-%m-%d"),
    )
    df = pd.DataFrame(data)
    print(df)
```

                       begin    open   close    high     low  value
    0    2023-11-07 08:59:00  109200  109300  109300  109200      0
    1    2023-11-07 09:00:00  109190  109180  109190  109100      0
    2    2023-11-07 09:01:00  109160  109090  109160  109080      0
    3    2023-11-07 09:02:00  109090  109120  109120  109080      0
    4    2023-11-07 09:03:00  109120  109130  109170  109110      0
    ..                   ...     ...     ...     ...     ...    ...
    829  2023-11-07 23:45:00  108860  108890  108890  108860      0
    830  2023-11-07 23:46:00  108880  108880  108880  108880      0
    831  2023-11-07 23:47:00  108880  108860  108880  108860      0
    832  2023-11-07 23:48:00  108860  108880  108880  108850      0
    833  2023-11-07 23:49:00  108850  108890  108900  108850      0
    
    [834 rows x 6 columns]
    


```python
df = df.loc[(df['open'] == 109450) | (df['close'] == 110330)]
print(df)
```

                       begin    open   close    high     low  value
    545  2023-11-07 18:12:00  109450  109460  109480  109420      0
    550  2023-11-07 18:17:00  109450  109410  109460  109410      0
    583  2023-11-07 19:05:00  109450  109430  109470  109350      0
    

### Выводы

Дневные котировки на MOEX за дату храняться с 19:05 предыдущего дня и до 18:49
