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

### 2.1 第1版南区桥-3维草图

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

### 3.3 由道路中线生成道路面（simple）+视域分析

|  |   |
|---|---|
|  <img src="./imgs_parae/068.jpg" height="auto" width="auto"  title="digit-x"> | <img src="./imgs_parae/069.jpg" height="auto" width="auto"  title="digit-x">  |

* 寻找坡度相同的路径（略），代码待修正

* 道路线汇聚推衍（略），代码待修正

### 3.4 A* search algorithm A*寻路算法

参考：[Introduction to A* Pathfinding](https://www.raywenderlich.com/3016-introduction-to-a-pathfinding)

|  |   |
|---|---|
|  <img src="./imgs_parae/070.jpg" height="auto" width="auto"  title="digit-x"> | <img src="./imgs_parae/071.jpg" height="auto" width="300"  title="digit-x">  <img src="./imgs_parae/072.jpg" height="auto" width="300"  title="digit-x"> <img src="./imgs_parae/073.jpg" height="auto" width="300"  title="digit-x"> |

```python
#用Python编写A*寻路算法
import rhinoscriptsyntax as rs
from Grasshopper import DataTree
from Grasshopper.Kernel.Data import GH_Path
import Rhino,Grasshopper,System,math,sys,copy
def treetodic(treedata): #定义树型数据转字典的函数
    dicname={}
    for i in range(treedata.BranchCount):
        branchlst=treedata.Branch(i)
        lst=[]
        for k in range(len(branchlst)):
            lst.append(branchlst[k])
        dicname[i]=lst
    return dicname
    
def flatten(nested): #定义嵌套列表递归展平的程序
    try:
        for sublist in nested:
            for element in flatten(sublist):
                yield element
    except TypeError:
        yield nested

tr=treetodic(centerpoint)
trpointnested=list(flatten(tr.values()))
boundarylist=[]
openlist=[]
closelist=[]
for i in trpointnested:
    num=rs.PointInPlanarClosedCurve(i,boundary)
    if num==0:
        closelist.append(i)
    else:
        boundarylist.append(i)
for i in boundarylist:
    temp=[]
    for p in closecurve:        
        numo=rs.PointInPlanarClosedCurve(i,p)
        temp.append(numo)
    if temp.count(0)==len(closecurve):
        openlist.append(i)
    else:closelist.append(i)

def dictotree(dicdata,datatype):  #定义字典转树型数据的函数用于输出
    treedata=DataTree[datatype]()
    for i in dicdata.keys():
        p=int(i)
        ghpath=GH_Path(p)
        treedata.AddRange(dicdata[i],ghpath)
    return treedata
    
def matrix(cp): #定义矩阵获取相邻点的函数
    matrix={}
    keys=cp.keys()
    for key in keys:
        if key==0:
            tempdic={}
            sublst=cp[key]
            sublstup=cp[key+1]
            for idx in range(len(sublst)):
                if idx==0:
                    matrix[sublst[idx]]=[sublst[idx+1],sublstup[idx],sublstup[idx+1]]
                elif idx==len(sublst)-1:
                    matrix[sublst[idx]]=[sublst[idx-1],sublstup[idx],sublstup[idx-1]]
                else:
                    matrix[sublst[idx]]=[sublst[idx+1],sublst[idx-1],sublstup[idx],sublstup[idx-1],sublstup[idx+1]]
        elif key==max(keys):
            tempdic={}
            sublst=cp[key]
            sublstdown=cp[key-1]
            for idx in range(len(sublst)):
                if idx==0:
                    matrix[sublst[idx]]=[sublst[idx+1],sublstdown[idx],sublstdown[idx+1]]
                elif idx==len(sublst)-1:
                    matrix[sublst[idx]]=[sublst[idx-1],sublstdown[idx],sublstdown[idx-1]]
                else:
                    matrix[sublst[idx]]=[sublst[idx+1],sublst[idx-1],sublstdown[idx],sublstdown[idx+1],sublstdown[idx-1]]
        else:
            tempdic={}
            sublst=cp[key]
            sublstup=cp[key+1]
            sublstdown=cp[key-1]
            for idx in range(len(sublst)):
                if idx==0:
                    tempdic[idx]={sublst[idx]:[sublst[idx+1],sublstup[idx],sublstup[idx+1],sublstdown[idx],sublstdown[idx+1]]}
                    matrix[sublst[idx]]=[sublst[idx+1],sublstup[idx],sublstup[idx+1],sublstdown[idx],sublstdown[idx+1]]
                elif idx==len(sublst)-1:
                    matrix[sublst[idx]]=[sublst[idx-1],sublstup[idx],sublstup[idx-1],sublstdown[idx],sublstdown[idx-1]]
                else:
                    matrix[sublst[idx]]=[sublst[idx+1],sublst[idx-1],sublstup[idx],sublstup[idx-1],sublstup[idx+1],sublstdown[idx],sublstdown[idx+1],sublstdown[idx-1]]
    return matrix
    
matrix=matrix(tr) #执行矩阵函数

def s_e_closestpoint(startpoint,endpoint,openlist): #定义获取开始和结束点临近几何中心点的程序
    startpoint=startpoint
    endpoint=endpoint
    openlist=openlist
    alst=[]
    def closestpoint(sepoint,openlist):
        dist=sys.maxsize
        for i in openlist:
            distance=rs.Distance(sepoint,i)
            if distance<dist:
                dist=distance
                closestp=i
        return closestp
    a=closestpoint(startpoint,openlist)
    alst.append(a)
    openlistcopy=copy.deepcopy(openlist)
    openlistcopy.remove(a)
    for i in range(3):
        b=closestpoint(startpoint,openlistcopy)
        alst.append(b)
        openlistcopy.remove(b)
    def s_e_point(alst,startpoint,endpoint):
        distmin=sys.maxsize
        for i in alst:
            distse=rs.Distance(startpoint,i)+rs.Distance(i,endpoint)
            if distse<distmin:
                distmin=distse
                closestpse=i
        return closestpse
    return s_e_point(alst,startpoint,endpoint)


class Node_Elem:  #定义开放列表和关闭列表的元素类型，parent用于在成功的时候回溯路径
    def __init__(self, parent, point, dist):
        self.parent=parent
        self.point=point
        self.dist=dist
        
class A_Star: #定义A*寻路算法类
    def __init__(self,s_closestpoint,e_closestpoint,openlist,closelist):  #openlist和closelist用于判断选取的点是
否是有效点
        self.startpoint=s_closestpoint
        self.endpoint=e_closestpoint
        self.openlist=openlist
        self.closelist=closelist
        self.open=[]
        self.close=[]
        self.path=[]
        
    def find_path(self): #查找路径的入口函数
        p=Node_Elem(None,self.startpoint,0.0) #构建开始节点
        while True:
            self.extend_round(p) #扩展F值最小的节点
            if not self.open: #如果开放列表为空则不存在路径，返回
                return
            idx,p=self.get_best() #获取F值最小的节点
            if p.point==self.endpoint: #找到并生成路径，返回
                while p: #从结束点回溯到开时点，开始点的parent==None
                    self.path.append(p.point)
                    p=p.parent
                return
            self.close.append(p) #把节点追加到关闭列表并从开放列表中删除
            del self.open[idx]
            
    def extend_round(self,p):
        point=p.point
        for new_point in matrix[point]:
            if new_point in self.closelist or new_point in self.close :
                continue
            node=Node_Elem(p,new_point,p.dist+rs.Distance(p.point,new_point))
            if node.point in self.close:
                continue
            if node.point in self.open: #新节点在开放列表中
                i=self.open.index(node.point)
                if self.open[i].dist>node.dist: #现在的这个路径比之前到这个节点的路径更好，则使用现在的
路径
                    self.open[i].parent=p
                    self.open[i].dist=node.dist
                continue
            self.open.append(node)

    def get_best(self):
        best=None
        bv=sys.maxsize
        bi=-1
        for idx,i in enumerate(self.open):
            value=i.dist+rs.Distance(self.endpoint,i.point)*1.2 #获取F值，F=G+H
            if value<bv:
                best=i
                bv=value
                bi=idx
        return bi,best
        

def find_path():  #定义执行函数
    s_closestpoint=s_e_closestpoint(startpoint,endpoint,openlist)
    e_closestpoint=s_e_closestpoint(endpoint,startpoint,openlist)
    a_star=A_Star(s_closestpoint,e_closestpoint,openlist,closelist)
    a_star.find_path()
    path=a_star.path
    return path


if start==True:
    path=find_path()

```

### 3.5 平台
设计推敲时，可能会尝试多种形式，被遗弃形式的的代码仍然可以保留，备日后用。

**平台，挑台，坡地台阶和平桥草图**

<img src="./imgs_parae/091.jpg" height="auto" width="500"  title="digit-x">

<img src="./imgs_parae/067.jpg" height="auto" width="auto"  title="digit-x">

|   |   |   |
|---|---|---|
| <img src="./imgs_parae/062.jpg" height="auto" width="400"  title="digit-x">  | <img src="./imgs_parae/063.jpg" height="auto" width="400"  title="digit-x">  |  <img src="./imgs_parae/064.jpg" height="auto" width="400"  title="digit-x"> |
|  <img src="./imgs_parae/065.jpg" height="auto" width="400"  title="digit-x"> | <img src="./imgs_parae/066.jpg" height="auto" width="400"  title="digit-x">  |   |

### 3.6 挑台
挑台的结构与A-B 桥的结构一致，因此可以直接复用大部分代码。 在代码复用时，有部分代码参数需要调整，否则会出现移动方向和偏移位置的错误，因此有必要再梳理代码，考虑周全，直至当代码复用时，不在出现不必要的调整。当确定代码复用时不再出现错误，可以构建cluster，引出所需参数和输出对象，使之趋于工业级别的代码水平。

<img src="./imgs_parae/080.jpg" height="auto" width="auto"  title="digit-x"> 

|   |   |   |
|---|---|---|
|  | <img src="./imgs_parae/075.jpg" height="auto" width="400"  title="digit-x">  |  <img src="./imgs_parae/076.jpg" height="auto" width="400"  title="digit-x"> |
|  <img src="./imgs_parae/077.jpg" height="auto" width="400"  title="digit-x"> | <img src="./imgs_parae/078.jpg" height="auto" width="400"  title="digit-x">  |  <img src="./imgs_parae/079.jpg" height="auto" width="400"  title="digit-x">  |

### 3.7 C-D 段坡地台阶及跨河平桥

<img src="./imgs_parae/081.jpg" height="auto" width="auto"  title="digit-x"> 

|   |   |   |
|---|---|---|
| <img src="./imgs_parae/083.jpg" height="auto" width="400"  title="digit-x">  |  <img src="./imgs_parae/089.jpg" height="auto" width="400"  title="digit-x"> | <img src="./imgs_parae/084.jpg" height="auto" width="400"  title="digit-x">|
|  <img src="./imgs_parae/085.jpg" height="auto" width="400"  title="digit-x"> | <img src="./imgs_parae/087.jpg" height="auto" width="400"  title="digit-x">  |  <img src="./imgs_parae/088.jpg" height="auto" width="400"  title="digit-x">  |

### 3.8 北区
