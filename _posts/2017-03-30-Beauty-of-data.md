---
layout: post
title: Basic Data Analysis
---

Hi folks! Let's check out how we can plot a simple histogram analyzing the price of Gucci ties available in a reasonably big data sample.
Before we proceed, let's download 'data.csv' from [Akhil's Github Repo](https://github.com/akhil-sreehari/TieAnalysis)

Let's fire up the jupyter notebook or pycharm or any editor you like.

{% highlight js %}
//We will be the using the following 3 imports

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

{% endhighlight %}

Okay. This seems fairly straightforward! We've imported the essentials. What to do next?

{% highlight js %}

// Creating a dataframe raw_data from the csv file available to us.
raw_data = pd.read_csv('data.csv', encoding='utf-8',skiprows=1, delimiter='\t', names=['id', 'price', 'name',
                                                                           'brand', 'brand_name',
                                                                           'image', 'desc', 'vendor',
                                                                           'striped', 'material'])
//Just to get a feel of things
type(raw_data)
raw_data.iloc[1:2]

//Finding the total price of all the ties available to us (without using innate df method - by iterating the df)
data_list = []
for i, v in raw_data.iterrows():
    data_list.append(v)
data_list

prices = []
for data in data_list:
    prices.append(data[1])
prices_total = float( '%.2f' % sum(prices))

//Let's fing the average, min and max now!
float('%.2f' % (prices_total/ len(prices)))
min(prices)
max(prices)

//Adding a new field bool_field with a condition, i.e, brand > 1000
raw_data.loc[raw_data.brand > 1000, 'bool_field'] = False
raw_data.iloc[:,2:]

//Fill all False values with the string unknown
raw_data = raw_data.replace(False, 'unknown')

//Find DF with Gucci brand_name and average
len(raw_data.loc[raw_data.brand_name.str.contains('Gucci')])
gucci_dataframe = raw_data.loc[raw_data.brand_name.str.contains('Gucci')]

gucci_sum = 0
for k, v in gucci_dataframe[['price']].iterrows():
    gucci_sum += v
gucci_sum/ len(gucci_dataframe[['price']])

//Export result to a csv file
gucci_dataframe.to_csv('exported_from_gucci_dataframe.csv', sep='\t', encoding='utf-8')

//Export result to an excel file
gucci_dataframe.to_excel('exported_from_gucci_dataframe.xls')

//Plotting a line chart for a better idea about the gucci tie prices
plt.plot(gucci_dataframe['price'].tolist())
plt.xlabel('Price level')
plt.ylabel('Number of ties')
plt.show()

//Plotting a bar chart
plt.hist(gucci_dataframe['price'].tolist())
plt.xlabel('Price level')
plt.ylabel('Number of ties')
plt.show()                                                             
{% endhighlight %}


Thanks!
