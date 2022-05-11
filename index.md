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

```
receivers.rename(columns={"Unnamed: 0_level_0":"Player"}, inplace=True)
receivers.drop(columns={"Unnamed: 1_level_0":"Bye"}, inplace=True)
receivers.rename(columns={"Unnamed: 2_level_0":"Pts"}, inplace=True)

import matplotlib.pyplot as plt

receivers["Touches"] = receivers["Passing"]["Cmp"] + receivers["Rushing"]["Att"] + receivers["Receiving"]["Rec"]
receivers["Points per Touch"] = receivers["Pts"]["Pts*"]/receivers["Touches"]

receivers.head()
```
