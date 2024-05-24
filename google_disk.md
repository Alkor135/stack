# Работа с файлом на google диске


```python
from google.colab import drive  # Импорт библиотеки для работы с гугл диском
drive.mount('/content/drive')  # Подключение гугл диска
# drive.mount('/gd')  # Подключение гугл диска
```

    Mounted at /content/drive
    


```python
import pandas as pd
from pathlib import *

df = pd.read_csv(Path('/content/drive/MyDrive/data_quote/data_prepare_RTS_range_mvc_tpsl_sec/SPFB.RTS_00_range250_splice_2022.txt'), delimiter=',')
df
```





  <div id="df-42ffc51f-1498-499e-a56b-81eddd8b2d06">
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
      <td>20220103</td>
      <td>70000</td>
      <td>159950.0</td>
      <td>160200.0</td>
      <td>159950.0</td>
      <td>160200.0</td>
      <td>1066</td>
      <td>159970.0</td>
      <td>135</td>
      <td>1</td>
      <td>1</td>
      <td>0.08</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20220103</td>
      <td>70001</td>
      <td>160600.0</td>
      <td>160960.0</td>
      <td>160710.0</td>
      <td>160710.0</td>
      <td>857</td>
      <td>160960.0</td>
      <td>121</td>
      <td>3</td>
      <td>1</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20220103</td>
      <td>70002</td>
      <td>160700.0</td>
      <td>160730.0</td>
      <td>160480.0</td>
      <td>160480.0</td>
      <td>431</td>
      <td>160600.0</td>
      <td>147</td>
      <td>3</td>
      <td>4</td>
      <td>0.52</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20220103</td>
      <td>70006</td>
      <td>160470.0</td>
      <td>160600.0</td>
      <td>160350.0</td>
      <td>160350.0</td>
      <td>2401</td>
      <td>160510.0</td>
      <td>350</td>
      <td>3</td>
      <td>63</td>
      <td>0.36</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20220103</td>
      <td>70109</td>
      <td>160340.0</td>
      <td>160510.0</td>
      <td>160260.0</td>
      <td>160260.0</td>
      <td>2858</td>
      <td>160370.0</td>
      <td>212</td>
      <td>3</td>
      <td>275</td>
      <td>0.56</td>
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
      <th>12773</th>
      <td>20220131</td>
      <td>232158</td>
      <td>144950.0</td>
      <td>145170.0</td>
      <td>144920.0</td>
      <td>145170.0</td>
      <td>2495</td>
      <td>145050.0</td>
      <td>263</td>
      <td>0</td>
      <td>638</td>
      <td>0.52</td>
    </tr>
    <tr>
      <th>12774</th>
      <td>20220131</td>
      <td>233236</td>
      <td>145180.0</td>
      <td>145370.0</td>
      <td>145120.0</td>
      <td>145120.0</td>
      <td>2573</td>
      <td>145330.0</td>
      <td>227</td>
      <td>0</td>
      <td>317</td>
      <td>0.16</td>
    </tr>
    <tr>
      <th>12775</th>
      <td>20220131</td>
      <td>233753</td>
      <td>145110.0</td>
      <td>145200.0</td>
      <td>144950.0</td>
      <td>144950.0</td>
      <td>3099</td>
      <td>145100.0</td>
      <td>247</td>
      <td>-1</td>
      <td>433</td>
      <td>0.40</td>
    </tr>
    <tr>
      <th>12776</th>
      <td>20220131</td>
      <td>234506</td>
      <td>144940.0</td>
      <td>145180.0</td>
      <td>144930.0</td>
      <td>145180.0</td>
      <td>725</td>
      <td>145080.0</td>
      <td>89</td>
      <td>0</td>
      <td>135</td>
      <td>0.60</td>
    </tr>
    <tr>
      <th>12777</th>
      <td>20220131</td>
      <td>234721</td>
      <td>145190.0</td>
      <td>145260.0</td>
      <td>145110.0</td>
      <td>145120.0</td>
      <td>1333</td>
      <td>145230.0</td>
      <td>277</td>
      <td>0</td>
      <td>900</td>
      <td>2.00</td>
    </tr>
  </tbody>
</table>
<p>12778 rows × 12 columns</p>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-42ffc51f-1498-499e-a56b-81eddd8b2d06')"
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
          document.querySelector('#df-42ffc51f-1498-499e-a56b-81eddd8b2d06 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-42ffc51f-1498-499e-a56b-81eddd8b2d06');
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



