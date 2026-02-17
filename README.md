# -IPL-2025-Analysis

IPL  2025 Perform to EDA

Code cell <HDNznH04uySH>
# %% [code]

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np



Code cell <G54eXlKfx8PC>
# %% [code]
vr =pd.read_csv('/content/ipl_batsman 2025.csv')

Code cell <XocMVQCLyRax>
# %% [code]
vr

Code cell <vzr3d7OtyfG8>
# %% [code]
vr.to_dict()

Code cell <H5OQ3E6Gyj1q>
# %% [code]
vr.head()

Code cell <nZOSJEvNzqiJ>
# %% [code]
vr.describe()

Code cell <duVQx0NF1Fzq>
# %% [code]
vr.info()

Code cell <oX7UcNdY2pZp>
# %% [code]
vr.isnull().sum()

Code cell <0XJmUh0O2w5->
# %% [code]
vr.duplicated().sum()

Code cell <Mtltu8QB23z5>
# %% [code]
vr.dropna(inplace=True)

Code cell <htLg76Pu3BEH>
# %% [code]
vr.sum()

Text cell <5cY_4nWt5UsT>
# %% [markdown]
Perform to chart

Code cell <ptdDIODY5aCN>
# %% [code]
sns.barplot(x='batting_position', y='runs_scored',data=vr,color='red')
plt.title('Bating position Vice runs')
plt.show()

Code cell <KzyZVYGT7D88>
# %% [code]
plt.figure(figsize=(12,6))
sns.barplot(x='batting_position', y='strike_rate'.data=vr.color = red')
plt.show()

Text cell <_0udfDhm7tKE>
# %% [markdown]
  2. Runs Scored Distribution
****

  ##. To understand how runs are spread across players and identify skewness.



Code cell <xLCVTPn-6aNL>
# %% [code]
plt.figure(figsize=(8,5))
sns.histplot(vr["runs_scored"], bins=30, kde=True)
plt.title("Distribution of Runs Scored by Batsmen")
plt.xlabel("Runs")
plt.ylabel("Frequency")
plt.show()

Text cell <Ddq73N2wmvU6>
# %% [markdown]
* Most players score low to moderate runs

* A small number of players contribute very high runs

* Distribution is right-skewed, common in T20 cricket



Text cell <J8a5F0Qc1GN1>
# %% [markdown]
**3. Strike rate Analysis**

Code cell <-lDedfc1nqDZ>
# %% [code]
plt.figure(figsize=(8,5))
sns.histplot(vr["strike_rate"], bins=30, kde=True)
plt.title("Strike Rate Distribution")
plt.xlabel("Strike Rate")
plt.ylabel("Frequency")
plt.show()

Text cell <qy254PpMoY9T>
# %% [markdown]
* Majority of batsmen operate in the 120–160 strike rate range

* Few players play ultra-aggressive roles (>180 SR)

Text cell <a2DBeJwQomp2>
# %% [markdown]
**4. Run vs strike rate relationship**

Code cell <jhJ2QkaMof8j>
# %% [code]
plt.figure(figsize=(8,6))

sns.scatterplot(x="runs_scored", y="strike_rate", data=vr)
plt.title("Relationship between Runs Scored and Strike Rate")
plt.xlabel("Runs Scored")
plt.ylabel("Strike Rate")
plt.show()

Text cell <6mOJzM6ipmgo>
# %% [markdown]
* High strike rate does not always mean high runs

* Top performers combine volume + efficiency

* Useful for identifying impact players




Code cell <SFIsmkUwqBMm>
# %% [code]
vr["Boundaries"] = vr["fours"] + vr["sixes"]

# Aggregate per player
player_boundaries = (
    vr
    .groupby("striker", as_index=False)["Boundaries"]
    .sum()
)

Code cell <Kg81ETgDqqJ9>
# %% [code]
top10 = player_boundaries.sort_values(
    "Boundaries", ascending=False
).head(10)

Code cell <kha8RfYcqxKc>
# %% [code]
plt.figure(figsize=(8,5))

sns.barplot(data=top10, x="Boundaries", y="striker")

plt.title("Top 10 Boundary Hitters – IPL 2025")
plt.xlabel("Total Boundaries (Fours + Sixes)")
plt.ylabel("Player")

plt.tight_layout()
plt.show()


Text cell <2RuIFgTcrBnC>
# %% [markdown]
* To identify the most aggressive batsmen, we aggregated total boundaries (fours + sixes) scored by each player across the season. After grouping by player and summing boundary counts, the top 10 boundary hitters were visualized using a horizontal bar chart for better readability.

* This analysis highlights players who consistently contribute quick runs and play a key role in shifting match momentum in T20 cricket.

Text cell <6ZAkplGprGn0>
# %% [markdown]
6. Ball paced vs **runs**

Code cell <5ddjHfDqtYGf>
# %% [code]
sns.scatterplot(
    data=vr,x="balls_faced",y="runs_scored",
    alpha=0.7
)
plt.title("Runs vs Balls Faced")
plt.xlabel("Balls Faced")
plt.ylabel("Runs")
plt.show()


Text cell <e38WQbzwucqL>
# %% [markdown]
* Linear relationship indicates consistency

* Outliers show high strike rate innings



Code cell <cSgYLXUCucPH>
# %% [code]
top_scorers = vr.sort_values("runs_scored", ascending=False).head(10)

plt.figure(figsize=(8,5))
sns.barplot(data=top_scorers, x="runs_scored", y="striker")
plt.title("Top 10 Run Scorers – IPL 2025")
plt.xlabel("Runs")
plt.ylabel("Player")
plt.show()


Text cell <lcbZud2-vVPJ>
# %% [markdown]
* this shows us the highest score scored by an individual player in a single inning
* also shows player who often plays anchor or consistent finisher roles


Text cell <VNcSKSALvcca>
# %% [markdown]
**7 .Batting position Analysis**

Code cell <jEFgc571vmHO>
# %% [code]
sns.boxplot(data=vr, x='batting_position', y='runs_scored')
plt.title('Batting Position vs Runs Scored')
plt.show()

Text cell <J_IPfArcwiE4>
# %% [markdown]
* Top-order batsmen generally score higher due to more balls faced.
* Middle and lower order players play specialized finishing roles.


Text cell <TOxEDoctwnkV>
# %% [markdown]
**Running Between the Wickets**

Code cell <yNFnEo83w2z_>
# %% [code]
vr["running_runs"] = (
    vr["singles"]
    + 2*vr["doubles"]
    + 3*vr["triples"]
    + 5*vr["fives"]
)

sns.scatterplot(
    data=vr,
    x="running_runs",
    y="runs_scored",
    alpha=0.6
)
plt.title("Running Runs vs Total Runs")
plt.show()

Text cell <AoT9gKHJxson>
# %% [markdown]
* unning between the wickets plays a vital role for anchors and players on slower pitches.


