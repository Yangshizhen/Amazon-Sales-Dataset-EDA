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

# 检查数据类型
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
### 1 导入需要使用的库 
### 2 加载数据 
###    2.1 读取数据 
###    2.2 查看数据的列名 
###    2.3 打印数据的行数和列数 
### 3 查看数据类型 
###    3.1 使用空格替换数据中多余的符号
###    3.2 更改为正确的数据类型
###    3.3 检查数据类型
### 4 处理异常值
###    4.1 读取异常值数量 
###    4.2 处理异常值数据 
### 5 查看异常值数据是否更改完成


# 处理缺失值
df.describe()

## 统计缺失值数量
df.isnull().sum().sort_values(ascending=False)

## 统计每一列缺失值数量占总体数量的比重
round(df.isnull().sum() / len(df) * 100, 2).sort_values(ascending=False)

## 统计总的缺失值数量
df.isnull().sum().sum()

## 绘图
## 所有绘图操作均需在 figure 这个图形对象上进行
plt.figure(figsize=(22,10))

## 绘制缺失值热力图
sns.heatmap(df.isnull(),yticklabels=False,cbar=False,cmap='viridis')
plt.show()

## 绘制缺失值条形图
plt.figure(figsize=(22,10))
missing_percentage = df.isnull().sum() / len(df) * 100
missing_percentage.plt(kind='bar')

plt.xlabel('Columns')
plt.ylabel('Percentage')
plt.title('Percentage of Missing Values in each Column')
plt.show()

## 读取缺失值所在的行
df[df['rating_count'].isnull()]

## 利用中位数处理缺失值
df['rating_count'] = df['rating_count'].fillna(value=df['rating_count'].median())
df['rating_count'][282]

## 检测缺失值是否处理完成
df.isnull().sum().sort_values(ascending=False)

### 解题思路：
### 1 查看数据的分布情况
### 2 处理缺失值
###    2.1 统计每一列缺失值的数量
###    2.2 计算每一列缺失值数量占总体数量的比重
###    2.3 统计数据的总缺失值
###    2.4 绘制热力图、条形图展示缺失值的基本分布
###    2.5 利用中位数、标准方差等方法补充缺失值
### 3 再次检查缺失值是否已处理

# 处理重复值
df.duplicated()

## 统计重复值数量
df.duplicated().any()

# 数据可视化

## 散点图
plt.scatter(df['autual_price'],df['rating'])
plt.xlabel("Autual Price")
plt.ylabel("Rating")
plt.show()

## 直方图
plt.hist(df['autual_price'])
plt.xlabel("Autual Price")
plt.ylabel("Frequency")
plt.show()

## 绘制热力图
## 对文件中的数据全部转换为数值
from sklearn.preprocessing import LabelEncoder
le_product_id = LabelEncoder()
le_category = LabelEncoder()
le_review_id = LabelEncoder()
le_review_content = LabelEncoder()
le_product_name = LabelEncoder()
le_user_name = LabelEncoder()
le_about_product = LabelEncoder()
le_user_id = LabelEncoder()
le_review_title = LabelEncoder()
le_img_link = LabelEncoder()
le_product_link = LabelEncoder()

df['product_id'] = le_product_id.fit_transform(df['product_id'])
df['category'] = le_category.fit_transform(df['category'])
df['review_id'] = le_review_id.fit_transform(df['review_id'])
df['review_content'] = le_review_content.fit_transform(df['review_content'])
df['product_name'] = le_product_name.fit_transform(df['product_name'])
df['user_name'] = le_user_name.fit_transform(df['user_name'])
df['about_product'] = le_about_product.fit_transform(df['about_product'])
df['user_id'] = le_user_id.fit_transform(df['user_id'])
df['review_title'] = le_review_title.fit_transform(df['review_title'])
df['img_link'] = le_img_link.fit_transform(df['img_link'])
df['product_link'] = le_product_link.fit_transform(df['product_link'])

## 热力图heatmap
correlation_matrix = df.corr()
sns.heatmap(correlation_matrix,annot=True)
plt.show()

# 相关分析

## 皮尔逊相关系数
pearson_correlation_matrix = df.corr()
print(pearson_correlation_matrix)

sns.heatmap(
  pearson_correlation_matrix,
  annot=True,
  cmap="coolwarm"
)
plt.title("Correlation Matrix (Pearson)")
plt.show()

## 斯皮尔曼相关系数
spearman_correlation_matrix = df.corr(method="spearman")
print(spearman_correlation_matrix)

sns.heatmap(spearman_correlation_matrix,annot=True,cmap="coolwarm")
plt.title("Correlation Matrix (Spearman)")
plt.show()

correlation_coefficient = np.corrcoef(df['autual_price'],df['rating'])[0,1]
print(correlation_coefficient)

# 分组和聚合
grouped_df = df.groupby('category')['rating'].mean()
print(grouped_df)

mean_sales_by_category = df.groupby('category')['rating'].mean()
print(mean_sales_by_category)

median_sales_by_age = df.groupby('review_content')['rating'].median()
print(median_sales_by_age)

std_price_by_brand = df.groupby('product_name')['rating'].std()
print(std_price_by_brand)

