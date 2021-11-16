> Created on Sat Sep 25 17:07:45 2021 @author: Richie Bao-python_code_archi_la_design_method_study 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# code数学学公式+机器学习-回归+地形生成+代码图解与逆向工程 

## 1. 地形生成<基于随机森林回归模型|结合参数化(grasshopper)

<a href=""><img src="./imgs_pyd/030.png" height="auto" width="auto" title="caDesign"></a>

### 1.1 GH中的控制结构线（ACO）+训练数据的输出+读取预测高程结果显示生成的地表

<a href=""><img src="./imgs_pyd/032.jpg" height="auto" width="auto" title="caDesign"></a>

---

**ACO**

> 对于蚁群算法python实现研习的原始程序来自于ChinaUnix Rockins'Blog， 也可以参考GitHub trevlovett给出的程序，可以在GitHub中直接搜索ant colony查找获取。

```python
import rhinoscriptsyntax as rs
import os
import sys
import sets
import random
import string
from string import *
from math import *
BestTour = []                   
CitySet = sets.Set()            
CityList = []                   
PheromoneTrailList = []         
PheromoneDeltaTrailList = []    
CityDistanceList = []           
AntList = []                    
BTC=[]
    
class BACA:
    def __init__(self, cityCount=14, antCount=10, q=80,
                 alpha=2, beta=5, rou=0.3, nMax=10):
        self.CityCount = cityCount
        self.AntCount = antCount
        self.Q = q
        self.Alpha = alpha
        self.Beta = beta
        self.Rou = rou
        self.Nmax = nMax
        self.Shortest = 10e6
        random.seed()
        for nCity in range(self.CityCount):
            BestTour.append(0)
        for row in range(self.CityCount):
            pheromoneList = []
            pheromoneDeltaList = []
            for col in range(self.CityCount):
                pheromoneList.append(100)              
                pheromoneDeltaList.append(0)            
            PheromoneTrailList.append(pheromoneList)
            PheromoneDeltaTrailList.append(pheromoneDeltaList)
        
    def ReadCityInfo(self, fileName):
        for i in fileName.keys():
            cityN, cityX, cityY = i,fileName[i][1],fileName[i][2]
            CitySet.add(int(cityN))               
            CityList.append((int(cityN),float(cityX),float(cityY)))
        for row in range(self.CityCount):
            distanceList = []
            for col in range(self.CityCount):
                distance = sqrt(pow(CityList[row][1]-CityList[col][1],2)+pow(CityList[row][2]-CityList[col][2],2))
                distanceList.append(distance)
            CityDistanceList.append(distanceList)

    def PutAnts(self):
        for antNum in range(self.AntCount):
            city = random.randint(1, self.CityCount)
            ant = ANT(city)
            AntList.append(ant)
    def Search(self):
        for iter in range(self.Nmax):
            self.PutAnts()
            for ant in AntList:
                for ttt in range(len(CityList)):
                    ant.MoveToNextCity(self.Alpha, self.Beta)
                ant.UpdatePathLen()
            tmpLen = AntList[0].CurrLen
            tmpTour = AntList[0].TabuCityList
            for ant in AntList[1:]:
                if ant.CurrLen < tmpLen:
                    tmpLen = ant.CurrLen
                    tmpTour = ant.TabuCityList
            if tmpLen < self.Shortest:
                self.Shortest = tmpLen
                BestTour = tmpTour
            print (iter,":",self.Shortest,":",BestTour)
            BTC.append(BestTour)
            self.UpdatePheromoneTrail()


    def UpdatePheromoneTrail(self):
        for ant in AntList:
            for city in ant.TabuCityList[0:-1]:
                idx = ant.TabuCityList.index(city)
                nextCity = ant.TabuCityList[idx+1]
                PheromoneDeltaTrailList[city-1][nextCity-1] = self.Q/ant.CurrLen
                PheromoneDeltaTrailList[nextCity-1][city-1] = self.Q/ant.CurrLen
            lastCity = ant.TabuCityList[-1]
            firstCity = ant.TabuCityList[0]
            PheromoneDeltaTrailList[lastCity-1][firstCity-1] = self.Q/ant.CurrLen
            PheromoneDeltaTrailList[firstCity-1][lastCity-1] = self.Q/ant.CurrLen
        for (city1,city1X,city1Y) in CityList:
            for (city2,city2X,city2Y) in CityList:
                PheromoneTrailList[city1-1][city2-1] = ((1-self.Rou)*PheromoneTrailList[city1-1][city2-1] +
                                                    PheromoneDeltaTrailList[city1-1][city2-1])
                PheromoneDeltaTrailList[city1-1][city2-1] = 0
    
class ANT:
    def __init__(self, currCity = 0):
        self.TabuCitySet = sets.Set()            
        self.TabuCityList = []                   
        self.AllowedCitySet = sets.Set()         
        self.TransferProbabilityList = []        
        self.CurrCity = 0                       
        self.CurrLen = 0.0                      
        self.AddCity(currCity)
        pass
    def SelectNextCity(self, alpha, beta):
        if len(self.AllowedCitySet) == 0:
            return (0)
        sumProbability = 0.0
        for city in self.AllowedCitySet:
            sumProbability = sumProbability + (pow(PheromoneTrailList[self.CurrCity-1][city-1], alpha)* pow(1.0/CityDistanceList[self.CurrCity-1][city-1], beta))
        self.TransferProbabilityList = []
        for city in self.AllowedCitySet:
            transferProbability = (pow(PheromoneTrailList[self.CurrCity-1][city-1], alpha)* pow(1.0/CityDistanceList[self.CurrCity-1][city-1], beta))/sumProbability
            self.TransferProbabilityList.append((city, transferProbability))
        select = 0.0
        for city,cityProb in self.TransferProbabilityList:
            if cityProb > select:
                select = cityProb
        threshold = select * random.random()
        for (cityNum, cityProb) in self.TransferProbabilityList:
            if cityProb >= threshold:
                return (cityNum)
        return (0)
    def MoveToNextCity(self, alpha, beta):
        nextCity = self.SelectNextCity(alpha, beta)
        if nextCity > 0:
            self.AddCity(nextCity)
    def ClearTabu(self):
        self.TabuCityList = []
        self.TabuCitySet.clear()
        self.AllowedCitySet = CitySet - self.TabuCitySet
    def UpdatePathLen(self):
        for city in self.TabuCityList[0:-1]:
            nextCity = self.TabuCityList[self.TabuCityList.index(city)+1]
            self.CurrLen = self.CurrLen + CityDistanceList[city-1][nextCity-1]
        lastCity = self.TabuCityList[-1]
        firstCity = self.TabuCityList[0]
        self.CurrLen = self.CurrLen + CityDistanceList[lastCity-1][firstCity-1]
    def AddCity(self,city):
        if city <= 0:
            return
        self.CurrCity = city
        self.TabuCityList.append(city)
        self.TabuCitySet.add(city)
        self.AllowedCitySet = CitySet - self.TabuCitySet

citydic={}
for i in range(len(Cities)):
    citydic[i+1]=[i+1,rs.PointCoordinates(Cities[i])[0],rs.PointCoordinates(Cities[i])[1]]
cCount=len(Cities)
antCount=int(antCount)
nMax=int(nMax)
if run==True:
    theant=BACA(cityCount=cCount, antCount=antCount, q=80,alpha=2, beta=5, rou=0.3, nMax=nMax)
    theant.ReadCityInfo(citydic)
    theant.Search()
    BTCO=BTC[-1]
    closep=BTCO[:]
    closep.append(BTCO[0])
    Route=[Cities[i-1] for i in closep ]

```

**获得的初始结构线：**

<a href=""><img src="./imgs_pyd/033.jpg" height="auto" width="500" title="caDesign"></a>

输出的文件：

1. paraPolyline.txt->空间折线的坐标

2. paraPolylinePred.txt->设计区域内容随机生成的点，输出x,y坐标

---

**NetLogo下Ants程序-辅助理解蚁群算法**

In this project, a colony of ants forages for food. Though each ant follows a set of simple rules, the colony as a whole acts in a sophisticated way.

When an ant finds a piece of food, it carries the food back to the nest, dropping a chemical as it moves. When other ants “sniff” the chemical, they follow the chemical toward the food. As more ants carry food to the nest, they reinforce the chemical trail.

```NetLogo
patches-own [
  chemical             ;; amount of chemical on this patch
  food                 ;; amount of food on this patch (0, 1, or 2)
  nest?                ;; true on nest patches, false elsewhere
  nest-scent           ;; number that is higher closer to the nest
  food-source-number   ;; number (1, 2, or 3) to identify the food sources
]

;;;;;;;;;;;;;;;;;;;;;;;;
;;; Setup procedures ;;;
;;;;;;;;;;;;;;;;;;;;;;;;

to setup
  clear-all
  set-default-shape turtles "bug"
  create-turtles population
  [ set size 2         ;; easier to see
    set color red  ]   ;; red = not carrying food
  setup-patches
  reset-ticks
end

to setup-patches
  ask patches
  [ setup-nest
    setup-food
    recolor-patch ]
end

to setup-nest  ;; patch procedure
  ;; set nest? variable to true inside the nest, false elsewhere
  set nest? (distancexy 0 0) < 5
  ;; spread a nest-scent over the whole world -- stronger near the nest
  set nest-scent 200 - distancexy 0 0
end

to setup-food  ;; patch procedure
  ;; setup food source one on the right
  if (distancexy (0.6 * max-pxcor) 0) < 5
  [ set food-source-number 1 ]
  ;; setup food source two on the lower-left
  if (distancexy (-0.6 * max-pxcor) (-0.6 * max-pycor)) < 5
  [ set food-source-number 2 ]
  ;; setup food source three on the upper-left
  if (distancexy (-0.8 * max-pxcor) (0.8 * max-pycor)) < 5
  [ set food-source-number 3 ]
  ;; set "food" at sources to either 1 or 2, randomly
  if food-source-number > 0
  [ set food one-of [1 2] ]
end

to recolor-patch  ;; patch procedure
  ;; give color to nest and food sources
  ifelse nest?
  [ set pcolor violet ]
  [ ifelse food > 0
    [ if food-source-number = 1 [ set pcolor cyan ]
      if food-source-number = 2 [ set pcolor sky  ]
      if food-source-number = 3 [ set pcolor blue ] ]
    ;; scale color to show chemical concentration
    [ set pcolor scale-color green chemical 0.1 5 ] ]
end

;;;;;;;;;;;;;;;;;;;;;
;;; Go procedures ;;;
;;;;;;;;;;;;;;;;;;;;;

to go  ;; forever button
  ask turtles
  [ if who >= ticks [ stop ] ;; delay initial departure
    ifelse color = red
    [ look-for-food  ]       ;; not carrying food? look for it
    [ return-to-nest ]       ;; carrying food? take it back to nest
    wiggle
    fd 1 ]
  diffuse chemical (diffusion-rate / 100)
  ask patches
  [ set chemical chemical * (100 - evaporation-rate) / 100  ;; slowly evaporate chemical
    recolor-patch ]
  tick
end

to return-to-nest  ;; turtle procedure
  ifelse nest?
  [ ;; drop food and head out again
    set color red
    rt 180 ]
  [ set chemical chemical + 60  ;; drop some chemical
    uphill-nest-scent ]         ;; head toward the greatest value of nest-scent
end

to look-for-food  ;; turtle procedure
  if food > 0
  [ set color orange + 1     ;; pick up food
    set food food - 1        ;; and reduce the food source
    rt 180                   ;; and turn around
    stop ]
  ;; go in the direction where the chemical smell is strongest
  if (chemical >= 0.05) and (chemical < 2)
  [ uphill-chemical ]
end

;; sniff left and right, and go where the strongest smell is
to uphill-chemical  ;; turtle procedure
  let scent-ahead chemical-scent-at-angle   0
  let scent-right chemical-scent-at-angle  45
  let scent-left  chemical-scent-at-angle -45
  if (scent-right > scent-ahead) or (scent-left > scent-ahead)
  [ ifelse scent-right > scent-left
    [ rt 45 ]
    [ lt 45 ] ]
end

;; sniff left and right, and go where the strongest smell is
to uphill-nest-scent  ;; turtle procedure
  let scent-ahead nest-scent-at-angle   0
  let scent-right nest-scent-at-angle  45
  let scent-left  nest-scent-at-angle -45
  if (scent-right > scent-ahead) or (scent-left > scent-ahead)
  [ ifelse scent-right > scent-left
    [ rt 45 ]
    [ lt 45 ] ]
end

to wiggle  ;; turtle procedure
  rt random 40
  lt random 40
  if not can-move? 1 [ rt 180 ]
end

to-report nest-scent-at-angle [angle]
  let p patch-right-and-ahead angle 1
  if p = nobody [ report 0 ]
  report [nest-scent] of p
end

to-report chemical-scent-at-angle [angle]
  let p patch-right-and-ahead angle 1
  if p = nobody [ report 0 ]
  report [chemical] of p
end


; Copyright 1997 Uri Wilensky.
; See Info tab for full copyright and license.
```

(wikipedia)蚁群算法（Ant Colony Optimization, ACO），又称蚂蚁算法，是一种用来在图中寻找优化路径的机率型算法。它由Marco Dorigo于1992年在他的博士论文“Ant system: optimization by a colony of cooperating agents”中提出，其灵感来源于蚂蚁在寻找食物过程中发现路径的行为。蚁群算法是一种模拟进化算法，初步的研究表明该算法具有许多优良的性质。

人工蚂蚁的搜索主要包括三种智能行为：

（1）蚂蚁的记忆行为。一只蚂蚁搜索过的路径在下次搜索时就不再被该蚂蚁选择，因此在蚁群算法中建立禁忌表(TabuCityList)进行模拟。

（2）蚂蚁利用信息素进行相互通信。蚂蚁在所选择的路径上会释放一种信息素的物质，当其他蚂蚁进行路径选择时，会根据路径上的信息素浓度进行选择，这样信息素就成为蚂蚁之间进行通信的媒介。

（3）蚂蚁的集群活动。通过一只蚂蚁的运动很难达到事物源，但整个蚁群进行搜索就完全不同。当某些路径上通过的蚂蚁越来越多时，路径上留下的信息素数量也就越多，导致信息素强

<a href=""><img src="./imgs_pyd/Animation_ants.gif" height="auto" width="auto" title="caDesign"></a>

<a href=""><img src="./imgs_pyd/034.jpg" height="auto" width="auto" title="caDesign"></a>

> markdown下书写公式用TeX编辑器，[TeX equation editor (Mathematical Formulas)](http://atomurl.net/math/)


```python
import os,sys,random,string #调入基本的python模块
from string import * #调入字符串模块的所有函数
from math import * #调入数学模块的所有函数
BestTour = []  #用于放置最佳路径选择城市的顺序             
CitySet = set() #城市集合，sets数据类型是个无序的没有重复元素的集合，两个sets之间可以做‘-’差集，即在第一个集合中移出第二个集合中也存在的元素         
CityList = []  #城市列表即存放代表城市的序号              
PheromoneTrailList = []  #信息素列表(矩阵)
PheromoneDeltaTrailList = []  #释放信息素列表(矩阵)  
CityDistanceList = []  #两两城市距离列表(矩阵)       
AntList = [] #蚂蚁列表
class BACA: #定义类BACA，执行蚁群基本算法
    def __init__(self, cityCount=100, antCount=50, q=80,alpha=2, beta=5, rou=0.3, nMax=5): #初始化方法，传入形式参数，定义默认值
        self.CityCount = cityCount #城市数量，本例中为手工输入，也可以根据城市数据列表编写程序求得
        self.AntCount = antCount #蚂蚁数量
        self.Q = q #信息素增加强度系数
        self.Alpha = alpha #表征信息素重要程度的参数
        self.Beta = beta #表征启发式因子重要程度的参数
        self.Rou = rou #信息素蒸发系数 
        self.Nmax = nMax #最大迭代次数
        self.Shortest = 10e6 #初始最短距离应该尽可能大，至少大于估算的最大城市旅行距离
        random.seed() #设置随机种子        
        # 初始化全局数据结构及值
        for nCity in range(self.CityCount): #循环城市总数的次数(即循环range(0, 51)，为0-50，不包括51)
            BestTour.append(0) #设置最佳路径初始值均为0           
        for row in range(self.CityCount): #再次循环城市总数的次数
            pheromoneList = [] #定义空的信息素列表
            pheromoneDeltaList = [] #定义空的释放信息素列表
            for col in range(self.CityCount): #循环城市总数的次数
                pheromoneList.append(100)  #定义一个城市到所有城市路径信息素的初始值             
                pheromoneDeltaList.append(0) #定义一个城市到所有城市路径释放信息素的初始值         
            PheromoneTrailList.append(pheromoneList) #建立每个城市到所有城市路径信息素的初始值列表矩阵
            PheromoneDeltaTrailList.append(pheromoneDeltaList) #建立每个城市到所有城市路径释放信息素的初始值列表矩阵

    def ReadCityInfo(self, fileName):#定义读取城市文件的方法
        file = open(fileName) #打开城市文件
        for line in file.readlines(): #逐行读取文件
            cityN, cityX, cityY = str.split(line) #分别提取城市序号、X坐标和Y坐标，使用空格切分
            CitySet.add(int(cityN)) #在城市集合中逐步追加所有的城市序号               
            CityList.append((int(cityN),float(cityX),float(cityY))) #在城市列表中逐步追加每个城市序号、X坐标和Y坐标的建立的元组
        for row in range(self.CityCount): #循环城市总数的次数
            distanceList = [] #建立临时储存距离的空列表
            for col in range(self.CityCount): #再次循环城市总数次数
                distance = sqrt(pow(CityList[row][1]-CityList[col][1],2)+pow(CityList[row][2]-CityList[col][2],2)) #逐一计算每个城市到所有城市的距离值
                distanceList.append(distance) #追加一个城市到所有城市的距离值
            CityDistanceList.append(distanceList) #追加每个城市到所有城市的距离值，为矩阵即包含子列表
    def PutAnts(self): #定义蚂蚁所选择城市以及将城市作为参数定义蚂蚁的方法和属性
        for antNum in range(self.AntCount): #循环蚂蚁总数的次数
            city = random.randint(1, self.CityCount) #随机选择一个城市
            ant = ANT(city) #蚂蚁类ANT的实例化，即按照每只蚂蚁随机选择的城市作为传入参数，使之具有ANT蚂蚁类的方法和属性
            AntList.append(ant) #将定义的每只蚂蚁追加到列表中

    def Search(self): #定义搜索最佳旅行路径的方法，主程序
        for iter in range(self.Nmax): #循环指定的迭代次数
            self.PutAnts() #执行self.PutAnts()方法，定义蚂蚁选择的初始城市和蚂蚁具有的方法和属性
            for ant in AntList: #循环遍历蚂蚁列表，由self.PutAnts()方法定义获取
                for ttt in range(len(CityList)): #循环遍历城市总数次数
                    ant.MoveToNextCity(self.Alpha, self.Beta) #执行蚂蚁的ant.MoveToNextCity()方法，获取蚂蚁每次旅行时的旅行路径长度CurrLen，禁忌城市列表TabuCityList等属性值
                    ant.UpdatePathLen() #使用ant.UpdatePathLen()方法更新蚂蚁旅行路径长度
            tmpLen = AntList[0].CurrLen #将蚂蚁列表中第一只蚂蚁的旅行路径长度赋值给新的变量tmpLen
            tmpTour = AntList[0].TabuCityList #将获取蚂蚁列表第一只蚂蚁的禁忌城市列表赋值给新的变量tmpTour
            for ant in AntList[1:]: #循环遍历蚂蚁列表，从索引值1开始，除第一只外
                if ant.CurrLen < tmpLen: #如果循环到的蚂蚁旅行路径长度小于tmpLen即前次循环蚂蚁旅行路径长度，开始值为蚂蚁列表第一只蚂蚁的旅行路径长度
                    tmpLen = ant.CurrLen #更新变量temLen的值
                    tmpTour = ant.TabuCityList #更新变量tmpTour的值，即根新禁忌城市列表
            if tmpLen < self.Shortest: #如果从蚂蚁列表中获取的最短路径小于初始化时定义的长度
                self.Shortest = tmpLen #更新旅行路径最短长度
                BestTour = tmpTour #更新初始化时定义的最佳旅行城市次序列表
            print (iter,":",self.Shortest,":",BestTour) #打印当前迭代次数，最短旅行路径长度和最佳旅行城市次序列表
            self.UpdatePheromoneTrail() #完成每次迭代需要使用self.UpdatePheromoneTrail()方法更新信息素

    def UpdatePheromoneTrail(self): #定义更新信息素的方法，需要参考前文对于蚁群算法的阐述
        for ant in AntList: #循环遍历蚂蚁列表
            for city in ant.TabuCityList[0:-1]: #循环遍历蚂蚁的禁忌城市列表
                idx = ant.TabuCityList.index(city) #获取当前循环禁忌城市的索引值
                nextCity = ant.TabuCityList[idx+1] #获取当前循环禁忌城市紧邻的下一个禁忌城市
                PheromoneDeltaTrailList[city-1][nextCity-1] = self.Q/ant.CurrLen #逐次更新释放信息素列表，注意矩阵行列所代表的意义，[city-1]为选取的子列表即当前城市与所有城市间路径的释放信息素值，初始值均为0，[nextCity-1]为在子列表中对应紧邻的下一个城市，释放信息素为Q信息素增加强度系数与蚂蚁当前旅行路径长度CurrLen的比值，路径长度越小释放信息素越大，反之则越小
                PheromoneDeltaTrailList[nextCity-1][city-1] = self.Q/ant.CurrLen #在二维矩阵中，每个城市路径均出现两次，分别为[city-1]对应的nextCity-1]和[nextCity-1]对应的[city-1]，因此都需要更新，注意城市序列因为从1开始，而列表索引值均从0开始，所以需要减1
            lastCity = ant.TabuCityList[-1] #获取禁忌城市列表的最后一个城市
            firstCity = ant.TabuCityList[0] #获取禁忌城市列表的第一个城市
            PheromoneDeltaTrailList[lastCity-1][firstCity-1] = self.Q/ant.CurrLen #因为蚂蚁旅行需要返回到开始的城市，因此需要更新禁忌城市列表最后一个城市到第一个城市旅行路径的释放信息素值，即最后一个城市对应第一个城市释放信息素值
            PheromoneDeltaTrailList[firstCity-1][lastCity-1] = self.Q/ant.CurrLen #同理更新第一城市对应最后一个城市的释放信息素值
        for (city1,city1X,city1Y) in CityList: #循环遍历城市列表，主要是提取city1即城市的序号
            for (city2,city2X,city2Y) in CityList: #再次循环遍历城市列表，主要是提取city2即城市序号，循环两次的目的仍然是对应列表矩阵的数据结构
                PheromoneTrailList[city1-1][city2-1] = ((1-self.Rou)*PheromoneTrailList[city1-1][city2-1] +PheromoneDeltaTrailList[city1-1][city2-1]) #更新信息素列表，值为每一旅行路径前次信息素蒸发后的值加本次循环释放的信息素，蒸发的信息素由1-Rou信息素蒸发系数*当前路径信息素确定
                PheromoneDeltaTrailList[city1-1][city2-1] = 0 #将释放信息素列表值再次初始化为0，用于下次循环

class ANT: #定义蚂蚁类，使得蚂蚁具有相应的方法和属性
    def __init__(self, currCity = 0): #蚂蚁类的初始化方法，默认传入当前城市序号为0
        self.TabuCitySet = set()  #定义禁忌城市集合，定义集合的目的是集合本身要素不重复并且之间可以做差集运算，例如AddCity()方法中self.AllowedCitySet = CitySet - self.TabuCitySet，可以方便的从城市集合中去除禁忌城市列表的城市获取允许的城市列表
        self.TabuCityList = [] #定义禁忌城市空列表
        self.AllowedCitySet = set() #定义允许城市集合
        self.TransferProbabilityList = [] #定义城市选择可能性列表
        self.CurrCity = 0 #定义当前城市初始值为0
        self.CurrLen = 0.0 #定义当前旅行路径长度为0
        self.AddCity(currCity) #执行AddCity()方法，获取每次迭代的当前城市CurrCity、禁忌城市列表TabuCityList和允许城市列表AllowedCitySet的值
        pass #空语句，此行为空，不运行任何操作

    def SelectNextCity(self, alpha, beta): #定义蚂蚁选择下一个城市的方法，需要参考前文阐述的蚁群算法
        if len(self.AllowedCitySet) == 0: #如果允许城市集合为0则返回0
            return (0)
        sumProbability = 0.0 #定义概率，可能性初始值为0
        for city in self.AllowedCitySet: #循环遍历允许城市集合
            sumProbability = sumProbability + (pow(PheromoneTrailList[self.CurrCity-1][city-1], alpha) * pow(1.0/CityDistanceList[self.CurrCity-1][city-1], beta))  #蚂蚁选择下一个城市的可能性为信息素与城市距离之间关系综合因素确定，其中alpha为表征信息素重要程度的参数，eta为表征启发式因子重要程度的参数，该语句为前文蚁群算法阐述选择下一个转移城市概率公式的分母部分                                
        self.TransferProbabilityList = [] #建立选择下一个城市可能性空列表
        for city in self.AllowedCitySet: #循环遍历允许城市列表
            transferProbability = (pow(PheromoneTrailList[self.CurrCity-1][city-1], alpha)* pow(1.0/CityDistanceList[self.CurrCity-1][city-1], beta))/sumProbability #根据选择下一个转移城市概率公式获取蚂蚁选择下一个城市概率的列表
            self.TransferProbabilityList.append((city, transferProbability)) #将城市序号和对应的转移城市概率追加到转移概率列表中
        select = 0.0 #设置初始选择值为0
        for city,cityProb in self.TransferProbabilityList: #循环遍历转移概率列表
            if cityProb > select: #如果概率大于前一个选择值，则更新概率值
                select = cityProb
        threshold = select * random.random() #将概率值乘以一个0-1的随机数，获取阙值
        for (cityNum, cityProb) in self.TransferProbabilityList: #再次循环遍历概率列表
            if cityProb >= threshold: #如果概率大于阙值则返回对应的城市序号
                return (cityNum)
        return (0) #否则返回0

    def MoveToNextCity(self, alpha, beta): #定义转移城市方法
        nextCity = self.SelectNextCity(alpha, beta) #执行SelectNextCity()，选择下一个城市的方法，获取选择城市的序号并赋值给新的变量nextCity
        if nextCity > 0: #如果选择的城市序号大于0则执行self.AddCity()方法，获取每次迭代的当前城市CurrCity、禁忌城市列表TabuCityList和允许城市列表AllowedCitySet的值
            self.AddCity(nextCity) #执行self.AddCity()方法

    def ClearTabu(self): #定义清除禁忌城市方法，以用于下一次循环
        self.TabuCityList = [] #初始化禁忌城市列表为空
        self.TabuCitySet.clear() #初始化禁忌城市集合为空
        self.AllowedCitySet = CitySet - self.TabuCitySet #初始化允许城市集合

    def UpdatePathLen(self): #定义更新履行路径长度方法
        for city in self.TabuCityList[0:-1]: #循环遍历禁忌城市列表
            nextCity = self.TabuCityList[self.TabuCityList.index(city)+1] #获取禁忌城市列表中的下一个城市序号
            self.CurrLen = self.CurrLen + CityDistanceList[city-1][nextCity-1] #从城市间距离值中提取当前循环城市与下一个城市之间的距离，并逐次求和
        lastCity = self.TabuCityList[-1] #提取禁忌列表中的最后一个城市
        firstCity = self.TabuCityList[0] #提取禁忌列表中第一个城市
        self.CurrLen = self.CurrLen + CityDistanceList[lastCity-1][firstCity-1] #将最后一个城市和第一个城市距离值加到当前旅行路径长度获取循环全部城市的路径长度

    def AddCity(self,city): #定义增加城市到禁忌城市列表中的方法
        if city <= 0: #如果城市序号小于0，则返回
            return
        self.CurrCity = city #更新当前城市序号
        self.TabuCityList.append(city) #将当前城市追加的禁忌城市列表中，因为已经旅行过的城市不应该再进入
        self.TabuCitySet.add(city) #将当前城市追加到禁忌城市集合中，用于差集运送
        self.AllowedCitySet = CitySet - self.TabuCitySet #使用集合差集的方法获取允许城市列表     
```

<a href=""><img src="./imgs_pyd/ACO_diagram.jpg" height="auto" width="auto" title="caDesign"></a>


```python
theBaca = BACA() #BACA类的实例化
theBaca.ReadCityInfo("./data/acatsp.tsp") #读入城市数据
theBaca.Search() #执行Search()方法
os.system("pause") #暂停控制台    
```

    0 : 45041.18005838155 : [93, 53, 40, 37, 38, 22, 73, 88, 46, 12, 26, 47, 99, 79, 9, 84, 41, 68, 75, 67, 11, 5, 4, 10, 71, 51, 19, 52, 18, 39, 48, 24, 50, 81, 29, 28, 45, 25, 35, 89, 55, 27, 17, 54, 86, 77, 8, 95, 34, 82, 1, 14, 80, 90, 16, 61, 7, 60, 74, 30, 21, 64, 32, 36, 44, 2, 15, 70, 98, 78, 100, 65, 58, 59, 97, 92, 23, 33, 31, 20, 57, 43, 69, 3, 6, 13, 87, 91, 94, 49, 72, 76, 66, 62, 42, 85, 56, 63, 83, 96]
    1 : 44684.632900223245 : [73, 22, 37, 38, 40, 53, 36, 32, 44, 2, 15, 70, 98, 78, 47, 26, 79, 99, 9, 84, 41, 68, 75, 67, 11, 5, 4, 91, 94, 24, 50, 81, 29, 28, 45, 25, 97, 89, 55, 27, 92, 17, 54, 86, 77, 8, 95, 34, 82, 1, 14, 80, 90, 16, 61, 30, 74, 60, 7, 64, 21, 88, 46, 12, 58, 49, 59, 35, 87, 13, 6, 3, 69, 43, 57, 20, 31, 33, 23, 96, 83, 48, 39, 18, 52, 19, 71, 51, 10, 72, 76, 66, 62, 56, 42, 85, 63, 100, 65, 93]
    2 : 44684.632900223245 : [73, 22, 37, 38, 40, 53, 36, 32, 44, 2, 15, 70, 98, 78, 47, 26, 79, 99, 9, 84, 41, 68, 75, 67, 11, 5, 4, 91, 94, 24, 50, 81, 29, 28, 45, 25, 97, 89, 55, 27, 92, 17, 54, 86, 77, 8, 95, 34, 82, 1, 14, 80, 90, 16, 61, 30, 74, 60, 7, 64, 21, 88, 46, 12, 58, 49, 59, 35, 87, 13, 6, 3, 69, 43, 57, 20, 31, 33, 23, 96, 83, 48, 39, 18, 52, 19, 71, 51, 10, 72, 76, 66, 62, 56, 42, 85, 63, 100, 65, 93]
    3 : 44684.632900223245 : [73, 22, 37, 38, 40, 53, 36, 32, 44, 2, 15, 70, 98, 78, 47, 26, 79, 99, 9, 84, 41, 68, 75, 67, 11, 5, 4, 91, 94, 24, 50, 81, 29, 28, 45, 25, 97, 89, 55, 27, 92, 17, 54, 86, 77, 8, 95, 34, 82, 1, 14, 80, 90, 16, 61, 30, 74, 60, 7, 64, 21, 88, 46, 12, 58, 49, 59, 35, 87, 13, 6, 3, 69, 43, 57, 20, 31, 33, 23, 96, 83, 48, 39, 18, 52, 19, 71, 51, 10, 72, 76, 66, 62, 56, 42, 85, 63, 100, 65, 93]
    4 : 44684.632900223245 : [73, 22, 37, 38, 40, 53, 36, 32, 44, 2, 15, 70, 98, 78, 47, 26, 79, 99, 9, 84, 41, 68, 75, 67, 11, 5, 4, 91, 94, 24, 50, 81, 29, 28, 45, 25, 97, 89, 55, 27, 92, 17, 54, 86, 77, 8, 95, 34, 82, 1, 14, 80, 90, 16, 61, 30, 74, 60, 7, 64, 21, 88, 46, 12, 58, 49, 59, 35, 87, 13, 6, 3, 69, 43, 57, 20, 31, 33, 23, 96, 83, 48, 39, 18, 52, 19, 71, 51, 10, 72, 76, 66, 62, 56, 42, 85, 63, 100, 65, 93]
    




    0



**代码的逆向工程**

1. [Visual Paradigm](https://www.visual-paradigm.com/) 

2. [Pyreverse : UML Diagrams for Python](https://www.logilab.org/blogentry/6883) -free

```
-a N, -A    depth of research for ancestors
-s N, -S    depth of research for associated classes
-A, -S      all ancestors, resp. all associated
-m[yn]      add or remove the module name
-f MOD      filter the attributes : PUB_ONLY/SPECIAL/OTHER/ALL
-k          show only the classes (no attributes and methods)
-b          show 'builtin' objects
```

pyreverse <<location of the python repo>>  in terminal; 
    
pyreverse -ASmy ACO_note.py
    
pyreverse -a 3 -s 3 -b ACO_note.py


```python
import pydot
(graph,)=pydot.graph_from_dot_file('./data/classes.dot')
graph.write_png('./data/ACO_UML.png')
```

<a href=""><img src="./imgs_pyd/ACO_UML.png" height="auto" width="auto" title="caDesign"></a>

### 1.2 RandomForestRegressor 回归预测高程

[mean_squared_error 均方误差](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)

[r2_score 判定系数](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.r2_score.html)



```python
import re
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_squared_error
from sklearn.metrics import r2_score
import matplotlib.pyplot as plt

fn=r'C:\Users\richi\omen_richiebao\omen_github\python_code_archi _la_design_method_study\data\paraPolyline.txt'
pred_fn=r'C:\Users\richi\omen_richiebao\omen_github\python_code_archi _la_design_method_study\data\paraPolylinePred.txt'

'''读取文本数据'''
def txtReading(fn):
    f=open(fn,'r')
    dataList=[]
    pat=re.compile('{(.*?)}')
    while True:
        line=f.readline().strip()    
        if len(line)!=0:
            line=pat.findall(line)[0].split(',')
            line=[float(i) for i in line]
            dataList.append(line)
        if not line:break
    f.close()
    dataArray=np.array(dataList)
    return dataArray

dataArray=txtReading(fn)
X=dataArray[...,0:2] #解释变量(X,Y值)
y=dataArray[...,2] #目标变量(Z值)
X_train,X_test,y_train,y_test=train_test_split(X,y,test_size=0.3,random_state=1)

forest=RandomForestRegressor(n_estimators=1000,criterion='mse',random_state=1,n_jobs=-1)
forest.fit(X_train,y_train)
y_train_pred=forest.predict(X_train)
y_test_pred=forest.predict(X_test)

print('MSE train:%.3f,test:%.3f'%(mean_squared_error(y_train,y_train_pred),mean_squared_error(y_test,y_test_pred)))
print('R^2 train:%.3f,test:%.3f'%(r2_score(y_train,y_train_pred),r2_score(y_test,y_test_pred)))

plt.scatter(y_train_pred,y_train_pred-y_train,c='blue',marker='o',label='Training data')
plt.scatter(y_test_pred,y_test_pred-y_test,c='lightgreen',marker='s',label='Test data')
plt.xlabel('Predicted values')
plt.ylabel('Residuals')
plt.legend(loc='upper left')
plt.hlines(y=0,xmin=-10,xmax=50,lw=2,color='red')
plt.xlim([-10,50])
plt.show()

'''预测值(高程Z值)，并写入.txt文件，用于grasshopper读取'''
dataPred=txtReading(pred_fn) #读取预测解释变量
dataX=dataPred[...,0:2]
predValue=forest.predict(dataX)
prd_w_fn=r'C:\Users\richi\omen_richiebao\omen_github\python_code_archi _la_design_method_study\data\paraPolylineY.txt'
wf=open(prd_w_fn,'w')
for i in range(len(predValue)):
    if i==len(predValue)-1:
        wf.write(str(predValue[i]))
    else:
        wf.write(str(predValue[i])+',')    

wf.close()
```

    MSE train:0.172,test:1.024
    R^2 train:0.997,test:0.984
    


    
<a href=""><img src="./imgs_pyd/pyd_04.png" height="auto" width="auto" title="caDesign"></a>
    


预测结果

<a href=""><img src="./imgs_pyd/031.jpg" height="auto" width="500" title="caDesign"></a>

> 第7次课-2021_11_10_Wed_7-8节 :2课时 课程完成时间标识线。与第8次课穿插。

---

## 2. 机器学习-回归

机器学习库[scikit-learn](https://scikit-learn.org/stable/) Machine Learning in Python

<a href=""><img src="./imgs_pyd/035.jpg" height="auto" width="auto" title="caDesign"></a>

### 2.1 最简单的一个官方案例，理解基本流程-[Linear Regression](https://scikit-learn.org/stable/auto_examples/linear_model/plot_ols.html#sphx-glr-auto-examples-linear-model-plot-ols-py)


```python
print(__doc__)


# Code source: Jaques Grobler
# License: BSD 3 clause


import matplotlib.pyplot as plt
import numpy as np
from sklearn import datasets, linear_model
from sklearn.metrics import mean_squared_error, r2_score

# Load the diabetes dataset
diabetes_X, diabetes_y = datasets.load_diabetes(return_X_y=True)

# Use only one feature
diabetes_X = diabetes_X[:, np.newaxis, 2]

# Split the data into training/testing sets
diabetes_X_train = diabetes_X[:-20]
diabetes_X_test = diabetes_X[-20:]

# Split the targets into training/testing sets
diabetes_y_train = diabetes_y[:-20]
diabetes_y_test = diabetes_y[-20:]

# Create linear regression object
regr = linear_model.LinearRegression()

# Train the model using the training sets
regr.fit(diabetes_X_train, diabetes_y_train)

# Make predictions using the testing set
diabetes_y_pred = regr.predict(diabetes_X_test)

# The coefficients
print('Coefficients: \n', regr.coef_)
# The mean squared error
print('Mean squared error: %.2f'
      % mean_squared_error(diabetes_y_test, diabetes_y_pred))
# The coefficient of determination: 1 is perfect prediction
print('Coefficient of determination: %.2f'
      % r2_score(diabetes_y_test, diabetes_y_pred))

# Plot outputs
plt.scatter(diabetes_X_test, diabetes_y_test,  color='black')
plt.plot(diabetes_X_test, diabetes_y_pred, color='blue', linewidth=3)

plt.xticks(())
plt.yticks(())

plt.show()
```

    Automatically created module for IPython interactive environment
    Coefficients: 
     [938.23786125]
    Mean squared error: 2548.07
    Coefficient of determination: 0.47
    


    
<a href=""><img src="./imgs_pyd/pyd_05.png" height="auto" width="auto" title="caDesign"></a>
    



```python
help(linear_model.LinearRegression())
```

    Help on LinearRegression in module sklearn.linear_model._base object:
    
    class LinearRegression(sklearn.base.MultiOutputMixin, sklearn.base.RegressorMixin, LinearModel)
     |  LinearRegression(*, fit_intercept=True, normalize=False, copy_X=True, n_jobs=None)
     |  
     |  Ordinary least squares Linear Regression.
     |  
     |  LinearRegression fits a linear model with coefficients w = (w1, ..., wp)
     |  to minimize the residual sum of squares between the observed targets in
     |  the dataset, and the targets predicted by the linear approximation.
     |  
     |  Parameters
     |  ----------
     |  fit_intercept : bool, default=True
     |      Whether to calculate the intercept for this model. If set
     |      to False, no intercept will be used in calculations
     |      (i.e. data is expected to be centered).
     |  
     |  normalize : bool, default=False
     |      This parameter is ignored when ``fit_intercept`` is set to False.
     |      If True, the regressors X will be normalized before regression by
     |      subtracting the mean and dividing by the l2-norm.
     |      If you wish to standardize, please use
     |      :class:`sklearn.preprocessing.StandardScaler` before calling ``fit`` on
     |      an estimator with ``normalize=False``.
     |  
     |  copy_X : bool, default=True
     |      If True, X will be copied; else, it may be overwritten.
     |  
     |  n_jobs : int, default=None
     |      The number of jobs to use for the computation. This will only provide
     |      speedup for n_targets > 1 and sufficient large problems.
     |      ``None`` means 1 unless in a :obj:`joblib.parallel_backend` context.
     |      ``-1`` means using all processors. See :term:`Glossary <n_jobs>`
     |      for more details.
     |  
     |  Attributes
     |  ----------
     |  coef_ : array of shape (n_features, ) or (n_targets, n_features)
     |      Estimated coefficients for the linear regression problem.
     |      If multiple targets are passed during the fit (y 2D), this
     |      is a 2D array of shape (n_targets, n_features), while if only
     |      one target is passed, this is a 1D array of length n_features.
     |  
     |  rank_ : int
     |      Rank of matrix `X`. Only available when `X` is dense.
     |  
     |  singular_ : array of shape (min(X, y),)
     |      Singular values of `X`. Only available when `X` is dense.
     |  
     |  intercept_ : float or array of shape (n_targets,)
     |      Independent term in the linear model. Set to 0.0 if
     |      `fit_intercept = False`.
     |  
     |  See Also
     |  --------
     |  sklearn.linear_model.Ridge : Ridge regression addresses some of the
     |      problems of Ordinary Least Squares by imposing a penalty on the
     |      size of the coefficients with l2 regularization.
     |  sklearn.linear_model.Lasso : The Lasso is a linear model that estimates
     |      sparse coefficients with l1 regularization.
     |  sklearn.linear_model.ElasticNet : Elastic-Net is a linear regression
     |      model trained with both l1 and l2 -norm regularization of the
     |      coefficients.
     |  
     |  Notes
     |  -----
     |  From the implementation point of view, this is just plain Ordinary
     |  Least Squares (scipy.linalg.lstsq) wrapped as a predictor object.
     |  
     |  Examples
     |  --------
     |  >>> import numpy as np
     |  >>> from sklearn.linear_model import LinearRegression
     |  >>> X = np.array([[1, 1], [1, 2], [2, 2], [2, 3]])
     |  >>> # y = 1 * x_0 + 2 * x_1 + 3
     |  >>> y = np.dot(X, np.array([1, 2])) + 3
     |  >>> reg = LinearRegression().fit(X, y)
     |  >>> reg.score(X, y)
     |  1.0
     |  >>> reg.coef_
     |  array([1., 2.])
     |  >>> reg.intercept_
     |  3.0000...
     |  >>> reg.predict(np.array([[3, 5]]))
     |  array([16.])
     |  
     |  Method resolution order:
     |      LinearRegression
     |      sklearn.base.MultiOutputMixin
     |      sklearn.base.RegressorMixin
     |      LinearModel
     |      sklearn.base.BaseEstimator
     |      builtins.object
     |  
     |  Methods defined here:
     |  
     |  __init__(self, *, fit_intercept=True, normalize=False, copy_X=True, n_jobs=None)
     |      Initialize self.  See help(type(self)) for accurate signature.
     |  
     |  fit(self, X, y, sample_weight=None)
     |      Fit linear model.
     |      
     |      Parameters
     |      ----------
     |      X : {array-like, sparse matrix} of shape (n_samples, n_features)
     |          Training data
     |      
     |      y : array-like of shape (n_samples,) or (n_samples, n_targets)
     |          Target values. Will be cast to X's dtype if necessary
     |      
     |      sample_weight : array-like of shape (n_samples,), default=None
     |          Individual weights for each sample
     |      
     |          .. versionadded:: 0.17
     |             parameter *sample_weight* support to LinearRegression.
     |      
     |      Returns
     |      -------
     |      self : returns an instance of self.
     |  
     |  ----------------------------------------------------------------------
     |  Data and other attributes defined here:
     |  
     |  __abstractmethods__ = frozenset()
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors inherited from sklearn.base.MultiOutputMixin:
     |  
     |  __dict__
     |      dictionary for instance variables (if defined)
     |  
     |  __weakref__
     |      list of weak references to the object (if defined)
     |  
     |  ----------------------------------------------------------------------
     |  Methods inherited from sklearn.base.RegressorMixin:
     |  
     |  score(self, X, y, sample_weight=None)
     |      Return the coefficient of determination R^2 of the prediction.
     |      
     |      The coefficient R^2 is defined as (1 - u/v), where u is the residual
     |      sum of squares ((y_true - y_pred) ** 2).sum() and v is the total
     |      sum of squares ((y_true - y_true.mean()) ** 2).sum().
     |      The best possible score is 1.0 and it can be negative (because the
     |      model can be arbitrarily worse). A constant model that always
     |      predicts the expected value of y, disregarding the input features,
     |      would get a R^2 score of 0.0.
     |      
     |      Parameters
     |      ----------
     |      X : array-like of shape (n_samples, n_features)
     |          Test samples. For some estimators this may be a
     |          precomputed kernel matrix or a list of generic objects instead,
     |          shape = (n_samples, n_samples_fitted),
     |          where n_samples_fitted is the number of
     |          samples used in the fitting for the estimator.
     |      
     |      y : array-like of shape (n_samples,) or (n_samples, n_outputs)
     |          True values for X.
     |      
     |      sample_weight : array-like of shape (n_samples,), default=None
     |          Sample weights.
     |      
     |      Returns
     |      -------
     |      score : float
     |          R^2 of self.predict(X) wrt. y.
     |      
     |      Notes
     |      -----
     |      The R2 score used when calling ``score`` on a regressor uses
     |      ``multioutput='uniform_average'`` from version 0.23 to keep consistent
     |      with default value of :func:`~sklearn.metrics.r2_score`.
     |      This influences the ``score`` method of all the multioutput
     |      regressors (except for
     |      :class:`~sklearn.multioutput.MultiOutputRegressor`).
     |  
     |  ----------------------------------------------------------------------
     |  Methods inherited from LinearModel:
     |  
     |  predict(self, X)
     |      Predict using the linear model.
     |      
     |      Parameters
     |      ----------
     |      X : array_like or sparse matrix, shape (n_samples, n_features)
     |          Samples.
     |      
     |      Returns
     |      -------
     |      C : array, shape (n_samples,)
     |          Returns predicted values.
     |  
     |  ----------------------------------------------------------------------
     |  Methods inherited from sklearn.base.BaseEstimator:
     |  
     |  __getstate__(self)
     |  
     |  __repr__(self, N_CHAR_MAX=700)
     |      Return repr(self).
     |  
     |  __setstate__(self, state)
     |  
     |  get_params(self, deep=True)
     |      Get parameters for this estimator.
     |      
     |      Parameters
     |      ----------
     |      deep : bool, default=True
     |          If True, will return the parameters for this estimator and
     |          contained subobjects that are estimators.
     |      
     |      Returns
     |      -------
     |      params : mapping of string to any
     |          Parameter names mapped to their values.
     |  
     |  set_params(self, **params)
     |      Set the parameters of this estimator.
     |      
     |      The method works on simple estimators as well as on nested objects
     |      (such as pipelines). The latter have parameters of the form
     |      ``<component>__<parameter>`` so that it's possible to update each
     |      component of a nested object.
     |      
     |      Parameters
     |      ----------
     |      **params : dict
     |          Estimator parameters.
     |      
     |      Returns
     |      -------
     |      self : object
     |          Estimator instance.
    
    

### 2.2 明白的应用——需要理解最基本的知识

**code数学学公式**

就像书面表达音乐的最佳方式是乐谱，那么最能巧妙展示数学特点的就是数学公式。很多人，尤其规划、建筑和景观专业，因为专业本身形象思维的特点，大学多年的图式思维训练，对逻辑思维有所抵触，实际上好的设计规划，除了具有一定的审美意识，空间设计能力，好的逻辑思维会让设计师在空间形式设计能力上亦有所提升，能够感觉到这种空间能力的变化，更别提，建筑大专业本身就是工科体系，是关乎到人们生命安全的严谨的事情。

当开始接触公式时，可能不适应，但是当你逐步的用公式代替所要阐述的方法时， 你会慢慢喜欢上这种方式，即使我们在小学就开始学习公式。

再者，很多时候公式，甚至文字内容让人费解，怎么也读不懂作者所阐述的究竟是什么，尤其包含大部分公式，而案例都是“白纸黑字”的论述时。这是因为我们只是看的到，但是摸不到，你很难去调整参数再实验作者的案例，但是在python等语言的世界中，数据、代码都是实实在在的东西，你可以用print()查看每一步数据的变化，尤其不易理解的地方，一比较前后的数据变化，和所用的什么数据，一切都会迎刃而解。这也是，为什么本书的所有阐述都是基于代码的，公式看不懂，没有问题；文字看不懂，也没有问题，我们只要逐行的运行代码，似乎就会明朗开来，而且尽可能的将数据以图示的方式表述，来增加理解的方式，让问题更容易浮出水面。

[SymPy](https://www.sympy.org/en/index.html)

[简单回归，多元回归](https://richiebao.github.io/Urban-Spatial-Data-Analysis_python/#/./notebook_code/regression)

* 简单线性回归

在统计学中，线性回归（linear regression）是利用称为线性回归方程的最小平方函数对一个或多个自变量和因变量之间关系进行建模的一种回归分析。这种函数是一个或多个称为回归系数的模型参数的线性组合。只有一个自变量的情况称为简单（线性）回归（simple linear regression），大于一个自变量情况的叫多元回归（multivariable linear regression）。

* 回归分析的流程:

1. 为了讨论十分具有求解回归方程的意义，画出自变量和因变量的散点图；
2. 求解回归方程；
3. 确认回归方程的精度；
4. 进行回归系数的检验；
5. 总体回归$Ax+b$的估计；
6. 进行预测

#### 2.2.1 建立数据集
使用'漫画统计学之回归分析'中最高温度（$^{\circ}C$）与冰红茶销售量(杯)的数据，首先建立基于DataFrame格式的数据集，该数据集使用了时间戳（timestamp）作为索引。


```python
import pandas as pd
import util_pyd
from scipy import stats

dt=pd.date_range('2020-07-22', periods=14, freq='D')
dt_temperature_iceTeaSales={"dt":dt,"temperature":[29,28,34,31,25,29,32,31,24,33,25,31,26,30],"iceTeaSales":[77,62,93,84,59,64,80,75,58,91,51,73,65,84]}
iceTea_df=pd.DataFrame(dt_temperature_iceTeaSales).set_index("dt")
util_pyd.print_html(iceTea_df,14)
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>temperature</th>
      <th>iceTeaSales</th>
    </tr>
    <tr>
      <th>dt</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-07-22</th>
      <td>29</td>
      <td>77</td>
    </tr>
    <tr>
      <th>2020-07-23</th>
      <td>28</td>
      <td>62</td>
    </tr>
    <tr>
      <th>2020-07-24</th>
      <td>34</td>
      <td>93</td>
    </tr>
    <tr>
      <th>2020-07-25</th>
      <td>31</td>
      <td>84</td>
    </tr>
    <tr>
      <th>2020-07-26</th>
      <td>25</td>
      <td>59</td>
    </tr>
    <tr>
      <th>2020-07-27</th>
      <td>29</td>
      <td>64</td>
    </tr>
    <tr>
      <th>2020-07-28</th>
      <td>32</td>
      <td>80</td>
    </tr>
    <tr>
      <th>2020-07-29</th>
      <td>31</td>
      <td>75</td>
    </tr>
    <tr>
      <th>2020-07-30</th>
      <td>24</td>
      <td>58</td>
    </tr>
    <tr>
      <th>2020-07-31</th>
      <td>33</td>
      <td>91</td>
    </tr>
    <tr>
      <th>2020-08-01</th>
      <td>25</td>
      <td>51</td>
    </tr>
    <tr>
      <th>2020-08-02</th>
      <td>31</td>
      <td>73</td>
    </tr>
    <tr>
      <th>2020-08-03</th>
      <td>26</td>
      <td>65</td>
    </tr>
    <tr>
      <th>2020-08-04</th>
      <td>30</td>
      <td>84</td>
    </tr>
  </tbody>
</table>



#### 2.2.2求解回归方程
求解回归方程使用了两种方法，一种是逐步计算的方式；另一种是直接使用sklearn库的LinearRegression模型。逐步计算的方式可以更为深入的理解回归模型，而熟悉基本计算过程之后，直接应用sklearn机器学习库中的模型也会对各种参数的配置有个比较清晰的了解。首先计算温度与销量之间的相关系数，确定二者之间存在关联，其p_value=7.661412804450245e-06，小于0.05的显著性水平，确定pearson's r=0.90能够表明二者之间是强相关性。

求解回归方程即是使所有真实值与预测值之差的和为最小，求出a和b，就是所有变量残差`residual`的平方`s_residual`的和`S_residual`为最小。因为温度与销量为线性相关，因此使用一元一次方程式：$y=ax+b$，$x$为自变量温度，$y$为因变量销量，$a$和$b$为回归系数（参数），分别称为斜率（slop）和截距(intercept)，求解a和b的过程，可以使用最小二乘法（least squares method），又称最小平方法，通过最小化误差的平方（残差平方和）寻找数据的最佳函数匹配。为残差平方和：$(−34𝑎−𝑏+93)^{2} +(−33𝑎−𝑏+91)^{2}+(−32𝑎−𝑏+80)^{2}+(−31𝑎−𝑏+73)^{2}+(−31𝑎−𝑏+75)^{2}+(−31𝑎−𝑏+84)^{2}+(−30𝑎−𝑏+84)^{2}+(−29𝑎−𝑏+64)^{2}+(−29𝑎−𝑏+77)^{2}+(−28𝑎−𝑏+62)^{2}+(−26𝑎−𝑏+65)^{2}+(−25𝑎−𝑏+51)^{2}+(−25𝑎−𝑏+59)^{2}+(−24𝑎−𝑏+58)^{2}$， 

先对$a$和$b$分别求微分$\frac{df}{da} $和$\frac{df}{db} $，是$\triangle a$即$a$在横轴上的增量，及$\triangle b$即$b$在横轴上的增量趋近于无穷小，无限接近$a$和$b$时，因变量的变化量，这个因变量就是残差平方和的值。残差平方和的值是由$a$和$b$确定的，当$a$和$b$取不同的值时，残差平方和的值随之变化，当残差平方和的值为0时，说明由自变量温度所有值通过回归方程预测的销量，与真实值的差值之和为0；单个温度值通过回归模型预测的销量与真实值之差则趋于0。在实际计算中，手工推算时，对残差平方和关于$a$和$b$求微分，是对公式进行整理，最终获得求解回归方程回归系数的公式为：$a= \frac{ S_{xy} }{ S_{xx} } $其中$S_{xy}$即变量`SS_xy`是$x$和$y$的离差积，$S_{xx}$即变量`SS_x`是$x$的离差平方和。求得$a$后，可以根据推导公式：$b= \overline{y} - \overline{x} a$计算$b$。

在python语言中，使用相关库则可以避免上述繁琐的手工推导过程，在逐步计算中，使用sympy库约简残差平方和公式为：$12020⋅a^{2}   + 816⋅a⋅b - 60188⋅a + 14⋅b^{2}  - 2032⋅b + 75936$， 并直接分别对$a$和$b$微分，获得结果为：$ \frac{df}{da} =24040⋅a + 816⋅b - 60188$和$ \frac{df}{db} =816⋅a + 28⋅b - 2032$，另二者分别为0，使用sympy库的solve求解二元一次方程组，计算获取$a$和$b$值。

最后使用sklearn库的LinearRegression模型求解决回归模型，仅需要几行代码，所得结果与上述同。可以用sklearn返回的参数，建立回归方程公式，但是在实际的应用中并不会这么做，而是直接应用以变量形式代表的回归模型直接预测值。


```python
import math
import sympy
from sympy import diff,Eq,solveset,solve,simplify,pprint
import matplotlib.pyplot as plt
from matplotlib import cm
import numpy as np

r_=stats.pearsonr(iceTea_df.temperature,iceTea_df.iceTeaSales)
print("_"*50)
print(
    "pearson's r:",r_[0],"\n",
    "p_value:",r_[1]
     )
print("_"*50)

#原始数据散点图
fig, axs=plt.subplots(1,3,figsize=(25,8))
axs[0].plot(iceTea_df.temperature,iceTea_df.iceTeaSales,'o',label='ground truth',color='r')
axs[0].set(xlabel='temperature',ylabel='ice tea sales')


#A - 使用‘最小二乘法’逐步计算
#1 - 求出x和y的离差及离差平方和
iceTea_df["x_deviation"]=iceTea_df.temperature.apply(lambda row: row-iceTea_df.temperature.mean())
iceTea_df["y_deviation"]=iceTea_df.iceTeaSales.apply(lambda row: row-iceTea_df.iceTeaSales.mean())
iceTea_df["S_x_deviation"]=iceTea_df.temperature.apply(lambda row: math.pow(row-iceTea_df.temperature.mean(),2))
iceTea_df["S_y_deviation"]=iceTea_df.iceTeaSales.apply(lambda row: math.pow(row-iceTea_df.iceTeaSales.mean(),2))
SS_x=iceTea_df["S_x_deviation"].sum()
SS_y=iceTea_df["S_y_deviation"].sum()

#2 - 求出x和y的离差积及其其和
iceTea_df["S_xy_deviation"]=iceTea_df.apply(lambda row: (row["temperature"]-iceTea_df.temperature.mean())*(row["iceTeaSales"]-iceTea_df.iceTeaSales.mean()),axis=1)
SS_xy=iceTea_df["S_xy_deviation"].sum()

#3 - 运算过程
a,b=sympy.symbols('a b')
iceTea_df["prediciton"]=iceTea_df.temperature.apply(lambda row: a*row+b)
iceTea_df["residual"]=iceTea_df.apply(lambda row: row.iceTeaSales-(a*row.temperature+b),axis=1)
iceTea_df["s_residual"]=iceTea_df.apply(lambda row: (row.iceTeaSales-(a*row.temperature+b))**2,axis=1)
S_residual=iceTea_df["s_residual"].sum()
S_residual_simplify=simplify(S_residual)
print("S_residual simplification(Binary quadratic equation):")
pprint(S_residual_simplify) #残差平方和为一个二元二次函数
print("_"*50)

#打印残差平方和图形
S_residual_simplif_=sympy.lambdify([a,b],S_residual_simplify,"numpy")
a_=np.arange(-100,100,5)
a_3d=np.repeat(a_[:,np.newaxis],a_.shape[0],axis=1).T
b_=np.arange(-100,100,5)
b_3d=np.repeat(b_[:,np.newaxis],b_.shape[0],axis=1)
z=S_residual_simplif_(a_3d,b_3d)
from sklearn import preprocessing
z_scaled=preprocessing.scale(z) #标准化z值，同 from scipy.stats import zscore方法

axs[1]=fig.add_subplot(1,3,2, projection='3d')
axs[1].plot_wireframe(a_3d,b_3d,z_scaled)
axs[1].contour(a_3d,b_3d,z_scaled, zdir='z', offset=-2, cmap=cm.coolwarm)
axs[1].contour(a_3d,b_3d,z_scaled, zdir='x', offset=-100, cmap=cm.coolwarm)
axs[1].contour(a_3d,b_3d,z_scaled, zdir='y', offset=100, cmap=cm.coolwarm)

#4 - 对残差平方和S_residual关于a和b求微分，并使其为0
diff_S_residual_a=diff(S_residual,a)
diff_S_residual_b=diff(S_residual,b)
print("diff_S_residual_a=",)
pprint(diff_S_residual_a)
print("\n")
print("diff_S_residual_b=",)
pprint(diff_S_residual_b)

Eq_residual_a=Eq(diff_S_residual_a,0) #设所求a微分为0
Eq_residual_b=Eq(diff_S_residual_b,0) #设所求b微分为0
slop_intercept=solve((Eq_residual_a,Eq_residual_b),(a,b)) #计算二元一次方程组
print("_"*50)
print("slop and intercept:\n")
pprint(slop_intercept)
slop=slop_intercept[a]
intercept=slop_intercept[b]

#用求解回归方程回归系数的推导公式之间计算斜率slop和截距intercept
print("_"*50)
slop_=SS_xy/SS_x
print("derivation formula to calculate the slop=",slop_)
intercept_=iceTea_df.iceTeaSales.mean()-iceTea_df.temperature.mean()*slop_
print("derivation formula to calculate the intercept=",intercept_)
print("_"*50)

#5 - 建立简单线性回归方程
x=sympy.Symbol('x')
fx=slop*x+intercept
print("linear regression_fx=:\n")
pprint(fx)
fx_=sympy.lambdify(x,fx,"numpy")

#在残差平方和图形上标出a,b的位置
axs[1].text(slop,intercept,-1.7,"a/b",color="red",size=20)
axs[1].scatter(slop,intercept,-2,color="red",s=80)
axs[1].view_init(60,340) #可以旋转图形的角度，方便观察

#6 - 绘制简单线性回归方程的图形
axs[0].plot(iceTea_df.temperature,fx_(iceTea_df.temperature),'o-',label='prediction',color='blue')

#绘制真实值与预测值之间的连线
i=0
for t in iceTea_df.temperature:
    axs[0].arrow(t, iceTea_df.iceTeaSales[i], t-t, fx_(t)-iceTea_df.iceTeaSales[i], head_width=0.1, head_length=0.1,color="gray",linestyle="--" )
    i+=1

#B - 使用sklearn库sklearn.linear_model.LinearRegression()，Ordinary least squares Linear Regression-普通最小二乘线性回归，获取回归方程
from sklearn.linear_model import LinearRegression
X,y=iceTea_df.temperature.to_numpy().reshape(-1,1),iceTea_df.iceTeaSales.to_numpy()

#拟合模型
LR=LinearRegression().fit(X,y)
#模型参数
print("_"*50)
print("Sklearn slop:%.2f,intercept:%.2f"%(LR.coef_, LR.intercept_))
#模型预测
axs[2].plot(iceTea_df.temperature,iceTea_df.iceTeaSales,'o',label='ground truth',color='r')
axs[2].plot(X,LR.predict(X),'o-',label='linear regression prediction')
axs[2].set(xlabel='temperature',ylabel='ice tea sales')

axs[0].legend(loc='upper left', frameon=False)
axs[2].legend(loc='upper left', frameon=False)

axs[0].set_title('step by step manual calculation')
axs[1].set_title('sum of squares of residuals')
axs[2].set_title('using the Sklearn libray')
plt.show()
util_pyd.print_html(iceTea_df,14)
```

    __________________________________________________
    pearson's r: 0.9069229780508894 
     p_value: 7.661412804450245e-06
    __________________________________________________
    S_residual simplification(Binary quadratic equation):
           2                           2                 
    12020⋅a  + 816⋅a⋅b - 60188⋅a + 14⋅b  - 2032⋅b + 75936
    __________________________________________________
    diff_S_residual_a=
    24040⋅a + 816⋅b - 60188
    
    
    diff_S_residual_b=
    816⋅a + 28⋅b - 2032
    __________________________________________________
    slop and intercept:
    
    ⎧   1697     -8254 ⎫
    ⎨a: ────, b: ──────⎬
    ⎩   454       227  ⎭
    __________________________________________________
    derivation formula to calculate the slop= 3.7378854625550666
    derivation formula to calculate the intercept= -36.361233480176224
    __________________________________________________
    linear regression_fx=:
    
    1697⋅x   8254
    ────── - ────
     454     227 
    __________________________________________________
    Sklearn slop:3.74,intercept:-36.36
    


    
<a href=""><img src="./imgs_pyd/pyd_06.png" height="auto" width="auto" title="caDesign"></a>
    





<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>temperature</th>
      <th>iceTeaSales</th>
      <th>x_deviation</th>
      <th>y_deviation</th>
      <th>S_x_deviation</th>
      <th>S_y_deviation</th>
      <th>S_xy_deviation</th>
      <th>prediciton</th>
      <th>residual</th>
      <th>s_residual</th>
    </tr>
    <tr>
      <th>dt</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-07-22</th>
      <td>29</td>
      <td>77</td>
      <td>-0.142857</td>
      <td>4.428571</td>
      <td>0.020408</td>
      <td>19.612245</td>
      <td>-0.632653</td>
      <td>29*a + b</td>
      <td>-29*a - b + 77</td>
      <td>(-29*a - b + 77)**2</td>
    </tr>
    <tr>
      <th>2020-07-23</th>
      <td>28</td>
      <td>62</td>
      <td>-1.142857</td>
      <td>-10.571429</td>
      <td>1.306122</td>
      <td>111.755102</td>
      <td>12.081633</td>
      <td>28*a + b</td>
      <td>-28*a - b + 62</td>
      <td>(-28*a - b + 62)**2</td>
    </tr>
    <tr>
      <th>2020-07-24</th>
      <td>34</td>
      <td>93</td>
      <td>4.857143</td>
      <td>20.428571</td>
      <td>23.591837</td>
      <td>417.326531</td>
      <td>99.224490</td>
      <td>34*a + b</td>
      <td>-34*a - b + 93</td>
      <td>(-34*a - b + 93)**2</td>
    </tr>
    <tr>
      <th>2020-07-25</th>
      <td>31</td>
      <td>84</td>
      <td>1.857143</td>
      <td>11.428571</td>
      <td>3.448980</td>
      <td>130.612245</td>
      <td>21.224490</td>
      <td>31*a + b</td>
      <td>-31*a - b + 84</td>
      <td>(-31*a - b + 84)**2</td>
    </tr>
    <tr>
      <th>2020-07-26</th>
      <td>25</td>
      <td>59</td>
      <td>-4.142857</td>
      <td>-13.571429</td>
      <td>17.163265</td>
      <td>184.183673</td>
      <td>56.224490</td>
      <td>25*a + b</td>
      <td>-25*a - b + 59</td>
      <td>(-25*a - b + 59)**2</td>
    </tr>
    <tr>
      <th>2020-07-27</th>
      <td>29</td>
      <td>64</td>
      <td>-0.142857</td>
      <td>-8.571429</td>
      <td>0.020408</td>
      <td>73.469388</td>
      <td>1.224490</td>
      <td>29*a + b</td>
      <td>-29*a - b + 64</td>
      <td>(-29*a - b + 64)**2</td>
    </tr>
    <tr>
      <th>2020-07-28</th>
      <td>32</td>
      <td>80</td>
      <td>2.857143</td>
      <td>7.428571</td>
      <td>8.163265</td>
      <td>55.183673</td>
      <td>21.224490</td>
      <td>32*a + b</td>
      <td>-32*a - b + 80</td>
      <td>(-32*a - b + 80)**2</td>
    </tr>
    <tr>
      <th>2020-07-29</th>
      <td>31</td>
      <td>75</td>
      <td>1.857143</td>
      <td>2.428571</td>
      <td>3.448980</td>
      <td>5.897959</td>
      <td>4.510204</td>
      <td>31*a + b</td>
      <td>-31*a - b + 75</td>
      <td>(-31*a - b + 75)**2</td>
    </tr>
    <tr>
      <th>2020-07-30</th>
      <td>24</td>
      <td>58</td>
      <td>-5.142857</td>
      <td>-14.571429</td>
      <td>26.448980</td>
      <td>212.326531</td>
      <td>74.938776</td>
      <td>24*a + b</td>
      <td>-24*a - b + 58</td>
      <td>(-24*a - b + 58)**2</td>
    </tr>
    <tr>
      <th>2020-07-31</th>
      <td>33</td>
      <td>91</td>
      <td>3.857143</td>
      <td>18.428571</td>
      <td>14.877551</td>
      <td>339.612245</td>
      <td>71.081633</td>
      <td>33*a + b</td>
      <td>-33*a - b + 91</td>
      <td>(-33*a - b + 91)**2</td>
    </tr>
    <tr>
      <th>2020-08-01</th>
      <td>25</td>
      <td>51</td>
      <td>-4.142857</td>
      <td>-21.571429</td>
      <td>17.163265</td>
      <td>465.326531</td>
      <td>89.367347</td>
      <td>25*a + b</td>
      <td>-25*a - b + 51</td>
      <td>(-25*a - b + 51)**2</td>
    </tr>
    <tr>
      <th>2020-08-02</th>
      <td>31</td>
      <td>73</td>
      <td>1.857143</td>
      <td>0.428571</td>
      <td>3.448980</td>
      <td>0.183673</td>
      <td>0.795918</td>
      <td>31*a + b</td>
      <td>-31*a - b + 73</td>
      <td>(-31*a - b + 73)**2</td>
    </tr>
    <tr>
      <th>2020-08-03</th>
      <td>26</td>
      <td>65</td>
      <td>-3.142857</td>
      <td>-7.571429</td>
      <td>9.877551</td>
      <td>57.326531</td>
      <td>23.795918</td>
      <td>26*a + b</td>
      <td>-26*a - b + 65</td>
      <td>(-26*a - b + 65)**2</td>
    </tr>
    <tr>
      <th>2020-08-04</th>
      <td>30</td>
      <td>84</td>
      <td>0.857143</td>
      <td>11.428571</td>
      <td>0.734694</td>
      <td>130.612245</td>
      <td>9.795918</td>
      <td>30*a + b</td>
      <td>-30*a - b + 84</td>
      <td>(-30*a - b + 84)**2</td>
    </tr>
  </tbody>
</table>



#### 2.2.3 确认回归方程的精度
确认回归方程（模型）的精度是计算判断系数（决定系数，coefficient of determination），记为$R^{2} $或$r^{2} $，用于表示实测值（图表中的点）与回归方程拟合程度的指标。其复(重)相关系数计算公式为：$R=   \frac{\sum_{i=1}^n  ( y_{i} - \overline{y} )^{2} ( \widehat{y}_{i} - \overline{ \widehat{y} }  )^{2}  }{ \sqrt{(\sum_{i=1}^n (y_{i}- \overline{y} )^{2} )(\sum_{i=1}^n ( \widehat{y}_{i} - \overline{ \widehat{y} })^{2} )} } $，其中$y$为观测值，$\overline{y}$为观测值的均值，$\widehat{y}$为预测值，$\overline{ \widehat{y} } $为预测值的均值。而判定系数$R^{2} $则为重相关系数的平方。判定系数的取值在0到1，其值越接近于1，回归方程的精度越高。第二种计算公式为：$R^{2} =1- \frac{ SS_{res} }{ SS_{tot} }=1- \frac{ \sum_{i=1}^n   e_{i} ^{2}  }{SS_{tot}}  =1- \frac{  \sum_{i=1}^n  (y_{i} -   \widehat{y} _{i} )^{2}  }{ \sum_{i=1}^n  ( y_{i} - \overline{y} )^{2}  } $，其中$SS_{res}$为残差平方和，$SS_{tot}$为观测值离差平方和（(总平方和，或总的离差平方和)），$e_{i}$为残差，$y_{i}$为观测值，$\widehat{y}$为预测值，$\overline{y}$为观测值均值。第三种是直接使用sklearn库提供的`r2_score`方法直接计算。

根据计算结果第1，2，3种方法结果一致。在后续的实验中，直接使用sklearn提供的方法进行计算。


```python
def coefficient_of_determination(observed_vals,predicted_vals):
    import pandas as pd
    import numpy as np
    import math
    '''
    function - 回归方程的判定系数
    
    Paras:
    observed_vals - 观测值（实测值）
    predicted_vals - 预测值
    '''
    vals_df=pd.DataFrame({'obs':observed_vals,'pre':predicted_vals})
    #观测值的离差平方和(总平方和，或总的离差平方和)
    obs_mean=vals_df.obs.mean()
    SS_tot=vals_df.obs.apply(lambda row:(row-obs_mean)**2).sum()
    #预测值的离差平方和
    pre_mean=vals_df.pre.mean()
    SS_reg=vals_df.pre.apply(lambda row:(row-pre_mean)**2).sum()
    #观测值和预测值的离差积和
    SS_obs_pre=vals_df.apply(lambda row:(row.obs-obs_mean)*(row.pre-pre_mean), axis=1).sum()
    
    #残差平方和
    SS_res=vals_df.apply(lambda row:(row.obs-row.pre)**2,axis=1).sum()
    
    #判断系数
    R_square_a=(SS_obs_pre/math.sqrt(SS_tot*SS_reg))**2
    R_square_b=1-SS_res/SS_tot
            
    return R_square_a,R_square_b
    
R_square_a,R_square_b=coefficient_of_determination(iceTea_df.iceTeaSales.to_list(),fx_(iceTea_df.temperature).to_list())   
print("R_square_a=%.5f,R_square_b=%.5f"%(R_square_a,R_square_b))

from sklearn.metrics import r2_score
R_square_=r2_score(iceTea_df.iceTeaSales.to_list(),fx_(iceTea_df.temperature).to_list())
print("using sklearn libray to calculate r2_score=",R_square_)
```

    R_square_a=0.82251,R_square_b=0.82251
    using sklearn libray to calculate r2_score= 0.8225092881166944
    

## [Numpy](https://numpy.org/)

关于numpy.random.seed()问题：

1. 如果使用相同的seed( )值，则每次生成的随即数都相同； 
2. 如果不设置这个值，则系统根据时间来自己选择这个值，此时每次生成的随机数因时间差异而不同；
3. 设置的seed()值仅一次有效。

### 引例


```python
import numpy as np
num=0
print('随机种子仅一次有效,因此随机结果因时间差异而不同\t')
while(num<5):
    print(np.random.random())
    num+=1
print('为了获取相同的随机数结果，每次均需要调用seed种子\t')
while(num<10):
    np.random.seed(19680801) 
    print(np.random.random())
    num+=1
```

    随机种子仅一次有效,因此随机结果因时间差异而不同	
    0.9901802350232211
    0.0609770912940687
    0.003735980452566845
    0.8125032132485621
    0.5827367203472397
    为了获取相同的随机数结果，每次均需要调用seed种子	
    0.7003673039391197
    0.7003673039391197
    0.7003673039391197
    0.7003673039391197
    0.7003673039391197
    


```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

np.random.seed(19680801)  #固定随机状态，确保每次运行后输出结果相同

points=np.random.random((2,20,3)) #秩(rank)为3的的多维数组，即3轴(axes)，0轴第1个维度长度为2，1轴第2个维度长度为20，2轴第3个维度长度为3。
#print(points)
#points[1,:,2]+=1  #0轴索引值为1项，1轴全部，2轴索引值为2项的值均加1.
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
colors=('b','r','g','c','m','y','k','w')
markers=('o','^','+','*','v','x','_','.')
for i in range(points.shape[0]):  #循环0轴
    #print(points.reshape(-1,3),points)
    #print(points[i,:,0],points[i,:,1],points[i,:,2])
    #print(points[i])
    ax.scatter(points[i,:,0],points[i,:,1],points[i,:,2], c=points[i], marker=markers[i])
ax.set_xlabel('X coordi')
ax.set_ylabel('Y coordi')
ax.set_zlabel('Z coordi')
fig.set_figheight(12)
fig.set_figwidth(12)
plt.show()
```


    
<a href=""><img src="./imgs_pyd/pyd_07.png" height="auto" width="auto" title="caDesign"></a>
    


<a href=""><img src="./imgs_pyd/036.png" height="auto" width="auto" title="caDesign"></a>


```python
print("points.ndim->{}".format(points.ndim))
print("_"*50)
print("points.shape->{}".format(points.shape))
print("_"*50)
print("points.size->{}".format(points.size))
print("_"*50)
print("points.dtype->{}".format(points.dtype))
print("_"*50)
print("points.itemsize->{}".format(points.itemsize))
```

    points.ndim->3
    __________________________________________________
    points.shape->(2, 20, 3)
    __________________________________________________
    points.size->120
    __________________________________________________
    points.dtype->float64
    __________________________________________________
    points.itemsize->8
    


```python
points.reshape(2,4,5,3)
```




    array([[[[0.7003673 , 0.74275081, 0.70928001],
             [0.56674552, 0.97778533, 0.70633485],
             [0.24791576, 0.15788335, 0.69769852],
             [0.71995667, 0.25774443, 0.34154678],
             [0.96876117, 0.6945071 , 0.46638326]],
    
            [[0.7028127 , 0.51178587, 0.92874137],
             [0.7397693 , 0.62243903, 0.65154547],
             [0.39680761, 0.54323939, 0.79989953],
             [0.72154473, 0.29536398, 0.16094588],
             [0.20612551, 0.13432539, 0.48060502]],
    
            [[0.34252181, 0.36296929, 0.97291764],
             [0.11094361, 0.38826409, 0.78306588],
             [0.97289726, 0.48320961, 0.33642111],
             [0.56741904, 0.04794151, 0.38893703],
             [0.90630365, 0.16101821, 0.74362113]],
    
            [[0.63297416, 0.32418002, 0.92237653],
             [0.23722644, 0.82394557, 0.75060714],
             [0.11378445, 0.84536125, 0.92393213],
             [0.22083679, 0.93305388, 0.48899874],
             [0.47471864, 0.08916747, 0.22994818]]],
    
    
           [[[0.71593741, 0.49612616, 0.76648938],
             [0.89679732, 0.77222302, 0.92717429],
             [0.61465203, 0.60906377, 0.68468487],
             [0.25101297, 0.83783764, 0.11861562],
             [0.79723474, 0.94900427, 0.14806288]],
    
            [[0.90687198, 0.78837333, 0.76840584],
             [0.59849648, 0.44214562, 0.72303802],
             [0.41661825, 0.2268104 , 0.45422734],
             [0.84794375, 0.93665595, 0.95603618],
             [0.39209432, 0.70832467, 0.12951583]],
    
            [[0.35379639, 0.40427152, 0.6485339 ],
             [0.03307097, 0.53800936, 0.13171312],
             [0.52093493, 0.10248479, 0.15798038],
             [0.92002965, 0.78422978, 0.39704754],
             [0.15464683, 0.72119325, 0.8368309 ]],
    
            [[0.9378552 , 0.57671382, 0.19502656],
             [0.11779397, 0.9506002 , 0.2836723 ],
             [0.95656242, 0.90942546, 0.32842519],
             [0.95176557, 0.11632148, 0.87431415],
             [0.98522547, 0.75033538, 0.98515647]]]])



<a href=""><img src="./imgs_pyd/037.jpg" height="auto" width="500" title="caDesign"></a>
<a href=""><img src="./imgs_pyd/038.png" height="auto" width="auto" title="caDesign"></a>
<a href=""><img src="./imgs_pyd/040.png" height="auto" width="auto" title="caDesign"></a>

### 创建数组

#### np.array( )

* list和tuple


```python
listArray_1=np.array([9,8,7,6,5])
print("listArray_1->{}".format(listArray_1))
print("_"*50)
listArray_2=np.array([[9,8,7],[6,5,4],[3,2,1]])
print("listArray_2->{}".format(listArray_2))
print("_"*50)
tupleArray=np.array((9,8,7,6,5))
print("tupleArray->{}".format(tupleArray))
```

    listArray_1->[9 8 7 6 5]
    __________________________________________________
    listArray_2->[[9 8 7]
     [6 5 4]
     [3 2 1]]
    __________________________________________________
    tupleArray->[9 8 7 6 5]
    

#### numpy内嵌

* (开始值，终值，步长) `np.arange(start,end,step)`


```python
np.arange(0,1,0.2)
```




    array([0. , 0.2, 0.4, 0.6, 0.8])



* (开始值，终值，数量) `np.linspace(start,end,count)`


```python
np.linspace(0,1,10)
```




    array([0.        , 0.11111111, 0.22222222, 0.33333333, 0.44444444,
           0.55555556, 0.66666667, 0.77777778, 0.88888889, 1.        ])



* This is particularly useful for evaluating functions of multiple dimensions on a regular grid. `np.indices((v1,v2,...vn))`


```python
np.indices((3,3))
```




    array([[[0, 0, 0],
            [1, 1, 1],
            [2, 2, 2]],
    
           [[0, 1, 2],
            [0, 1, 2],
            [0, 1, 2]]])



* 等比数列，开始值10^startPower，终值10^endPower，数量 `np.logspace(startPower,endPower,count)`


```python
np.logspace(0,3,10)
```




    array([   1.        ,    2.15443469,    4.64158883,   10.        ,
             21.5443469 ,   46.41588834,  100.        ,  215.443469  ,
            464.15888336, 1000.        ])



#### 从字符串

* dtype

\_ data type/数据类型

<a href=""><img src="./imgs_pyd/041.png" height="auto" width="600" title="caDesign"></a>

\_ 位置

查看


```python
array=np.linspace(10,20,9)
print("array.dtype->{}".format(array.dtype))
print("_"*50)
print("array.dtype.name->{}".format(array.dtype.name))
```

    array.dtype->float64
    __________________________________________________
    array.dtype.name->float64
    

定义时


```python
array=np.linspace(10,20,9,dtype=int)
print("array->{}".format(array))
print("_"*50)
print("array.dtype->{}".format(array.dtype))
```

    array->[10 11 12 13 15 16 17 18 20]
    __________________________________________________
    array.dtype->int32
    

* np.fromstring(s,dtype=?)


```python
s=r'abcdefgh'
np.fromstring(s,dtype=np.int8)
```

    C:\Users\richi\AppData\Local\Temp/ipykernel_980/3655350550.py:2: DeprecationWarning: The binary mode of fromstring is deprecated, as it behaves surprisingly on unicode inputs. Use frombuffer instead
      np.fromstring(s,dtype=np.int8)
    




    array([ 97,  98,  99, 100, 101, 102, 103, 104], dtype=int8)



* np.genfromtxt(BytesIO(string.encode(),delimeter=" ",...))


```python
import io

string="7天连锁酒店(西安东大街建国路口店),108.96238170549796,34.25976353340253,建国路111号奥达大厦1层"
dataArray=np.genfromtxt(io.BytesIO(string.encode()),delimiter=",",autostrip=True)
dataArray
```




    array([         nan, 108.96238171,  34.25976353,          nan])



#### 占位数组

* np.zeros(())


```python
np.zeros((3,4))
```




    array([[0., 0., 0., 0.],
           [0., 0., 0., 0.],
           [0., 0., 0., 0.]])



* np.ones(())


```python
np.ones((2,3,4),dtype=np.int16)
```




    array([[[1, 1, 1, 1],
            [1, 1, 1, 1],
            [1, 1, 1, 1]],
    
           [[1, 1, 1, 1],
            [1, 1, 1, 1],
            [1, 1, 1, 1]]], dtype=int16)



* np.empty(())


```python
np.empty((2,3),dtype=float)
```




    array([[0., 0., 0.],
           [1., 1., 0.]])



### 索引

> 与python类似

1. A-索引+分片
2. B-数组索引
3. C-布尔索引

<a href=""><img src="./imgs_pyd/039.jpg" height="auto" width="1200" title="caDesign"></a>

### 形状shape

* array.reshape()


```python
array=np.arange(0,20,2)

print("array->{}".format(array))
print("_"*50)
print("array.shape->{}".format(array.shape))
print("_"*50)
print("array.reshape(2,5)->{}".format(array.reshape(2,5)))
print("_"*50)
print("array.reshape(3,-1)->{}".format(array.reshape(3,-1)))
```

    array->[ 0  2  4  6  8 10 12 14 16 18]
    __________________________________________________
    array.shape->(10,)
    __________________________________________________
    array.reshape(2,5)->[[ 0  2  4  6  8]
     [10 12 14 16 18]]
    __________________________________________________
    


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_980/67734161.py in <module>
          7 print("array.reshape(2,5)->{}".format(array.reshape(2,5)))
          8 print("_"*50)
    ----> 9 print("array.reshape(3,-1)->{}".format(array.reshape(3,-1)))
    

    ValueError: cannot reshape array of size 10 into shape (3,newaxis)



```python
print("array.reshape(5,-1)->{}".format(array.reshape(5,-1)))
print("_"*50)
print("array.reshape(-1,2)->{}".format(array.reshape(-1,2)))
```

    array.reshape(5,-1)->[[ 0  2]
     [ 4  6]
     [ 8 10]
     [12 14]
     [16 18]]
    __________________________________________________
    array.reshape(-1,2)->[[ 0  2]
     [ 4  6]
     [ 8 10]
     [12 14]
     [16 18]]
    

### 基本运算

不指定轴，默认条件下，按元素运算


```python
aArray=np.arange(0,12,2)
bArray=np.arange(10,22,2)
print("aArray->{}".format(aArray))
print("_"*50)
print("bArray->{}".format(bArray))
print("_"*50)
print("aArray+bArray->{}".format(aArray+bArray))
print("_"*50)
print("aArray-bArray->{}".format(aArray-bArray))
print("_"*50)
print("aArray**2->{}".format(aArray**2))
print("_"*50)
print("aArray>4->{}".format(aArray>4))
aArrayR=aArray.reshape(2,3)
bArrayR=bArray.reshape(2,3)
print("_"*50)
print("aArrayR->{}".format(aArrayR))
print("_"*50)
print("bArrayR->{}".format(bArrayR))
print("_"*50)
print("aArrayR*bArrayR->{}".format(aArrayR*bArrayR))
print("_"*50)
#print("aArrayR*=2->{}".format(aArrayR*=2))
print("_"*50)
#print("bArrayR+=aArrayR->{}".format(bArrayR+=aArrayR))
print("_"*50)
print("bArrayR.sum()->{}".format(bArrayR.sum()))
```

    aArray->[ 0  2  4  6  8 10]
    __________________________________________________
    bArray->[10 12 14 16 18 20]
    __________________________________________________
    aArray+bArray->[10 14 18 22 26 30]
    __________________________________________________
    aArray-bArray->[-10 -10 -10 -10 -10 -10]
    __________________________________________________
    aArray**2->[  0   4  16  36  64 100]
    __________________________________________________
    aArray>4->[False False False  True  True  True]
    __________________________________________________
    aArrayR->[[ 0  2  4]
     [ 6  8 10]]
    __________________________________________________
    bArrayR->[[10 12 14]
     [16 18 20]]
    __________________________________________________
    aArrayR*bArrayR->[[  0  24  56]
     [ 96 144 200]]
    __________________________________________________
    __________________________________________________
    __________________________________________________
    bArrayR.sum()->90
    

#### 指定轴的运算


```python
print("aArrayR.sum(axis=0)->{}".format(aArrayR.sum(axis=0)))
print("_"*50)
print("aArrayR.sum(axis=1)->{}".format(aArrayR.sum(axis=1)))
print("_"*50)
print("aArrayR.max(axis=0)->{}".format(aArrayR.sum(axis=0)))
print("_"*50)
print("aArrayR.max(axis=1)->{}".format(aArrayR.sum(axis=1)))
```

    aArrayR.sum(axis=0)->[ 6 10 14]
    __________________________________________________
    aArrayR.sum(axis=1)->[ 6 24]
    __________________________________________________
    aArrayR.max(axis=0)->[ 6 10 14]
    __________________________________________________
    aArrayR.max(axis=1)->[ 6 24]
    

#### 通用函数/ufunc(universal function)


```python
print("aArray->{}".format(aArray))
print("_"*50)
print("bArray->{}".format(bArray))
print("_"*50)
print("np.exp(aArray)->{}".format(np.exp(aArray)))
print("_"*50)
print("np.sqrt(bArray)->{}".format(np.sqrt(bArray)))
print("_"*50)
print("np.add(aArray,bArray)->{}".format(np.add(aArray,bArray)))
```

    aArray->[ 0  2  4  6  8 10]
    __________________________________________________
    bArray->[10 12 14 16 18 20]
    __________________________________________________
    np.exp(aArray)->[1.00000000e+00 7.38905610e+00 5.45981500e+01 4.03428793e+02
     2.98095799e+03 2.20264658e+04]
    __________________________________________________
    np.sqrt(bArray)->[3.16227766 3.46410162 3.74165739 4.         4.24264069 4.47213595]
    __________________________________________________
    np.add(aArray,bArray)->[10 14 18 22 26 30]
    

| A            | B                                           |
|--------------|---------------------------------------------|
| y = x1 + x2  | add(x1, x2 [, y])                           |
| y = x1 - x2  | subtract(x1, x2 [, y])                      |
| y = x1 * x2  | multiply (x1, x2 [, y])                     |
| y = x1 / x2  | divide (x1, x2 [, y]), 如果两个数组的元素为整数，那么用整数除法 |
| y = x1 / x2  | true divide (x1, x2 [, y]), 总是返回精确的商        |
| y = x1 // x2 | floor divide (x1, x2 [, y]), 总是对返回值取整       |
| y = -x       | negative(x [,y])                            |
| y = x1 ** x2 | power(x1, x2 [, y])                         |
| y = x1 % x2  | remainder(x1, x2 [, y]), mod(x1, x2, [, y]) |

* 广播

当我们使用ufunc函数对两个数组进行计算时，ufunc函数会对这两个数组的对应元素进行计算，因此它要求这两个数组有相同的大小(shape相同)。如果两个数组的shape不同的话，会进行如下的广播(broadcasting)处理：

1. 让所有输入数组都向其中shape最长的数组看齐，shape中不足的部分都通过在前面加1补齐
2. 输出数组的shape是输入数组shape的各个轴上的最大值
3. 如果输入数组的某个轴和输出数组的对应轴的长度相同或者其长度为1时，这个数组能够用来计算，否则出错
4. 当输入数组的某个轴的长度为1时，沿着此轴运算时都用此轴上的第一组值 


```python
a=np.arange(0,12,2).reshape(-1,1)
b=np.arange(0,4)
c=a+b
print("a.shape->{},b.shape->{},c.shape->{}".format(a.shape,b.shape,c.shape))
print("_"*50)
print("a->{}".format(a))
print("_"*50)
print("b->{}".format(b))
print("_"*50)
print("c->{}".format(c))
```

    a.shape->(6, 1),b.shape->(4,),c.shape->(6, 4)
    __________________________________________________
    a->[[ 0]
     [ 2]
     [ 4]
     [ 6]
     [ 8]
     [10]]
    __________________________________________________
    b->[0 1 2 3]
    __________________________________________________
    c->[[ 0  1  2  3]
     [ 2  3  4  5]
     [ 4  5  6  7]
     [ 6  7  8  9]
     [ 8  9 10 11]
     [10 11 12 13]]
    

a和b的shape长度不同，根据规则1，b的shape向a对齐，于是b的shape补齐为(1,5)。当调整b后，shape分别为(6,1)和(1,4)，根据规则2，输出数组为各轴最大值即(6,5)。当输入数组某个轴的长度为1时，根据规则3，沿此轴运算时，都用此轴的第1组值。

\_ ogrid对象

因为常用此种广播计算，因此numpy提供了一种快速产生上述数组的方法，即ogrid对象。

1.开始值:结束值:步长

2.开始值:结束值:长度j，当第三个参数为虚数时，它表示返回的数组的长度


```python
a,b=np.ogrid[0:10:2,0:10:2]
c,d=np.ogrid[0:10:4j,0:10:5j]
print("a->{}".format(a))
print("_"*50)
print("b->{}".format(b))
print("_"*50)
print("c->{}".format(c))
print("_"*50)
print("d->{}".format(d))
```

    a->[[0]
     [2]
     [4]
     [6]
     [8]]
    __________________________________________________
    b->[[0 2 4 6 8]]
    __________________________________________________
    c->[[ 0.        ]
     [ 3.33333333]
     [ 6.66666667]
     [10.        ]]
    __________________________________________________
    d->[[ 0.   2.5  5.   7.5 10. ]]
    

* ufunc的方法

\_ reduce方法

<op>.reduce (array=, axis=0, dtype=None) 沿着axis轴对array进行操作，将<op>运算符插入到沿axis轴的所有子数组或者元素当中


```python
np.add([1,2,3])
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_980/1802363341.py in <module>
    ----> 1 np.add([1,2,3])
    

    ValueError: invalid number of arguments



```python
print("np.add.reduce([1,2,3])->{}".format(np.add.reduce([1,2,3])))
print("_"*50)
print("np.add.reduce([[1,2,3],[4,5,6]],axis=1)->{}".format(np.add.reduce([[1,2,3],[4,5,6]],axis=1)))
```

    np.add.reduce([1,2,3])->6
    __________________________________________________
    np.add.reduce([[1,2,3],[4,5,6]],axis=1)->[ 6 15]
    

\_ accumulate 方法

累加计算，包括所有中间结果


```python
print("np.add.accumulate([1,2,3])->{}".format(np.add.accumulate([1,2,3])))
print("_"*50)
print("np.add.accumulate([[1,2,3],[4,5,6]],axis=1)->{}".format(np.add.accumulate([[1,2,3],[4,5,6]],axis=1)))
```

    np.add.accumulate([1,2,3])->[1 3 6]
    __________________________________________________
    np.add.accumulate([[1,2,3],[4,5,6]],axis=1)->[[ 1  3  6]
     [ 4  9 15]]
    

\_ reduceat方法

reduceat方法通过indices参数（列表形式）指定一系列插入reduce的起始和终止位置，规则：如果indice中某元素小于其后元素，则相应结果为对以这两个元素为位置产生的slice里的数组元素进行reduce；

否则结果是这个元素对应的数组元素；

对于最后一个元素，因为其后再没元素，结果为对[该元素:]进行reduce。


```python
a=np.array([1, 2, 3, 4])
print("a->{}".format(a))
print("_"*50)
print("np.add.reduceat(a,indices=[1,3,0,2,3,0,2])->{}".format(np.add.reduceat(a,indices=[1,3,0,2,3,0,2])))
```

    a->[1 2 3 4]
    __________________________________________________
    np.add.reduceat(a,indices=[1,3,0,2,3,0,2])->[5 4 3 3 4 3 7]
    

\_ outer方法

按一维数组进行计算，如果传入参数是多维数组，则先将此数组展平为一维数组之后再进行运算，结果是列向量和行向量的矩阵乘积。广播方式计算


```python
np.multiply.outer([1,2,3,4,5,6],[2,3,4])
```




    array([[ 2,  3,  4],
           [ 4,  6,  8],
           [ 6,  9, 12],
           [ 8, 12, 16],
           [10, 15, 20],
           [12, 18, 24]])



* np.ix_()函数


```python
a=np.array([12, 13, 14, 15])
b=np.array([2, 5, 1])
c=np.array([21, 22, 23, 25, 28])

ax,bx,cx=np.ix_(a,b,c)
print("ax->{}".format(ax))
print("_"*50)
print("bx->{}".format(bx))
print("_"*50)
print("cx->{}".format(cx))
print("_"*50)
print("bx*cx->{}".format(bx*cx))

result=ax+bx*cx
print("_"*50)
print("ax+bx*cx->{}".format(result))
print("_"*50)
print("result.shape->{}".format(result.shape))
```

    ax->[[[12]]
    
     [[13]]
    
     [[14]]
    
     [[15]]]
    __________________________________________________
    bx->[[[2]
      [5]
      [1]]]
    __________________________________________________
    cx->[[[21 22 23 25 28]]]
    __________________________________________________
    bx*cx->[[[ 42  44  46  50  56]
      [105 110 115 125 140]
      [ 21  22  23  25  28]]]
    __________________________________________________
    ax+bx*cx->[[[ 54  56  58  62  68]
      [117 122 127 137 152]
      [ 33  34  35  37  40]]
    
     [[ 55  57  59  63  69]
      [118 123 128 138 153]
      [ 34  35  36  38  41]]
    
     [[ 56  58  60  64  70]
      [119 124 129 139 154]
      [ 35  36  37  39  42]]
    
     [[ 57  59  61  65  71]
      [120 125 130 140 155]
      [ 36  37  38  40  43]]]
    __________________________________________________
    result.shape->(4, 3, 5)
    

<a href=""><img src="./imgs_pyd/042.png" height="auto" width="1200" title="caDesign"></a>

\_ 自定义函数

`numpy.ufunc.identity`

The identity value.

Data attribute containing the identity element for the ufunc, if it has one. If it does not, the attribute value is None.


```python
import numpy as np

def ix_cal(ufct,*vectors):
    vs=np.ix_(*vectors)
    r=ufct.identity
    print(ufct.identity,vs)
    for v in vs:
        r=ufct(r,v)
    return r

a=np.array([12,13,14,15])
b=np.array([2,5,1])
c=np.array([21,22,23,25,28])
result=ix_cal(np.add,a,b,c)
print("_"*50)
print(result)
```

    0 (array([[[12]],
    
           [[13]],
    
           [[14]],
    
           [[15]]]), array([[[2],
            [5],
            [1]]]), array([[[21, 22, 23, 25, 28]]]))
    __________________________________________________
    [[[35 36 37 39 42]
      [38 39 40 42 45]
      [34 35 36 38 41]]
    
     [[36 37 38 40 43]
      [39 40 41 43 46]
      [35 36 37 39 42]]
    
     [[37 38 39 41 44]
      [40 41 42 44 47]
      [36 37 38 40 43]]
    
     [[38 39 40 42 45]
      [41 42 43 45 48]
      [37 38 39 41 44]]]
    

### 数组的组合与分割 

#### 组合

* 水平组合


```python
print("aArrayR->{}".format(aArrayR))
print("_"*50)
print("bArrayR->{}".format(bArrayR))
print("_"*50)
print("np.hstack((aArrayR,bArrayR))->{}".format(np.hstack((aArrayR,bArrayR))))
print("_"*50)
print("np.concatenate((aArrayR,bArrayR),axis=1)->{}".format(np.concatenate((aArrayR,bArrayR),axis=1)))
```

    aArrayR->[[ 0  2  4]
     [ 6  8 10]]
    __________________________________________________
    bArrayR->[[10 12 14]
     [16 18 20]]
    __________________________________________________
    np.hstack((aArrayR,bArrayR))->[[ 0  2  4 10 12 14]
     [ 6  8 10 16 18 20]]
    __________________________________________________
    np.concatenate((aArrayR,bArrayR),axis=1)->[[ 0  2  4 10 12 14]
     [ 6  8 10 16 18 20]]
    

* 垂直组合


```python
print("np.vstack((aArrayR,bArrayR))->{}".format(np.vstack((aArrayR,bArrayR))))
print("_"*50)
print("np.concatenate((aArrayR,bArrayR),axis=0)->{}".format(np.concatenate((aArrayR,bArrayR),axis=0)))
```

    np.vstack((aArrayR,bArrayR))->[[ 0  2  4]
     [ 6  8 10]
     [10 12 14]
     [16 18 20]]
    __________________________________________________
    np.concatenate((aArrayR,bArrayR),axis=0)->[[ 0  2  4]
     [ 6  8 10]
     [10 12 14]
     [16 18 20]]
    

* 深度组合

将一系列数组沿纵轴(深度)方向层叠组合


```python
np.dstack((aArrayR,bArrayR))
```




    array([[[ 0, 10],
            [ 2, 12],
            [ 4, 14]],
    
           [[ 6, 16],
            [ 8, 18],
            [10, 20]]])



* 列组合


```python
aArray=np.array([ 0,  2,  4,  6,  8, 10])
bArray=np.array([10, 12, 14, 16, 18, 20])

print("aArray->{}".format(aArray))
print("_"*50)
print("bArray->{}".format(bArray))
print("_"*50)
print("np.column_stack((aArray,bArray))->{}".format(np.column_stack((aArray,bArray))))
print("_"*50)
print("aArrayR->{}".format(aArrayR))
print("_"*50)
print("bArrayR->{}".format(bArrayR))
print("np.column_stack((aArrayR,bArrayR))->{}".format(np.column_stack((aArrayR,bArrayR))))
```

    aArray->[ 0  2  4  6  8 10]
    __________________________________________________
    bArray->[10 12 14 16 18 20]
    __________________________________________________
    np.column_stack((aArray,bArray))->[[ 0 10]
     [ 2 12]
     [ 4 14]
     [ 6 16]
     [ 8 18]
     [10 20]]
    __________________________________________________
    aArrayR->[[ 0  2  4]
     [ 6  8 10]]
    __________________________________________________
    bArrayR->[[10 12 14]
     [16 18 20]]
    np.column_stack((aArrayR,bArrayR))->[[ 0  2  4 10 12 14]
     [ 6  8 10 16 18 20]]
    

* 行组合


```python
print("np.row_stack((aArray,bArray))->{}".format(np.row_stack((aArray,bArray))))
print("_"*50)
print("np.row_stack((aArrayR,bArrayR))->{}".format(np.row_stack((aArrayR,bArrayR))))
```

    np.row_stack((aArray,bArray))->[[ 0  2  4  6  8 10]
     [10 12 14 16 18 20]]
    __________________________________________________
    np.row_stack((aArrayR,bArrayR))->[[ 0  2  4]
     [ 6  8 10]
     [10 12 14]
     [16 18 20]]
    

#### 分割

* 水平分割


```python
print("np.hsplit(aArray,2)->{}".format(np.hsplit(aArray,2)))
print("_"*50)
print("np.split(aArrayR,3,axis=1)->{}".format(np.split(aArrayR,3,axis=1)))
```

    np.hsplit(aArray,2)->[array([0, 2, 4]), array([ 6,  8, 10])]
    __________________________________________________
    np.split(aArrayR,3,axis=1)->[array([[0],
           [6]]), array([[2],
           [8]]), array([[ 4],
           [10]])]
    

* 垂直分割


```python
cArrayR=np.arange(0,20).reshape(4,5)
print("cArrayR->{}".format(cArrayR))
print("_"*50)
print("np.vsplit(cArrayR,2)->{}".format(np.vsplit(cArrayR,2)))
print("_"*50)
print("np.split(cArrayR,2,axis=0)->{}".format(np.split(cArrayR,2,axis=0)))
```

    cArrayR->[[ 0  1  2  3  4]
     [ 5  6  7  8  9]
     [10 11 12 13 14]
     [15 16 17 18 19]]
    __________________________________________________
    np.vsplit(cArrayR,2)->[array([[0, 1, 2, 3, 4],
           [5, 6, 7, 8, 9]]), array([[10, 11, 12, 13, 14],
           [15, 16, 17, 18, 19]])]
    __________________________________________________
    np.split(cArrayR,2,axis=0)->[array([[0, 1, 2, 3, 4],
           [5, 6, 7, 8, 9]]), array([[10, 11, 12, 13, 14],
           [15, 16, 17, 18, 19]])]
    

* 深度分割


```python
dArray=np.arange(0,27).reshape(3,3,3)
print("dArray->{}".format(dArray))
print("_"*50)
print("np.dsplit(dArray,3)->{}".format(np.dsplit(dArray,3)))
```

    dArray->[[[ 0  1  2]
      [ 3  4  5]
      [ 6  7  8]]
    
     [[ 9 10 11]
      [12 13 14]
      [15 16 17]]
    
     [[18 19 20]
      [21 22 23]
      [24 25 26]]]
    __________________________________________________
    np.dsplit(dArray,3)->[array([[[ 0],
            [ 3],
            [ 6]],
    
           [[ 9],
            [12],
            [15]],
    
           [[18],
            [21],
            [24]]]), array([[[ 1],
            [ 4],
            [ 7]],
    
           [[10],
            [13],
            [16]],
    
           [[19],
            [22],
            [25]]]), array([[[ 2],
            [ 5],
            [ 8]],
    
           [[11],
            [14],
            [17]],
    
           [[20],
            [23],
            [26]]])]
    

### 数组的复制
#### 简单的赋值

复制与被复制关联。不复制数组对象及其数据


```python
print("aArray->{}".format(aArray))
print("_"*50)
eArray=aArray
print("eArray->{}".format(eArray))
print("_"*50)
print("eArray is aArray->{}".format(eArray is aArray))
print("_"*50)
eArray.shape=(2,3)
print("eArray.shape=(2,3);eArray->{}".format(eArray))
print("_"*50)
print("aArray->{}".format(aArray))
```

    aArray->[[ 0  2  4]
     [ 6  8 10]]
    __________________________________________________
    eArray->[[ 0  2  4]
     [ 6  8 10]]
    __________________________________________________
    eArray is aArray->True
    __________________________________________________
    eArray.shape=(2,3);eArray->[[ 0  2  4]
     [ 6  8 10]]
    __________________________________________________
    aArray->[[ 0  2  4]
     [ 6  8 10]]
    

#### 浅复制/视图

不同的数组对象分享同一数据。


```python
aArray=np.array([ 0,  2,  4,  6,  8, 10])
print("aArray->{}".format(aArray))
print("_"*50)
fArray=aArray.view()
print("fArray->{}".format(fArray))
print("_"*50)
fArray.shape=(2,3)
print("fArray.shape=(2,3);fArray->{}".format(fArray))
print("_"*50)
print("aArray.shape->{}".format(aArray.shape))
print("_"*50)
fArray[0][2]=999
print("fArray[0][2]=999;fArray->{}".format(fArray))
print("_"*50)
print("aArray->{}".format(aArray))
```

    aArray->[ 0  2  4  6  8 10]
    __________________________________________________
    fArray->[ 0  2  4  6  8 10]
    __________________________________________________
    fArray.shape=(2,3);fArray->[[ 0  2  4]
     [ 6  8 10]]
    __________________________________________________
    aArray.shape->(6,)
    __________________________________________________
    fArray[0][2]=999;fArray->[[  0   2 999]
     [  6   8  10]]
    __________________________________________________
    aArray->[  0   2 999   6   8  10]
    

#### 深复制
完全复制数组。复制和被复制不再存在关联


```python
print("aArray->{}".format(aArray))
print("_"*50)
gArray=aArray.copy()
print("gArray->{}".format(gArray))
gArray[...,2]=777
print("_"*50)
print("gArray[...,2]=777;gArray->{}".format(gArray))
print("_"*50)
print("aArray->{}".format(aArray))
```

    aArray->[  0   2 999   6   8  10]
    __________________________________________________
    gArray->[  0   2 999   6   8  10]
    __________________________________________________
    gArray[...,2]=777;gArray->[  0   2 777   6   8  10]
    __________________________________________________
    aArray->[  0   2 999   6   8  10]
    

### 数组的形状变换
#### 数组转置 `array.transpose()`


```python
array=np.arange(0,12,2).reshape(2,3)
print("array->{}".format(array))
print("_"*50)
print("array.transpose()->{}".format(array.transpose()))
```

    array->[[ 0  2  4]
     [ 6  8 10]]
    __________________________________________________
    array.transpose()->[[ 0  6]
     [ 2  8]
     [ 4 10]]
    

#### 数组展平

* array.flatten()


```python
array=np.arange(0,12,2).reshape(2,3)
print("array->{}".format(array))
print("_"*50)
print("array.flatten()->{}".format(array.flatten()))
```

    array->[[ 0  2  4]
     [ 6  8 10]]
    __________________________________________________
    array.flatten()->[ 0  2  4  6  8 10]
    

* array.ravel()


```python
array.ravel()
```




    array([ 0,  2,  4,  6,  8, 10])



### 数组转换为其它数据格式
#### array.tostring()


```python
array.tostring()
```

    C:\Users\richi\AppData\Local\Temp/ipykernel_980/1492779419.py:1: DeprecationWarning: tostring() is deprecated. Use tobytes() instead.
      array.tostring()
    




    b'\x00\x00\x00\x00\x02\x00\x00\x00\x04\x00\x00\x00\x06\x00\x00\x00\x08\x00\x00\x00\n\x00\x00\x00'



#### array.tolist()


```python
array.tolist()
```




    [[0, 2, 4], [6, 8, 10]]



### 数组的方法

`help(np)` 通过tab键或者help函数查看更多方法

* 创建数组 `arange, array, copy, empty, empty_like, eye, fromfile, fromfunction, identity, linspace, logspace, mgrid, ogrid, ones, ones_like, r , zeros, zeros_like `

* 转化 `astype, atleast 1d, atleast 2d, atleast 3d, mat `

* 操作 `array split, column stack, concatenate, diagonal, dsplit, dstack, hsplit, hstack, item, newaxis, ravel, repeat, reshape, resize, squeeze, swapaxes, take, transpose, vsplit, vstack `

* 询问 `all, any, nonzero, where `

* 排序 `argmax, argmin, argsort, max, min, ptp, searchsorted, sort `

* 运算 `choose, compress, cumprod, cumsum, inner, fill, imag, prod, put, putmask, real, sum `

* 基本统计 `cov, mean, std, var `

* 基本线性代数 `cross, dot, outer, svd, vdot`

<a href=""><img src="./icon/ant_03.png" height="auto" width="200" title="caDesign"></a>
