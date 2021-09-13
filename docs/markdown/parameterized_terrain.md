> Created on Fri Sep 10 23\36\38 2021 @author: Richie Bao-parameterize_archi_la_design_and_data_analysis 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# 从一个设计场地开始-参数化地形

> 以__风景园林工程与技术__ （西安建筑科技大学建筑学院，杨建辉老师主讲） 课程为依托，阐释参数化技术方法的应用。

基础.dwg图纸调入倒RH中待用。

<img src="./imgs_parae/001.jpg" height="auto" width="auto"  title="digit-x">

## 1. 梳理.dwg格式等高线 
拿到场地之后，首先要梳理获取的.dwg格式文件的等高线，因为基于AutoCAD绘制的等高线往往不规范，存在各种乱线、重叠线、穿插各种线型，为进一步的设计带来阻碍。

### 1.1 基本处理程序编写

* 根据图纸绘制常出现的问题，对应编写解决问题的代码，包括：去除非等高线，焊接连续的断线等，并将其封装()

|  code | results  |
|---|---|
| <img src="./imgs_parae/002.jpg" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/008.jpg" height="auto" width="415"  title="digit-x"> |

> 在设计时，建议保持一版等高线未被构筑切断的版本，用于三维地形的建立。

* 调整等高线高程,建立相对高程，以及显示查看高程值

|  code | results  |
|---|---|
| <img src="./imgs_parae/003.jpg" height="auto" width="auto"  title="digit-x"> <img src="./imgs_parae/005.jpg" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/009.jpg" height="auto" width="auto"  title="digit-x"> |
| <img src="./imgs_parae/004.jpg" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/010.jpg" height="auto" width="auto"  title="digit-x"> |

* 生成三维地形表面

建立三维地形格网+高程赋色与图例

|  code | results  |
|---|---|
| <img src="./imgs_parae/006.jpg" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/011.jpg" height="auto" width="auto"  title="digit-x"> |
| <img src="./imgs_parae/007.jpg" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/012.jpg" height="auto" width="auto"  title="digit-x"> |

### 1.2 根据主要等高线构建地形
在等高线设计的时候，可以实时观察三维地形表面的变化，这有利于地形设计的观察与调整。

|  code | results  |
|---|---|
| <img src="./imgs_parae/015.jpg" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/013.jpg" height="auto" width="400"  title="digit-x">  <img src="./imgs_parae/014.jpg" height="auto" width="400"  title="digit-x">|

### 1.3 调入地理信息高程与坡度数据

<img src="./imgs_parae/018.jpg" height="auto" width="auto"  title="digit-x"> 

|   |   |
|---|---|
| <img src="./imgs_parae/016.jpg" height="auto" width="auto"  title="digit-x">   | <img src="./imgs_parae/017.jpg" height="auto" width="auto"  title="digit-x">   |

### 1.4 地形生成-磁场版

调整组件Populate Geometry输入端Seed参数，可以获取不计其数的地形设计衍生结果

* 地形生成 + 高程重分类的封装

<img src="./imgs_parae/022.jpg" height="auto" width="auto"  title="digit-x"> 

|   |   |   |
|---|---|---|
| <img src="./imgs_parae/019.jpg" height="auto" width="auto"  title="digit-x">   | <img src="./imgs_parae/020.jpg" height="auto" width="auto"  title="digit-x">   | <img src="./imgs_parae/021.jpg" height="auto" width="auto"  title="digit-x">   |

### 1.5 [地形生成-随机森林回归版](https://github.com/richieBao/python-urbanPlanning/tree/master/12_%E5%9C%B0%E5%BD%A2%E7%94%9F%E6%88%90%E4%B8%8E%E5%9B%9E%E5%BD%92%E9%A2%84%E6%B5%8B)


### 1.6 土方计算、平整土地与标注

* 方格网土方计算

|   |   |
|---|---|
| <img src="./imgs_parae/025.jpg" height="auto" width="auto"  title="digit-x">   | <img src="./imgs_parae/027.jpg" height="auto" width="300"  title="digit-x">   |

* 寻找土地平整高程位置

> 需进一步修正

|   |   |   |
|---|---|---|
|  <img src="./imgs_parae/026.jpg" height="auto" width="auto"  title="digit-x"> | <img src="./imgs_parae/023.jpg" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/024.jpg" height="auto" width="auto"  title="digit-x"> |

### 1.7 高程、坡度、坡向、起伏度和重分类

<img src="./imgs_parae/028.jpg" height="auto" width="auto"  title="digit-x">

> 待修改 水文分析/基于NetLogo的水文分析

### 1.8 可视区域分析＋最优视域区域

|   |   |
|---|---|
| <img src="./imgs_parae/031.jpg" height="auto" width="auto"  title="digit-x">  | <img src="./imgs_parae/029.jpg" height="auto" width="300"  title="digit-x">  |
|<img src="./imgs_parae/032.jpg" height="auto" width="auto"  title="digit-x"> |<img src="./imgs_parae/030.jpg" height="auto" width="auto"  title="digit-x"> |

> 寻找最优视域区间时，可以尝试寻找GH中循环计算的方法，或调出数据在PY中计算，或在GHPython中尝试计算。

### 未来潜在探索的方向（敬请关注）
１．等高线（高程点）聚类的方法，归类等高线或高程点；

２．等高线邻近拓步关系的确定；

３.　学习已有经典山水数据（自然，人工）（拓扑关系？），生成可控地形。