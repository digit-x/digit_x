> Created on Fri Sep 17 20\22\36 2021 @author: Richie Bao-parameterize_archi_la_design_and_data_analysis 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# 景观建筑的代码设计逻辑+逻辑的复用+表皮展平（+结构优化+生态协同）

## 1. 总体布局与3维概念+设计代码复用和设计调整
北区的设计直接复用之前设计的一个参数化建筑模型，根据该场地的边界限定，调整参数，使之适宜于该区的环境，同时根据地形关系，增加挡土墙，以及贴合地形表面的铺地形式（同样复用之前设计），和一个位于一角限定空间的构筑。

* 布局草图

<img src="./imgs_parae/100.jpg" height="auto" width="500"  title="digit-x"> 

* 建筑结构线设计逻辑

<img src="./imgs_parae/105.jpg" height="auto" width="auto"  title="digit-x">

* 参数化构建逻辑

<img src="./imgs_parae/106.jpg" height="auto" width="auto"  title="digit-x">



|   |   |
|---|---|
| <img src="./imgs_parae/092.jpg" height="auto" width="auto"  title="digit-x">   | <img src="./imgs_parae/093.jpg" height="auto" width="auto"  title="digit-x">   |
| <img src="./imgs_parae/094.jpg" height="auto" width="auto"  title="digit-x">   | <img src="./imgs_parae/095.jpg" height="auto" width="auto"  title="digit-x">   |
| <img src="./imgs_parae/096.jpg" height="auto" width="auto"  title="digit-x">   |  <img src="./imgs_parae/097.jpg" height="auto" width="auto"  title="digit-x">  |
| <img src="./imgs_parae/098.jpg" height="auto" width="auto"  title="digit-x">   | <img src="./imgs_parae/099.jpg" height="auto" width="auto"  title="digit-x">   |
|<img src="./imgs_parae/101.jpg" height="auto" width="auto"  title="digit-x"> |<img src="./imgs_parae/102.jpg" height="auto" width="auto"  title="digit-x"> |
|<img src="./imgs_parae/103.jpg" height="auto" width="auto"  title="digit-x"> ||





### 1.1 建筑结构线实现的途径

建筑参数化模型设计的结构线，给出了两种实现形式，一种是纯粹的GH编写，另一种是用python编写。

* GH代码复用，以及过期组件的替换

因为GH和组件的升级，尤其部分库不再升级导致无法安装到新版本的GH中，需要替换组件。

<img src="./imgs_parae/104.jpg" height="auto" width="auto"  title="digit-x"> 

* python编写建筑结构线

```python
import rhinoscriptsyntax as rs
import math
import random


def basiclines(basicpoint,lengthunit,angle,offsetvalue,topbplineheight,\
multiple1,multiple2,multiple3):
    bpoint0=(basicpoint[0],basicpoint[1],basicpoint[2])
    bpoints=[]
    bpoints.append(bpoint0)
    lengthunit=lengthunit
    multiple1=multiple1
    bpoint1=(bpoint0[0]+multiple1*lengthunit,bpoint0[1],bpoint0[2])
    bpoints.append(bpoint1)
    
    angle=angle 
    multiple2=multiple2
    hypotenuse=multiple2*lengthunit
    bpoint2=(bpoint1[0]+hypotenuse*math.sin(angle),\
    bpoint1[1]+hypotenuse*math.cos(angle),bpoint1[2])
    bpoints.append(bpoint2)
    
    multiple3=multiple3
    bpoint3=(bpoint2[0]+multiple3*lengthunit,bpoint2[1],bpoint2[2])
    bpoints.append(bpoint3)
    
    bpline0=rs.AddPolyline(bpoints)
    bplines=[]
    bplines.append(bpline0)
    
    for i in range(1,4):
        dividecurvelength=rs.CopyObject (bpline0,[0,0,5*i])
        bplines.append(dividecurvelength)
    offsetbplines=[]
    offsetvalue=offsetvalue
    for j in bplines:
        offsetbpline=rs.OffsetCurve(j,[0,0,0],offsetvalue)
        offsetbplines.append(offsetbpline)
    topbplineheight=topbplineheight
    topbplinecenter=rs.OffsetCurve(bplines[-1],[0,0,0],offsetvalue/2)
    topbpline=rs.CopyObject(topbplinecenter,[0,0,topbplineheight])
    rs.DeleteObject(topbplinecenter)
    
    return bplines,offsetbplines,topbpline      
    
def basicpoints(bplines,lengthunit):
    
    basicplanepoints=[]
    for u in range(len(bplines)):
        basicplanepoint=rs.DivideCurveLength (bplines[u],lengthunit,True,True)
        basicplanepoints.append(basicplanepoint)
    
    lengthunit=lengthunit

    planeunit=1
    randomselectionp=[]
    pupoints=[]
    for o in range(len(basicplanepoints)-1):
        for p in range(len(basicplanepoints[0])):
            basicplanepointscor=[basicplanepoints[o][p][0],basicplanepoints[o][p][1],\
            basicplanepoints[o][p][2]]
            
            
            pupoints.append(basicplanepointscor)
            
            pupoint1=[basicplanepointscor[0]+planeunit,\
            basicplanepointscor[1],\
            basicplanepointscor[2]]
            pupoints.append(pupoint1)
            rs.AddPoint(pupoint1)
            
            pupoint2=[basicplanepointscor[0]+planeunit,\
            basicplanepointscor[1]+planeunit,\
            basicplanepointscor[2]]
            pupoints.append(pupoint2)
            rs.AddPoint(pupoint2)
            
            pupoint3=[basicplanepointscor[0],\
            basicplanepointscor[1]+planeunit,\
            basicplanepointscor[2]]
            pupoints.append(pupoint3)
            rs.AddPoint(pupoint3)
            
            pupoint4=[basicplanepointscor[0]-planeunit,\
            basicplanepointscor[1]+planeunit,\
            basicplanepointscor[2]]
            pupoints.append(pupoint4)
            rs.AddPoint(pupoint4)
            
            pupoint5=[basicplanepointscor[0]-planeunit,\
            basicplanepointscor[1],\
            basicplanepointscor[2]]
            pupoints.append(pupoint5)
            rs.AddPoint(pupoint5)
            
            pupoint6=[basicplanepointscor[0]-planeunit,\
            basicplanepointscor[1]-planeunit,\
            basicplanepointscor[2]]
            pupoints.append(pupoint6)
            rs.AddPoint(pupoint6)
            
            pupoint7=[basicplanepointscor[0],\
            basicplanepointscor[1]-planeunit,\
            basicplanepointscor[2]]
            pupoints.append(pupoint7)
            rs.AddPoint(pupoint7)
            
            pupoint8=[basicplanepointscor[0]+planeunit,\
            basicplanepointscor[1]-planeunit,\
            basicplanepointscor[2]]
            pupoints.append(pupoint8)
            rs.AddPoint(pupoint8)
            
            randomselectionp.append(random.choice(pupoints))
            pupoints=[] 
               
    pupoints0=randomselectionp[:len(basicplanepoints[0])]
    pupoints1=randomselectionp[len(basicplanepoints[0]):-len(basicplanepoints[0])]
    pupoints2=randomselectionp[-len(basicplanepoints[0]):]
    
    pupoints4sub=basicplanepoints[-1]
    pupoints4=[]
    for e in range(len(pupoints4sub)):
        pupoints4.append([pupoints4sub[e][0],pupoints4sub[e][1],pupoints4sub[e][2]])    
    
    sectionpolylinesparts=[]
    for q in range(len(pupoints0)):
        sectionpolylinesparts.append(rs.AddPolyline((pupoints0[q],pupoints1[q],\
        pupoints2[q],pupoints4[q])))
    
    return sectionpolylinesparts,pupoints4    
    
def mainfunction():
    basicpoint=rs.GetPoint('Select one point:')
    if not basicpoint:return
    
    values=[5,120,12,5,4,3,4]
    lengthunit=values[0]
    angle=values[1]
    offsetvalue=values[2]
    topbplineheight=values[3]
    multiple1=values[4]
    multiple2=values[5]
    multiple3=values[6]
    
    while True:
        prompt='Setting'
        result=rs.GetString(prompt,'Insert:',('Lengthunit','Angle','Offsetvalue',\
        'Topbplineheight','Multiple1','Multiple2','Multiple3','Insert'))
        if not result:return        
        
        result=result.upper()
        if result=='LENGTHUNIT':
            f=rs.GetReal('Lengthunit:',values[0])
            if f is not None:lengthunit=f
        elif result=='ANGLE':
            f=rs.GetReal('Angle(110-120):',values[1],110,120)
            if f is not None:angle=f
        elif result=='OFFSETVALUE':
            f=rs.GetReal('Offsetvalue:',values[2])
            if f is not None:offsetvalue=f
        elif result=='TOPBPLINEHEIGHT':
            f=rs.GetReal('Topbplineheight:',values[3])
            if f is not None:topbplineheight=f
        elif result=='MULTIPLE1':
            f=rs.GetReal('Multiple1:',values[4])
            if f is not None:multiple1=f
        elif result=='MULTIPLE2':
            f=rs.GetReal('Multiple2:',values[5])
            if f is not None:multiple2=f
        elif result=='MULTIPLE3':
            f=rs.GetReal('Multiple3:',values[5])
            if f is not None:multiple3=f
            
        elif result=='INSERT':break

    bplines,offsetbplines,topbpline=basiclines(basicpoint,lengthunit,angle,\
    offsetvalue,topbplineheight,multiple1,multiple2,multiple3)
    
    sectionpolylinespart0,pupoints40=basicpoints(bplines,lengthunit)
    sectionpolylinespart1,pupoints41=basicpoints(offsetbplines,lengthunit)
    
    topdivide=rs.DivideCurveLength(topbpline,lengthunit,True,True)
    topdividepoints=[]
    for a in range(len(topdivide)):
        topdividepoints.append([topdivide[a][0],topdivide[a][1],topdivide[a][2]])
    
    toppolylines=[]
    for s in range(len(topdividepoints)):
        toppolylines.append(rs.AddPolyline([pupoints40[s],topdividepoints[s],pupoints41[s]]))
    print(basicpoint)  
        
mainfunction()
```

**在PythonScript中调用代码**

<img src="./imgs_parae/107.jpg" height="auto" width="auto"  title="digit-x"> 

**在GHPython中调用代码**

<img src="./imgs_parae/109.jpg" height="auto" width="auto"  title="digit-x"> 
<img src="./imgs_parae/108.jpg" height="auto" width="auto"  title="digit-x"> 

### 1.2 增加室内准备/仓储室和洗手间（概念，暂无细节）

<img src="./imgs_parae/110.jpg" height="auto" width="auto"  title="digit-x"> 

<img src="./imgs_parae/116.jpg" height="auto" width="auto"  title="digit-x"> 

|   |   |
|---|---|
| <img src="./imgs_parae/117.jpg" height="auto" width="auto"  title="digit-x">   |  <img src="./imgs_parae/111.jpg" height="auto" width="auto"  title="digit-x">  |
|<img src="./imgs_parae/112.jpg" height="auto" width="auto"  title="digit-x">    | <img src="./imgs_parae/113.jpg" height="auto" width="auto"  title="digit-x">   |
| <img src="./imgs_parae/114.jpg" height="auto" width="auto"  title="digit-x">   | <img src="./imgs_parae/115.jpg" height="auto" width="auto"  title="digit-x">   |

### 1.3 展平表皮


```python
import rhinoscriptsyntax as rs
mesh=mesh
print(mesh)
meshes=rs.ExplodeMeshes(mesh)
refplane=rs.WorldXYPlane()
oriplanepoints0=rs.MeshVertices(meshes[0])
oriplanepoints1=rs.MeshVertices(meshes[1])
xymeshes=[]
for i in range(len(meshes)):
    if i ==0:
        mesh0point=rs.MeshVertices(meshes[i])
        mesh0points=[]
        for r in mesh0point:
            mesh0points.append([r[0],r[1],r[2]])
        xymesh0=rs.OrientObject(meshes[i],mesh0points,\
        [[0,10,0],[10,0,0],[0,0,0]],1)
        xymeshes.append(xymesh0)
    else:
        vertices2=rs.MeshVertices(meshes[i])
        vertices1=rs.MeshVertices(meshes[i-1])
        vertices2lst=[]
        vertices1lst=[]
        for q in vertices2:
            vertices2lst.append([q[0],q[1],q[2]])
        for p in vertices1:
            vertices1lst.append([p[0],p[1],p[2]])
        ver=[m for m in vertices1lst for n in vertices2lst if m==n]
        a=ver[0]
        b=ver[1]
        indexa=vertices1lst.index(a)
        indexb=vertices1lst.index(b)
        cref=[m for m in vertices1lst if m not in ver][0]
        cv=[m for m in vertices2lst if m not in ver][0]
        refvertice=rs.MeshVertices(xymeshes[i-1])
        refvertices=[]
        for x in refvertice:
            refvertices.append([x[0],x[1],x[2]])
        indexc=[c for c in range(0,3) if c !=indexa and c!=indexb]
        refverticespoint=rs.MirrorObject(rs.AddPoint(refvertices[indexc[0]]),refvertices[indexa],refvertices[indexb])
        mirrorpoint=[rs.PointCoordinates(refverticespoint)]
        for z in mirrorpoint:
            mirrorpoint=[z[0],z[1],z[2]]
        xymesh=rs.OrientObject(meshes[i],[a,b,cv],[refvertices[indexa],refvertices[indexb],mirrorpoint],1) 
        xymeshes.append(xymesh)
    vertices2lst=[]
    vertices1lst=[] 
    ver=[]
```


|   |   |
|---|---|
| <img src="./imgs_parae/120.jpg" height="auto" width="auto"  title="digit-x">   |  <img src="./imgs_parae/118.jpg" height="auto" width="auto"  title="digit-x">  | 

<img src="./imgs_parae/119.jpg" height="auto" width="auto"  title="digit-x">  

**构筑-盒体的展开**

|   |   |
|---|---|
| <img src="./imgs_parae/124.jpg" height="auto" width="auto"  title="digit-x">   | <img src="./imgs_parae/121.jpg" height="auto" width="auto"  title="digit-x">   |
| <img src="./imgs_parae/122.jpg" height="auto" width="auto"  title="digit-x">   |  <img src="./imgs_parae/123.jpg" height="auto" width="auto"  title="digit-x">  |

```python
# Python GrouperacCylinder(MTL)
import Rhino
import rhinoscriptsyntax as rs
from Grasshopper import DataTree
from Grasshopper.Kernel.Data import GH_Path
data=TreeData
branches=data.Branches
PT=DataTree[Rhino.Geometry.GeometryBase]()
def grouper(branches,dt):
    for m in range(len(branches)-1):
        a=branches[m]
        b=branches[m+1]
        for i in range(len(a)-1):
            lst=[]
            lst.append(b[i])
            lst.append(a[i])
            lst.append(b[i+1])
            lst.append(a[i+1])
            dt.AddRange(lst,GH_Path(m,i))
    return dt
PLst=grouper(branches,PT)
```

```python
# MeshTopology
import rhinoscriptsyntax as rs
import Rhino
import System
from Grasshopper import DataTree
from Grasshopper.Kernel.Data import GH_Path
mesh=rs.ExplodeMeshes(Mesh)
meshv={}
for i in range(len(mesh)):
    meshv[i]=rs.MeshVertices(mesh[i])
meshvkeys=meshv.keys()
meshvvalues=meshv.values()
tdic={}.fromkeys(meshvkeys)
for i in meshvkeys:
    tlist=[]
    a=meshvvalues[i]
    for n in meshvvalues:
        temp=[]
        for f in a:
            if f in n:
                temp.append(True)
        tlist.append(temp)
    tdic[i]=tlist
topologydic={}.fromkeys(meshvkeys)
for i in meshvkeys:
    b=tdic[i]
    tempa=[]
    for j in range(len(b)):
        if len(b[j])==2:
            tempa.append(j)
    topologydic[i]=tempa
MeshFlatten=DataTree[System.Int32]()
for i in  meshvkeys:
    path=GH_Path(i)
    v=topologydic[i]
    MeshFlatten.AddRange(v,path)
```

```python
# flattenbox
import rhinoscriptsyntax as rs
import Rhino
import System
import random
import copy
from Grasshopper import DataTree
from Grasshopper.Kernel.Data import GH_Path
mesh=rs.ExplodeMeshes(Mesh)
meshv={}
for i in range(len(mesh)):
    meshv[i]=rs.MeshVertices(mesh[i])
meshvkeys=meshv.keys()
meshvvalues=meshv.values()
tdic={}.fromkeys(meshvkeys)
datat=MeshTopology
Glst={}
for i in range(datat.BranchCount):
    branchlst=datat.Branch(i)
    lst=[]
    for k in range(len(branchlst)):
        lst.append(branchlst[k])
    Glst[i]=lst
ordorlst=[]
allowlst=copy.deepcopy(Glst)
startmesh=random.choice(allowlst.keys())
def order(allowlst,startmesh):
    if len(allowlst)==0:
        return ordorlst
    else:
        ordorlst.append(startmesh)
        tmesh=allowlst[startmesh]
        allowlst.pop(startmesh)
        if tmesh==[]:
            allowlst=copy.deepcopy(Glst)
            return order(allowlst,random.choice(allowlst.keys()))
        else:
            sidxt=random.choice(tmesh)
            startmesh=sidxt
        for i in allowlst.keys():
            if startmesh in allowlst[i]:
                allowlst[i].remove(startmesh)
            elif sidxt in allowlst[i]:
                allowlst[i].remove(sidxt)
            else:pass
        return order(allowlst,startmesh)

for i in range(int(Count)):
    try:
        u=order(allowlst,startmesh)
    except KeyError:
        pass

ordermesh=[mesh[i] for i in u]
Order=u

def flattenmesh(meshes,LocationPoint):
    lp=[]
    for i in LocationPoint:
        lp.append(rs.PointCoordinates(i))
    xymeshes=[]
    for i in range(len(meshes)):
        if i ==0:
            mesh0point=rs.MeshVertices(meshes[i])
            xymesh0=rs.OrientObject(meshes[i], mesh0point,lp,1)
            xymeshes.append(xymesh0)
        else:
            vertices2=rs.MeshVertices(meshes[i])
            vertices1=rs.MeshVertices(meshes[i-1])
            ver=[m for m in vertices1 for n in vertices2 if m==n]
            a=ver[0]
            b=ver[1]
            indexa=vertices1.index(a)
            indexb=vertices1.index(b)
            d=[m for m in vertices2 if m not in ver][0]
            refvertice=rs.MeshVertices(xymeshes[i-1])
            indexc=[c for c in range(0,3) if c !=indexa and c!=indexb]
            refverticespoint=rs.MirrorObject(rs.AddPoint(refvertice[indexc[0]]),refvertice[indexa],refvertice[indexb])
            mirrorpoint=rs.PointCoordinates(refverticespoint)
            xymesh=rs.OrientObject(meshes[i],[a,b,d],[refvertice[indexa],refvertice[indexb],mirrorpoint],1) 
            xymeshes.append(xymesh)
        vertices2lst=[]
        vertices1lst=[] 
        ver=[]
    return xymeshes

BoxFlatten=flattenmesh(ordermesh,LocationPoint)
```

### 未来潜在探索的方向（敬请关注）
1. Mesh方法的系统梳理，代码整理，与构筑体与表皮，及建造方法；
2. 构建建筑规范参数化数据库；
3. 建筑常用设计元素的参数化设计库。



