
# Netflix Shows & Movies

## Aim

To analyze Netflix dataset and compare movies vs TV shows, top producing countries, and release year trends.

## Procedure / Algorithm

  1)Load dataset (netflix_titles.csv).
  
  2)Count movies vs TV shows.
  
  3)Group by country → top contributors.
  
  4)Create pivot table (release year vs type).
  
  5)Visualize with bar & line charts.

## Program
import pandas as pd
import numpy as np

url="https://raw.githubusercontent.com/allenkong221/netflix-titles-dataset/main/netflix_titles.csv"
df=pd.read_csv(url)

print("Shape:",df.shape)

print("Columns:",df.columns,"\n")

print("Types:",df['type'].value_counts(),"\n")

df['date_added']=df['date_added'].astype(str).str.strip()
df['date_added']=pd.to_datetime(df['date_added'],errors='coerce')
df['year_added']=df['date_added'].dt.year.astype('Int64')
df['month_added']=df['date_added'].dt.month_name()

df.head()

count_by_type = df.groupby('type')['title'].count()
print("Count by Type:\n", count_by_type, "\n")

pivot_country_type = df.pivot_table(
index='country',
columns='type',
values='title',
aggfunc='count',
fill_value=0
)

pivot_country_type['Total'] = pivot_country_type.sum(axis=1)
max_country = pivot_country_type['Total'].idxmax()  # country with most titles
max_count = pivot_country_type['Total'].max()       
# number of titles
print("Pivot Table (Country vs Type):\n", pivot_country_type.head(), "\n")
print(f"Largest Overall: {max_country} with {max_count} titles\n")

top_directors = df['director'].value_counts().head(5)
print("Top 5 Directors:\n", top_directors, "\n")

trend=df.groupby(['year_added','type']).size().unstack(fill_value=0)
print("Yearly Trend by Type\n")
print(trend.head())

df_genre = (
df[['show_id','listed_in']]
.dropna()
.assign(listed_in=df['listed_in'].str.split(', '))
.explode('listed_in')
)

df_expanded = df.merge(df_genre, on='show_id', how='left')

print("Columns after merge:\n", df_expanded.columns)

print("\nExpanded Genre Sample:\n",
df_expanded[['title','listed_in_y']].head())


top_genres = df_expanded['listed_in_y'].value_counts().head(5)
print("\nTop 5 Genres:\n", top_genres, "\n")

## Ouptut
<img width="759" height="226" alt="image" src="https://github.com/user-attachments/assets/a92f9dcf-9c42-469a-9245-c895cb61d7cc" />

<img width="871" height="171" alt="image" src="https://github.com/user-attachments/assets/55a85b41-1ac8-4bd2-a077-0a8e0e8cca3c" />

<img width="615" height="527" alt="image" src="https://github.com/user-attachments/assets/5553b192-1771-422b-829f-b5dae609d57f" />

<img width="514" height="402" alt="image" src="https://github.com/user-attachments/assets/bdf68c33-216e-462a-9c26-425e1f8c0c57" />

<img width="602" height="332" alt="image" src="https://github.com/user-attachments/assets/ecba8329-15a2-428f-baff-c87053c0e15a" />






## Result
Helps Netflix in content planning & investments.
