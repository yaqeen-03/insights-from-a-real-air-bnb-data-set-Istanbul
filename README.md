# insights-from-a-real-air-bnb-data-set-Istanbul
python code for insights on airbwb booking of Istanbul we are using calendar add listing csv files
Please find the link for downloading
https://data.insideairbnb.com/turkey/marmara/istanbul/2024-09-28/data/calendar.csv.gz
<img width="1094" alt="Screenshot 1446-07-14 at 9 53 25 AM" src="https://github.com/user-attachments/assets/6717219e-7185-4ccf-8f4e-79ecb498217c" />


```diff
import pandas as pd
calendar=pd.read_csv("calendar.csv")
```

# ✅ Questions that you might want to address about the given data

Let`s dive into exciting insights!

# 1. Want to know the number of available and unavailable rooms

```diff
calendar.available.value_counts()
```

<img width="301" alt="Screenshot 1446-07-28 at 9 39 51 AM" src="https://github.com/user-attachments/assets/74006dba-f4c9-4c2c-b0b7-11c002ae4bc7" />

f (false) means not available, t(true) means available.
