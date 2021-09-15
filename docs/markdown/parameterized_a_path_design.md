> Created on Mon Sep 13 10\04\07 2021 @author: Richie Bao-parameterize_archi_la_design_and_data_analysis 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# 尺度+一条路径的设计+结构与构造

> 以__风景园林工程与技术__ （西安建筑科技大学建筑学院，杨建辉老师主讲） 课程为依托，阐释参数化技术方法的应用。

## 1. 为人设计，因此切记先+人~ 人~，人模型！！！，建立尺度感...

<img src="./imgs_parae/033.jpg" height="auto" width="auto"  title="digit-x">

|   |   |
|---|---|
| <img src="./imgs_parae/035.jpg" height="auto" width="auto"  title="digit-x">  | <img src="./imgs_parae/034.jpg" height="auto" width="400"  title="digit-x">  <img src="./imgs_parae/036.jpg" height="auto" width="400"  title="digit-x">|

## 2. 入口的桥设计与标注
需要确定桥与地形高程之间的关系，以及高差问题。

### 2.1 第1版3维草图

| 草图  |   |
|---|---|
|  <img src="./imgs_parae/046.jpg" height="auto" width="400"  title="digit-x"> | <img src="./imgs_parae/047.jpg" height="auto" width="400"  title="digit-x">  |

|   |   |   |
|---|---|---|
|  <img src="./imgs_parae/037.jpg" height="auto" width="400"  title="digit-x"> |  <img src="./imgs_parae/038.jpg" height="auto" width="400"  title="digit-x"> | <img src="./imgs_parae/039.jpg" height="auto" width="400"  title="digit-x">  |
|  <img src="./imgs_parae/040.jpg" height="auto" width="400"  title="digit-x"> |  <img src="./imgs_parae/048.jpg" height="auto" width="400"  title="digit-x"> |  <img src="./imgs_parae/049.jpg" height="auto" width="400"  title="digit-x"> |

<img src="./imgs_parae/050.jpg" height="auto" width="auto"  title="digit-x"> 

<img src="./imgs_parae/044.jpg" height="auto" width="auto"  title="digit-x"> 

GH做设计时，第一版的代码总是会很凌乱，更多的心思在设计上，而代码则是尽量快速的实现想法。

### 2.2 设计调整与代码梳理(第2版草图)

根据情况，要阶段性的梳理代码，增加注释，避免之后遗忘，而不得不花费更多的时间去弄明白自己之前写代码的逻辑。对于常用到的模块要cluster打包（类似构建函数），从而让代码更条理清晰，以及方便日后调用。

<img src="./imgs_parae/051.jpg" height="auto" width="auto"  title="digit-x"> 

这一阶段，也要适当的修改设计，例如结构设计的尺度是否合理，是否需要重新是设计某一部分提升设计合理性等。参数化在一开始时，虽然需要花费些时间写代码实现设计，但是在后续的调整过程中，可以通过调整参数实时观察尺度，尺寸变化，即如果设计逻辑不变，可以任意调整参数变化设计对象。

| 第2版草图  |   |
|---|---|
|  <img src="./imgs_parae/052.jpg" height="auto" width="400"  title="digit-x"> | <img src="./imgs_parae/053.jpg" height="auto" width="400"  title="digit-x">  |

* 爆炸图

<img src="./imgs_parae/054.jpg" height="auto" width="auto"  title="digit-x"> 

* 三维标注示例

<img src="./imgs_parae/056.jpg" height="auto" width="auto"  title="digit-x"> 

<img src="./imgs_parae/055.jpg" height="auto" width="auto"  title="digit-x"> 

**设计阶段结构受力分析，反推设计-待**

**基础-待**

## 3. 道路+看台+台阶

### 3.1 设计路径结构线对应标高变化

查看路径结构线的高程变化，进而确定进一步设计的合理性。

|  |   |
|---|---|
|  <img src="./imgs_parae/058.jpg" height="auto" width="400"  title="digit-x"> | <img src="./imgs_parae/057.jpg" height="auto" width="auto"  title="digit-x">  |

### 3.2 道路三维构造表达试探

> 标准图集转参数化模型，可自由调整尺寸，2或3维的试探，+标注的想法，一直在犹豫是否用力过猛，在未来进一步探索时会再考量。

|   |   |   |
|---|---|---|
| <img src="./imgs_parae/061.jpg" height="auto" width="400"  title="digit-x">  |  <img src="./imgs_parae/059.jpg" height="auto" width="400"  title="digit-x"> |  <img src="./imgs_parae/060.jpg" height="auto" width="400"  title="digit-x"> |

### 3.3 