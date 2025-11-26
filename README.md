# Super-Bowl-Analysis
Analysing Superbowl data using Python

import pandas as pd
from matplotlib import pyplot as plt

tv = pd.read_csv("datasets/tv.csv")
super_bowls = pd.read_csv("datasets/super_bowls.csv")
halftime_musicians = pd.read_csv("datasets/halftime_musicians.csv")

tv = tv.merge(super_bowls[["super_bowl", "date"]], on="super_bowl", how="left")

tv["year"] = pd.to_datetime(tv["date"]).dt.year

yearly_viewership = tv.groupby("year")["avg_us_viewers"].mean().sort_index()

viewership_increased = yearly_viewership.iloc[-1] > yearly_viewership.iloc[0]
print("Has TV viewership increased over time?", viewership_increased)

plt.figure(figsize=(10, 6))
tv_sorted = tv.sort_values("super_bowl")
plt.plot(tv_sorted["super_bowl"], tv_sorted["avg_us_viewers"], marker='o')
plt.xlabel("Super Bowl Edition")
plt.ylabel("Average US Viewers")
plt.title("Average US Viewers by Super Bowl Edition")
plt.grid(True)
plt.show()

difference_count = (super_bowls['difference_pts'] > 40).sum()
print("Number of Super Bowl matches with a difference greater than 40:", difference_count)

plt.figure(figsize=(8, 5))
plt.hist(super_bowls["difference_pts"], bins=10, edgecolor="black")
plt.xlabel("Points Difference")
plt.ylabel("Frequency")
plt.title("Distribution of the Points Difference")
plt.show()

songs_per_musician = halftime_musicians.groupby("musician")["num_songs"].sum().sort_values(ascending=False)

most_songs_musician = songs_per_musician.idxmax()
print("Musician with the most songs in the Super Bowl:", most_songs_musician)
