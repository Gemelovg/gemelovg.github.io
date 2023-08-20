---
title: Buying stocks or better not!
date: 2022-09-03
catergories: [python]
tags: [python,pandas,matplotlib,excel,plot]
---



# Using pandas and yahoo finance to plot stock data.
As someone how likes to lose money on the stock market, Im always looking for ways to make it happend faster. 
I just found out about the yahoo finance library for python. 




## Going to start by importing said yahoo library

```python
import yfinance as yf
import pandas as pd
```

It works like this. You resquest the infomartion from a ticker with start and the end date. 
Lets look at NVIDIA **NVDA**. I bought arround november 2021

## Read File and rename Colums to something easier to read.

```python
NVDA = yf.download("NVDA", start="2020-01-01", end="2022-09-03")
NVDA.head()
```
It looks something like this

```python
	Open	High	Low	Close	Adj Close	Volume
Date						
2020-01-02	59.687500	59.977501	59.180000	59.977501	59.803612	23753600
2020-01-03	58.775002	59.457500	58.525002	59.017502	58.846394	20538400
2020-01-06	58.080002	59.317501	57.817501	59.264999	59.093170	26263600
2020-01-07	59.549999	60.442501	59.097500	59.982498	59.808590	31485600
2020-01-08	59.939999	60.509998	59.537498	60.095001	59.920769	27710800
```

## Looking good, lets add a new column, _change_ that will move the period by 365 days to compare the price from the previous year.

```python
NVDA['change'] = NVDA.Close.pct_change(365)
NVDA['diff'] = NVDA.Close - NVDA.change
NVDA.fillna(0)
```
```python
Open	High	Low	Close	Adj Close	Volume	change	diff
Date								
2020-01-02	59.687500	59.977501	59.180000	59.977501	59.803612	23753600	0.000000	0.000000
2020-01-03	58.775002	59.457500	58.525002	59.017502	58.846394	20538400	0.000000	0.000000
2020-01-06	58.080002	59.317501	57.817501	59.264999	59.093170	26263600	0.000000	0.000000
2020-01-07	59.549999	60.442501	59.097500	59.982498	59.808590	31485600	0.000000	0.000000
2020-01-08	59.939999	60.509998	59.537498	60.095001	59.920769	27710800	0.000000	0.000000
...	...	...	...	...	...	...	...	...
2022-08-29	160.199997	163.380005	157.669998	158.009995	158.009995	49613200	0.241973	157.768022
2022-08-30	159.600006	160.389999	151.820007	154.679993	154.679993	53018100	0.204134	154.475859
2022-08-31	153.839996	155.399994	149.589996	150.940002	150.940002	57371000	0.144677	150.795325
2022-09-01	142.089996	143.800003	132.699997	139.369995	139.369995	117886500	0.066274	139.303721
2022-09-02	141.000000	141.710007	135.910004	136.470001	136.470001	74259000	0.079412	136.390590
```

## Looking good, lets plot.

```python
NVDA['diff'].plot()
```

![img-description](https://i.imgur.com/D7Ds2VI.png)


So, from a all time high near the end of 2021, the stock is now sitting at 136 dollars, about 58% loss. Not a great look.


Lets try Netflix an other tech gigant. Its ticker is **NFLX** and will plot it along side Nvidia.


```python
NFLX = yf.download("NFLX", start="2020-01-01", end="2022-09-03")
NFLX['change'] = NFLX.Close.pct_change(365)
NFLX['diff'] = NFLX.Close - NFLX.change
NVDA['diff'].plot()
NFLX['diff'].plot()
```

![img-description](https://i.imgur.com/IQfIoKc.png)

Neflix had an all time high of 690 dollars, now sitting at 226, that is a 67% loss. Tech isnt looking good. 
Lets look at something diffent. Retail. Costco is a big player. Lets plot.

```python
COST = yf.download("COST", start="2020-01-01", end="2022-09-03")
COST['change'] = COST.Close.pct_change(365)
COST['diff'] = COST.Close - COST.change
NVDA['diff'].plot()
NFLX['diff'].plot()
COST['diff'].plot()
```

![img-description](https://i.imgur.com/dYAyUwy.png)


We can see **Costco** has been very stable compared to other tech companies, if fact if you boought at the sametime I did, You are in the green.


Lets try an ETF, ETF are a pool of stocks that is traded the same way as stocks, so instead of buying Nvidia, Intel and Apple stocks, You can by QQQ that has all 3 and more.
Lets try **XLE**. It traks the energy sector of the S&P500


```python
XLE = yf.download("XLE", start="2020-01-01", end="2022-09-03")
XLE['change'] = XLE.Close.pct_change(365)
XLE['diff'] = XLE.Close - XLE.change
NVDA['diff'].plot()
NFLX['diff'].plot()
COST['diff'].plot()
XLE['diff'].plot()
```


![img-description](https://i.imgur.com/DLFPXPp.png)

Even though the scale is not right, We can see this stock has **increased** its value.

Thus diversification is the best to invest.

Full notebook at 

https://app.datacamp.com/workspace/w/175aa01c-4626-483a-8d88-70060fed4c14/edit