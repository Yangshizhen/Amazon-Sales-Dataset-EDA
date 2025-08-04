# Amazon-Sales-Dataset-EDA
Analyze Amazon sales data to identify key factors affecting revenue and propose optimization recommendations.

# 导入库

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import scipy as sp

%matplotlib inline

# 加载数据
df = pd.read_excel(r"amazon.xlsx")

# 读取数据
## 前五行
df.head()
## 读取列名
df.columns
## 统计行数和列数
print(f"The Number Of Rows are {df.shape[0]}, and Columns are {df.shape[1]}.")

# 读取数据的类型
df.info()

## 修改数据类型
df['discounted_price']
df['discounted_price'] = df['discounted_price'].str.replace('₹','')
df['discounted_price'] = df['discounted_price'].str.replace(',','')
df['discounted_price'] = df['discounted_price'].astype('float64')
df['discounted_price']

df['autual_price']
df['autual_price'] = df['autual_price'].str.replace('₹','')
df['autual_price'] = df['autual_price'].str.replace(',','')
df['autual_price'] = df['autual_price'].astype('float64')
df['autual_price']

df['discount_percentage']

df['rating']

df.info()

# 处理异常值

## 统计异常值的数量
df['rating'].value_counts()
df['rating_count'].value_counts()

## 读取异常值所在的行
df.query('rating == "|" ')

## 处理异常值数据
df['rating'] = df['rating'].str.replace('|', '3.9').astype('float64')
df['rating']

### 解题思路： 
# 1 导入需要使用的库 
# 2 加载数据 
#    2.1 读取数据 
#    2.2 查看数据的列名 
#    2.3 打印数据的行数和列数 
# 3 查看数据类型 
#    3.1 使用空格替换数据中多余的符号 
#    3.2 更改为正确的数据类型 
# 4 处理异常值 
#    4.1 读取异常值数量 
#    4.2 补足异常值数据 5 再次查看数据类型是否更改正确


# 处理缺失值
df.describe()

# 统计缺失值数量
df.isnull().sum().sort_values(ascending=False)

# 统计每一列缺失值数量占总体数量的比重
round(df.isnull().sum() / len(df) * 100, 2).sort_values(ascending=False)

# 统计总的缺失值数量
df.isnull().sum().sum()

# 绘图
## 所有绘图操作均需在 figure 这个图形对象上进行
plt.figure(figsize=(22,10))

# 绘制缺失值热力图
sns.heatmap(df.isnull(),yticklabels=False,cbar=False,cmap='viridis')
plt.show()

# 绘制缺失值条形图
plt.figure(figsize=(22,10))
missing_percentage = df.isnull().sum() / len(df) * 100
missing_percentage.plt(kind='bar')

plt.xlabel('Columns')
plt.ylabel('Percentage')
plt.title('Percentage of Missing Values in each Column')
plt.show()

# 读取缺失值所在的行
df[df['rating_count'].isnull()]

# 利用中位数处理缺失值
df['rating_count'] = df['rating_count'].fillna(value=df['rating_count'].median())
df['rating_count'][282]

# 检测缺失值是否处理完成
df.isnull().sum().sort_values(ascending=False)



