> Created on Wed Sep 22 00\40\00 2021 @author: Richie Bao-parameterize_archi_la_design_and_data_analysis 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# 植被

## 1. 辅助种植制图

**单株种植+列值+片值+片值(疏)+灌木, 三维树,种植标注**

<img src="./imgs_parae/252.jpg" height="auto" width="auto"  title="digit-x">

<img src="./imgs_parae/249.jpg" height="auto" width="auto"  title="digit-x">

<img src="./imgs_parae/250.jpg" height="auto" width="auto"  title="digit-x">

<img src="./imgs_parae/251.jpg" height="auto" width="500"  title="digit-x">

> 参数化绘制植被方法仍需改进

## 2. Cellular Automata（CA）下植被的扩散+DBSCAN聚类

<img src="./imgs_parae/253.gif" height="auto" width="500"  title="digit-x">
<img src="./imgs_parae/254.jpg" height="auto" width="500"  title="digit-x">

<img src="./imgs_parae/255.jpg" height="auto" width="auto"  title="digit-x">

* GH 调整数据写入.csv文件

```python
__author__ = "richiebao"
__version__ = "2021.09.23"

import rhinoscriptsyntax as rs
formated_pts_string=[str(rs.PointCoordinates(pt)) for pt in pts]
```

* 应用`scikit-learn`库下的`DBSCAN`聚类，并写入文件供GH读取

```python
# -*- coding: utf-8 -*-
"""
Created on Wed Sep 22 23:16:29 2021

@author: richiebao
"""

import pandas as pd
from sklearn.cluster import DBSCAN
import matplotlib.pyplot as plt
import numpy as np

CA_vegetation_fp=r'../data/CA_vegetation_pts.csv'
CA_vegetation=pd.read_csv(CA_vegetation_fp,sep=",",header=None) 
# print(CA_vegetation)
X=CA_vegetation.to_numpy()
print(X.shape)
db=DBSCAN(eps=0.5, min_samples=10).fit(X)
labels=db.labels_
print(labels)
print(np.unique(labels))

fig, ax=plt.subplots()
ax.scatter(X[:,0],X[:,1],s=5,marker='o',c=labels)
plt.show()

label_df=pd.DataFrame(labels,columns=["cluster_label"])
print(label_df)
label_df.to_csv('../data/CA_vegetation_pts_cluster.csv', index=False)


if __name__ == "__main__":
    pass
```

```python
# unique values from a list
import rhinoscriptsyntax as rs

def unique(lst): 
    # initialize a null list
    unique_list=[]     
    # traverse for all elements
    for x in lst:
        # check if exists in unique_list or not
        if x not in unique_list:
            unique_list.append(x)
    return unique_list
            
unique_values=unique(lst)      
```

## 3. 植物群落-浅尝

|   |   |
|---|---|
|  <img src="./imgs_parae/256.jpg" height="auto" width="auto"  title="digit-x"> |   <img src="./imgs_parae/257.jpg" height="auto" width="auto"  title="digit-x"> |

## 4. 搬运出的植物群落斑块

<img src="./imgs_parae/262.jpg" height="auto" width="auto"  title="digit-x"> 

<img src="./imgs_parae/261.jpg" height="auto" width="auto"  title="digit-x"> 

```netlogo
globals [patch-color file]
breed [sample_ts sample_t]
breed [bugs bug]

to import-images
  set file user-file
  import-pcolors file
end

to sample-color

  if mouse-down? [ask sample_ts [die]
  ask patch mouse-xcor mouse-ycor[set patch-color pcolor sprout-sample_ts 1 [pen-down set pen-size 10]]]
end

to reset
  ct cp import-pcolors file
end

to setup
  ct
  set-default-shape bugs "bug"
  ask patches [if (random-float 100 < density and pcolor = patch-color)[set pcolor yellow]]
  create-bugs number [set color white setxy random-xcor random-ycor set size 5.5]
  ask bugs-on patches with [pcolor = black ] [die]
end

to go
  ask patches with [pcolor = red] [set pcolor patch-color]
  search-for-chip
  find-new-pile
  put-down-chip
  ask bugs-on patches with [pcolor = black ] [set color white move-to one-of patches with [pcolor = patch-color] ]
end

to search-for-chip
  ifelse pcolor = yellow [set pcolor patch-color set color orange
     ifelse ([pcolor] of patch-ahead 20) != black [fd 15] [rt random 180 fd 15]] [wiggle search-for-chip]
end

to find-new-pile
  if pcolor != yellow [wiggle find-new-pile]
end

to put-down-chip
  ifelse pcolor = patch-color
  [set pcolor yellow  set color white get-away] [rt random 360 fd 1 put-down-chip]
end

to get-away
  ifelse [pcolor] of patch-ahead 20 != black
  [rt random 360
  fd 15

  if pcolor != patch-color [get-away]]
  [rt random 180 fd 15 if pcolor != patch-color [get-away]]
end


to wiggle
  ifelse [pcolor] of patch-ahead 1 != black
  [fd 1 rt random 50 lt random 50]
  [rt random 180 fd 1]

end

to boundary
  ask patches with [pcolor = patch-color]  with [any? different-colored-neighbours yellow] [set pcolor red]
end

to-report different-colored-neighbours [colorindex]
  report neighbors with [pcolor != [pcolor] of myself and pcolor = colorindex]
end

to save-data
  let save-file user-new-file
  if is-string? save-file [if file-exists? save-file [file-delete save-file] file-open save-file write-to-file]
  file-close
end

to write-to-file
  foreach sort patches [ ?1 -> ask ?1 [file-print (word pxcor" "pycor " " pcolor)] ]
end
```

<img src="./imgs_parae/260.jpg" height="auto" width="auto"  title="digit-x"> 

|   |   |
|---|---|
| <img src="./imgs_parae/258.jpg" height="auto" width="auto"  title="digit-x">   |  <img src="./imgs_parae/259.jpg" height="auto" width="auto"  title="digit-x">  |

<img src="./imgs_parae/263.jpg" height="auto" width="auto"  title="digit-x"> 

### 未来潜在探索的方向（敬请关注）
１. 深入植物群落参数化方法的研究；

２. GH下实现multi-agent多智能体（多代理）的方法；

３. 建立植物群落数据库，方便植被规划调用。