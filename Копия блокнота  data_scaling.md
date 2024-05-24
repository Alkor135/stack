```python
#@title Установка нужных версий библиотек
!wget 'https://drive.google.com/uc?export=download&id=1oSFOP0j25OZAuhD8YXxyQXNTdr2lUdtn' -O requirements.txt
!pip install -r requirements.txt
```

    --2022-04-22 10:23:59--  https://drive.google.com/uc?export=download&id=1oSFOP0j25OZAuhD8YXxyQXNTdr2lUdtn
    Resolving drive.google.com (drive.google.com)... 74.125.195.139, 74.125.195.113, 74.125.195.101, ...
    Connecting to drive.google.com (drive.google.com)|74.125.195.139|:443... connected.
    HTTP request sent, awaiting response... 303 See Other
    Location: https://doc-0g-c0-docs.googleusercontent.com/docs/securesc/ha0ro937gcuc7l7deffksulhg5h7mbp1/rc8a93kblc3ht6rq7fk5to4r3jmrvemb/1650623025000/14904333240138417226/*/1oSFOP0j25OZAuhD8YXxyQXNTdr2lUdtn?e=download [following]
    Warning: wildcards not supported in HTTP.
    --2022-04-22 10:24:00--  https://doc-0g-c0-docs.googleusercontent.com/docs/securesc/ha0ro937gcuc7l7deffksulhg5h7mbp1/rc8a93kblc3ht6rq7fk5to4r3jmrvemb/1650623025000/14904333240138417226/*/1oSFOP0j25OZAuhD8YXxyQXNTdr2lUdtn?e=download
    Resolving doc-0g-c0-docs.googleusercontent.com (doc-0g-c0-docs.googleusercontent.com)... 74.125.195.132, 2607:f8b0:400e:c09::84
    Connecting to doc-0g-c0-docs.googleusercontent.com (doc-0g-c0-docs.googleusercontent.com)|74.125.195.132|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 74 [text/plain]
    Saving to: ‘requirements.txt’
    
    requirements.txt    100%[===================>]      74  --.-KB/s    in 0s      
    
    2022-04-22 10:24:00 (727 KB/s) - ‘requirements.txt’ saved [74/74]
    
    Collecting scikit-learn==0.22.2.post1
      Downloading scikit_learn-0.22.2.post1-cp37-cp37m-manylinux1_x86_64.whl (7.1 MB)
    [K     |████████████████████████████████| 7.1 MB 8.6 MB/s 
    [?25hCollecting pandas==1.1.5
      Downloading pandas-1.1.5-cp37-cp37m-manylinux1_x86_64.whl (9.5 MB)
    [K     |████████████████████████████████| 9.5 MB 35.5 MB/s 
    [?25hRequirement already satisfied: matplotlib==3.2.2 in /usr/local/lib/python3.7/dist-packages (from -r requirements.txt (line 3)) (3.2.2)
    Collecting numpy==1.19.5
      Downloading numpy-1.19.5-cp37-cp37m-manylinux2010_x86_64.whl (14.8 MB)
    [K     |████████████████████████████████| 14.8 MB 18.6 MB/s 
    [?25hRequirement already satisfied: joblib>=0.11 in /usr/local/lib/python3.7/dist-packages (from scikit-learn==0.22.2.post1->-r requirements.txt (line 1)) (1.1.0)
    Requirement already satisfied: scipy>=0.17.0 in /usr/local/lib/python3.7/dist-packages (from scikit-learn==0.22.2.post1->-r requirements.txt (line 1)) (1.4.1)
    Requirement already satisfied: pytz>=2017.2 in /usr/local/lib/python3.7/dist-packages (from pandas==1.1.5->-r requirements.txt (line 2)) (2022.1)
    Requirement already satisfied: python-dateutil>=2.7.3 in /usr/local/lib/python3.7/dist-packages (from pandas==1.1.5->-r requirements.txt (line 2)) (2.8.2)
    Requirement already satisfied: cycler>=0.10 in /usr/local/lib/python3.7/dist-packages (from matplotlib==3.2.2->-r requirements.txt (line 3)) (0.11.0)
    Requirement already satisfied: kiwisolver>=1.0.1 in /usr/local/lib/python3.7/dist-packages (from matplotlib==3.2.2->-r requirements.txt (line 3)) (1.4.2)
    Requirement already satisfied: pyparsing!=2.0.4,!=2.1.2,!=2.1.6,>=2.0.1 in /usr/local/lib/python3.7/dist-packages (from matplotlib==3.2.2->-r requirements.txt (line 3)) (3.0.8)
    Requirement already satisfied: typing-extensions in /usr/local/lib/python3.7/dist-packages (from kiwisolver>=1.0.1->matplotlib==3.2.2->-r requirements.txt (line 3)) (4.1.1)
    Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.7/dist-packages (from python-dateutil>=2.7.3->pandas==1.1.5->-r requirements.txt (line 2)) (1.15.0)
    Installing collected packages: numpy, scikit-learn, pandas
      Attempting uninstall: numpy
        Found existing installation: numpy 1.21.6
        Uninstalling numpy-1.21.6:
          Successfully uninstalled numpy-1.21.6
      Attempting uninstall: scikit-learn
        Found existing installation: scikit-learn 1.0.2
        Uninstalling scikit-learn-1.0.2:
          Successfully uninstalled scikit-learn-1.0.2
      Attempting uninstall: pandas
        Found existing installation: pandas 1.3.5
        Uninstalling pandas-1.3.5:
          Successfully uninstalled pandas-1.3.5
    [31mERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
    tensorflow 2.8.0 requires tf-estimator-nightly==2.8.0.dev2021122109, which is not installed.
    yellowbrick 1.4 requires scikit-learn>=1.0.0, but you have scikit-learn 0.22.2.post1 which is incompatible.
    tensorflow 2.8.0 requires numpy>=1.20, but you have numpy 1.19.5 which is incompatible.
    imbalanced-learn 0.8.1 requires scikit-learn>=0.24, but you have scikit-learn 0.22.2.post1 which is incompatible.
    datascience 0.10.6 requires folium==0.2.1, but you have folium 0.8.3 which is incompatible.
    albumentations 0.1.12 requires imgaug<0.2.7,>=0.2.5, but you have imgaug 0.2.9 which is incompatible.[0m
    Successfully installed numpy-1.19.5 pandas-1.1.5 scikit-learn-0.22.2.post1
    



# Масштабирование данных

## Для чего нужно масштабирование

Давайте обучим модель для регрессии из `sklearn` на датасете про недвижимость.


```python
from sklearn.datasets import fetch_california_housing
import pandas as pd


data = fetch_california_housing()
X = pd.DataFrame(data['data'], columns=data['feature_names'])
y = data['target']

X.head()
```

    Downloading Cal. housing from https://ndownloader.figshare.com/files/5976036 to /root/scikit_learn_data
    





  <div id="df-83421011-8cf3-44fe-b892-b337836fb97d">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8.3252</td>
      <td>41.0</td>
      <td>6.984127</td>
      <td>1.023810</td>
      <td>322.0</td>
      <td>2.555556</td>
      <td>37.88</td>
      <td>-122.23</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8.3014</td>
      <td>21.0</td>
      <td>6.238137</td>
      <td>0.971880</td>
      <td>2401.0</td>
      <td>2.109842</td>
      <td>37.86</td>
      <td>-122.22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.2574</td>
      <td>52.0</td>
      <td>8.288136</td>
      <td>1.073446</td>
      <td>496.0</td>
      <td>2.802260</td>
      <td>37.85</td>
      <td>-122.24</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5.6431</td>
      <td>52.0</td>
      <td>5.817352</td>
      <td>1.073059</td>
      <td>558.0</td>
      <td>2.547945</td>
      <td>37.85</td>
      <td>-122.25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3.8462</td>
      <td>52.0</td>
      <td>6.281853</td>
      <td>1.081081</td>
      <td>565.0</td>
      <td>2.181467</td>
      <td>37.85</td>
      <td>-122.25</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-83421011-8cf3-44fe-b892-b337836fb97d')"
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
          document.querySelector('#df-83421011-8cf3-44fe-b892-b337836fb97d button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-83421011-8cf3-44fe-b892-b337836fb97d');
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




Разобьем выборку на обучение и тест с помощью `train_test_split`.


```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=10, test_size=0.2)
X_train.shape, X_test.shape
```




    ((16512, 8), (4128, 8))



Инициализируем модель `KNeighborsRegressor` для задачи регрессии из `sklearn`.


```python
from sklearn.neighbors import KNeighborsRegressor

knn = KNeighborsRegressor()
```

И обучаем на обучающей выборке.


```python
knn.fit(X_train, y_train)
```




    KNeighborsRegressor(algorithm='auto', leaf_size=30, metric='minkowski',
                        metric_params=None, n_jobs=None, n_neighbors=5, p=2,
                        weights='uniform')



И теперь можем посчитать метрику качества.

Про метрики качества для задачи регрессии можете посмотреть [здесь](https://youtu.be/vh2smjQyhp8)


```python
from sklearn.metrics import r2_score

pred_train = knn.predict(X_train)
pred_test = knn.predict(X_test)


print(f'Train R2 {r2_score(y_train, pred_train):.2f}')
print(f'Test R2 {r2_score(y_test, pred_test):.2f}')
```

    Train R2 0.45
    Test R2 0.18
    

А метрики получаются совсем плохие.

Давайте попробуем другую модель.


```python
from sklearn.linear_model import LinearRegression

lr = LinearRegression()
lr.fit(X_train, y_train)
```




    LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None, normalize=False)



И так же сделаем предсказания этой моделью.


```python
pred_train = lr.predict(X_train)
pred_test = lr.predict(X_test)


print(f'Train R2 {r2_score(y_train, pred_train):.2f}')
print(f'Test R2 {r2_score(y_test, pred_test):.2f}')
```

    Train R2 0.61
    Test R2 0.60
    

Метрики уже выше, но не настолько хорошие, чтобы пускать модель к реальным данным.

Здесь модель линейная, простая, поэтому и высокого качества могли и не ожидать. 

Давайте возьмем ещё другую модель, может быть задача очень сложная, что наши предыдущие модели машинного обучения не справляются с ней.


```python
from sklearn.tree import DecisionTreeRegressor

tree_1 = DecisionTreeRegressor(random_state=1, max_depth=9)
tree_1.fit(X_train, y_train)
```




    DecisionTreeRegressor(ccp_alpha=0.0, criterion='mse', max_depth=9,
                          max_features=None, max_leaf_nodes=None,
                          min_impurity_decrease=0.0, min_impurity_split=None,
                          min_samples_leaf=1, min_samples_split=2,
                          min_weight_fraction_leaf=0.0, presort='deprecated',
                          random_state=1, splitter='best')



И так же сделаем предсказания этой моделью.


```python
pred_train = tree_1.predict(X_train)
pred_test = tree_1.predict(X_test)


print(f'Train R2 {r2_score(y_train, pred_train):.2f}')
print(f'Test R2 {r2_score(y_test, pred_test):.2f}')
```

    Train R2 0.79
    Test R2 0.72
    

Ну вот, совсем другое дело, значит проблема не в данных, а в моделях, раз модель дерева решений обучилась.



### Что может быть не так с моделями?

А KNN, линейные модели и другие модели очень чувстительны к входным данным. Давайте глянем на эти данные.



```python
X.describe()
```





  <div id="df-ee1eb775-45cc-4973-8fa8-9e14266eb760">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>20640.000000</td>
      <td>20640.000000</td>
      <td>20640.000000</td>
      <td>20640.000000</td>
      <td>20640.000000</td>
      <td>20640.000000</td>
      <td>20640.000000</td>
      <td>20640.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>3.870671</td>
      <td>28.639486</td>
      <td>5.429000</td>
      <td>1.096675</td>
      <td>1425.476744</td>
      <td>3.070655</td>
      <td>35.631861</td>
      <td>-119.569704</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.899822</td>
      <td>12.585558</td>
      <td>2.474173</td>
      <td>0.473911</td>
      <td>1132.462122</td>
      <td>10.386050</td>
      <td>2.135952</td>
      <td>2.003532</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.499900</td>
      <td>1.000000</td>
      <td>0.846154</td>
      <td>0.333333</td>
      <td>3.000000</td>
      <td>0.692308</td>
      <td>32.540000</td>
      <td>-124.350000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.563400</td>
      <td>18.000000</td>
      <td>4.440716</td>
      <td>1.006079</td>
      <td>787.000000</td>
      <td>2.429741</td>
      <td>33.930000</td>
      <td>-121.800000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3.534800</td>
      <td>29.000000</td>
      <td>5.229129</td>
      <td>1.048780</td>
      <td>1166.000000</td>
      <td>2.818116</td>
      <td>34.260000</td>
      <td>-118.490000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>4.743250</td>
      <td>37.000000</td>
      <td>6.052381</td>
      <td>1.099526</td>
      <td>1725.000000</td>
      <td>3.282261</td>
      <td>37.710000</td>
      <td>-118.010000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>15.000100</td>
      <td>52.000000</td>
      <td>141.909091</td>
      <td>34.066667</td>
      <td>35682.000000</td>
      <td>1243.333333</td>
      <td>41.950000</td>
      <td>-114.310000</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-ee1eb775-45cc-4973-8fa8-9e14266eb760')"
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
          document.querySelector('#df-ee1eb775-45cc-4973-8fa8-9e14266eb760 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-ee1eb775-45cc-4973-8fa8-9e14266eb760');
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




Есть признаки с маленькими значениями (`MedInc`, `AveBedrms`), а есть признаки со значениями побольше (`Latitude`, `HouseAge`), а есть прям с очень большими значениями (`Population` и `AveOccup`).

Значения в признаках **отличаются на порядки** и это огромная **проблема** для чувствительных моделей.



Имея такие данные, модель может ошибочно считать, что раз признак большой (`Population`), то он полезный, но какие-нибудь маленькие признаки (`MedInc`) могут оказаться в разы полезней больших. А вот такие большие будут перетягивать на себя всё внимание модели.

<img src='https://drive.google.com/uc?id=19e_j7M6XJ2pvypKfXqwn3V6eXwYIUO8m'>

А значит нужно наши признаки уравнивать, масштабировать, чтобы модель изначально считала, что всё признаки одинаково полезные и никому не давала перетягивать одеяло. 

## Масштабирование данных

Про то, зачем нужно масштабирование данных поговорили, теперь обсудим, как его делать.

А есть два основных вида масштабирования данных: нормализация и стандартизация.

Пойдем по порядку.

### Нормализация

Для того, чтобы сделать нормализацию данных нужно посчитать в каждом признаке его минимум (min) и максимум (max), а затем сделать следующее вычисление:

$$x = \frac{x - min}{max - min}$$

После такого преобразования $min = 0, max = 1$.

#### Через sklearn

Давайте для начала обучим sklearn версию, а затем напишим свою реализацию.


```python
from sklearn.preprocessing import MinMaxScaler

mms = MinMaxScaler()
mms.fit(X_train)
```




    MinMaxScaler(copy=True, feature_range=(0, 1))



Нормализатор обучился, давайте посмотрим, какие были минимальные значения в признаках.


```python
mms.data_min_
```




    array([   0.4999    ,    1.        ,    0.84615385,    0.33333333,
              3.        ,    0.69230769,   32.54      , -124.35      ])




```python
X_train.min()
```




    MedInc          0.499900
    HouseAge        1.000000
    AveRooms        0.846154
    AveBedrms       0.333333
    Population      3.000000
    AveOccup        0.692308
    Latitude       32.540000
    Longitude    -124.350000
    dtype: float64



И посмотрим, какие были максимальные значения в признаках.


```python
mms.data_max_
```




    array([ 1.50001000e+01,  5.20000000e+01,  1.41909091e+02,  3.40666667e+01,
            3.56820000e+04,  5.99714286e+02,  4.19500000e+01, -1.14310000e+02])



Кстати, небольшой лайфхак, если хотите видеть не научную запись с `e`, а обычную запись чисел, то можете воспользоваться следующей строчкой кода


```python
import numpy as np
np.set_printoptions(suppress=True)
```


```python
mms.data_max_
```




    array([   15.0001    ,    52.        ,   141.90909091,    34.06666667,
           35682.        ,   599.71428571,    41.95      ,  -114.31      ])




```python
X_train.max()
```




    MedInc           15.000100
    HouseAge         52.000000
    AveRooms        141.909091
    AveBedrms        34.066667
    Population    35682.000000
    AveOccup        599.714286
    Latitude         41.950000
    Longitude      -114.310000
    dtype: float64



А теперь можем нормализовать нашу обучающую выборку.


```python
mms.transform(X_train)
```




    array([[0.13939808, 0.39215686, 0.03581217, ..., 0.00233227, 0.1360255 ,
            0.77988048],
           [0.14701177, 0.88235294, 0.03816278, ..., 0.00252513, 0.63336876,
            0.14043825],
           [0.32103695, 0.58823529, 0.02966471, ..., 0.00282055, 0.54091392,
            0.18525896],
           ...,
           [0.34714694, 0.09803922, 0.02877568, ..., 0.0050506 , 0.50797024,
            0.25498008],
           [0.11765355, 0.66666667, 0.01348573, ..., 0.00609956, 0.15302869,
            0.60956175],
           [0.15009448, 0.29411765, 0.02202461, ..., 0.00294788, 0.50584485,
            0.24601594]])




```python
X_train_norm = pd.DataFrame(mms.transform(X_train), columns=X_train.columns)
X_train_norm
```





  <div id="df-cf523317-b7d2-4e25-a3d2-b30b11e26c6a">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.139398</td>
      <td>0.392157</td>
      <td>0.035812</td>
      <td>0.029696</td>
      <td>0.101460</td>
      <td>0.002332</td>
      <td>0.136026</td>
      <td>0.779880</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.147012</td>
      <td>0.882353</td>
      <td>0.038163</td>
      <td>0.029968</td>
      <td>0.022534</td>
      <td>0.002525</td>
      <td>0.633369</td>
      <td>0.140438</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.321037</td>
      <td>0.588235</td>
      <td>0.029665</td>
      <td>0.019907</td>
      <td>0.041173</td>
      <td>0.002821</td>
      <td>0.540914</td>
      <td>0.185259</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.160205</td>
      <td>0.294118</td>
      <td>0.027323</td>
      <td>0.022704</td>
      <td>0.048964</td>
      <td>0.004528</td>
      <td>0.161530</td>
      <td>0.621514</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.101061</td>
      <td>0.784314</td>
      <td>0.031278</td>
      <td>0.034030</td>
      <td>0.026262</td>
      <td>0.001939</td>
      <td>0.275239</td>
      <td>0.367530</td>
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
      <th>16507</th>
      <td>0.250003</td>
      <td>0.705882</td>
      <td>0.030225</td>
      <td>0.021082</td>
      <td>0.018078</td>
      <td>0.002054</td>
      <td>0.572795</td>
      <td>0.181275</td>
    </tr>
    <tr>
      <th>16508</th>
      <td>0.048689</td>
      <td>0.333333</td>
      <td>0.011987</td>
      <td>0.022423</td>
      <td>0.053533</td>
      <td>0.003393</td>
      <td>0.153029</td>
      <td>0.610558</td>
    </tr>
    <tr>
      <th>16509</th>
      <td>0.347147</td>
      <td>0.098039</td>
      <td>0.028776</td>
      <td>0.018742</td>
      <td>0.060456</td>
      <td>0.005051</td>
      <td>0.507970</td>
      <td>0.254980</td>
    </tr>
    <tr>
      <th>16510</th>
      <td>0.117654</td>
      <td>0.666667</td>
      <td>0.013486</td>
      <td>0.019703</td>
      <td>0.060456</td>
      <td>0.006100</td>
      <td>0.153029</td>
      <td>0.609562</td>
    </tr>
    <tr>
      <th>16511</th>
      <td>0.150094</td>
      <td>0.294118</td>
      <td>0.022025</td>
      <td>0.022723</td>
      <td>0.046834</td>
      <td>0.002948</td>
      <td>0.505845</td>
      <td>0.246016</td>
    </tr>
  </tbody>
</table>
<p>16512 rows × 8 columns</p>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-cf523317-b7d2-4e25-a3d2-b30b11e26c6a')"
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
          document.querySelector('#df-cf523317-b7d2-4e25-a3d2-b30b11e26c6a button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-cf523317-b7d2-4e25-a3d2-b30b11e26c6a');
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




Значения на первый взгляд стали меньше, давайте глянем через `describe()`


```python
X_train_norm.describe()
```





  <div id="df-fca4180e-0f8b-429c-92d5-7c483577b117">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.231956</td>
      <td>0.541389</td>
      <td>0.032506</td>
      <td>0.022676</td>
      <td>0.039882</td>
      <td>0.003897</td>
      <td>0.329404</td>
      <td>0.475445</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.130436</td>
      <td>0.247553</td>
      <td>0.018406</td>
      <td>0.014861</td>
      <td>0.031845</td>
      <td>0.010724</td>
      <td>0.227318</td>
      <td>0.199838</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.142019</td>
      <td>0.333333</td>
      <td>0.025438</td>
      <td>0.019949</td>
      <td>0.021918</td>
      <td>0.002900</td>
      <td>0.147715</td>
      <td>0.253984</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.209138</td>
      <td>0.549020</td>
      <td>0.031022</td>
      <td>0.021210</td>
      <td>0.032582</td>
      <td>0.003548</td>
      <td>0.182784</td>
      <td>0.582669</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.292396</td>
      <td>0.705882</td>
      <td>0.036928</td>
      <td>0.022704</td>
      <td>0.048404</td>
      <td>0.004322</td>
      <td>0.550478</td>
      <td>0.631474</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-fca4180e-0f8b-429c-92d5-7c483577b117')"
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
          document.querySelector('#df-fca4180e-0f8b-429c-92d5-7c483577b117 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-fca4180e-0f8b-429c-92d5-7c483577b117');
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




Действительно, минимальные значения везде равны 0, а максимальные везде равны 1.

И давайте вдобавок нормализуем тестовую выборку. При этом пользуемся `min` и `max` значениями с обучающей выборке, на тесте ничего никогда не считается.


```python
X_test_norm = pd.DataFrame(mms.transform(X_test), columns=X_train.columns)
X_test_norm.describe()
```





  <div id="df-cdb131c0-5856-4957-804e-e2578b236286">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>4128.000000</td>
      <td>4128.000000</td>
      <td>4128.000000</td>
      <td>4128.000000</td>
      <td>4128.000000</td>
      <td>4128.000000</td>
      <td>4128.000000</td>
      <td>4128.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.234496</td>
      <td>0.544198</td>
      <td>0.032414</td>
      <td>0.022440</td>
      <td>0.039816</td>
      <td>0.004262</td>
      <td>0.325244</td>
      <td>0.478847</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.133330</td>
      <td>0.243659</td>
      <td>0.013532</td>
      <td>0.010172</td>
      <td>0.031321</td>
      <td>0.032298</td>
      <td>0.225655</td>
      <td>0.198421</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.002015</td>
      <td>0.003294</td>
      <td>0.000056</td>
      <td>0.000465</td>
      <td>0.002125</td>
      <td>0.004980</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.143938</td>
      <td>0.333333</td>
      <td>0.025715</td>
      <td>0.019910</td>
      <td>0.022142</td>
      <td>0.002904</td>
      <td>0.148778</td>
      <td>0.258715</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.210390</td>
      <td>0.549020</td>
      <td>0.031240</td>
      <td>0.021195</td>
      <td>0.032680</td>
      <td>0.003552</td>
      <td>0.180659</td>
      <td>0.584661</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.294279</td>
      <td>0.705882</td>
      <td>0.036861</td>
      <td>0.022743</td>
      <td>0.047619</td>
      <td>0.004325</td>
      <td>0.548353</td>
      <td>0.631474</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.436515</td>
      <td>0.408432</td>
      <td>0.451778</td>
      <td>2.074450</td>
      <td>0.988310</td>
      <td>0.982072</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-cdb131c0-5856-4957-804e-e2578b236286')"
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
          document.querySelector('#df-cdb131c0-5856-4957-804e-e2578b236286 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-cdb131c0-5856-4957-804e-e2578b236286');
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




При этом где-то мы не получили чистый 0 в `min` и 1 в `max`, это как раз-таки из-за того, что используем `min` и `max` с обучения.

В этом ничего страшного нет, на тесте нет цели получить сто процентный 0 и 1, цель - **перевести данные в такую же шкалу**, что получили на трейне.

Ведь действительно у нас на обучении самый густонаселенный дом вмещает 35к людей, а вот на тесте всего лишь 16к, меньше в два раза, чем на обучении, поэтому и максимальное значение в `Population` с теста оказывается на половине шкалы с обучения. 


```python
X_train['Population'].max(), X_test['Population'].max()
```




    (35682.0, 16122.0)




```python
import matplotlib.pyplot as plt
import numpy as np


x = np.arange(0, 36000)
y = 0 * x
plt.plot(x, y)
plt.scatter([X_train['Population'].max()], [0], label='max train population', marker='*', c='r', s=100)
plt.scatter([X_test['Population'].max()], [0], label='max test population', marker='o', c='g')
plt.grid()
plt.legend();
```


    
![png](%D0%9A%D0%BE%D0%BF%D0%B8%D1%8F%20%D0%B1%D0%BB%D0%BE%D0%BA%D0%BD%D0%BE%D1%82%D0%B0%20%20data_scaling_files/%D0%9A%D0%BE%D0%BF%D0%B8%D1%8F%20%D0%B1%D0%BB%D0%BE%D0%BA%D0%BD%D0%BE%D1%82%D0%B0%20%20data_scaling_56_0.png)
    


Тоже самое будет и в масштабированном виде.


```python
X_train_norm['Population'].max(), X_test_norm['Population'].max()
```




    (1.0, 0.4517783570167325)




```python
x = np.linspace(0, 1, 5)
y = 0 * x
plt.plot(x, y)
plt.scatter([X_train_norm['Population'].max()], [0], label='max train population', marker='*', c='r', s=100)
plt.scatter([X_test_norm['Population'].max()], [0], label='max test population', marker='o', c='g')
plt.grid()
plt.legend();
```


    
![png](%D0%9A%D0%BE%D0%BF%D0%B8%D1%8F%20%D0%B1%D0%BB%D0%BE%D0%BA%D0%BD%D0%BE%D1%82%D0%B0%20%20data_scaling_files/%D0%9A%D0%BE%D0%BF%D0%B8%D1%8F%20%D0%B1%D0%BB%D0%BE%D0%BA%D0%BD%D0%BE%D1%82%D0%B0%20%20data_scaling_59_0.png)
    


Так что это абсолютная норма, что на тесте данные не принимают в максимальном 1, а в минимальном 0.

#### Самостоятельно

С sklearn версией разобрались, сейчас давайте напишим свой нормализатор.

Для начала нужно посчитать минимальные и максимальные значения в исходных данных.


```python
mins = X_train.min()
maxs = X_train.max()

display(mins, maxs)
```


    MedInc          0.499900
    HouseAge        1.000000
    AveRooms        0.846154
    AveBedrms       0.333333
    Population      3.000000
    AveOccup        0.692308
    Latitude       32.540000
    Longitude    -124.350000
    dtype: float64



    MedInc           15.000100
    HouseAge         52.000000
    AveRooms        141.909091
    AveBedrms        34.066667
    Population    35682.000000
    AveOccup        599.714286
    Latitude         41.950000
    Longitude      -114.310000
    dtype: float64


А затем нужно пройтись по всем признакам и вычесть минимум и поделть на разницу минимума и максимуму:

$$x = \frac{x - min}{max - min}$$



```python
X_train_my_norm = X_train.copy()

for col in X_train.columns:
    X_train_my_norm[col] = (X_train[col] - mins[col]) / (maxs[col] - mins[col])

X_train_my_norm.describe()
```





  <div id="df-99cec1f1-1498-4cb8-a396-c88bf26ba9e3">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.231956</td>
      <td>0.541389</td>
      <td>0.032506</td>
      <td>0.022676</td>
      <td>0.039882</td>
      <td>0.003897</td>
      <td>0.329404</td>
      <td>0.475445</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.130436</td>
      <td>0.247553</td>
      <td>0.018406</td>
      <td>0.014861</td>
      <td>0.031845</td>
      <td>0.010724</td>
      <td>0.227318</td>
      <td>0.199838</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.142019</td>
      <td>0.333333</td>
      <td>0.025438</td>
      <td>0.019949</td>
      <td>0.021918</td>
      <td>0.002900</td>
      <td>0.147715</td>
      <td>0.253984</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.209138</td>
      <td>0.549020</td>
      <td>0.031022</td>
      <td>0.021210</td>
      <td>0.032582</td>
      <td>0.003548</td>
      <td>0.182784</td>
      <td>0.582669</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.292396</td>
      <td>0.705882</td>
      <td>0.036928</td>
      <td>0.022704</td>
      <td>0.048404</td>
      <td>0.004322</td>
      <td>0.550478</td>
      <td>0.631474</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-99cec1f1-1498-4cb8-a396-c88bf26ba9e3')"
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
          document.querySelector('#df-99cec1f1-1498-4cb8-a396-c88bf26ba9e3 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-99cec1f1-1498-4cb8-a396-c88bf26ba9e3');
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




Или же такой проход в цикле могли бы заменить на более приятную операцию, работая со всеми признаками за раз.


```python
X_train_my_norm = (X_train - mins) / (maxs - mins)
X_train_my_norm.describe()
```





  <div id="df-31a248e8-6c37-4cc7-a84a-a1b79bc1e0f3">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
      <td>16512.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.231956</td>
      <td>0.541389</td>
      <td>0.032506</td>
      <td>0.022676</td>
      <td>0.039882</td>
      <td>0.003897</td>
      <td>0.329404</td>
      <td>0.475445</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.130436</td>
      <td>0.247553</td>
      <td>0.018406</td>
      <td>0.014861</td>
      <td>0.031845</td>
      <td>0.010724</td>
      <td>0.227318</td>
      <td>0.199838</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.142019</td>
      <td>0.333333</td>
      <td>0.025438</td>
      <td>0.019949</td>
      <td>0.021918</td>
      <td>0.002900</td>
      <td>0.147715</td>
      <td>0.253984</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.209138</td>
      <td>0.549020</td>
      <td>0.031022</td>
      <td>0.021210</td>
      <td>0.032582</td>
      <td>0.003548</td>
      <td>0.182784</td>
      <td>0.582669</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.292396</td>
      <td>0.705882</td>
      <td>0.036928</td>
      <td>0.022704</td>
      <td>0.048404</td>
      <td>0.004322</td>
      <td>0.550478</td>
      <td>0.631474</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-31a248e8-6c37-4cc7-a84a-a1b79bc1e0f3')"
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
          document.querySelector('#df-31a248e8-6c37-4cc7-a84a-a1b79bc1e0f3 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-31a248e8-6c37-4cc7-a84a-a1b79bc1e0f3');
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




И так же нормализуем тестовую выборку.


```python
X_test_my_norm = (X_test - mins) / (maxs - mins)
X_test_my_norm.describe()
```





  <div id="df-2324d4dc-5b6d-46aa-a76b-e022a5ff2052">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>4128.000000</td>
      <td>4128.000000</td>
      <td>4128.000000</td>
      <td>4128.000000</td>
      <td>4128.000000</td>
      <td>4128.000000</td>
      <td>4128.000000</td>
      <td>4128.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.234496</td>
      <td>0.544198</td>
      <td>0.032414</td>
      <td>0.022440</td>
      <td>0.039816</td>
      <td>0.004262</td>
      <td>0.325244</td>
      <td>0.478847</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.133330</td>
      <td>0.243659</td>
      <td>0.013532</td>
      <td>0.010172</td>
      <td>0.031321</td>
      <td>0.032298</td>
      <td>0.225655</td>
      <td>0.198421</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.002015</td>
      <td>0.003294</td>
      <td>0.000056</td>
      <td>0.000465</td>
      <td>0.002125</td>
      <td>0.004980</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.143938</td>
      <td>0.333333</td>
      <td>0.025715</td>
      <td>0.019910</td>
      <td>0.022142</td>
      <td>0.002904</td>
      <td>0.148778</td>
      <td>0.258715</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.210390</td>
      <td>0.549020</td>
      <td>0.031240</td>
      <td>0.021195</td>
      <td>0.032680</td>
      <td>0.003552</td>
      <td>0.180659</td>
      <td>0.584661</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.294279</td>
      <td>0.705882</td>
      <td>0.036861</td>
      <td>0.022743</td>
      <td>0.047619</td>
      <td>0.004325</td>
      <td>0.548353</td>
      <td>0.631474</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.436515</td>
      <td>0.408432</td>
      <td>0.451778</td>
      <td>2.074450</td>
      <td>0.988310</td>
      <td>0.982072</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-2324d4dc-5b6d-46aa-a76b-e022a5ff2052')"
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
          document.querySelector('#df-2324d4dc-5b6d-46aa-a76b-e022a5ff2052 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-2324d4dc-5b6d-46aa-a76b-e022a5ff2052');
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




#### Обучение модели на масштабированных данных


```python
knn = KNeighborsRegressor()
knn.fit(X_train_norm, y_train)
```




    KNeighborsRegressor(algorithm='auto', leaf_size=30, metric='minkowski',
                        metric_params=None, n_jobs=None, n_neighbors=5, p=2,
                        weights='uniform')



Для справки, что было до



```
Train R2 0.45
Test R2 0.18
```



И что стало после, а качество выросло очень прилично.

Вот в чём была проблема, в немасштабированных данных, в признаках, которые собой перекрывали все остальные признаки.


```python
pred_train = knn.predict(X_train_norm)
pred_test = knn.predict(X_test_norm)


print(f'Train R2 {r2_score(y_train, pred_train):.2f}')
print(f'Test R2 {r2_score(y_test, pred_test):.2f}')
```

    Train R2 0.81
    Test R2 0.70
    

Давайте попробуем другую модель.


```python
lr = LinearRegression()
lr.fit(X_train_norm, y_train)
```




    LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None, normalize=False)



Для справки, что было до



```
Train R2 0.61
Test R2 0.60
```




```python
pred_train = lr.predict(X_train_norm)
pred_test = lr.predict(X_test_norm)


print(f'Train R2 {r2_score(y_train, pred_train):.2f}')
print(f'Test R2 {r2_score(y_test, pred_test):.2f}')
```

    Train R2 0.61
    Test R2 0.60
    

А ничего сильно не поменялось, а это потому что **линейным моделям** больше подходит следующий метод масштабирования данных - **стандартизация** (ну или мы выжали все соки из линейно регрессии)

### Стандартизация

Для того, чтобы сделать стандартизацию данных нужно посчитать в каждом признаке его среднее значение (`mean`) и стандартное отклонение (`std`), а затем сделать следующее вычисление:

$$x = \frac{x - mean}{std}$$

После такого преобразования $mean = 0, std = 1$.

#### Через sklearn

И по-классике, для начала обучим sklearn версию, а затем напишим свою реализацию.


```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
scaler.fit(X_train)
```




    StandardScaler(copy=True, with_mean=True, with_std=True)



Стандартизатор обучился, посмотрим, какими были средние значения в признаках.


```python
scaler.mean_
```




    array([   3.86330436,   28.61082849,    5.43161001,    1.09826536,
           1425.94797723,    3.0269867 ,   35.63968932, -119.57653706])




```python
X_train.mean()
```




    MedInc           3.863304
    HouseAge        28.610828
    AveRooms         5.431610
    AveBedrms        1.098265
    Population    1425.947977
    AveOccup         3.026987
    Latitude        35.639689
    Longitude     -119.576537
    dtype: float64



И посмотрим, какие были стандартные отклонения в признаках.


```python
scaler.scale_
```




    array([   1.89128773,   12.62480616,    2.59629804,    0.50128107,
           1136.1680751 ,    6.4238777 ,    2.1390021 ,    2.00630999])




```python
X_train.std()
```




    MedInc           1.891345
    HouseAge        12.625188
    AveRooms         2.596377
    AveBedrms        0.501296
    Population    1136.202481
    AveOccup         6.424072
    Latitude         2.139067
    Longitude        2.006371
    dtype: float64



А теперь можем привести к стандартному виду нашу обучающую выборку.


```python
X_train_std = pd.DataFrame(scaler.transform(X_train), columns=X_train.columns)
X_train_std
```





  <div id="df-29910263-cb3f-42ff-bfe1-db0dc70f2650">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.709625</td>
      <td>-0.602847</td>
      <td>0.179607</td>
      <td>0.472386</td>
      <td>1.933739</td>
      <td>-0.145955</td>
      <td>-0.850719</td>
      <td>1.523462</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.651252</td>
      <td>1.377381</td>
      <td>0.307321</td>
      <td>0.490737</td>
      <td>-0.544768</td>
      <td>-0.127971</td>
      <td>1.337217</td>
      <td>-1.676442</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.682972</td>
      <td>0.189244</td>
      <td>-0.154399</td>
      <td>-0.186345</td>
      <td>0.040533</td>
      <td>-0.100424</td>
      <td>0.930486</td>
      <td>-1.452150</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.550104</td>
      <td>-0.998893</td>
      <td>-0.281640</td>
      <td>0.001908</td>
      <td>0.285215</td>
      <td>0.058794</td>
      <td>-0.738517</td>
      <td>0.730962</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-1.003551</td>
      <td>0.981336</td>
      <td>-0.066721</td>
      <td>0.764036</td>
      <td>-0.427708</td>
      <td>-0.182591</td>
      <td>-0.238284</td>
      <td>-0.540028</td>
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
      <th>16507</th>
      <td>0.138369</td>
      <td>0.664499</td>
      <td>-0.123953</td>
      <td>-0.107235</td>
      <td>-0.684712</td>
      <td>-0.171880</td>
      <td>1.070738</td>
      <td>-1.472087</td>
    </tr>
    <tr>
      <th>16508</th>
      <td>-1.405077</td>
      <td>-0.840475</td>
      <td>-1.114885</td>
      <td>-0.017000</td>
      <td>0.428680</td>
      <td>-0.046999</td>
      <td>-0.775918</td>
      <td>0.676135</td>
    </tr>
    <tr>
      <th>16509</th>
      <td>0.883153</td>
      <td>-1.790984</td>
      <td>-0.202702</td>
      <td>-0.264699</td>
      <td>0.646077</td>
      <td>0.107527</td>
      <td>0.785558</td>
      <td>-1.103251</td>
    </tr>
    <tr>
      <th>16510</th>
      <td>-0.876336</td>
      <td>0.506081</td>
      <td>-1.033440</td>
      <td>-0.200042</td>
      <td>0.646077</td>
      <td>0.205342</td>
      <td>-0.775918</td>
      <td>0.671151</td>
    </tr>
    <tr>
      <th>16511</th>
      <td>-0.627617</td>
      <td>-0.998893</td>
      <td>-0.569503</td>
      <td>0.003167</td>
      <td>0.218323</td>
      <td>-0.088550</td>
      <td>0.776208</td>
      <td>-1.148109</td>
    </tr>
  </tbody>
</table>
<p>16512 rows × 8 columns</p>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-29910263-cb3f-42ff-bfe1-db0dc70f2650')"
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
          document.querySelector('#df-29910263-cb3f-42ff-bfe1-db0dc70f2650 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-29910263-cb3f-42ff-bfe1-db0dc70f2650');
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




Глянем на `describe()`


```python
X_train_std.describe()
```





  <div id="df-85b92cb7-79c6-461b-a37a-60e4019bff9e">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1.651200e+04</td>
      <td>1.651200e+04</td>
      <td>1.651200e+04</td>
      <td>1.651200e+04</td>
      <td>1.651200e+04</td>
      <td>1.651200e+04</td>
      <td>1.651200e+04</td>
      <td>1.651200e+04</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1.344747e-20</td>
      <td>2.056790e-17</td>
      <td>2.323050e-16</td>
      <td>9.214177e-17</td>
      <td>-6.575140e-17</td>
      <td>-7.259868e-17</td>
      <td>1.456926e-15</td>
      <td>3.342395e-15</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.000030e+00</td>
      <td>1.000030e+00</td>
      <td>1.000030e+00</td>
      <td>1.000030e+00</td>
      <td>1.000030e+00</td>
      <td>1.000030e+00</td>
      <td>1.000030e+00</td>
      <td>1.000030e+00</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-1.778367e+00</td>
      <td>-2.187030e+00</td>
      <td>-1.766152e+00</td>
      <td>-1.525954e+00</td>
      <td>-1.252410e+00</td>
      <td>-3.634376e-01</td>
      <td>-1.449129e+00</td>
      <td>-2.379225e+00</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>-6.895325e-01</td>
      <td>-8.404746e-01</td>
      <td>-3.840347e-01</td>
      <td>-1.834951e-01</td>
      <td>-5.641313e-01</td>
      <td>-9.305279e-02</td>
      <td>-7.992930e-01</td>
      <td>-1.108235e+00</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>-1.749360e-01</td>
      <td>3.082594e-02</td>
      <td>-8.066830e-02</td>
      <td>-9.862835e-02</td>
      <td>-2.292337e-01</td>
      <td>-3.257503e-02</td>
      <td>-6.450154e-01</td>
      <td>5.365756e-01</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>4.633857e-01</td>
      <td>6.644990e-01</td>
      <td>2.402373e-01</td>
      <td>1.868945e-03</td>
      <td>2.676118e-01</td>
      <td>3.960738e-02</td>
      <td>9.725613e-01</td>
      <td>7.808051e-01</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5.888472e+00</td>
      <td>1.852636e+00</td>
      <td>5.256618e+01</td>
      <td>6.576830e+01</td>
      <td>3.015051e+01</td>
      <td>9.288584e+01</td>
      <td>2.950119e+00</td>
      <td>2.624987e+00</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-85b92cb7-79c6-461b-a37a-60e4019bff9e')"
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
          document.querySelector('#df-85b92cb7-79c6-461b-a37a-60e4019bff9e button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-85b92cb7-79c6-461b-a37a-60e4019bff9e');
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




И тут небольшой лайфхак, если и в pandas хочется избавиться от научной записи чисел, то выполняйте следующую строку кода


```python
pd.set_option('display.float_format', lambda x: '%0.4f' % x)
```


```python
X_train_std.describe()
```





  <div id="df-82aaa644-2ad9-43ab-ab8b-941fb814c6c7">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>-0.0000</td>
      <td>-0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-1.7784</td>
      <td>-2.1870</td>
      <td>-1.7662</td>
      <td>-1.5260</td>
      <td>-1.2524</td>
      <td>-0.3634</td>
      <td>-1.4491</td>
      <td>-2.3792</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>-0.6895</td>
      <td>-0.8405</td>
      <td>-0.3840</td>
      <td>-0.1835</td>
      <td>-0.5641</td>
      <td>-0.0931</td>
      <td>-0.7993</td>
      <td>-1.1082</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>-0.1749</td>
      <td>0.0308</td>
      <td>-0.0807</td>
      <td>-0.0986</td>
      <td>-0.2292</td>
      <td>-0.0326</td>
      <td>-0.6450</td>
      <td>0.5366</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.4634</td>
      <td>0.6645</td>
      <td>0.2402</td>
      <td>0.0019</td>
      <td>0.2676</td>
      <td>0.0396</td>
      <td>0.9726</td>
      <td>0.7808</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5.8885</td>
      <td>1.8526</td>
      <td>52.5662</td>
      <td>65.7683</td>
      <td>30.1505</td>
      <td>92.8858</td>
      <td>2.9501</td>
      <td>2.6250</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-82aaa644-2ad9-43ab-ab8b-941fb814c6c7')"
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
          document.querySelector('#df-82aaa644-2ad9-43ab-ab8b-941fb814c6c7 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-82aaa644-2ad9-43ab-ab8b-941fb814c6c7');
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




И да, теперь среднее во всех признаках равно 0, а стандартное отклонение равно 1.

И давайте стандартизируем тестовую выборку. При этом пользуемся `mean` и `std` значениями с обучающей выборке, на тесте ничего никогда не считается.


```python
X_test_std = pd.DataFrame(scaler.transform(X_test), columns=X_train.columns)
X_test_std.describe()
```





  <div id="df-1a0e2810-1f0f-4910-8702-b15106baead0">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>4128.0000</td>
      <td>4128.0000</td>
      <td>4128.0000</td>
      <td>4128.0000</td>
      <td>4128.0000</td>
      <td>4128.0000</td>
      <td>4128.0000</td>
      <td>4128.0000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.0195</td>
      <td>0.0113</td>
      <td>-0.0050</td>
      <td>-0.0159</td>
      <td>-0.0021</td>
      <td>0.0340</td>
      <td>-0.0183</td>
      <td>0.0170</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.0222</td>
      <td>0.9843</td>
      <td>0.7352</td>
      <td>0.6845</td>
      <td>0.9836</td>
      <td>3.0118</td>
      <td>0.9927</td>
      <td>0.9929</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-1.7784</td>
      <td>-2.1870</td>
      <td>-1.6567</td>
      <td>-1.3043</td>
      <td>-1.2506</td>
      <td>-0.3201</td>
      <td>-1.4398</td>
      <td>-2.3543</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>-0.6748</td>
      <td>-0.8405</td>
      <td>-0.3690</td>
      <td>-0.1861</td>
      <td>-0.5571</td>
      <td>-0.0926</td>
      <td>-0.7946</td>
      <td>-1.0846</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>-0.1653</td>
      <td>0.0308</td>
      <td>-0.0688</td>
      <td>-0.0997</td>
      <td>-0.2262</td>
      <td>-0.0322</td>
      <td>-0.6544</td>
      <td>0.5465</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.4778</td>
      <td>0.6645</td>
      <td>0.2366</td>
      <td>0.0045</td>
      <td>0.2430</td>
      <td>0.0398</td>
      <td>0.9632</td>
      <td>0.7808</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5.8885</td>
      <td>1.8526</td>
      <td>21.9507</td>
      <td>25.9592</td>
      <td>12.9348</td>
      <td>193.0775</td>
      <td>2.8987</td>
      <td>2.5353</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-1a0e2810-1f0f-4910-8702-b15106baead0')"
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
          document.querySelector('#df-1a0e2810-1f0f-4910-8702-b15106baead0 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-1a0e2810-1f0f-4910-8702-b15106baead0');
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




При этом та же самая история, где-то мы не получили чистый 0 в `mean` и 1 в `std`, это снова из-за того, что используем `mean` и `std` с обучения.

В этом ничего страшного нет, на тесте нет цели получить сто процентный 0 и 1, цель - **перевести данные в такую же шкалу**, что получили на трейне.

#### Самостоятельно

С sklearn версией разобрались, сейчас давайте напишим свой нормализатор.

Для начала нужно посчитать среднее и стандартное отклонение в исходных данных.


```python
means = X_train.mean()
stds = X_train.std()

display(means, stds)
```


    MedInc          3.8633
    HouseAge       28.6108
    AveRooms        5.4316
    AveBedrms       1.0983
    Population   1425.9480
    AveOccup        3.0270
    Latitude       35.6397
    Longitude    -119.5765
    dtype: float64



    MedInc          1.8913
    HouseAge       12.6252
    AveRooms        2.5964
    AveBedrms       0.5013
    Population   1136.2025
    AveOccup        6.4241
    Latitude        2.1391
    Longitude       2.0064
    dtype: float64


А затем нужно пройтись по всем признакам и вычесть среднее и поделить на стандартное отклонение:

$$x = \frac{x - mean}{std}$$



```python
X_train_my_std = X_train.copy()

for col in X_train.columns:
    X_train_my_std[col] = (X_train[col] - means[col]) / stds[col]

X_train_my_std.describe()
```





  <div id="df-e4916403-b740-47bb-b702-38ca52441826">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>-0.0000</td>
      <td>0.0000</td>
      <td>-0.0000</td>
      <td>0.0000</td>
      <td>-0.0000</td>
      <td>0.0000</td>
      <td>-0.0000</td>
      <td>-0.0000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-1.7783</td>
      <td>-2.1870</td>
      <td>-1.7661</td>
      <td>-1.5259</td>
      <td>-1.2524</td>
      <td>-0.3634</td>
      <td>-1.4491</td>
      <td>-2.3792</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>-0.6895</td>
      <td>-0.8404</td>
      <td>-0.3840</td>
      <td>-0.1835</td>
      <td>-0.5641</td>
      <td>-0.0930</td>
      <td>-0.7993</td>
      <td>-1.1082</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>-0.1749</td>
      <td>0.0308</td>
      <td>-0.0807</td>
      <td>-0.0986</td>
      <td>-0.2292</td>
      <td>-0.0326</td>
      <td>-0.6450</td>
      <td>0.5366</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.4634</td>
      <td>0.6645</td>
      <td>0.2402</td>
      <td>0.0019</td>
      <td>0.2676</td>
      <td>0.0396</td>
      <td>0.9725</td>
      <td>0.7808</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5.8883</td>
      <td>1.8526</td>
      <td>52.5646</td>
      <td>65.7663</td>
      <td>30.1496</td>
      <td>92.8830</td>
      <td>2.9500</td>
      <td>2.6249</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-e4916403-b740-47bb-b702-38ca52441826')"
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
          document.querySelector('#df-e4916403-b740-47bb-b702-38ca52441826 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-e4916403-b740-47bb-b702-38ca52441826');
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




Или же такой проход в цикле могли бы заменить на более приятную операцию, работая со всеми признаками за раз.


```python
X_train_my_std = (X_train - means) / stds
X_train_my_std.describe()
```





  <div id="df-1ca3fab3-06d5-403d-8f12-5adf918f965b">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
      <td>16512.0000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>-0.0000</td>
      <td>0.0000</td>
      <td>-0.0000</td>
      <td>0.0000</td>
      <td>-0.0000</td>
      <td>0.0000</td>
      <td>-0.0000</td>
      <td>-0.0000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.0000</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-1.7783</td>
      <td>-2.1870</td>
      <td>-1.7661</td>
      <td>-1.5259</td>
      <td>-1.2524</td>
      <td>-0.3634</td>
      <td>-1.4491</td>
      <td>-2.3792</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>-0.6895</td>
      <td>-0.8404</td>
      <td>-0.3840</td>
      <td>-0.1835</td>
      <td>-0.5641</td>
      <td>-0.0930</td>
      <td>-0.7993</td>
      <td>-1.1082</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>-0.1749</td>
      <td>0.0308</td>
      <td>-0.0807</td>
      <td>-0.0986</td>
      <td>-0.2292</td>
      <td>-0.0326</td>
      <td>-0.6450</td>
      <td>0.5366</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.4634</td>
      <td>0.6645</td>
      <td>0.2402</td>
      <td>0.0019</td>
      <td>0.2676</td>
      <td>0.0396</td>
      <td>0.9725</td>
      <td>0.7808</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5.8883</td>
      <td>1.8526</td>
      <td>52.5646</td>
      <td>65.7663</td>
      <td>30.1496</td>
      <td>92.8830</td>
      <td>2.9500</td>
      <td>2.6249</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-1ca3fab3-06d5-403d-8f12-5adf918f965b')"
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
          document.querySelector('#df-1ca3fab3-06d5-403d-8f12-5adf918f965b button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-1ca3fab3-06d5-403d-8f12-5adf918f965b');
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




И так же стандартизируем тестовую выборку.


```python
X_test_my_std = (X_test - means) / stds
X_test_my_std.describe()
```





  <div id="df-5552ba11-1358-4b61-8bd0-71d2879b7519">
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
      <th>MedInc</th>
      <th>HouseAge</th>
      <th>AveRooms</th>
      <th>AveBedrms</th>
      <th>Population</th>
      <th>AveOccup</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>4128.0000</td>
      <td>4128.0000</td>
      <td>4128.0000</td>
      <td>4128.0000</td>
      <td>4128.0000</td>
      <td>4128.0000</td>
      <td>4128.0000</td>
      <td>4128.0000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.0195</td>
      <td>0.0113</td>
      <td>-0.0050</td>
      <td>-0.0159</td>
      <td>-0.0021</td>
      <td>0.0340</td>
      <td>-0.0183</td>
      <td>0.0170</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.0222</td>
      <td>0.9843</td>
      <td>0.7352</td>
      <td>0.6845</td>
      <td>0.9835</td>
      <td>3.0117</td>
      <td>0.9927</td>
      <td>0.9929</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-1.7783</td>
      <td>-2.1870</td>
      <td>-1.6566</td>
      <td>-1.3043</td>
      <td>-1.2506</td>
      <td>-0.3201</td>
      <td>-1.4397</td>
      <td>-2.3542</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>-0.6748</td>
      <td>-0.8404</td>
      <td>-0.3690</td>
      <td>-0.1861</td>
      <td>-0.5571</td>
      <td>-0.0926</td>
      <td>-0.7946</td>
      <td>-1.0845</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>-0.1653</td>
      <td>0.0308</td>
      <td>-0.0688</td>
      <td>-0.0997</td>
      <td>-0.2261</td>
      <td>-0.0322</td>
      <td>-0.6543</td>
      <td>0.5465</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.4778</td>
      <td>0.6645</td>
      <td>0.2366</td>
      <td>0.0045</td>
      <td>0.2430</td>
      <td>0.0398</td>
      <td>0.9632</td>
      <td>0.7808</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5.8883</td>
      <td>1.8526</td>
      <td>21.9501</td>
      <td>25.9584</td>
      <td>12.9344</td>
      <td>193.0717</td>
      <td>2.8986</td>
      <td>2.5352</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-5552ba11-1358-4b61-8bd0-71d2879b7519')"
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
          document.querySelector('#df-5552ba11-1358-4b61-8bd0-71d2879b7519 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-5552ba11-1358-4b61-8bd0-71d2879b7519');
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




#### Обучение модели на масштабированных данных

Снова повторим обучение моделей, но уже на стандартизированных данных.


```python
knn = KNeighborsRegressor()
knn.fit(X_train_std, y_train)
```




    KNeighborsRegressor(algorithm='auto', leaf_size=30, metric='minkowski',
                        metric_params=None, n_jobs=None, n_neighbors=5, p=2,
                        weights='uniform')



Для справки, что было до



```
исходные данные
Train R2 0.45
Test R2 0.18

после нормализации
Train R2 0.81
Test R2 0.70
```



И снова качество выросло, при этом здесь оно такое же, как и после нормализации данных.


```python
pred_train = knn.predict(X_train_std)
pred_test = knn.predict(X_test_std)


print(f'Train R2 {r2_score(y_train, pred_train):.2f}')
print(f'Test R2 {r2_score(y_test, pred_test):.2f}')
```

    Train R2 0.81
    Test R2 0.70
    

Давайте попробуем другую модель.


```python
lr = LinearRegression()
lr.fit(X_train_std, y_train)
```




    LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None, normalize=False)



Для справки, что было до



```
исходные данные
Train R2 0.61
Test R2 0.60

после нормализации
Train R2 0.61
Test R2 0.60
```




```python
pred_train = lr.predict(X_train_std)
pred_test = lr.predict(X_test_std)


print(f'Train R2 {r2_score(y_train, pred_train):.2f}')
print(f'Test R2 {r2_score(y_test, pred_test):.2f}')
```

    Train R2 0.61
    Test R2 0.60
    

И здесь сильно не поменялось, делаем вывод, что в этом случае наши данные не вызывают дискомфорта у линейной модели, хоть масштабируй, хоть нет, результат будет одинаковым.

Но уж точно лишним не будет сделать масштабирование данных для таких моделей.

Ради интереса можем обучить и дерево решений и сравнить его метрики.


```python
tree_2 = DecisionTreeRegressor(random_state=1, max_depth=9)
tree_2.fit(X_train_norm, y_train)
```




    DecisionTreeRegressor(ccp_alpha=0.0, criterion='mse', max_depth=9,
                          max_features=None, max_leaf_nodes=None,
                          min_impurity_decrease=0.0, min_impurity_split=None,
                          min_samples_leaf=1, min_samples_split=2,
                          min_weight_fraction_leaf=0.0, presort='deprecated',
                          random_state=1, splitter='best')



```
метрики до
Train R2 0.79
Test R2 0.72
```


```python
pred_train = tree_2.predict(X_train_norm)
pred_test = tree_2.predict(X_test_norm)


print(f'Train R2 {r2_score(y_train, pred_train):.2f}')
print(f'Test R2 {r2_score(y_test, pred_test):.2f}')
```

    Train R2 0.79
    Test R2 0.72
    

А в целом, так всё и осталось, ведь дереву решений без разницы, какой масштаб у признака, дерево решений задает вопросы к данным.

А если мы применем масштабирование, то вопросы к признакам не поменяются, поменяется только значение, с которым сравнивается вопрос.

А если забыли, как строится **дерево решений**, то можете вспомнить, посмотрев [здесь](https://youtube.com/playlist?list=PLkJJmZ1EJno5eV954-PwtRJAw2lE6s-w1)


```python
from sklearn.tree import plot_tree

plt.figure(figsize=(10, 8))
plot_tree(tree_1, max_depth=1, filled=True, feature_names=X_train.columns);
```


    
![png](%D0%9A%D0%BE%D0%BF%D0%B8%D1%8F%20%D0%B1%D0%BB%D0%BE%D0%BA%D0%BD%D0%BE%D1%82%D0%B0%20%20data_scaling_files/%D0%9A%D0%BE%D0%BF%D0%B8%D1%8F%20%D0%B1%D0%BB%D0%BE%D0%BA%D0%BD%D0%BE%D1%82%D0%B0%20%20data_scaling_125_0.png)
    



```python
plt.figure(figsize=(10, 8))
plot_tree(tree_2, max_depth=1, filled=True, feature_names=X_train.columns);
```


    
![png](%D0%9A%D0%BE%D0%BF%D0%B8%D1%8F%20%D0%B1%D0%BB%D0%BE%D0%BA%D0%BD%D0%BE%D1%82%D0%B0%20%20data_scaling_files/%D0%9A%D0%BE%D0%BF%D0%B8%D1%8F%20%D0%B1%D0%BB%D0%BE%D0%BA%D0%BD%D0%BE%D1%82%D0%B0%20%20data_scaling_126_0.png)
    


## Практика
Практика доступна на платформе boosty https://boosty.to/machine_learrrning/posts/dab53732-2f3f-4a45-9ab3-4da4849cd584

Доступна
1. по подписке уровня light+ и выше
2. разовая оплата


## Summary

Вот мы и разобрались, зачем нужно масштабирование данных и как его применять.

- Масштабирование данных нужно для более **стабильного** обучения модели.
- Есть два основных вида масштабирование
    1. ***Нормализация***
        - После min = 0, max = 1 у всех признаков
        - Подход лучше зарекомендовал себя в подходах МЛ, которые работают с расстояниями (KNN) 
    2. ***Стандартизация***
        - После mean = 0, std = 1 у всех признаков
        - Подход лучше зарекомендовал себя в линейных подходах МЛ (LinearRegression, SVM) 
- Для моделей, основанных не **дереве решений** (DecisionTree, Bagging, RandomForest, Boosting) масштабирование данных **необязательно**

**Муррр** ♥
