# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:


### OUTPUT:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose


data = pd.read_csv("Amazon Sale Report.csv", low_memory=False)

data['Date'] = pd.to_datetime(data['Date'], errors='coerce')   
data['Amount'] = pd.to_numeric(data['Amount'], errors='coerce') 


data = data.dropna(subset=['Date', 'Amount'])

data = data.groupby('Date').sum(numeric_only=True)

data = data.sort_index()


data['amount_diff'] = data['Amount'] - data['Amount'].shift(1)

result = seasonal_decompose(data['Amount'], model='additive', period=12)
data['amount_seasonal_adj'] = data['Amount'] - result.seasonal

data['amount_log'] = np.log(data['Amount'].replace(0, np.nan)).interpolate()

data['amount_log_diff'] = data['amount_log'] - data['amount_log'].shift(1)

log_diff = data['amount_log_diff'].dropna()
result = seasonal_decompose(log_diff, model='additive', period=12)
data.loc[log_diff.index, 'amount_log_seasonal_adj'] = log_diff - result.seasonal

plt.figure(figsize=(16, 16))

plt.subplot(6, 1, 1)
plt.plot(data['Amount'], label='Original')
plt.legend(loc='best')
plt.title('Original Data')

plt.subplot(6, 1, 2)
plt.plot(data['amount_diff'], label='Regular Difference')
plt.legend(loc='best')

plt.subplot(6, 1, 3)
plt.plot(data['amount_seasonal_adj'], label='Seasonally Adjusted')
plt.legend(loc='best')

plt.subplot(6, 1, 4)
plt.plot(data['amount_log'], label='Log Transformation')
plt.legend(loc='best')

plt.subplot(6, 1, 5)
plt.plot(data['amount_log_diff'], label='Log + Regular Differencing')
plt.legend(loc='best')

plt.subplot(6, 1, 6)
plt.plot(data['amount_log_seasonal_adj'], label='Log + Diff + Seasonal Adj')
plt.legend(loc='best')

plt.tight_layout()
plt.show()

```

REGULAR DIFFERENCING:
<img width="1846" height="311" alt="image" src="https://github.com/user-attachments/assets/da852a09-dfd8-4f4f-85d8-39363cb05ffa" />



SEASONAL ADJUSTMENT:

<img width="1851" height="321" alt="image" src="https://github.com/user-attachments/assets/b0e39041-a249-42dd-bda2-3ff857b3f6cb" />

LOG TRANSFORMATION:

<img width="1834" height="307" alt="image" src="https://github.com/user-attachments/assets/fb30fc00-3828-4bee-8640-f2c0eb4174c1" />


### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
