# Проба сохранения и загрузки модели нейронной сети.


```python
import pandas as pd
from typing import Any
import numpy as np
import sqlite3
import matplotlib.pyplot as plt

from sklearn.metrics import mean_absolute_error
from sklearn.metrics import r2_score
from sklearn.model_selection import train_test_split
from keras.models import load_model
from keras.models import Sequential
from keras.layers import Dense, Dropout
import tensorflow as tf
tf.random.set_seed(9)


# connection = sqlite3.connect('rts_nn.db', check_same_thread=True)  # Создание соединения с БД для Colab
connection = sqlite3.connect(r'c:\Users\Alkor\gd\data_quote_db\RTS_nn.db', check_same_thread=True)  # Создание соединения с БД для Local
with connection:
  df_all = pd.read_sql('SELECT * FROM `All_opt`', connection)  # Загрузка данных из БД по всем опционам
  # df_all = pd.read_sql('SELECT * FROM `Nearest`', connection)  # Загрузка данных из БД по опционам с ближайшей датой экспирации

features = '-22500 -20000 -17500 -15000 -12500 -10000 -7500 -5000 -2500 0 2500 5000 7500 10000 12500 15000 17500 20000 22500'.split()  # Создание списка признаков
LABEL = 'close'
df = df_all[features + [LABEL]]
df = df.fillna(0)  # Замена NaN на "0"

a = df[LABEL]
percentile_min = int(np.percentile(a, 25) - (np.percentile(a, 75) - np.percentile(a, 25)) * 1.5)
percentile_25 = int(np.percentile(a, 25))
percentile_median = int(np.median(a))
percentile_75 = int(np.percentile(a, 75))
percentile_max = int(np.percentile(a, 75) + (np.percentile(a, 75) - np.percentile(a, 25)) * 1.5)

df = df[(df[LABEL] > percentile_min) & (df[LABEL] < percentile_max)]  # Осекаем все строки где 'close' ниже Перцентиле min и выше Перцентиле max

dataset = df.values  # Datframe преобразуем в Dataset для Keras
X = dataset[:,0:19]  # Все, что стоит перед запятой, относится к строкам массива, а все, что стоит после запятой, относится к столбцам массивов.
X = X.astype(np.float32)  # Смена типа для корректой работы Keras
y = dataset[:,19]  # Срез массива по labels
y = y.astype(np.float32)  # Смена типа для корректой работы Keras

X_train, X_val_and_test, y_train, y_val_and_test = train_test_split(X, y, test_size=0.3, random_state=3, shuffle=True)
X_val, X_test, y_val, y_test = train_test_split(X_val_and_test, y_val_and_test, test_size=0.5, random_state=0, shuffle=False)

model = Sequential()
model.add(Dense(512, activation='relu', input_shape=(X_train.shape[1], )))
model.add(Dropout(0.2))
model.add(Dense(512, activation='relu', ))
model.add(Dropout(0.1))
model.add(Dense(512, activation='relu', ))
model.add(Dropout(0.1))
model.add(Dense(512, activation='relu', ))
model.add(Dropout(0.1))
model.add(Dense(1))

model.compile(optimizer='adam', loss='mse', metrics=['mae'])

num_epochs = 100

model.fit(X_train, y_train,
          epochs=num_epochs, 
          batch_size=1312,
          validation_data=(X_test, y_test)
          )
```

    Epoch 1/100
    1/1 [==============================] - 1s 1s/step - loss: 9658532.0000 - mae: 2378.6267 - val_loss: 9935460.0000 - val_mae: 2407.0022
    Epoch 2/100
    1/1 [==============================] - 0s 111ms/step - loss: 9657095.0000 - mae: 2378.4473 - val_loss: 9934167.0000 - val_mae: 2406.8301
    Epoch 3/100
    1/1 [==============================] - 0s 106ms/step - loss: 9655248.0000 - mae: 2378.2170 - val_loss: 9932011.0000 - val_mae: 2406.5427
    Epoch 4/100
    1/1 [==============================] - 0s 107ms/step - loss: 9652205.0000 - mae: 2377.8342 - val_loss: 9928535.0000 - val_mae: 2406.0781
    Epoch 5/100
    1/1 [==============================] - 0s 116ms/step - loss: 9647422.0000 - mae: 2377.2292 - val_loss: 9923173.0000 - val_mae: 2405.3599
    Epoch 6/100
    1/1 [==============================] - 0s 108ms/step - loss: 9640197.0000 - mae: 2376.3369 - val_loss: 9915189.0000 - val_mae: 2404.3340
    Epoch 7/100
    1/1 [==============================] - 0s 101ms/step - loss: 9629205.0000 - mae: 2374.9714 - val_loss: 9903667.0000 - val_mae: 2402.9692
    Epoch 8/100
    1/1 [==============================] - 0s 109ms/step - loss: 9613496.0000 - mae: 2373.1526 - val_loss: 9887458.0000 - val_mae: 2401.0854
    Epoch 9/100
    1/1 [==============================] - 0s 105ms/step - loss: 9591561.0000 - mae: 2370.3613 - val_loss: 9865252.0000 - val_mae: 2398.4575
    Epoch 10/100
    1/1 [==============================] - 0s 107ms/step - loss: 9560831.0000 - mae: 2366.6646 - val_loss: 9835710.0000 - val_mae: 2395.0273
    Epoch 11/100
    1/1 [==============================] - 0s 106ms/step - loss: 9521021.0000 - mae: 2361.7639 - val_loss: 9797340.0000 - val_mae: 2390.4036
    Epoch 12/100
    1/1 [==============================] - 0s 109ms/step - loss: 9467921.0000 - mae: 2355.5044 - val_loss: 9748941.0000 - val_mae: 2384.5500
    Epoch 13/100
    1/1 [==============================] - 0s 107ms/step - loss: 9399551.0000 - mae: 2347.3057 - val_loss: 9689766.0000 - val_mae: 2377.9636
    Epoch 14/100
    1/1 [==============================] - 0s 106ms/step - loss: 9315068.0000 - mae: 2337.7283 - val_loss: 9620109.0000 - val_mae: 2370.1931
    Epoch 15/100
    1/1 [==============================] - 0s 109ms/step - loss: 9212253.0000 - mae: 2326.8416 - val_loss: 9542469.0000 - val_mae: 2361.8496
    Epoch 16/100
    1/1 [==============================] - 0s 111ms/step - loss: 9090949.0000 - mae: 2314.2412 - val_loss: 9463185.0000 - val_mae: 2354.5278
    Epoch 17/100
    1/1 [==============================] - 0s 112ms/step - loss: 8951022.0000 - mae: 2300.5137 - val_loss: 9394694.0000 - val_mae: 2354.2563
    Epoch 18/100
    1/1 [==============================] - 0s 110ms/step - loss: 8812896.0000 - mae: 2288.7725 - val_loss: 9358885.0000 - val_mae: 2363.1526
    Epoch 19/100
    1/1 [==============================] - 0s 111ms/step - loss: 8694974.0000 - mae: 2283.4192 - val_loss: 9390184.0000 - val_mae: 2390.7917
    Epoch 20/100
    1/1 [==============================] - 0s 112ms/step - loss: 8626858.0000 - mae: 2288.8979 - val_loss: 9522323.0000 - val_mae: 2433.3225
    Epoch 21/100
    1/1 [==============================] - 0s 109ms/step - loss: 8639676.0000 - mae: 2309.5215 - val_loss: 9711537.0000 - val_mae: 2478.6536
    Epoch 22/100
    1/1 [==============================] - 0s 108ms/step - loss: 8732139.0000 - mae: 2341.0510 - val_loss: 9814875.0000 - val_mae: 2501.4490
    Epoch 23/100
    1/1 [==============================] - 0s 113ms/step - loss: 8790545.0000 - mae: 2356.5508 - val_loss: 9783932.0000 - val_mae: 2495.9424
    Epoch 24/100
    1/1 [==============================] - 0s 113ms/step - loss: 8762016.0000 - mae: 2351.1609 - val_loss: 9672450.0000 - val_mae: 2472.6272
    Epoch 25/100
    1/1 [==============================] - 0s 112ms/step - loss: 8692062.0000 - mae: 2333.1040 - val_loss: 9542874.0000 - val_mae: 2442.9824
    Epoch 26/100
    1/1 [==============================] - 0s 113ms/step - loss: 8617159.0000 - mae: 2311.2676 - val_loss: 9434704.0000 - val_mae: 2416.3928
    Epoch 27/100
    1/1 [==============================] - 0s 112ms/step - loss: 8562959.0000 - mae: 2295.4233 - val_loss: 9360812.0000 - val_mae: 2394.9355
    Epoch 28/100
    1/1 [==============================] - 0s 114ms/step - loss: 8574875.0000 - mae: 2286.6948 - val_loss: 9316745.0000 - val_mae: 2378.1943
    Epoch 29/100
    1/1 [==============================] - 0s 112ms/step - loss: 8566067.0000 - mae: 2278.7444 - val_loss: 9293109.0000 - val_mae: 2367.0291
    Epoch 30/100
    1/1 [==============================] - 0s 113ms/step - loss: 8583396.0000 - mae: 2277.1030 - val_loss: 9280071.0000 - val_mae: 2359.8818
    Epoch 31/100
    1/1 [==============================] - 0s 113ms/step - loss: 8585870.0000 - mae: 2271.6035 - val_loss: 9270873.0000 - val_mae: 2356.0198
    Epoch 32/100
    1/1 [==============================] - 0s 114ms/step - loss: 8586588.0000 - mae: 2270.0098 - val_loss: 9262072.0000 - val_mae: 2354.4099
    Epoch 33/100
    1/1 [==============================] - 0s 113ms/step - loss: 8583104.0000 - mae: 2269.6621 - val_loss: 9252858.0000 - val_mae: 2354.6494
    Epoch 34/100
    1/1 [==============================] - 0s 111ms/step - loss: 8565634.0000 - mae: 2268.2712 - val_loss: 9244389.0000 - val_mae: 2356.8411
    Epoch 35/100
    1/1 [==============================] - 0s 111ms/step - loss: 8542488.0000 - mae: 2267.9109 - val_loss: 9239172.0000 - val_mae: 2361.2371
    Epoch 36/100
    1/1 [==============================] - 0s 112ms/step - loss: 8515129.0000 - mae: 2266.5303 - val_loss: 9240213.0000 - val_mae: 2367.7271
    Epoch 37/100
    1/1 [==============================] - 0s 114ms/step - loss: 8482352.0000 - mae: 2266.4724 - val_loss: 9250131.0000 - val_mae: 2376.5676
    Epoch 38/100
    1/1 [==============================] - 0s 113ms/step - loss: 8449525.0000 - mae: 2267.1321 - val_loss: 9269177.0000 - val_mae: 2386.8176
    Epoch 39/100
    1/1 [==============================] - 0s 115ms/step - loss: 8429510.0000 - mae: 2271.4685 - val_loss: 9293704.0000 - val_mae: 2396.8809
    Epoch 40/100
    1/1 [==============================] - 0s 106ms/step - loss: 8435654.0000 - mae: 2276.2273 - val_loss: 9315012.0000 - val_mae: 2405.0906
    Epoch 41/100
    1/1 [==============================] - 0s 112ms/step - loss: 8421338.0000 - mae: 2280.1497 - val_loss: 9323334.0000 - val_mae: 2409.3254
    Epoch 42/100
    1/1 [==============================] - 0s 111ms/step - loss: 8420732.0000 - mae: 2282.2002 - val_loss: 9311132.0000 - val_mae: 2408.3010
    Epoch 43/100
    1/1 [==============================] - 0s 110ms/step - loss: 8398318.0000 - mae: 2282.7607 - val_loss: 9278688.0000 - val_mae: 2402.0669
    Epoch 44/100
    1/1 [==============================] - 0s 110ms/step - loss: 8386324.0000 - mae: 2275.7803 - val_loss: 9232668.0000 - val_mae: 2391.9797
    Epoch 45/100
    1/1 [==============================] - 0s 111ms/step - loss: 8356545.5000 - mae: 2269.0532 - val_loss: 9181993.0000 - val_mae: 2380.2822
    Epoch 46/100
    1/1 [==============================] - 0s 112ms/step - loss: 8325568.5000 - mae: 2259.5942 - val_loss: 9133436.0000 - val_mae: 2368.1992
    Epoch 47/100
    1/1 [==============================] - 0s 106ms/step - loss: 8284127.5000 - mae: 2251.4143 - val_loss: 9090994.0000 - val_mae: 2357.3032
    Epoch 48/100
    1/1 [==============================] - 0s 111ms/step - loss: 8274604.5000 - mae: 2247.4893 - val_loss: 9054498.0000 - val_mae: 2348.5930
    Epoch 49/100
    1/1 [==============================] - 0s 108ms/step - loss: 8255909.0000 - mae: 2241.9365 - val_loss: 9022240.0000 - val_mae: 2341.8484
    Epoch 50/100
    1/1 [==============================] - 0s 109ms/step - loss: 8237604.0000 - mae: 2235.9797 - val_loss: 8992789.0000 - val_mae: 2337.2588
    Epoch 51/100
    1/1 [==============================] - 0s 109ms/step - loss: 8212567.5000 - mae: 2231.7659 - val_loss: 8965016.0000 - val_mae: 2334.6267
    Epoch 52/100
    1/1 [==============================] - 0s 111ms/step - loss: 8174294.0000 - mae: 2227.9497 - val_loss: 8939031.0000 - val_mae: 2333.6270
    Epoch 53/100
    1/1 [==============================] - 0s 113ms/step - loss: 8133383.0000 - mae: 2223.7388 - val_loss: 8914951.0000 - val_mae: 2333.6230
    Epoch 54/100
    1/1 [==============================] - 0s 114ms/step - loss: 8116368.5000 - mae: 2224.1362 - val_loss: 8891586.0000 - val_mae: 2333.6692
    Epoch 55/100
    1/1 [==============================] - 0s 121ms/step - loss: 8057730.5000 - mae: 2219.6384 - val_loss: 8866182.0000 - val_mae: 2333.3933
    Epoch 56/100
    1/1 [==============================] - 0s 116ms/step - loss: 8019594.0000 - mae: 2220.7822 - val_loss: 8831824.0000 - val_mae: 2330.7458
    Epoch 57/100
    1/1 [==============================] - 0s 109ms/step - loss: 7987587.0000 - mae: 2218.4849 - val_loss: 8781244.0000 - val_mae: 2323.8208
    Epoch 58/100
    1/1 [==============================] - 0s 120ms/step - loss: 7918989.0000 - mae: 2207.6750 - val_loss: 8712872.0000 - val_mae: 2312.3203
    Epoch 59/100
    1/1 [==============================] - 0s 112ms/step - loss: 7836617.0000 - mae: 2193.5576 - val_loss: 8628906.0000 - val_mae: 2297.1851
    Epoch 60/100
    1/1 [==============================] - 0s 117ms/step - loss: 7788181.0000 - mae: 2185.4646 - val_loss: 8536781.0000 - val_mae: 2280.3887
    Epoch 61/100
    1/1 [==============================] - 0s 111ms/step - loss: 7746763.5000 - mae: 2176.3230 - val_loss: 8441469.0000 - val_mae: 2263.3621
    Epoch 62/100
    1/1 [==============================] - 0s 115ms/step - loss: 7673686.5000 - mae: 2161.7822 - val_loss: 8348612.0000 - val_mae: 2248.4260
    Epoch 63/100
    1/1 [==============================] - 0s 111ms/step - loss: 7596905.0000 - mae: 2145.1709 - val_loss: 8260861.0000 - val_mae: 2236.5212
    Epoch 64/100
    1/1 [==============================] - 0s 114ms/step - loss: 7501054.0000 - mae: 2134.4954 - val_loss: 8179972.5000 - val_mae: 2228.6558
    Epoch 65/100
    1/1 [==============================] - 0s 114ms/step - loss: 7404993.5000 - mae: 2122.9939 - val_loss: 8105897.5000 - val_mae: 2222.9070
    Epoch 66/100
    1/1 [==============================] - 0s 117ms/step - loss: 7321744.0000 - mae: 2117.2891 - val_loss: 8029980.0000 - val_mae: 2216.1587
    Epoch 67/100
    1/1 [==============================] - 0s 111ms/step - loss: 7256672.5000 - mae: 2105.1125 - val_loss: 7937709.0000 - val_mae: 2204.2129
    Epoch 68/100
    1/1 [==============================] - 0s 114ms/step - loss: 7158642.5000 - mae: 2090.7351 - val_loss: 7823750.5000 - val_mae: 2185.6614
    Epoch 69/100
    1/1 [==============================] - 0s 113ms/step - loss: 7072062.5000 - mae: 2071.6035 - val_loss: 7704443.5000 - val_mae: 2163.6047
    Epoch 70/100
    1/1 [==============================] - 0s 109ms/step - loss: 7023439.0000 - mae: 2062.6057 - val_loss: 7606864.5000 - val_mae: 2146.4370
    Epoch 71/100
    1/1 [==============================] - 0s 112ms/step - loss: 6921787.5000 - mae: 2044.0918 - val_loss: 7548468.0000 - val_mae: 2142.2166
    Epoch 72/100
    1/1 [==============================] - 0s 111ms/step - loss: 6901734.0000 - mae: 2037.4719 - val_loss: 7517566.0000 - val_mae: 2143.8855
    Epoch 73/100
    1/1 [==============================] - 0s 109ms/step - loss: 6748091.0000 - mae: 2026.9690 - val_loss: 7469588.0000 - val_mae: 2140.0833
    Epoch 74/100
    1/1 [==============================] - 0s 110ms/step - loss: 6750349.5000 - mae: 2025.8409 - val_loss: 7384610.5000 - val_mae: 2126.3391
    Epoch 75/100
    1/1 [==============================] - 0s 113ms/step - loss: 6638787.0000 - mae: 2001.4509 - val_loss: 7325516.5000 - val_mae: 2116.6516
    Epoch 76/100
    1/1 [==============================] - 0s 108ms/step - loss: 6602832.5000 - mae: 1991.6575 - val_loss: 7287367.0000 - val_mae: 2109.9609
    Epoch 77/100
    1/1 [==============================] - 0s 115ms/step - loss: 6607474.5000 - mae: 1990.0483 - val_loss: 7281954.0000 - val_mae: 2110.9055
    Epoch 78/100
    1/1 [==============================] - 0s 112ms/step - loss: 6504335.5000 - mae: 1974.7714 - val_loss: 7279573.5000 - val_mae: 2112.7285
    Epoch 79/100
    1/1 [==============================] - 0s 115ms/step - loss: 6459434.5000 - mae: 1966.9180 - val_loss: 7272731.5000 - val_mae: 2110.2288
    Epoch 80/100
    1/1 [==============================] - 0s 117ms/step - loss: 6477028.5000 - mae: 1975.3295 - val_loss: 7243818.0000 - val_mae: 2100.0662
    Epoch 81/100
    1/1 [==============================] - 0s 118ms/step - loss: 6454352.5000 - mae: 1969.3074 - val_loss: 7247117.5000 - val_mae: 2099.5254
    Epoch 82/100
    1/1 [==============================] - 0s 112ms/step - loss: 6411667.0000 - mae: 1959.6340 - val_loss: 7284162.0000 - val_mae: 2107.2910
    Epoch 83/100
    1/1 [==============================] - 0s 118ms/step - loss: 6394859.5000 - mae: 1955.0980 - val_loss: 7291797.0000 - val_mae: 2107.5740
    Epoch 84/100
    1/1 [==============================] - 0s 115ms/step - loss: 6395295.5000 - mae: 1960.2452 - val_loss: 7294379.5000 - val_mae: 2106.8428
    Epoch 85/100
    1/1 [==============================] - 0s 118ms/step - loss: 6387960.5000 - mae: 1958.2277 - val_loss: 7297847.5000 - val_mae: 2107.2485
    Epoch 86/100
    1/1 [==============================] - 0s 120ms/step - loss: 6316298.5000 - mae: 1945.9539 - val_loss: 7328788.0000 - val_mae: 2116.5254
    Epoch 87/100
    1/1 [==============================] - 0s 115ms/step - loss: 6321938.5000 - mae: 1943.7837 - val_loss: 7309285.0000 - val_mae: 2112.3718
    Epoch 88/100
    1/1 [==============================] - 0s 117ms/step - loss: 6300146.5000 - mae: 1936.7648 - val_loss: 7274388.0000 - val_mae: 2101.4470
    Epoch 89/100
    1/1 [==============================] - 0s 118ms/step - loss: 6234558.5000 - mae: 1922.6156 - val_loss: 7286541.0000 - val_mae: 2108.1062
    Epoch 90/100
    1/1 [==============================] - 0s 122ms/step - loss: 6268832.5000 - mae: 1929.4120 - val_loss: 7334602.5000 - val_mae: 2125.1082
    Epoch 91/100
    1/1 [==============================] - 0s 115ms/step - loss: 6271667.0000 - mae: 1930.0507 - val_loss: 7273044.0000 - val_mae: 2108.3713
    Epoch 92/100
    1/1 [==============================] - 0s 121ms/step - loss: 6246359.5000 - mae: 1931.0294 - val_loss: 7229495.0000 - val_mae: 2094.9824
    Epoch 93/100
    1/1 [==============================] - 0s 115ms/step - loss: 6237941.5000 - mae: 1928.0237 - val_loss: 7219585.5000 - val_mae: 2094.6423
    Epoch 94/100
    1/1 [==============================] - 0s 112ms/step - loss: 6150421.0000 - mae: 1918.3567 - val_loss: 7221681.5000 - val_mae: 2099.4429
    Epoch 95/100
    1/1 [==============================] - 0s 111ms/step - loss: 6153206.5000 - mae: 1911.1389 - val_loss: 7219550.5000 - val_mae: 2101.5493
    Epoch 96/100
    1/1 [==============================] - 0s 113ms/step - loss: 6210828.5000 - mae: 1930.4684 - val_loss: 7162019.0000 - val_mae: 2080.6689
    Epoch 97/100
    1/1 [==============================] - 0s 117ms/step - loss: 6125373.0000 - mae: 1908.1892 - val_loss: 7164939.0000 - val_mae: 2084.8989
    Epoch 98/100
    1/1 [==============================] - 0s 117ms/step - loss: 6098305.5000 - mae: 1917.5913 - val_loss: 7190424.5000 - val_mae: 2096.4136
    Epoch 99/100
    1/1 [==============================] - 0s 112ms/step - loss: 6107560.5000 - mae: 1911.6249 - val_loss: 7150716.0000 - val_mae: 2083.6228
    Epoch 100/100
    1/1 [==============================] - 0s 114ms/step - loss: 6147825.5000 - mae: 1912.5602 - val_loss: 7119140.0000 - val_mae: 2073.7878
    




    <keras.callbacks.History at 0x1773ddffbe0>




```python
pred_validate = model.predict(X_val)  # На выходе двумерный массив numpy с предсказаниями

mae = int(mean_absolute_error(pred_validate, y_val))  # Средняя абсолютная ошибка для данных
r2 = r2_score(y_val, pred_validate)  # Коэффициент детерминации


tmp = sum(pred_validate.tolist(),[])  # Преобразование двумерного массива numpy в одномерный список
pred_validate = np.array(tmp)  # Список в массив numpy

pred_df = pd.DataFrame({'true': y_val.astype(int),  # Реальные данные
                        'pred': pred_validate.astype(int)})  # Предсказанные данные
# Соотношение предсказаний к реалу с одним знаком (положительных/отрицательных)
count_odinakov_znak = 0
count_razn_znak = 0
for row in pred_df.itertuples(index=False):
    if (row[0] < 0 and row[1] < 0) or (row[0] > 0 and row[1] > 0):
        count_odinakov_znak += 1
    else:
        count_razn_znak += 1
print(f'{count_odinakov_znak =},  {count_razn_znak =}. Соотношение={count_razn_znak/count_odinakov_znak*100:.2f}%')

print(f'{LABEL=}')  # Какая цена предсказывается
print('Отсечение выбросов в выборке по перцентилям (info).')
print(f'Средняя абсолютная ошибка {mae=:,} R2_score={r2:.3f}. Эпох: {num_epochs}\n')
model.summary()
```

    9/9 [==============================] - 0s 3ms/step
    count_odinakov_znak =179,  count_razn_znak =93. Соотношение=51.96%
    LABEL='close'
    Отсечение выбросов в выборке по перцентилям (info).
    Средняя абсолютная ошибка mae=1,772 R2_score=0.324. Эпох: 100
    
    Model: "sequential_4"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     dense_20 (Dense)            (None, 512)               10240     
                                                                     
     dropout_16 (Dropout)        (None, 512)               0         
                                                                     
     dense_21 (Dense)            (None, 512)               262656    
                                                                     
     dropout_17 (Dropout)        (None, 512)               0         
                                                                     
     dense_22 (Dense)            (None, 512)               262656    
                                                                     
     dropout_18 (Dropout)        (None, 512)               0         
                                                                     
     dense_23 (Dense)            (None, 512)               262656    
                                                                     
     dropout_19 (Dropout)        (None, 512)               0         
                                                                     
     dense_24 (Dense)            (None, 1)                 513       
                                                                     
    =================================================================
    Total params: 798,721
    Trainable params: 798,721
    Non-trainable params: 0
    _________________________________________________________________
    


```python
model.save('my_model.h5')  # creates a HDF5 file 'my_model.h5'
del model  # deletes the existing model

# returns a compiled model
# identical to the previous one
model_load = load_model('my_model.h5')
```


```python
pred_validate = model_load.predict(X_val)  # На выходе двумерный массив numpy с предсказаниями

mae = int(mean_absolute_error(pred_validate, y_val))  # Средняя абсолютная ошибка для данных
r2 = r2_score(y_val, pred_validate)  # Коэффициент детерминации


tmp = sum(pred_validate.tolist(),[])  # Преобразование двумерного массива numpy в одномерный список
pred_validate = np.array(tmp)  # Список в массив numpy

pred_df = pd.DataFrame({'true': y_val.astype(int),  # Реальные данные
                        'pred': pred_validate.astype(int)})  # Предсказанные данные
# Соотношение предсказаний к реалу с одним знаком (положительных/отрицательных)
count_odinakov_znak = 0
count_razn_znak = 0
for row in pred_df.itertuples(index=False):
    if (row[0] < 0 and row[1] < 0) or (row[0] > 0 and row[1] > 0):
        count_odinakov_znak += 1
    else:
        count_razn_znak += 1
print(f'{count_odinakov_znak=},  {count_razn_znak=}. Соотношение={count_razn_znak/count_odinakov_znak*100:.2f}%')

print(f'{LABEL=}')  # Какая цена предсказывается
print('Отсечение выбросов в выборке по перцентилям (info).')
print(f'Средняя абсолютная ошибка {mae=:,} R2_score={r2:.3f}. Эпох: {num_epochs}\n')
model_load.summary()
```

    9/9 [==============================] - 0s 3ms/step
    count_odinakov_znak=179,  count_razn_znak=93. Соотношение=51.96%
    LABEL='close'
    Отсечение выбросов в выборке по перцентилям (info).
    Средняя абсолютная ошибка mae=1,772 R2_score=0.324. Эпох: 100
    
    Model: "sequential_4"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     dense_20 (Dense)            (None, 512)               10240     
                                                                     
     dropout_16 (Dropout)        (None, 512)               0         
                                                                     
     dense_21 (Dense)            (None, 512)               262656    
                                                                     
     dropout_17 (Dropout)        (None, 512)               0         
                                                                     
     dense_22 (Dense)            (None, 512)               262656    
                                                                     
     dropout_18 (Dropout)        (None, 512)               0         
                                                                     
     dense_23 (Dense)            (None, 512)               262656    
                                                                     
     dropout_19 (Dropout)        (None, 512)               0         
                                                                     
     dense_24 (Dense)            (None, 1)                 513       
                                                                     
    =================================================================
    Total params: 798,721
    Trainable params: 798,721
    Non-trainable params: 0
    _________________________________________________________________
    
