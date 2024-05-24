# Закачка через wget


```python
!wget -P sample_data/ https://drive.google.com/file/d/1Czrj2NsGHE2PofFeYRC8nUOL89qv3uaL/view?usp=sharing
```

    --2022-12-30 10:12:03--  https://drive.google.com/file/d/1Czrj2NsGHE2PofFeYRC8nUOL89qv3uaL/view?usp=sharing
    Resolving drive.google.com (drive.google.com)... 74.125.204.138, 74.125.204.102, 74.125.204.139, ...
    Connecting to drive.google.com (drive.google.com)|74.125.204.138|:443... connected.
    HTTP request sent, awaiting response... 404 Not Found
    2022-12-30 10:12:04 ERROR 404: Not Found.
    
    


```python
!ls
```

    sample_data
    


```python
!ls sample_data/
```

    anscombe.json		      mnist_test.csv
    california_housing_test.csv   mnist_train_small.csv
    california_housing_train.csv  README.md
    


```python
import pandas as pd
from pathlib import Path

df = pd.read_csv(Path('sample_data/california_housing_test.csv'), delimiter=',')
df
```





  <div id="df-3b25426d-03a1-4cc9-a1be-54420d3fe43a">
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
      <th>longitude</th>
      <th>latitude</th>
      <th>housing_median_age</th>
      <th>total_rooms</th>
      <th>total_bedrooms</th>
      <th>population</th>
      <th>households</th>
      <th>median_income</th>
      <th>median_house_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-122.05</td>
      <td>37.37</td>
      <td>27.0</td>
      <td>3885.0</td>
      <td>661.0</td>
      <td>1537.0</td>
      <td>606.0</td>
      <td>6.6085</td>
      <td>344700.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-118.30</td>
      <td>34.26</td>
      <td>43.0</td>
      <td>1510.0</td>
      <td>310.0</td>
      <td>809.0</td>
      <td>277.0</td>
      <td>3.5990</td>
      <td>176500.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-117.81</td>
      <td>33.78</td>
      <td>27.0</td>
      <td>3589.0</td>
      <td>507.0</td>
      <td>1484.0</td>
      <td>495.0</td>
      <td>5.7934</td>
      <td>270500.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-118.36</td>
      <td>33.82</td>
      <td>28.0</td>
      <td>67.0</td>
      <td>15.0</td>
      <td>49.0</td>
      <td>11.0</td>
      <td>6.1359</td>
      <td>330000.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-119.67</td>
      <td>36.33</td>
      <td>19.0</td>
      <td>1241.0</td>
      <td>244.0</td>
      <td>850.0</td>
      <td>237.0</td>
      <td>2.9375</td>
      <td>81700.0</td>
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
    </tr>
    <tr>
      <th>2995</th>
      <td>-119.86</td>
      <td>34.42</td>
      <td>23.0</td>
      <td>1450.0</td>
      <td>642.0</td>
      <td>1258.0</td>
      <td>607.0</td>
      <td>1.1790</td>
      <td>225000.0</td>
    </tr>
    <tr>
      <th>2996</th>
      <td>-118.14</td>
      <td>34.06</td>
      <td>27.0</td>
      <td>5257.0</td>
      <td>1082.0</td>
      <td>3496.0</td>
      <td>1036.0</td>
      <td>3.3906</td>
      <td>237200.0</td>
    </tr>
    <tr>
      <th>2997</th>
      <td>-119.70</td>
      <td>36.30</td>
      <td>10.0</td>
      <td>956.0</td>
      <td>201.0</td>
      <td>693.0</td>
      <td>220.0</td>
      <td>2.2895</td>
      <td>62000.0</td>
    </tr>
    <tr>
      <th>2998</th>
      <td>-117.12</td>
      <td>34.10</td>
      <td>40.0</td>
      <td>96.0</td>
      <td>14.0</td>
      <td>46.0</td>
      <td>14.0</td>
      <td>3.2708</td>
      <td>162500.0</td>
    </tr>
    <tr>
      <th>2999</th>
      <td>-119.63</td>
      <td>34.42</td>
      <td>42.0</td>
      <td>1765.0</td>
      <td>263.0</td>
      <td>753.0</td>
      <td>260.0</td>
      <td>8.5608</td>
      <td>500001.0</td>
    </tr>
  </tbody>
</table>
<p>3000 rows × 9 columns</p>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-3b25426d-03a1-4cc9-a1be-54420d3fe43a')"
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
          document.querySelector('#df-3b25426d-03a1-4cc9-a1be-54420d3fe43a button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-3b25426d-03a1-4cc9-a1be-54420d3fe43a');
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



