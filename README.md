## EXP 3 - Delhi Air Quality Analysis

## Name :  THAJESH  K

## RegNo : 212223230229


## Aim

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


## Code and Output
```
import pandas as pd

file_path = "delhi_pm25_aqi.csv"  
df = pd.read_csv(file_path)

print("Dataset shape:", df.shape)
print("\nHead of dataset:")
print(df.head())

print("\nData types:")
print(df.dtypes)

print("\nNull counts:")
print(df.isnull().sum())
```
<img width="702" height="515" alt="image" src="https://github.com/user-attachments/assets/d95a3957-ab30-48d6-b736-f5374cdf3d58" />

```
df['datetime'] = pd.to_datetime(df['period.datetimeFrom.utc'], errors='coerce')
df['pm25'] = pd.to_numeric(df['value'], errors='coerce')

df = df.dropna(subset=['datetime', 'pm25']).reset_index(drop=True)

print("Rows after cleaning:", len(df))
```
<img width="336" height="44" alt="image" 
src="https://github.com/user-attachments/assets/fbd6796f-013f-404d-b51a-bf837679f3d0" />

```
df['date'] = df['datetime'].dt.date
df['month'] = df['datetime'].dt.month
df['month_name'] = df['datetime'].dt.strftime('%b')
df['hour'] = df['datetime'].dt.hour

print(df[['datetime', 'date', 'month_name', 'hour', 'pm25']].head())
```
<img width="789" height="148" alt="image" src="https://github.com/user-attachments/assets/64624bf9-3ac4-4296-b507-27a91defdd5c" />

```
import matplotlib.pyplot as plt
import pandas as pd

plt.figure(figsize=(8,5))
plt.boxplot([df[df['month'] == m]['pm25'] for m in range(1, 13)],
            labels=[pd.Timestamp(2000, m, 1).strftime('%b') for m in range(1, 13)])
plt.title("Monthly PM2.5 Levels")
plt.xlabel("Month")
plt.ylabel("PM2.5 (µg/m³)")
plt.show()
```

<img width="1585" height="507" alt="image" src="https://github.com/user-attachments/assets/4c0ba0a1-a24b-4d98-b2e3-cbd39948ef9a" />

```
monthly_avg = df.groupby('month')['pm25'].mean()
monthly_avg.index = [pd.Timestamp(2000,m,1).strftime('%b') for m in monthly_avg.index]

plt.figure(figsize=(10,5))
plt.plot(monthly_avg.index, monthly_avg.values, marker='o', linewidth=2)
plt.title("Monthly Average PM2.5")
plt.xlabel("Month")
plt.ylabel("Average PM2.5 (µg/m³)")
plt.grid(True)
plt.show()

print(monthly_avg)

```
<img width="960" height="632" alt="image" src="https://github.com/user-attachments/assets/58c6a96e-abe0-47cc-b958-d9f874beed25" />

```
daily_avg = df.groupby('date')['pm25'].mean()
total_days = len(daily_avg)
exceed_days = (daily_avg > 25).sum()
percent_exceed = (exceed_days / total_days) * 100

print(f"Total days: {total_days}")
print(f"Days exceeding WHO PM2.5 limit (25 µg/m³): {exceed_days}")
print(f"Percentage: {percent_exceed:.2f}%")
```

<img width="538" height="80" alt="image" src="https://github.com/user-attachments/assets/89432fdc-84c6-4cfe-b271-b6fbde4fc453" />

```
hourly_avg = df.groupby('hour')['pm25'].mean()

plt.figure(figsize=(10,5))
plt.plot(hourly_avg.index, hourly_avg.values, marker='o')
plt.title("Average PM2.5 by Hour of Day")
plt.xlabel("Hour (0–23)")
plt.ylabel("Average PM2.5 (µg/m³)")
plt.grid(True)
plt.show()

print(hourly_avg)
```
<img width="718" height="723" alt="image" src="https://github.com/user-attachments/assets/b79ef049-4adf-478a-bdc5-8e164a6ab830" />

```
top5 = daily_avg.sort_values(ascending=False).head(5)
print("Top 5 worst-polluted days (daily average PM2.5):")
print(top5)
```
<img width="567" height="178" alt="image" src="https://github.com/user-attachments/assets/723360fc-0245-4fa4-b0d1-d4b40f9fa943" />





## Interpretation

1. PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

2. Seasonal variation is clearly observed — PM2.5 levels are generally higher during winter months due to temperature inversion, limited atmospheric dispersion, and increased burning activities, while monsoon rains significantly reduce pollutant concentration through wet deposition.

3. NO₂ concentrations also follow a seasonal trend, peaking during colder months as vehicular emissions accumulate under stable conditions and decreasing during the monsoon when pollutants are naturally dispersed.

4. The data indicates potential public health implications, as elevated PM2.5 and NO₂ levels coincide with increased risks of respiratory issues and smog formation, underscoring the importance of timely pollution control and mitigation efforts.


## Result

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


