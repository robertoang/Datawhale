pyplot API	OO API	description
text 1	text	在 Axes 1的任意位置添加text。
title	set_title 1	在 Axes 1添加title
figtext	text	在Figure的任意位置添加text.
suptitle	suptitle	在 Figure添加title
xlabel	set_xlabel	在Axes 1的x-axis添加label
ylabel	set_ylabel	在Axes 1的y-axis添加label
annotate	annotate	向Axes 1的任意位置添加带有可选箭头的标注.

属性值	含义
‘figure points’	以绘图区左下角为参考，单位是点数
‘figure pixels’	以绘图区左下角为参考，单位是像素数
‘figure fraction’	以绘图区左下角为参考，单位是百分比
‘axes points’	以子绘图区左下角为参考，单位是点数（一个figure可以有多个axes，默认为1个）
‘axes pixels’	以子绘图区左下角为参考，单位是像素数
‘axes fraction’	以子绘图区左下角为参考，单位是百分比
‘data’	以被注释的坐标点xy为参考 (默认值)
‘polar’	不使用本地数据坐标系，使用极坐标系


legend entry（图例条目）
图例有一个或多个legend entries组成。一个entry由一个key和一个label组成。

legend key（图例键）
每个 legend label左面的colored/patterned marker（彩色/图案标记）

legend label（图例标签）
描述由key来表示的handle的文本

legend handle（图例句柄）
用于在图例中生成适当图例条目的原始对象

#这个案例是显示多图例legend
import matplotlib.pyplot as plt
import numpy as np
x = np.random.uniform(-1, 1, 4)
y = np.random.uniform(-1, 1, 4)
p1, = plt.plot([1,2,3])
p2, = plt.plot([3,2,1])
l1 = plt.legend([p2, p1], ["line 2", "line 1"], loc='upper left')
 
p3 = plt.scatter(x[0:2], y[0:2], marker = 'D', color='r')
p4 = plt.scatter(x[2:], y[2:], marker = 'D', color='g')
# 下面这行代码由于添加了新的legend，所以会将l1从legend中给移除
plt.legend([p3, p4], ['label', 'label1'], loc='lower right', scatterpoints=1)
# 为了保留之前的l1这个legend，所以必须要通过plt.gca()获得当前的axes，然后将l1作为单独的artist
plt.gca().add_artist(l1)
