---
layout:post
title: "Naive Bayes From Scratch"
description: "Coding the Naive Bayes Classifier in Python"
mathjax: true
---

# Coding the Naive Bayes Classifier From Scratch

This post will walk through the basics of the Naive Bayes Classifier as well as show a python implementation of coding it from the ground up. While Naive Bayes is a fairly simple and straightforward algorithm, it has a number of real world use cases, including the canonical spam detection as well as sentiment analysis and weather detection. This post will walk through an example using [UCI's Banknote Authentication Dataset](https://archive.ics.uci.edu/ml/datasets/banknote+authentication "UCI ML Repo").

## Banknote Dataset

First, let's get started with the basics and __read in the dataset:__


```python
import pandas as pd
import numpy as np
url = url = 'http://archive.ics.uci.edu/ml/machine-learning-databases/00267/data_banknote_authentication.txt'
df = pd.read_csv(url, header=None)
df.columns = ['imgVariance','imgSkewness','imgCurtosis','imgEntropy','Class']
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>imgVariance</th>
      <th>imgSkewness</th>
      <th>imgCurtosis</th>
      <th>imgEntropy</th>
      <th>Class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3.62160</td>
      <td>8.6661</td>
      <td>-2.8073</td>
      <td>-0.44699</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.54590</td>
      <td>8.1674</td>
      <td>-2.4586</td>
      <td>-1.46210</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.86600</td>
      <td>-2.6383</td>
      <td>1.9242</td>
      <td>0.10645</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3.45660</td>
      <td>9.5228</td>
      <td>-4.0112</td>
      <td>-3.59440</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.32924</td>
      <td>-4.4552</td>
      <td>4.5718</td>
      <td>-0.98880</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



The dataset consists of 1372 observations. There are four features which describe images of genuine and forged banknotes, as well as as label indicating whether or not the note is genuine. Prior to diving into the classifier, let's __split our data into training and test sets:__


```python
msk = np.random.rand(len(df)) < 0.8
train_df = df[msk]
test_df = df[~msk]
```

## Why is it Naive? Why is it Bayes(ian)?

The Naive Bayes Classifier is a supervised learning algorithm so given a set of datapoints {$${x^1,...x^m}$$} our goal is to predict the correct {$${y^1,...,y^m}$$}. However, unlike discriminative classifier such as logistic regressions or decision trees which directly estimate \\(P(Y \mid X)\\)$ and create a decision boundaries to make predictions, the Naive Bayes Classifier is a __generative classifier__. It uses $$P(X\mid Y)$$ to then estimate $$P(Y \mid X)$$. And here is where good old Bayes Theorem helps you out.

$$\displaystyle P(X\mid Y)={\frac {P(Y\mid X)P(X)}{P(Y)}}$$
