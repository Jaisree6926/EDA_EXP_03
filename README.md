**EXP 3 - Delhi Air Quality Analysis**

**Aim**


To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.


**Procedure / Algorithm**

1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.


**Program**

**Name : Jaisree B**

**Reg No: 212224230100**

```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("delhi_pm25_aqi.csv")

print("First 5 rows:")
print(df.head())

print("\nData types and non-null counts:")
print(df.info())
print("\nNull values per column:")
print(df.isnull().sum())

df['datetime'] = pd.to_datetime(
df['period.datetimeFrom.utc'],
errors='coerce'
)

df['value'] = pd.to_numeric(df['value'], errors='coerce')

df = df.dropna(subset=['datetime', 'value'])

df['date'] = df['datetime'].dt.date
df['month'] = df['datetime'].dt.month_name()
df['hour'] = df['datetime'].dt.hour
print(df[['datetime', 'value', 'date', 'month', 'hour']].head())

plt.figure(figsize=(12,6))
month_order = [

'January','February','March','April','May','June',
'July','August','September','October','November','December'
]
sns.boxplot(
x='month',
y='value',
data=df,
order=month_order
)
plt.xticks(rotation=45)
plt.title("Monthly Distribution of PM2.5 – Delhi")
plt.xlabel("Month")
plt.ylabel("PM2.5 (μg/m3)")
plt.tight_layout()
plt.show()

monthly_avg = df.groupby('month')['value'].mean().reindex(month_order)
plt.figure(figsize=(10,5))
monthly_avg.plot(kind='bar')
plt.title("Monthly Average PM2.5 – Delhi")
plt.xlabel("Month")
plt.ylabel("Average PM2.5 (μg/m3)")
plt.tight_layout()
plt.show()

WHO_LIMIT = 25
# Daily average PM2.5
daily_avg = df.groupby('date')['value'].mean()
total_days = daily_avg.shape[0]
exceed_days = (daily_avg > WHO_LIMIT).sum()
percentage_exceed = (exceed_days / total_days) * 100
print(f"Total days: {total_days}")
print(f"Days exceeding WHO limit: {exceed_days}")
print(f"Percentage of unsafe days: {percentage_exceed:.2f}%")

hourly_avg = df.groupby('hour')['value'].mean().reset_index()
plt.figure(figsize=(10,5))
sns.lineplot(x='hour', y='value', data=hourly_avg, marker='o')
plt.title("Average PM2.5 by Hour of Day – Delhi")
plt.xlabel("Hour of Day")
plt.ylabel("PM2.5 (μg/m3)")
plt.xticks(range(0,24))
plt.tight_layout()
plt.show()

```


**Output**
<img width="1486" height="728" alt="Screenshot 2026-02-09 085042" src="https://github.com/user-attachments/assets/d5d0145c-ea84-4d7a-9389-88b125a9939d" />
<img width="741" height="173" alt="Screenshot 2026-02-09 085011" src="https://github.com/user-attachments/assets/3b4eca81-af0c-4247-949a-b7c0e69cb03c" />
<img width="879" height="485" alt="Screenshot 2026-02-09 085002" src="https://github.com/user-attachments/assets/1bcbb6e1-c6b6-408b-be6a-9c156d1f48c3" />
<img width="778" height="182" alt="Screenshot 2026-02-09 084954" src="https://github.com/user-attachments/assets/1711a90a-6737-43db-8909-5b048675761c" />
<img width="1239" height="612" alt="Screenshot 2026-02-09 085113" src="https://github.com/user-attachments/assets/1708be68-c24b-4731-b072-4e86e2f036fa" />
<img width="554" height="87" alt="Screenshot 2026-02-09 085059" src="https://github.com/user-attachments/assets/93d674fd-f13e-42fa-87c4-ebae8f317d3b" />
<img width="1234" height="615" alt="Screenshot 2026-02-09 085052" src="https://github.com/user-attachments/assets/0c63a7ff-18f8-431d-9b3c-a845234012d7" />



**Interpretation**

1)PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

2)  # write other insights

**Result**

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


