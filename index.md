## CMSC320 Final Project

This code below gets the receiver fantasy data as a pandas table called receivers.
```
import pandas as pd
import requests

url = 'https://www.footballdb.com/fantasy-football/index.html?pos=WR&yr=2021&wk=all&key=b6406b7aea3872d5bb677f064673c57f'

session = requests.Session()
headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.93 Safari/537.36'}
page = session.get(url, headers=headers)
pdtable = pd.read_html(page.content.decode())

receivers = pdtable[0]

receivers
```


The code below cleans the data and adds two new columns: (1)Points per Touch and (2)Touches
```
receivers.rename(columns={"Unnamed: 0_level_0":"Player"}, inplace=True)
receivers.drop(columns={"Unnamed: 1_level_0":"Bye"}, inplace=True)
receivers.rename(columns={"Unnamed: 2_level_0":"Pts"}, inplace=True)

receivers["Player Name"] = list(map(lambda x: re.sub("\..+", '', x)[:-1], receivers["Player"]["Player"]))

receivers.drop("Player", axis=1, inplace=True)
receivers.drop("Bye", axis=1, inplace=True)


import matplotlib.pyplot as plt

receivers["Touches"] = receivers["Passing"]["Cmp"] + receivers["Rushing"]["Att"] + receivers["Receiving"]["Rec"]
receivers["Points per Touch"] = receivers["Pts"]["Pts*"]/receivers["Touches"]

receivers.head()
```

This code splits our data into two different dataframes: (1)Looking at receivers with 50+ touches and (2)Looking at the top. 50 receivers.
```
receivers_50Touches = receivers[receivers['Touches'] > 50]
top50receivers = receivers[:50]

receivers_50Touches.reset_index()
```

This code creates a graph for each df plotting touches vs ppt. The two lines are the mean touches and the mean ppt.
```
plt.scatter(receivers_50Touches["Touches"], 
            receivers_50Touches["Points per Touch"], c="red")

plt.plot([np.mean(receivers_50Touches["Touches"]),np.mean(receivers_50Touches["Touches"])],
         [np.min(receivers_50Touches["Points per Touch"]), np.max(receivers_50Touches["Points per Touch"])], c="blue")
plt.plot([np.min(receivers_50Touches["Touches"]), np.max(receivers_50Touches["Touches"])], 
         [np.mean(receivers_50Touches["Points per Touch"]),np.mean(receivers_50Touches["Points per Touch"])], c="blue")

plt.title("Receivers with 50+ Touches")
plt.xlabel("Touches")
plt.ylabel("Points per Touch")
plt.show()


plt.scatter(top50receivers["Touches"], 
            top50receivers["Points per Touch"], c="red")

plt.plot([np.mean(top50receivers["Touches"]),np.mean(top50receivers["Touches"])],
         [np.min(top50receivers["Points per Touch"]), np.max(top50receivers["Points per Touch"])], c="blue")
plt.plot([np.min(top50receivers["Touches"]), np.max(top50receivers["Touches"])], 
         [np.mean(top50receivers["Points per Touch"]),np.mean(top50receivers["Points per Touch"])], c="blue")

plt.title("Top 50 Receivers")
plt.xlabel("Touches")
plt.ylabel("Points per Touch")
plt.show()
```

