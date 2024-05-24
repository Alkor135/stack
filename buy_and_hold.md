# Инвестиционная стратегия "Купил и Держи"
Исследование инвестиционной стратегии "Купил и держи". Основная идея провести исследование когда нет начального капитала, а инвестиционные отчисления происходят на ежемесячной основе.  


```python
# Установка и импорт необходимых библиотек
!pip install yfinance

from datetime import datetime, date
import pandas as pd
import yfinance as yf
```

Входные параметры


```python
#@title Параметры расчета { run: "auto" }
increment = 100 #@param {type:"integer"} # Сумма ежемесячного инвестирования
date_increment = 0 #@param {type:"integer"}
# increment: int = 100  # Сумма ежемесячного инвестирования
date_increment: int = 15  # Дата пополнения(число месяца)
start_date = date(1995, 1, 1)  # Дата старта инвестирования(год, месяц, число)
year_invest: int = 50  # Количество лет инвестирования
```


```python
# Загрузка данных в датафрейм
df = yf.download('SPY')
df = df.drop (columns = ['Adj Close', 'Volume'])  # Удаляем ненужные колонки
print(df)
```


```python
# Расчет конечной даты
end_date = date(start_date.year + year_invest, start_date.month, start_date.day)
print(f'Конечная дата {end_date}')

# Создание среза по датам
df = df[start_date:end_date]
print(df)
```
