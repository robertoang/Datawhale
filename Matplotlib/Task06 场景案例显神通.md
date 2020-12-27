1.用Drugs数据集，做出面积图的多子图形式。
注意，需要添加如下要素：
①添加每个子图标题，在子图右上方；
②添加整个画布的总标题，在画布左上方；
③添加X和Y轴的标签。

# 导入包
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

# 导入数据集并转成方便作图的格式
Dataset = pd.read_csv('C:/Users/13636/Desktop/数据可视化/fantastic-matplotlib-main/data/Drugs.csv')
group = Dataset.groupby(['YYYY','State']).agg('sum').reset_index()
df = group.pivot(index='YYYY', columns='State', values='DrugReports').reset_index()

# 初始化画布的设定
plt.style.use('seaborn-whitegrid') # 风格
palette = plt.get_cmap('Set1') # 颜色卡
plt.figure(figsize=(15, 10)) # 画布大小

# 绘制
num=0
for column in df.drop('YYYY', axis=1):
    num+=1
 
    # 设定子图在画布的位置
    plt.subplot(3,3, num)
 
    # 画线图    
    plt.fill_between( df['YYYY'], df[column], facecolor=palette(num), alpha=0.7)
    plt.plot(df['YYYY'], df[column], color=palette(num), alpha=0.6, linewidth=1.5)
    
 
    # 设定子图的X轴和Y轴的范围，注意，这里所有的子图都是用同一套X轴和Y轴
    plt.xlim(2009.3,2017.3)
    plt.ylim(0,50000)
 
    # 添加每个子图的标题
    plt.title(column, loc='right', fontsize=12, fontweight=0, color=palette(num) )

# 添加整个画布的标题
#plt.suptitle("How many DrugReports the 5 states have in past few years?", fontsize=13, fontweight=0, color='black', style='italic'，y=0.95)
plt.suptitle("How many DrugReports the 5 states have in past few years?", fontsize=13, fontweight=0, color='black', style='italic',x=0.28, y=0.95)
# 添加整个画布的横纵坐标的名称
plt.text(2014, -9500, 'Year', ha='center', va='center')
plt.text(1998, 60000, 'DrugReports', ha='center', va='center', rotation='vertic
