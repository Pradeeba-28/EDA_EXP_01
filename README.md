**#Experiment 1: EDA in IPL Dataset**  

**Aim:**  
To perform Exploratory Data Analysis (EDA) on the IPL matches dataset and derive insights about matches per season, winning teams, toss decisions, and top venues.

**Algorithm / Procedure:**  

**1.Import Libraries**  

  Import pandas for data handling.  
  Import matplotlib and seaborn for visualization.  
  
**2.Load Dataset**  

  Use pd.read_csv() to load the IPL matches dataset.  
  Check dataset shape using .shape.  
  View first 5 rows using .head().  
  
**3.Matches per Season (Univariate Analysis)**  

  Group data by season and count matches.  
  Plot a bar chart to visualize growth/decline in matches.  
  
**4.Top Winning Teams (Univariate Analysis)**  

  Use value_counts() on the winner column.  
  Plot top 5 winning teams in a bar chart.  
  
**5.Toss Decisions (Univariate Analysis)**  

  Count toss decision preferences (bat vs field).  
  Plot results using a bar chart.  
  
**6.Top Venues (Univariate Analysis)**  

  Count matches per venue.  
  Display top 5 venues with a horizontal bar chart.  
  
**7.Draw Insights**  

  Observe patterns in toss decisions.  
  Identify teams with consistent winning trends.  
  
  **Program**  
```python
import pandas as pd
# Load the IPL dataset
df = pd.read_csv("matches.csv")
```
**1. Dataset Understanding**
```python
# Q1. Dataset shape and first 5 records
print("\nQ1. DATASET UNDERSTANDING")
print("Rows:", df.shape[0])
print("Columns:", df.shape[1])
print(df.head())
```
**2.Columns and data types**
```python
# Q2. Column names and data types
print("\nQ2. COLUMNS AND DATA TYPES")
print(df.dtypes)
```
**3.Unique Identifier**
```python
# Q3. Check unique match ID
print("\nQ3. UNIQUE IDENTIFIER")
print("Unique IDs:", df["id"].nunique())
print("Is ID unique:", df["id"].is_unique)
```
**4. Data cleaning**
```python
# Q4. Missing values and duplicate records
print("\nQ4. DATA CLEANING")
print("Missing values:")
print(df.isnull().sum()[df.isnull().sum() > 0])
print("Duplicate records before cleaning:", df.duplicated().sum())

df["city"] = df["city"].fillna("Unknown")
df = df.drop_duplicates().copy()

print("Missing city values after cleaning:", df["city"].isnull().sum())
print("Duplicate records after cleaning:", df.duplicated().sum())
```
**5. Season analysis**
```python
# Q5. Matches played in each season
print("\nQ5. SEASON ANALYSIS")
season_matches = df.groupby("season").size()
print(season_matches)
print("Season with most matches:", season_matches.idxmax())
print("Highest number of matches:", season_matches.max())
```
**6. Top 5 winning teams**
```python
# Q6. Top 5 teams by wins
print("\nQ6. TOP 5 WINNING TEAMS")
team_wins = (
    df.dropna(subset=["winner"])
      .groupby("winner")
      .size()

      .sort_values(ascending=False)
)
print(team_wins.head(5))
```
**7.CSK matches**
```python
# Q7. Chennai Super Kings wins
print("\nQ7. CSK MATCHES")
csk = df[df["winner"] == "Chennai Super Kings"]
print(csk[["season", "team1", "team2", "winner"]].head(10))
print("Total CSK wins:", len(csk))
```
**8. Toss decision analysis**
```python
# Q8. Toss decision frequency and percentage
print("\nQ8. TOSS DECISION ANALYSIS")
toss_count = df["toss_decision"].value_counts()
toss_percent = df["toss_decision"].value_counts(normalize=True) * 100
print("Frequency:")
print(toss_count)
print("Percentage:")
print(toss_percent.round(2))
print("Most preferred:", toss_count.idxmax())
```
**9. Season wise toss analysis**
```python
# Q9. Season-wise toss decision analysis
print("\nQ9. SEASON-WISE TOSS ANALYSIS")
toss_season = pd.crosstab(df["season"], df["toss_decision"])
print(toss_season)
print("Most preferred decision by season:")
print(toss_season.idxmax(axis=1))
```
**10. Top 5 venues**
```python
# Q10. Top 5 venues
print("\nQ10. TOP 5 VENUES")
venues = df.groupby("venue").size().sort_values(ascending=False)
print(venues.head(5))
```
**11. Winning margin analysis**
```python
# Q11. Largest and top 10 winning margins
print("\nQ11. WINNING MARGIN ANALYSIS")
margin_df = df.dropna(subset=["result_margin"])
largest = margin_df.loc[margin_df["result_margin"].idxmax()]
print("Largest winning margin:", largest["result_margin"])
print(largest[[
    "season", "team1", "team2", "winner",
    "result", "result_margin"
]])

print("\nTop 10 winning margins:")
print(
    margin_df.sort_values("result_margin", ascending=False)
    [["season", "team1", "team2", "winner", "result_margin"]]
    .head(10).to_string(index=False)
)
```
**12. Match result analysis**
```python
# Q12. Match result frequency
print("\nQ12. MATCH RESULT ANALYSIS")
print(df["result"].value_counts())
```
**13. Datetime conversion**
```python
# Q13. Convert date to datetime
print("\nQ13. DATETIME CONVERSION")
print("Before conversion:", df["date"].dtype)
df["date"] = pd.to_datetime(df["date"], errors="coerce")
print("After conversion:", df["date"].dtype)
```
**14. Year extraction**
```python
# Q14. Extract year from date
print("\nQ14. YEAR EXTRACTION")
df["year"] = df["date"].dt.year
print(df[["date", "year"]].head(10).to_string(index=False))
```
**15. Win type creation**
```python
# Q15. Create win_type column
print("\nQ15. WIN TYPE CREATION")
win_type_map = {
    "runs": "Won by Runs",
    "wickets": "Won by Wickets",
    "tie": "Tie",
    "no result": "No Result"
}
df["win_type"] = df["result"].map(win_type_map).fillna("Other")
print(df[["result", "win_type"]].head(10).to_string(index=False))
```
```python
# Save the cleaned dataset
df.to_csv("IPL_Matches_Cleaned.csv", index=False)
print("\nCleaned dataset saved as IPL_Matches_Cleaned.csv")
```

  **Output**  
<img width="602" height="646" alt="image" src="https://github.com/user-attachments/assets/0a18ceb2-e91f-4db2-a678-c9d71407ca08" />


<img width="258" height="392" alt="image" src="https://github.com/user-attachments/assets/161b6602-533f-4259-92f5-0aa5babdcc39" />


<img width="202" height="60" alt="image" src="https://github.com/user-attachments/assets/fc14ad19-0305-48b2-96f7-081a7e83f0d7" />


<img width="332" height="237" alt="image" src="https://github.com/user-attachments/assets/e68fa249-5b9a-4e67-b82f-3912af33fc74" />


<img width="287" height="387" alt="image" src="https://github.com/user-attachments/assets/b5bd9d3b-b38b-4321-b139-cf7a55ec8166" />


<img width="302" height="147" alt="image" src="https://github.com/user-attachments/assets/fe35a75f-3a9d-4df7-b7cb-9b8d8c086527" />


<img width="540" height="437" alt="image" src="https://github.com/user-attachments/assets/5a5921aa-41cc-441d-9298-62d92f846d43" />


<img width="390" height="245" alt="image" src="https://github.com/user-attachments/assets/6069a597-726b-4af5-a498-98ca52c42972" />


<img width="282" height="702" alt="image" src="https://github.com/user-attachments/assets/89d5748f-7bd0-4030-b6b0-c8375ec25847" />


<img width="471" height="163" alt="image" src="https://github.com/user-attachments/assets/fcf264fd-499c-4016-8f62-fb262c9aff68" />


<img width="958" height="448" alt="image" src="https://github.com/user-attachments/assets/621d3829-2ffd-4a26-9ef9-9fa5b5e933b5" />


<img width="300" height="141" alt="image" src="https://github.com/user-attachments/assets/0b09d43d-bfa5-433c-ad13-8d1105058ea7" />


<img width="310" height="70" alt="image" src="https://github.com/user-attachments/assets/5b0a13b4-125e-4deb-a026-44ca9ca6e896" />


<img width="220" height="250" alt="image" src="https://github.com/user-attachments/assets/f308e445-0b21-4bd5-aa32-c3d307b4dd5e" />


<img width="276" height="247" alt="image" src="https://github.com/user-attachments/assets/1ca22b38-9abe-4677-98e0-1587b6e7e9b0" />


<img width="522" height="36" alt="image" src="https://github.com/user-attachments/assets/a6a06292-b267-4995-ae90-fab46867c6d7" />

 **Result**  
 
  This experiment is executed successfully



Highlight the stadiums hosting maximum matches.
