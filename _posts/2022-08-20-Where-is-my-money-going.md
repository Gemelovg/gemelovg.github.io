---
title: Where is my money going.
date: 2022-08-20
catergories: [python, pandas, matplotlib, excel, plot]
tags: [python, pandas, matplotlib, excel, plot]
---



# Where is my money going.
I think I have been sending a lot of my money on Amazon but I am not sure, let me take a look.




## Going to start by importing the libraries im going to use.

```python
import pandas as pd
import matplotlib.pyplot as plt
```


## Read File and rename Colums to something easier to read.

```python
bank = pd.read_excel('excel.xlsx')
colums=['Date', 'Description', 'Deposits','Price']
bank.columns=colums
bank.head()
```
It looks somethig like this

```python
Date	Description	Deposits	Price
0	27 Jul 2022|7	SORIANA 688 URBANIA	NaN	$ 77.90
1	27 Jul 2022|7	GAS EST SERV P UNIV 2	NaN	$ 613.70
2	27 Jul 2022|7	PAYPAL ETNTURISTAR	NaN	$ 1,560.00
3	25 Jul 2022|7	HOME DEPOT CORDILLERAS	NaN	$ 431.91
4	25 Jul 2022|7	PP*FASTSPRING	NaN	$ 223.77
```

## Lets remove the Deposits columns so people doesnt know much I make and We are not going to use it.

```python
bank = bank[['Date', 'Description','Price']]
bank.head()
```
Looking good, but something seems off...
```python
	Date	Description	Price
0	27 Jul 2022|7	SORIANA 688 URBANIA	$ 77.90
1	27 Jul 2022|7	GAS EST SERV P UNIV 2	$ 613.70
2	27 Jul 2022|7	PAYPAL ETNTURISTAR	$ 1,560.00
3	25 Jul 2022|7	HOME DEPOT CORDILLERAS	$ 431.91
4	25 Jul 2022|7	PP*FASTSPRING	$ 223.77
```

## Lets sum the whole colum and see how much I am spending...

```python
suma = bank['Price'].sum()
```

    >Output exceeds the size limit. Open the full output data in a text editor
    ---------------------------------------------------------------------------
    >TypeError                                 Traceback (most recent call last)
    c:\Users\gemel\OneDrive\Documentos\bank\bank.ipynb Cell 7 in <cell line: 1>()
    ----> 1 suma = bank['Price'].sum()
    ...
        46 def _sum(a, axis=None, dtype=None, out=None, keepdims=False,
        47          initial=_NoValue, where=True):
    ---> 48     return umr_sum(a, axis, dtype, out, keepdims, initial, where)

    >TypeError: can only concatenate str (not "int") to str


Oh no, the column is not numeric, lets fix that.

Numbers looks like this:  $ 1,560.00

Lets remove the dollar sign, and the commas.


```python
bank['Price'] = bank['Price'].str.replace('[$,]','').astype(float)
bank['Price']
```

```python
0       77.90
1      613.70
2     1560.00
3      431.91
4      223.77
5      682.76
6     1588.00
7      170.00
8      182.10
9      242.88
10      11.05
11        NaN
12     163.00
13     323.50
14     269.99
```

## We also have some empty spaces from the deposits column. Thets get rid of them.

```python
bank.dropna(inplace=True)
bank
```
```python
Date	Description	Price
0	27 Jul 2022|7	SORIANA 688 URBANIA	77.90
1	27 Jul 2022|7	GAS EST SERV P UNIV 2	613.70
2	27 Jul 2022|7	PAYPAL ETNTURISTAR	1560.00
3	25 Jul 2022|7	HOME DEPOT CORDILLERAS	431.91
4	25 Jul 2022|7	PP*FASTSPRING	223.77
5	24 Jul 2022|7	UBER* EATS	682.76
6	23 Jul 2022|7	HOME DEPOT	1588.00
7	22 Jul 2022|7	Domains	170.00
8	22 Jul 2022|7	SORIANA 688 URBANIA	182.10
9	21 Jul 2022|7	UBER* EATS	242.88
10	19 Jul 2022|7	PAYPAL *KINGUINDIGI	11.05
12	17 Jul 2022|7	ACNT*CINESONLINE	163.00
13	17 Jul 2022|7	WAL MART VALLARTA	323.50
14	17 Jul 2022|7	PAYPAL STEAM GAMES	269.99
16	16 Jul 2022|7	AMAZON MX MSI	1366.98
17	16 Jul 2022|7	AMAZON MX MSI	370.45
18	16 Jul 2022|7	**/TICKETMASTER MEXIC	1322.76
19	16 Jul 2022|7	SAMS VENTA EN LINEA	970.86
20	16 Jul 2022|7	SAMS VENTA EN LINEA	255.67
21	16 Jul 2022|7	PAYPAL CHEILMEXICO	1031.21
22	16 Jul 2022|7	**/TICKETMASTER	339.67
23	16 Jul 2022|7	AMAZON MX MKTPLACE MSI	550.42
24	16 Jul 2022|7	MERCADO PAGO 1	341.11
25	16 Jul 2022|7	AMAZON MX MKTPLACE MSI	470.42
26	16 Jul 2022|7	MERCADO PAGO 4	250.98
27	16 Jul 2022|7	REST RANCHERO BARBACOA	92.50
28	16 Jul 2022|7	UBER* EATS	244.65
33	14 Jul 2022|7	AMAZON MX	1159.00
```
No NaN columns, nice.


## Oh no an other problem. The Date column has a pipe and a number 7 at the end.

Lets remove them and convert it to datetime

```python
bank['Date'] = bank.apply(lambda x: x['Date'][:-2], axis=1)
bank['Date'] = pd.to_datetime(bank['Date'])
bank['Date'].head()
```

```
0   2022-07-27
1   2022-07-27
2   2022-07-27
3   2022-07-25
4   2022-07-25
```

## Lets use the Description column to count the number of different purchases I have done.

```python
bank.Description.value_counts()
```

```
UBER* EATS                                   3
SORIANA 688 URBANIA                          2
AMAZON MX MSI                                2
AMAZON MX MKTPLACE MSI                       2
SAMS VENTA EN LINEA                          2
REST RANCHERO BARBACOA                       1
MERCADO PAGO 4                               1
MERCADO PAGO 1                               1
**/TICKETMASTER                              1
PAYPAL CHEILMEXICO                           1
**/TICKETMASTER MEXIC                        1
PAYPAL STEAM GAMES                           1
GAS EST SERV P UNIV 2                        1
WAL MART VALLARTA                            1
ACNT*CINESONLINE                             1
PAYPAL *KINGUINDIGI                          1
Domains                                      1
HOME DEPOT                                   1
PP*FASTSPRING                                1
HOME DEPOT CORDILLERAS                       1
PAYPAL ETNTURISTAR                           1
AMAZON MX                                    1
Name: Description, dtype: int64
```

Wow. Even though we know some expenses are form the same store, since they are have exactly the same name, the dataframe count them as different. Let use just the first word as the name of the Store.


## Lets create a new column name Store and get the first word from the string.

```python
bank['Store'] = bank.Description.str.split().str.get(0)
bank['Store']
```
## Lets get the count again

```python
bank.Store.value_counts()
```

```
AMAZON              5
PAYPAL              4
UBER*               3
SORIANA             2
HOME                2
**/TICKETMASTER     2
SAMS                2
MERCADO             2
GAS                 1
PP*FASTSPRING       1
Domains             1
ACNT*CINESONLINE    1
WAL                 1
REST                1
Name: Store, dtype: int64
```

A lot of amazon and paypal, lets see the total amounts


```python
total = bank.groupby('Store').sum().sort_values('Price', ascending=False).reset_index()
total
```
```
Store	Price
0	AMAZON	3917.27
1	PAYPAL	2872.25
2	HOME	2019.91
3	**/TICKETMASTER	1662.43
4	SAMS	1226.53
5	UBER*	1170.29
6	GAS	613.70
7	MERCADO	592.09
8	WAL	323.50
9	SORIANA	260.00
10	PP*FASTSPRING	223.77
11	Domains	170.00
12	ACNT*CINESONLINE	163.00
13	REST	92.50
```
That seems to be a lot. 

## Lets plot to take a better look.


```python
plt.pie(total.Price, labels=total.Store, autopct='%.0f%%', shadow=True, rotatelabels='true')
plt.show
```

## Almost half of the charges are from Amazon or Paypal(probably ordering to go).


![img-description](https://i.imgur.com/pemjGqI_d.webp?maxwidth=760&fidelity=grand)_yeah_


# Now We know what I should spend less time doing!


Full notebook at 

https://app.datacamp.com/workspace/w/175aa01c-4626-483a-8d88-70060fed4c14/edit