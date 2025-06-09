import pandas as pd
import numpy as np
import seaborn as sns
import jieba
from wordcloud import WordCloud
from collections import Counter
import re
from datetime import datetime
import matplotlib.pyplot as plt

# 设置 Matplotlib 使用支持中文的字体
plt.rcParams['font.sans-serif'] = ['SimHei']  # 或 'Microsoft YaHei'
plt.rcParams['axes.unicode_minus'] = False  # 用于正常显示负号

df = pd.read_csv('D:/bilibili_comment/my_bilibili_comment.csv')

# 查看数据集的前几行
print(df.head())

# 查看数据集的列名和数据类型
print(df.info())

# 查看数据集的统计信息（数值型列）
print(df.describe())

# 查看各列的唯一值数量
print(df.nunique())

# 查看每列的缺失值数量
print(df.isnull().sum())

# 删除含有缺失值的行（根据实际情况选择是否删除某些列）
df.dropna(subset=['评论内容', '评论时间', '评论者名称', '评论者id',
                  '评论者主页', '评论者性别', '评论者头像', '评论者等级',
                  '点赞数', '回复数', '视频总评论数', '视频链接', '一级评论id',
                  '评论类型'], inplace=True)

# 查看数值型列的异常值情况
sns.boxplot(x=df['点赞数'])
plt.show()

# 删除异常值（以点赞数为例）
Q1 = df['点赞数'].quantile(0.25)
Q3 = df['点赞数'].quantile(0.75)
IQR = Q3 - Q1
df = df[(df['点赞数'] >= Q1 - 1.5 * IQR) & (df['点赞数'] <= Q3 + 1.5 * IQR)]

# 转换评论时间为日期格式
df['评论时间'] = pd.to_datetime(df['评论时间'])

# 提取评论日期
df['评论日期'] = df['评论时间'].dt.date

# 保存预处理后的数据为新的 CSV 文件
df.to_csv('bilibili_comments_cleaned.csv', index=False)


# 简单的情感分析规则
positive_keywords = ['好', '棒', '赞', '喜欢', '爱', '优秀', '厉害']
negative_keywords = ['差', '不好', '糟糕', '讨厌', '差劲', '垃圾']

def analyze_sentiment(comment):
    comment = comment.lower()
    positive_count = sum(keyword in comment for keyword in positive_keywords)
    negative_count = sum(keyword in comment for keyword in negative_keywords)
    if positive_count > negative_count:
        return '积极'
    elif negative_count > positive_count:
        return '消极'
    else:
        return '中性'

# 应用情感分析
df['情感倾向'] = df['评论内容'].apply(analyze_sentiment)

# 情感倾向分布
sentiment_counts = df['情感倾向'].value_counts()

# 绘制情感分布饼图
plt.figure(figsize=(8, 6))
sentiment_counts.plot(kind='pie', autopct='%1.1f%%', startangle=90, colors=['green', 'blue', 'red'])
plt.title('情感倾向分布')
plt.ylabel('')
plt.show()

# 绘制情感分布柱状图
plt.figure(figsize=(8, 6))
sentiment_counts.plot(kind='bar', color=['green', 'blue', 'red'])
plt.title('情感倾向分布')
plt.xlabel('情感倾向')
plt.ylabel('评论数量')
plt.show()

# 按天统计评论数量
daily_comments = df.groupby('评论日期').size()

# 绘制评论时间趋势折线图
plt.figure(figsize=(12, 6))
daily_comments.plot(kind='line')
plt.title('评论时间趋势分析')
plt.xlabel('日期')
plt.ylabel('评论数量')
plt.show()

# 用户评论数量分布
user_comment_counts = df['评论者id'].value_counts()

# 绘制用户评论数量分布柱状图
plt.figure(figsize=(12, 6))
user_comment_counts[:20].plot(kind='bar')
plt.title('用户评论数量分布')
plt.xlabel('用户ID')
plt.ylabel('评论数量')
plt.show()

# 中文分词
def chinese_tokenization(text):
    return " ".join(jieba.cut(text))

# 去除停用词和特殊符号
stopwords = set()
with open('stopwords.txt', 'r', encoding='utf-8') as f:
    for word in f:
        stopwords.add(word.strip())

def remove_stopwords(text):
    words = text.split()
    return " ".join([word for word in words if word not in stopwords and re.match(r'^[a-zA-Z0-9\u4e00-\u9fa5]+$', word)])

# 生成词云
df['分词内容'] = df['评论内容'].apply(chinese_tokenization)
df['分词内容'] = df['分词内容'].apply(remove_stopwords)

all_comments = " ".join(df['分词内容'].tolist())
wordcloud = WordCloud(font_path='simhei.ttf', background_color='white', width=800, height=400).generate(all_comments)

plt.figure(figsize=(10, 6))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis('off')
plt.title('热点话题词云图')
plt.show()

# 评论者性别分布
gender_counts = df['评论者性别'].value_counts()

# 绘制评论者性别分布饼图
plt.figure(figsize=(8, 6))
gender_counts.plot(kind='pie', autopct='%1.1f%%', startangle=90, colors=['blue', 'pink', 'gray'])
plt.title('评论者性别分布')
plt.ylabel('')
plt.show()

# 评论者等级分布
level_counts = df['评论者等级'].value_counts()

# 绘制评论者等级分布柱状图
plt.figure(figsize=(12, 6))
level_counts.plot(kind='bar')
plt.title('评论者等级分布')
plt.xlabel('评论者等级')
plt.ylabel('用户数量')
plt.show()

# 点赞数分布
plt.figure(figsize=(10, 6))
sns.histplot(df['点赞数'], bins=30, kde=True)
plt.title('点赞数分布')
plt.xlabel('点赞数')
plt.ylabel('评论数量')
plt.show()

# 回复数分布
plt.figure(figsize=(10, 6))
sns.histplot(df['回复数'], bins=30, kde=True)
plt.title('回复数分布')
plt.xlabel('回复数')
plt.ylabel('评论数量')
plt.show()

# 视频总评论数分布
plt.figure(figsize=(10, 6))
sns.histplot(df['视频总评论数'], bins=30, kde=True)
plt.title('视频总评论数分布')
plt.xlabel('视频总评论数')
plt.ylabel('视频数量')
plt.show()


































































