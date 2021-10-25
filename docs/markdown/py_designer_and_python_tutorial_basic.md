> Created on Sun Sep  5 15\42\24 2021 @author: Richie Bao-python_code_archi_la_design_method_study 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# 设计师与PYTHON | PYTHON基础速学

## 1. python应用的方式

### 1.1 软件嵌入python脚本语言

* 1.1.1 参数化平台下使用python (e.g. [rhinoceros](https://www.rhino3d.com/)+[grasshopper]()):

<a href=""><img src="./imgs_pyd/001.png" height="auto" width="auto" title="caDesign"></a>

* 1.1.2 BIM平台下使用python (e.g. [revit](https://www.autodesk.com/products/revit/overview)+[dynamo](https://dynamobim.org/))

<a href=""><img src="./imgs_pyd/003.png" height="auto" width="auto" title="caDesign"></a>

* 1.1.3 GIS平台下使用python (e.g. [QGIS](https://www.qgis.org/en/site/), [ArcGIS](https://www.esri.com/en-us/arcgis/products/arcgis-pro/overview))

<a href=""><img src="./imgs_pyd/002.png" height="auto" width="auto" title="caDesign"></a>

### 1.2 独立应用python

## 2. 开始学习的准备工作

### 2.1 python解释器/编辑器

序号 |解释器名称| 免费与否|推荐说明|
------------ |:-------------|:-------------|:-------------|
1 |[python 官方](https://www.python.org/downloads/)|免费|**不推荐**，但轻便，可以安装，偶尔用于随手简单代码的验证，乃至当计算器使用| 
2 |[anaconda](https://www.anaconda.com/)|个人版(Individual Edition)免费；<em>团队版(Team Edition)和企业版(Enterprise Edition)付费</em> |集成了众多科学包及其依赖项，**强烈推荐**，因为自行解决依赖项是一件十分令人苦恼的事情。其中包含Spyder和JupyterLab为本书所使用|
3 |[Visual Studio Code(VSC)](https://code.visualstudio.com/)|免费|**推荐使用**，用于查看代码非常方便，并支持多种语言格式；同时本书用docsify部署网页版时，即使用VSC实现|
4 |[PyCharm](https://www.jetbrains.com/pycharm/download/#section=windows)|付费|**一般推荐**，通常只在部署网页时使用，本书“实验用网络应用平台的部署部分”用到该平台|
5 |[Notepad++](https://notepad-plus-plus.org/)|免费|仅用于查看代码、文本编辑，**推荐使用**，轻便的纯文本代码编辑器，可以用于代码查看，尤其兼容众多文件格式，当有些文件乱码时，不妨尝试用此编辑器|

<a href="https://www.anaconda.com/"><img src="./imgs_pyd/anacondaimg.jpg" height="50" width="auto" title="caDesign">
<a href="https://www.python.org/downloads/"><img src="./imgs_pyd/python-logo@2x.png" height="50" width="auto" title="caDesign">
<a href="https://code.visualstudio.com/"><img src="./imgs_pyd/vsc.jpg" height="35" width="auto" title="caDesign" align="top">
<a href="https://www.jetbrains.com/pycharm/download/#section=windows"><img src="./imgs_pyd/PyCharm_Logo1.png" height="65" width="auto" title="caDesign" align="top">
<a href="https://notepad-plus-plus.org/"><img src="./imgs_pyd/notepadlogo.png" height="50" width="auto" title="caDesign">
<a href="https://jupyter.org/"><img src="./imgs_pyd/logo_svg.png" height="45" width="auto" title="caDesign">


其它解释器/编辑器：[gedit Text Editor](https://help.gnome.org/users/gedit/stable/), [Sublime Text](https://www.sublimetext.com/), [SPE IDE - Stani's Python Editor](https://pythonide.blogspot.com/), [SciTE](https://www.scintilla.org/SciTEDownload.html), [Geany](https://www.geany.org/); [Wing Python IDE](https://www.wingware.com/), [Pyzo](https://pyzo.org/), [PyScripter](https://sourceforge.net/projects/pyscripter/)
    
> IDE(Integrated development environmen, 集成开发环境)

> 部分同学下载anaconda比较慢（因位于国外服务器上），因此可以从该网盘下载，链接：https://pan.baidu.com/s/1L3FUxG0UcIzhMj6lj04A5w  ，提取码：n11a 。version:Anaconda3-2021.05-Windows-x86_64+Anaconda3-2021.05-MacOSX-x86_64

### 2.2 代码托管    
* [GitHub](https://github.com/richieBao)
    
__push and pull__ :1. 桌面窗口方式 [GitHub Desktop](https://desktop.github.com/); 2. Ubuntu(Linux)终端terminal代码方式`git add -all; git commit -m <NOTE>'; git push <REMOTENAME> <BRANCHNAME>`；3. 在线同步 [StackEdit](https://stackedit.io/app#); 4. 直接在线编辑。
    
* [Gitee](https://gitee.com/) 
    
* 服务器（可以自行租用），例如[阿里云](https://developer.aliyun.com/)
    
### 2.3 English+翻译词典
路径文件夹，文件名以英文命名。除必要（例如注释，中文数据），应均已英文书写。
    
### 2.4 用[Markdown](https://www.markdownguide.org/)记笔记
用使用[JupyterLab或者Jupyter Notebook](https://jupyter.org/)，[Visual Studio Code(VSC)](https://code.visualstudio.com/)或者其它Markdown编辑器。

## 3. 练习，记忆与搜索
（不断重复，与实际应用）


```python
from platform import python_version #查看Jupyter notebook的python版本，调入了platform库中的python_version方法查看版本
print(python_version())
```

    3.8.11
    

### 3.1 不知道运行结果，就很难写代码。善用print()
print()是python语言中使用最为频繁的语句，在代码编写、调试过程中，要不断的用该语句来查看数据的值、数据的变化、数据的结构、变量所代表的内容、监测程序进程、以及显示结果等等，print()实时查看数据反馈，才可知道代码编写的目的是否达到，并做出反馈，善用print()是用python写代码的基础


```python
print('Hello python!') #打印字符串
```

    Hello python!
    


```python
print("Hello world!")
print("_"*50)
print("编程让设计更具创造力！")
print("Everybody should learn how to code a computer, because it teaches you how to think, and allows designers more creative!")
```

    Hello world!
    __________________________________________________
    编程让设计更具创造力！
    Everybody should learn how to code a computer, because it teaches you how to think, and allows designers more creative!
    


```python
print("加：",3+3) #打印简单的运算
print("减：",3-1)
print("乘：",3*3)
print("除：", 3/3)
print("整除：", 3//2)
print("余数：", 3%2)
print("幂：",3**3)
```

    加： 6
    减： 2
    乘： 9
    除： 1.0
    整除： 1
    余数： 1
    幂： 27
    

**二元运算符**

| 运算         | 说明                                                |
|------------|---------------------------------------------------|
| a+b        | a加b                                               |
| a-b        | a减b                                               |
| a*b        | a乘以b                                              |
| a/b        | a除以b                                              |
| a//b       | a除以b后向下取整                                         |
| a**b       | a的b次方                                             |
| a&b        | 如果a和b均为True，则结果为True。对于整数，执行按位与操作。                |
| a|b        | 如果a和b任何一个为True，则结果为True。对于整数，执行按位或操作              |
| a^b        | 对于布尔值，如果a或b为True(但不都为True)，则结果为True。对于整数，执行按位异或操作 |
| a==b       | 如果a等于b，则结果为True                                   |
| a!=b       | 如果a不等于b，则结果为True                                  |
| a<=b、a<b   | 如果a小于等于(或小于)b，则结果为True                            |
| a>b、a>=b   | 如果a大于(或大于等于)b，则结果为True                            |
| a is b     | 如果变量a和b指向同一个python对象，则结果为True                     |
| a is not b | 如果变量a和b指向不同的python对象，则结果为True                     |

### 3.2 变量及赋值 (Variables and assignment)
代码读起来应该像流畅的英语散文，而不是加密的密码。好的易读的变量名的定义正是让代码变的流畅的基础。变量名不能以数字和特殊字符为开头，也不可以内置的函数名定义，也不存在空格，如果由几个单词或数字组成变量名，通常由下划线连接，或者每一新单词首字母大写。变量名定义不符合规范时，解释器会提示错误。


```python
func=2*x+1 #当程序逐行从上至下运行时，注意变量定义的顺序
x=5
print(func)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_13536/1607899601.py in <module>
    ----> 1 func=2*x+1 #当程序逐行从上至下运行时，注意变量定义的顺序
          2 x=5
          3 print(func)
    

    NameError: name 'x' is not defined



```python
x=5.0
monadic_equation=2*x+1
print("monadic_equation=",monadic_equation)
print("monadic_equation=%.2f"%monadic_equation) #%字符串格式化方法
print("monadic_equation={:.2f}".format(monadic_equation)) #format()字符串格式化方法
```

    monadic_equation= 11.0
    monadic_equation=11.00
    monadic_equation=11.00
    


```python
city_name="Xi'an"
coordinate_longitude=108.942292
coordiante_latitude=34.261013
print("The longitude of the Xi'an coordinate is {lon:.2f}, and the latitude is {lat}.".format(lon=coordinate_longitude,lat=coordiante_latitude))
```

    The longitude of the Xi'an coordinate is 108.94, and the latitude is 34.261013.
    


```python
x,y,b=2,5,7 #unpacking 序列解包。尝试，x,y,*z=0,1,2,3,4,5,6; x,y,*z=0,1; (x,y),(a,b)=(0,1),(2,3)
func_2=2*x+3*y+b
print("func_2={}".format(x,y,b,func_2))
```

    func_2=26
    

## 3.3 数据结构
### 3.3.1 数据结构类型

* 基础数据类型：list(列表),tuple(元组),dictionary(dict, 字典)，string(str,字符串)。

* 库扩展数据类型：array(数组，[NumPy](https://numpy.org/)); DataFrame(table/表/数据框，[pandas](https://pandas.pydata.org/))|series; GeoDataFrame(地理table/表/数据框，[GeoPandas](https://geopandas.org/docs/user_guide/data_structures.html))|GeoSeries; tensor(张量，深度学习库[PyTorch](https://pytorch.org/))

> CPU和GPU: 深入学习库，[PyTorch](https://pytorch.org/)和[TensorFlow](https://www.tensorflow.org/)

### 3.3.2 list


```python
lst_n=list(range(5,20,3)) #建立列表。range(start,stop,[,step])建立区间。
print(lst)
print("The list length={}".format(len(lst_n)))
print("Maximum value={}".format(max(lst_n)))
print("Minimum value={}".format(min(lst_n)))
```

    [5, 8, 11, 14, 17]
    The list length=5
    Maximum value=17
    Minimum value=5
    

* 分片(List Slices)


```python
lst_s=list(map(chr,range(100,110)))
print(lst_s)
print("_"*50)
print("[3:6]->{}".format(lst_s[3:6]))
print("[-3:-1]->{}".format(lst_s[-3:-1]))
print("[-3:]->{}".format(lst_s[-3:]))
print("[:3]->{}".format(lst_s[:3]))
print("[:]->{}".format(lst_s[:]))
```

    ['d', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm']
    __________________________________________________
    [3:6]->['g', 'h', 'i']
    [-3:-1]->['k', 'l']
    [-3:]->['k', 'l', 'm']
    [:3]->['d', 'e', 'f']
    [:]->['d', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm']
    

**列表索引示例**

<a href=""><img src="./imgs_pyd/004.png" height="auto" width="auto" title="caDesign"></a>


```python
help(map) #用help()方法查看说明
```

    Help on class map in module builtins:
    
    class map(object)
     |  map(func, *iterables) --> map object
     |  
     |  Make an iterator that computes the function using arguments from
     |  each of the iterables.  Stops when the shortest iterable is exhausted.
     |  
     |  Methods defined here:
     |  
     |  __getattribute__(self, name, /)
     |      Return getattr(self, name).
     |  
     |  __iter__(self, /)
     |      Implement iter(self).
     |  
     |  __next__(self, /)
     |      Implement next(self).
     |  
     |  __reduce__(...)
     |      Return state information for pickling.
     |  
     |  ----------------------------------------------------------------------
     |  Static methods defined here:
     |  
     |  __new__(*args, **kwargs) from builtins.type
     |      Create and return a new object.  See help(type) for accurate signature.
    
    


```python
help(chr)
```

    Help on built-in function chr in module builtins:
    
    chr(i, /)
        Return a Unicode string of one character with ordinal i; 0 <= i <= 0x10ffff.
    
    

* 带有步长(step length)参数的分片。List[a:b:s]

<a href=""><img src="./imgs_pyd/005.png" height="auto" width="auto" title="caDesign"></a>


```python
print(lst_s)
print("_"*50)
print("[0:10:2]->{}".format(lst_s[0:10:2]))
print("[::3]->{}".format(lst_s[::3]))
print("[9:3:-2]->{}".format(lst_s[9:3:-2]))
print("[20:3:-2]->{}".format(lst_s[20:3:-2]))
print("[7::-2]->{}".format(lst_s[7::-2]))
print("[:3:-2]->{}".format(lst_s[:3:-2]))
```

    ['d', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm']
    __________________________________________________
    [0:10:2]->['d', 'f', 'h', 'j', 'l']
    [::3]->['d', 'g', 'j', 'm']
    [9:3:-2]->['m', 'k', 'i']
    [20:3:-2]->['m', 'k', 'i']
    [7::-2]->['k', 'i', 'g', 'e']
    [:3:-2]->['m', 'k', 'i']
    

* 元素赋值+分片赋值+删除元素+列表相加+列表的乘法


```python
print(lst_s)
print("_"*50)
lst_s[5]=99 #元素赋值
print("lst_s[5]=99->{}".format(lst_s))
lst_none=lst_s+[None]*6
print("lst_s+[None]*6->{}".format(lst_none))
lst_none[13]=2015
print("lst_none[13]=2015->{}".format(lst_none))
lst_none[-6:-3]=list(range(100,104,2)) #分片赋值
print("lst_none[-6:-3]=list(range(100,104,2))->{}".format(lst_none))
lst_none[1:1]=[0,0,0,12]
print("lst_none[1:1]=[0,0,0,12]->{}".format(lst_none))
del lst_none[-2:] #删除元素
print("del lst_none[-2:]->{}".format(lst_none))
```

    ['d', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm']
    __________________________________________________
    lst_s[5]=99->['d', 'e', 'f', 'g', 'h', 99, 'j', 'k', 'l', 'm']
    lst_s+[None]*6->['d', 'e', 'f', 'g', 'h', 99, 'j', 'k', 'l', 'm', None, None, None, None, None, None]
    lst_none[13]=2015->['d', 'e', 'f', 'g', 'h', 99, 'j', 'k', 'l', 'm', None, None, None, 2015, None, None]
    lst_none[-6:-3]=list(range(100,104,2))->['d', 'e', 'f', 'g', 'h', 99, 'j', 'k', 'l', 'm', 100, 102, 2015, None, None]
    lst_none[1:1]=[0,0,0,12]->['d', 0, 0, 0, 12, 'e', 'f', 'g', 'h', 99, 'j', 'k', 'l', 'm', 100, 102, 2015, None, None]
    del lst_none[-2:]->['d', 0, 0, 0, 12, 'e', 'f', 'g', 'h', 99, 'j', 'k', 'l', 'm', 100, 102, 2015]
    

* 列表的方法

方法和函数是python语言编程中重要的两个概念，函数的表达方法是function(object)，function代表着将要执行的动作，object则是被执行的对象，例如list()是构建列表的函数，range()是构建区间的函数。方法实质上也是一种函数，只是不同对象具有不同的方法，方法表达的方式为object.method(item)，object为具有method方法的对象，可以是列表、字符串、序列等任何python中的对象，item为在method方法下操作的项值。

<a href=""><img src="./imgs_pyd/006.png" height="auto" width="auto" title="caDesign"></a>

> 函数和类在后文


```python
lst_s_2=list(map(chr,range(100,105)))
print(lst_s_2)
print("_"*50)
lst_s_2.append(99)
print("lst_s_2.append(99)->{}".format(lst_s_2))
lst_s_2.append(list(range(50,80,5)))
print("lst_s_2.append(list(range(50,80,5)))->{}".format(lst_s_2))
lst_spechars=['*',')','*']
lst_s_2.extend(lst_spechars)
print("lst_s_2.extend(lst_spechars)->{}".format(lst_s_2))
print("lst_s_2.count('*')={}".format(lst_s_2.count('*')))
print("lst_s_2.index('e')={}".format(lst_s_2.index('e')))
lst_s_2.insert(2,[1000,1200,1500])
print("lst_s_2.insert(2,[1000,1200,1500])->{}".format(lst_s_2))
print("lst_s_2.pop(7)_popup->{}".format(lst_s_2.pop(7)))
print("lst_s_2.pop(7)_retention->{}".format(lst_s_2))
lst_s_2.remove('*')
print("lst_s_2.remove('*')_retention->{}".format(lst_s_2))
list_n_2=[2,42,6,95,4,3]
list_n_2.sort()
print("list_n_2.sort()->{}".format(list_n_2))
```

    ['d', 'e', 'f', 'g', 'h']
    __________________________________________________
    lst_s_2.append(99)->['d', 'e', 'f', 'g', 'h', 99]
    lst_s_2.append(list(range(50,80,5)))->['d', 'e', 'f', 'g', 'h', 99, [50, 55, 60, 65, 70, 75]]
    lst_s_2.extend(lst_spechars)->['d', 'e', 'f', 'g', 'h', 99, [50, 55, 60, 65, 70, 75], '*', ')', '*']
    lst_s_2.count('*')=2
    lst_s_2.index('e')=1
    lst_s_2.insert(2,[1000,1200,1500])->['d', 'e', [1000, 1200, 1500], 'f', 'g', 'h', 99, [50, 55, 60, 65, 70, 75], '*', ')', '*']
    lst_s_2.pop(7)_popup->[50, 55, 60, 65, 70, 75]
    lst_s_2.pop(7)_retention->['d', 'e', [1000, 1200, 1500], 'f', 'g', 'h', 99, '*', ')', '*']
    lst_s_2.remove('*')_retention->['d', 'e', [1000, 1200, 1500], 'f', 'g', 'h', 99, ')', '*']
    list_n_2.sort()->[2, 3, 4, 6, 42, 95]
    

### 3.3.3 tuple
元组和列表类似，只是元组不能够修改，但是可以提取项值。元组的语法为tuple=(value1,value2,value3,..)，用小括号括起，中间与列表一样由“，”号隔开。最后也需要点逗号。


```python
tuple_1=2,3,5,
print("tuple_1=2,3,5,->{}".format(tuple_1))
print("3*(20*3,)->{}".format(3*(20*3,)))
print("tuple((5,8,9))->{}".format(tuple((5,8,9)))) #用()
print("tuple([5,8,9])->{}".format(tuple([5,8,9]))) #用[]
```

    tuple_1=2,3,5,->(2, 3, 5)
    3*(20*3,)->(60, 60, 60)
    tuple((5,8,9))->(5, 8, 9)
    tuple([5,8,9])->(5, 8, 9)
    

### 3.3.4 dict
字典是Python中另一种数据结构，语法结构为dictionary={ke0:value0,key1:value1,key2:value2,...}，由大括号包含的多个键（Key）：值（Value）对，直接用“,"逗号隔开，键/值对之间用":"冒号隔开。键和值可以是任何类型的数据，例如数字、字符串、元组，而每一组键/值并没有固定的顺序，这样只需要考虑使用键寻找值的方法，而不用像列表一样计算项值位置的索引值再提取项值。

<a href=""><img src="./imgs_pyd/007.png" height="auto" width="auto" title="caDesign"></a>

* 使用dict()函数建立字典


```python
import random
items=[(0,[i for i in range(5)]),(1,[random.sample(list(range(100,200,1)),3)]),(2,'python')] #[i for i in range(5)] 为列表推导式 list comprehension/derivation
print("items->{}".format(items))
dic=dict(items)
print("dic=dict(items)->{}".format(dic))
print("dic[1]->{}".format(dic[1]))
```

    items->[(0, [0, 1, 2, 3, 4]), (1, [[160, 154, 177]]), (2, 'python')]
    dic=dict(items)->{0: [0, 1, 2, 3, 4], 1: [[160, 154, 177]], 2: 'python'}
    dic[1]->[[160, 154, 177]]
    

* 字典的基本操作


```python
print("len(dic)->{}".format(len(dic)))
dic[3]=(random.random(),random.uniform(200,300))
print("dic[3]=(random.random(),random.uniform(200,300))->{}".format(dic))
del dic[1]
print("del dic[1]->{}".format(dic))
print("3 in dic->{}".format(3 in dic))
print("5 in dic->{}".format(5 in dic))
print("dic.keys()->{}".format(dic.keys())) #应该放在字典的方法一节里
print("dic.values()->{}".format(dic.values()))
print("dic.items()->{}".format(dic.items()))
print("_"*50)
print("list(dic.keys())->{}".format(list(dic.keys())))
```

    len(dic)->3
    dic[3]=(random.random(),random.uniform(200,300))->{0: [0, 1, 2, 3, 4], 1: [[160, 154, 177]], 2: 'python', 3: (0.027748392659592946, 220.63978785754304)}
    del dic[1]->{0: [0, 1, 2, 3, 4], 2: 'python', 3: (0.027748392659592946, 220.63978785754304)}
    3 in dic->True
    5 in dic->False
    dic.keys()->dict_keys([0, 2, 3])
    dic.values()->dict_values([[0, 1, 2, 3, 4], 'python', (0.027748392659592946, 220.63978785754304)])
    dic.items()->dict_items([(0, [0, 1, 2, 3, 4]), (2, 'python'), (3, (0.027748392659592946, 220.63978785754304))])
    __________________________________________________
    list(dic.keys())->[0, 2, 3]
    


```python
for k,v in enumerate(dic.items()): #for循环在后文
    print(k,v)
```

    0 (0, [0, 1, 2, 3, 4])
    1 (2, 'python')
    2 (3, (0.027748392659592946, 220.63978785754304))
    

* 字典的方法


```python
lst_A=list(range(6,20,3))
lst_B=list(range(100,150,15))
print("lst_A={},lst_B={}".format(lst_A,lst_B))
dic_2={0:lst_A,1:lst_B}
print("dic_2={}".format(dic_2))
print("_"*50)
dic_assignment=dic_2
print("dic_assignment={}".format(dic_assignment))
dic_2.clear()
print("dic_2.clear()->{}".format(dic_2))
print("dic_assignment={}".format(dic_assignment))
dic_2[5]=list(range(1,9,2))
print("dic_2[5]=list(range(1,9,2))->{}".format(dic_2))
dic_copy=dic_2.copy()
print("dic_copy=dic_2.copy()->{}".format(dic_copy))
dic_2[8]=[5,7]
print("dic_2[8]=[5,7]->{}".format(dic_2))
print("dic_copy={}".format(dic_copy))
dic_copy[5].remove(5)
print("dic_copy[5].remove(5)->{}".format(dic_copy))
dic_copy.setdefault(6,[77,99]) #返回指定键的值，如果不存在该键，则字典增加新的键/值对
print("dic_copy.setdefault(6,[77,99])->{}".format(dic_copy))
dic_2.pop(5) #移除指定键/值，并返回该值
print("dic_2.pop(5)->{}".format(dic_2))
dic_update={8:[5,7,6,3,2],9:[3,2,33,55,66]}
print("dic_update={}".format(dic_update))
dic_2.update(dic_update) #更新字典
print("dic_2.update(dic_update->{}".format(dic_2))
print("dic_2.get(9)->{}".format(dic_2.get(9)))
dic_2.popitem() #随即弹出一对键/值，并在该字典中移除
print("dic_2.popitem()->{}".format(dic_2))

dic_3={}.fromkeys([0,1,2,3,4,'A']) #给定键，建立值为空的字典
print("dic_3={}"+".fromkeys([0,1,2,3,4,'A'])->{}".format(dic_3)) #找下escape characters 脱字符
```

    lst_A=[6, 9, 12, 15, 18],lst_B=[100, 115, 130, 145]
    dic_2={0: [6, 9, 12, 15, 18], 1: [100, 115, 130, 145]}
    __________________________________________________
    dic_assignment={0: [6, 9, 12, 15, 18], 1: [100, 115, 130, 145]}
    dic_2.clear()->{}
    dic_assignment={}
    dic_2[5]=list(range(1,9,2))->{5: [1, 3, 5, 7]}
    dic_copy=dic_2.copy()->{5: [1, 3, 5, 7]}
    dic_2[8]=[5,7]->{5: [1, 3, 5, 7], 8: [5, 7]}
    dic_copy={5: [1, 3, 5, 7]}
    dic_copy[5].remove(5)->{5: [1, 3, 7]}
    dic_copy.setdefault(6,[77,99])->{5: [1, 3, 7], 6: [77, 99]}
    dic_2.pop(5)->{8: [5, 7]}
    dic_update={8: [5, 7, 6, 3, 2], 9: [3, 2, 33, 55, 66]}
    dic_2.update(dic_update->{8: [5, 7, 6, 3, 2], 9: [3, 2, 33, 55, 66]}
    dic_2.get(9)->[3, 2, 33, 55, 66]
    dic_2.popitem()->{8: [5, 7, 6, 3, 2]}
    dic_3={}.fromkeys([0,1,2,3,4,'A'])->{0: None, 1: None, 2: None, 3: None, 4: None, 'A': None}
    

> 最好分组记忆

d.keys()+d.values()+d.items()

d.get()+d.setdefault()

d.pop()+d.popitem()

d.copy()

d.update()

d.fromkeys()

### 3.3.5 string-基础

* 常用方法


```python
lst_s_3=list("Hello Python!")
print("lst_s_3=list(\"Hello Python!\")->{}".format(lst_s_3)) #"\" escape character
print("\"Hellow\"+\" Python!\"->{}".format("Hellow"+" Python!"))
print("\"+\".join(str(123456))->{}".format("+".join(str(123456))))
print("len(\"Hellow Python!\")->{}".format(len("Hellow Python!")))
coordinates="120.132007,30.300508,9.7"
print("coordinates.split(\",\")->{}".format(coordinates.split(",")))
print("eval(coordinates)->{}".format(eval(coordinates))) #通常用eval()方法将字符串，转换为对应数值形式；
print("\"Hello Python!\".lower()->{}".format("Hello Python!".lower()))
print("\"Hello Python!\".upper()->{}".format("Hello Python!".upper()))
print("\"Hello Python!\"[6:]->{}".format("Hello Python!"[6:]))
print("\"    Hello Python!    \".strip()->{}".format("    Hello Python!    ".strip()))
print("\"Hello Python!\".replace(\"Python\",\"Grasshopper\")->{}".format("Hello Python!".replace("Python","Grasshopper")))
hello_lst=list("Hello Python!")
hello_lst.sort()
print("hello_lst.sort()>{}".format(hello_lst))
print("\"Hello Python!\".find(\"Py\")->{}".format("Hello Python!".find("Py")))
```

    lst_s_3=list("Hello Python!")->['H', 'e', 'l', 'l', 'o', ' ', 'P', 'y', 't', 'h', 'o', 'n', '!']
    "Hellow"+" Python!"->Hellow Python!
    "+".join(str(123456))->1+2+3+4+5+6
    len("Hellow Python!")->14
    coordinates.split(",")->['120.132007', '30.300508', '9.7']
    eval(coordinates)->(120.132007, 30.300508, 9.7)
    "Hello Python!".lower()->hello python!
    "Hello Python!".upper()->HELLO PYTHON!
    "Hello Python!"[6:]->Python!
    "    Hello Python!    ".strip()->Hello Python!
    "Hello Python!".replace("Python","Grasshopper")->Hello Grasshopper!
    hello_lst.sort()>[' ', '!', 'H', 'P', 'e', 'h', 'l', 'l', 'n', 'o', 'o', 't', 'y']
    "Hello Python!".find("Py")->6
    

* 字符串格式化

%s称为转换说明符(conversion specifier)

<a href=""><img src="./imgs_pyd/008.png" height="auto" width="auto" title="caDesign"></a>


```python
format_str="Hello,%s and %s!"
values=("Python","Grasshopper")
new_str=format_str % values
print("new_str=format_str % values->{}".format(new_str))

format_str_2="Pi with three decimals:%.3f,and enter a value with percent sign:%.2f %%" #如果字符串里包含实际的%，则通过%%即两个百分号进行转义

from math import pi
new_str_2=format_str_2 % (pi,3.1415926)
print("new_str_2=format_str_2 % (pi,3.1415926)->{}".format(new_str_2))
```

    new_str=format_str % values->Hello,Python and Grasshopper!
    new_str_2=format_str_2 % (pi,3.1415926)->Pi with three decimals:3.142,and enter a value with percent sign:3.14 %
    

除了转换说明符%外，可以增加转换标志，-表示左对齐，+表示在转换值之前要加上正负号，“”空白字符，表示正数之前保留空格，0表示转换值若位数不够则用0填充；如果格式化字符串中包含*，那么可以在值元组中设置宽度；(.)点号加数字表示小数点后的位数精度，如果转换的是字符串，那么该数字则表示最大字段宽度。转换类型参看表：


| 转换类型  | 含义                             |
|-------|--------------------------------|
| d,i   | 带符号的十进制整数                      |
| o     | 不带符号的八进制                       |
| u     | 不带符号的十进制                       |
| x     | 不带符号的十六进制（小写）                  |
| X     | 不带符号的十六进制（大写）                  |
| e     |  科学计数法表示的浮点数（小写）               |
| E     | 科学计数法表示的浮点数（大写）                |
| f,F   | 十进制浮点数                         |
| g     | 如果指数大于-4或者小于精度值则和ｅ相同，其它情况与ｆ相同  |
| G     | 如果指数大于-4或者小于精度值则和E相同，其它情况与F相同  |
| C     | 单字符（接受整数或者单字符字符串）              |
| r     | 字符串（使用repr转换任意Python对象)        |
| s     | 字符串（使用str转换任意Python对象)         |


```python
format_str_3="%10f,%10.2f,%.2f,%.5s,%.*s,%d,%x,%f"
new_str_3=format_str_3%(pi,pi,pi,"Hello Python!",5,"Hello Python!",52,52,pi)
print("{}".format(new_str_3))
```

      3.141593,      3.14,3.14,Hello,Hello,52,34,3.141593
    

* Template()函数-模板字符串+$+st.substitute()


```python
from string import Template
s_template_1=Template("$x,glorious,$x!")
s_1=s_template_1.substitute(x="Python")
print("s_1=s_template_1.substitute(x=\"Python\")->{}".format(s_1))
s_template_2=Template("${x}thon is amazing!")
s_2=s_template_2.substitute(x="py")
print("s_2=s_template_2.substitute(x=\"py\")->{}".format(s_2))
s_template_3=Template("$x and $y are both amazing!")
substitute_dict=dict([('x','Python'),('y','Grasshopper')])
print("substitute_dict={}".format(substitute_dict))
s_3=s_template_3.substitute(substitute_dict)
print("s_3=s_template_3.substitute(substitute_dict)->{}".format(s_3))
```

    s_1=s_template_1.substitute(x="Python")->Python,glorious,Python!
    s_2=s_template_2.substitute(x="py")->python is amazing!
    substitute_dict={'x': 'Python', 'y': 'Grasshopper'}
    s_3=s_template_3.substitute(substitute_dict)->Python and Grasshopper are both amazing!
    

### 3.3.6 string-re(regular expression)正则表达式

字符串处理常常用到标准库模块中的re(regular expression)正则表达式，正则表达式非常强大，可以处理更复杂的字符串，其本质是可以匹配文本片断的模式。最简单的正则表达式是普通字符串，即大多数字母和字符一般都会和自身匹配，例如正则表达式'python'可以匹配字符串'python'。


```python
import re
pattern_1='Python'
text="Hello Python!"
print("re.findall(pattern_1,text)->{}".format(re.findall(pattern_1,text)))
pattern_2='python'
print("re.findall(pattern_2,text)->{}".format(re.findall(pattern_2,text)))
```

    re.findall(pattern_1,text)->['Python']
    re.findall(pattern_2,text)->[]
    

* 字符匹配-模式语法

<a href=""><img src="./imgs_pyd/009.jpg" height="auto" width="auto" title="caDesign"></a><a href=""><img src="./imgs_pyd/010.jpg" height="auto" width="auto" title="caDesign"></a>

**(.)点号**


```python
print("re.findall('.ython','Hello Python!')->{}".format(re.findall('.ython','Hello Python!')))
print("re.findall('.ython','Hello gython!')->{}".format(re.findall('.ython','Hello gython!')))
print("re.findall('.ython','Hello gPython!')->{}".format(re.findall('.ython','Hello gPython!')))
print("re.findall('.ython','Hello Pthon!')->{}".format(re.findall('.ython','Hello Pthon!')))
```

    re.findall('.ython','Hello Python!')->['Python']
    re.findall('.ython','Hello gython!')->['gython']
    re.findall('.ython','Hello gPython!')->['Python']
    re.findall('.ython','Hello Pthon!')->[]
    

**\* + ？ {m,n}与r’string’**


```python
print("re.findall(r'w?cadesign\.cn,w+\.cadesign\.cn','cadesign.cn,www.cadesign.cn')->{}".format(re.findall(r'w?cadesign\.cn,w+\.cadesign\.cn','cadesign.cn,www.cadesign.cn')))
print("re.findall(r'w{2}"+"\.cadesign\.cn','www.cadesign.cn')->{}".format(re.findall(r'w{2}\.cadesign\.cn','www.cadesign.cn')))
print("re.findall(r'w{1,3}"+"\.cadesign\.cn','www.cadesign.cn')->{}".format(re.findall(r'w{1,3}\.cadesign\.cn','www.cadesign.cn')))
```

    re.findall(r'w?cadesign\.cn,w+\.cadesign\.cn','cadesign.cn,www.cadesign.cn')->['cadesign.cn,www.cadesign.cn']
    re.findall(r'w{2}\.cadesign\.cn','www.cadesign.cn')->['ww.cadesign.cn']
    re.findall(r'w{1,3}\.cadesign\.cn','www.cadesign.cn')->['www.cadesign.cn']
    

**[] (^)**


```python
print("re.findall('[Py]*thon!','Hello Python!')->{}".format(re.findall('[Py]*thon!','Hello Python!')))
print("re.findall('[Py]*thon!','Hello Pthon!')->{}".format(re.findall('[Py]*thon!','Hello Pthon!')))
print("re.findall('[Py]*thon!','Hello ython!')->{}".format(re.findall('[Py]*thon!','Hello ython!')))
print("re.findall('[Py]*thon!','Hello ython!')->{}".format(re.findall('[Py]*thon!','Hello thon!')))
```

    re.findall('[Py]*thon!','Hello Python!')->['Python!']
    re.findall('[Py]*thon!','Hello Pthon!')->['Pthon!']
    re.findall('[Py]*thon!','Hello ython!')->['ython!']
    re.findall('[Py]*thon!','Hello ython!')->['thon!']
    

**(|)管道符号**


```python
print("re.findall('python|grasshopper','python')->{}".format(re.findall('python|grasshopper','python')))
print("re.findall('python|grasshopper','grasshopper')->{}".format(re.findall('python|grasshopper','grasshopper')))
print("re.findall('python|grasshopper','grasshopper and python')->{}".format(re.findall('python|grasshopper','grasshopper and python')))
```

    re.findall('python|grasshopper','python')->['python']
    re.findall('python|grasshopper','grasshopper')->['grasshopper']
    re.findall('python|grasshopper','grasshopper and python')->['grasshopper', 'python']
    

**\d \D**


```python
print("re.findall('\d','number=10')->{}".format(re.findall('\d','number=10')))
print("re.findall('\D','number=10')->{}".format(re.findall('\D','number=10')))
print("re.findall('[^0-9]','number=10')->{}".format(re.findall('[^0-9]','number=10')))
```

    re.findall('\d','number=10')->['1', '0']
    re.findall('\D','number=10')->['n', 'u', 'm', 'b', 'e', 'r', '=']
    re.findall('[^0-9]','number=10')->['n', 'u', 'm', 'b', 'e', 'r', '=']
    

* re模块的方法


```python
print("re.findall('[a-z]','python')->{}".format(re.findall('[a-z]','python-3.0')))
print("re.search('[a-z]+','python')->{}".format(re.search('[a-z]+','python')))
if re.search('[a-z]+','python'):
    print("re.search('[a-z]+','python')->found it!")
print("re.split(',','Hello,,,,,,Python!')->{}".format(re.split(',','Hello,,,,,,Python!')))
print("re.sub('Python','Grasshopper','Hello Python!')->{}".format(re.sub('Python','Grasshopper','Hello Python!')))

pattern_compile=re.compile('Python')
print("pattern_compile.findall('Hello,,,,,,Python!')->{}".format(pattern_compile.findall('Hello,,,,,,Python!')))

if re.match('p','python'):
    print("re.match('p','python')->found it!")
```

    re.findall('[a-z]','python')->['p', 'y', 't', 'h', 'o', 'n']
    re.search('[a-z]+','python')-><re.Match object; span=(0, 6), match='python'>
    re.search('[a-z]+','python')->found it!
    re.split(',','Hello,,,,,,Python!')->['Hello', '', '', '', '', '', 'Python!']
    re.sub('Python','Grasshopper','Hello Python!')->Hello Grasshopper!
    pattern_compile.findall('Hello,,,,,,Python!')->['Python']
    re.match('p','python')->found it!
    


```python
print("\'python\'.find(\'python\')->{}".format('python'.find('python')))
print("\'python\'.find(\'thon\')->{}".format('python'.find('thon')))
print("\'python\'.find(\'a\')->{}".format('python'.find('a')))
print("\'p\' in \'python\'->{}".format('p' in 'python'))
```

    'python'.find('python')->0
    'python'.find('thon')->2
    'python'.find('a')->-1
    'p' in 'python'->True
    


```python
print("\'Hello,,,,,,Python!\'.split(',')->{}".format( 'Hello,,,,,,Python!'.split(',')))
print("\'Hello Python!\'.replace(\'Python\',\'Grasshopper\')->{}".format( 'Hello Python!'.replace('Python','Grasshopper')))
```

    'Hello,,,,,,Python!'.split(',')->['Hello', '', '', '', '', '', 'Python!']
    'Hello Python!'.replace('Python','Grasshopper')->Hello Grasshopper!
    

* 匹配对象和组

re.search()和re.match()返回的MatchObject实例包含关于分组内容的信息，和匹配值的位置数据。那么什么是组？组就是放置在圆括号内的子模式，例如模式r'www\.(.*)\..{3}'中(.\*)即为组，组可以并行与嵌套多个，并可以通过m.group()方法返回组，m.start()方法获取组的开始索引值，m.end()则获取结束位置索引值，m.span()返回区间值。


```python
match_1=re.match(r'www\.(.*)\..{3}','www.python.org')
print("match_1.gourp(1)->{}".format(match_1.group(1)))
print("match_1.start(1)->{}".format(match_1.start(1)))
print("match_1.end(1)->{}".format(match_1.end(1)))
print("match_1.span(1)->{}".format(match_1.span(1)))
match_2=re.match(r'www\.(.*)\.(.{3})','www.python.org')
print("match_2.group(1)->{}".format(match_2.group(1)))
print("match_2.group(2)->{}".format(match_2.group(2)))
```

    match_1.gourp(1)->python
    match_1.start(1)->4
    match_1.end(1)->10
    match_1.span(1)->(4, 10)
    match_2.group(1)->python
    match_2.group(2)->org
    

### 3.3.7 array(numpy)

### 3.3.8 DataFrame(pandas) | GeoDataFrame(Geopandas)

## 3.4 基本语句
### 3.4.1 循环语句

* for循环


```python
lst_1=list(range(29,37,2))
print("lst_1={}".format(lst_1))
print("_"*50)
print("for i in lst_1:")
for i in lst_1:
    print(i)
print("for i in range(len(lst_1)):")
for i in range(len(lst_1)):
    print("idx={},val={}".format(i,lst_1[i]))
print("+"*50)   
dic_4=dict(a=2,b=3,c=6,d=0)
print("dic_4={}".format(dic_4))
print("_"*50)
print("for k in dic_4:")
for k in dic_4:
    print(k)
print("for k,v in dic_4.items():")
for k,v in dic_4.items():
    print("key={},val={}".format(k,v))
```

    lst_1=[29, 31, 33, 35]
    __________________________________________________
    for i in lst_1:
    29
    31
    33
    35
    for i in range(len(lst_1)):
    idx=0,val=29
    idx=1,val=31
    idx=2,val=33
    idx=3,val=35
    ++++++++++++++++++++++++++++++++++++++++++++++++++
    dic_4={'a': 2, 'b': 3, 'c': 6, 'd': 0}
    __________________________________________________
    for k in dic_4:
    a
    b
    c
    d
    for k,v in dic_4.items():
    key=a,val=2
    key=b,val=3
    key=c,val=6
    key=d,val=0
    

* while循环


```python
x=1
while x<=100:
    print("x={}".format(x))
    x+=10 #增量赋值; x*=2     
```

    x=1
    x=11
    x=21
    x=31
    x=41
    x=51
    x=61
    x=71
    x=81
    x=91
    


```python
x=1
while x<=100:
    print("x={}".format(x))
    x+=10 
    if x>=50:break    
    
import sys
print("sys.maxsize={}".format(sys.maxsize))
for i in range(sys.maxsize):
    print("i={}".format(i))  #?
    i+=10
    if i>=30:break
```

    x=1
    x=11
    x=21
    x=31
    x=41
    sys.maxsize=9223372036854775807
    i=0
    i=1
    i=2
    i=3
    i=4
    i=5
    i=6
    i=7
    i=8
    i=9
    i=10
    i=11
    i=12
    i=13
    i=14
    i=15
    i=16
    i=17
    i=18
    i=19
    i=20
    

**While True/break语句**


```python
topography_fp='./data/elevation.txt' #在处理GIS时，更多的是使用GeoPandas等处理地理信息的库;另，大数据的存储读取通常用Numpy,(Geo)Pandas提供的方法，以及HDF5(binary data format)。
f=open(topography_fp,'r')
elevation_dat=[]
while True:
    line=f.readline()
    elevation_dat.append(line)
    if not line:break
f.close()
print("elevation_dat[:10]={}".format(elevation_dat[:10]))
```

    elevation_dat[:10]=['43.963,6738.623,313.075 \n', '43.963,6688.623,330.3 \n', '43.963,6638.623,343.366 \n', '43.963,6588.623,355.478 \n', '43.963,6538.623,365.184 \n', '43.963,6488.623,362.951 \n', '43.963,6438.623,339.023 \n', '43.963,6388.623,313.132 \n', '43.963,6338.623,304.689 \n', '43.963,6288.623,304.635 \n']
    


```python
import pandas as pd
elevation_df=pd.read_csv(topography_fp, delimiter = ",",header=None) 
print(elevation_df)
```

                  0         1        2
    0        43.963  6738.623  313.075
    1        43.963  6688.623  330.300
    2        43.963  6638.623  343.366
    3        43.963  6588.623  355.478
    4        43.963  6538.623  365.184
    ...         ...       ...      ...
    14440  5343.963   238.623   31.519
    14441  5343.963   188.623   31.010
    14442  5343.963   138.623   30.498
    14443  5343.963    88.623   29.984
    14444  5343.963    38.623   29.467
    
    [14445 rows x 3 columns]
    

* 迭代的工具

**并行迭代**


```python
lst_a=[0,1,2,3]
lst_b=['point_a','point_b','point_c','point_d']
zip_lst=zip(lst_a,lst_b) #The zip() function takes iterables (can be zero or more), aggregates them in a tuple, and returns it.
print("zip_lst=zip(lst_a,lst_b)->{}".format(zip_lst))
print("dict(zip_lst)->{}".format(dict(zip_lst)))

zip_lst=zip(lst_a,lst_b) #迭代对象临时存储，读取完后为空
for a,b in zip_lst:
    print(a,b)
    
zip_lst=zip(lst_a,lst_b)
a,b=zip(*zip_lst)
print("a={},b={}".format(a,b))
```

    zip_lst=zip(lst_a,lst_b)-><zip object at 0x000001DA3FCF16C0>
    dict(zip_lst)->{0: 'point_a', 1: 'point_b', 2: 'point_c', 3: 'point_d'}
    0 point_a
    1 point_b
    2 point_c
    3 point_d
    a=(0, 1, 2, 3),b=('point_a', 'point_b', 'point_c', 'point_d')
    

**编号迭代**


```python
lst_c=['point_a','point_b','point_c','point_d']
dic_4={}
for idx,value in enumerate(lst_c):
    dic_4[idx]=value
print("dic_4={}".format(dic_4))
print("dict((i,v) for i,v in enumerate(lst_c))->{}".format(dict((i,v) for i,v in enumerate(lst_c)))) #list comprehension
print("_"*50)
for i,(a,b) in enumerate(zip(lst_a,lst_b)):
    print('%d:%s,%s'%(i,a,b))
```

    dic_4={0: 'point_a', 1: 'point_b', 2: 'point_c', 3: 'point_d'}
    dict((i,v) for i,v in enumerate(lst_c))->{0: 'point_a', 1: 'point_b', 2: 'point_c', 3: 'point_d'}
    __________________________________________________
    0:0,point_a
    1:1,point_b
    2:2,point_c
    3:3,point_d
    

**list comprehension(列表推导式)**

`[expr(val) for val in collection if condition]  /newlist=[expression for item in iterable if condition == True]  | [expression if condition else expression for item in iterable <if condition>]`


```python
print("[x*x for x in range(3,37,5) if x%2==0]->{}".format([x*x for x in range(3,37,5) if x%2==0]))
print("[(x,y) for x in range(3)for y in range(2)]->{}".format([(x,y) for x in range(3)for y in range(2)]))
print("[(x,y) for x,y in zip(range(3),range(2))]->{}".format([(x,y) for x,y in zip(range(3),range(2))]))
nested_list=[[a for a in range(1,10,3)],2,3,[b for b in range(60,100,7)],[[c for c in range(1000,2000,120)],3,9]]
print("[[a for a in range(1,10,3)],2,3,[b for b in range(60,100,7)],[[c for c in range(1000,2000,120)],3,9]]->{}".format(nested_list)) #嵌套列表推导式

flatten_lst=lambda lst: [m for n_lst in lst for m in flatten_lst(n_lst)] if type(lst) is list else [lst] #展平列表常用，可以放置到单独的.py文件中调用。lambda函数后文阐述
print("flatten_lst(nested_list)->{}".format(flatten_lst(nested_list)))
```

    [x*x for x in range(3,37,5) if x%2==0]->[64, 324, 784]
    [(x,y) for x in range(3)for y in range(2)]->[(0, 0), (0, 1), (1, 0), (1, 1), (2, 0), (2, 1)]
    [(x,y) for x,y in zip(range(3),range(2))]->[(0, 0), (1, 1)]
    [[a for a in range(1,10,3)],2,3,[b for b in range(60,100,7)],[[c for c in range(1000,2000,120)],3,9]]->[[1, 4, 7], 2, 3, [60, 67, 74, 81, 88, 95], [[1000, 1120, 1240, 1360, 1480, 1600, 1720, 1840, 1960], 3, 9]]
    flatten_lst(nested_list)->[1, 4, 7, 2, 3, 60, 67, 74, 81, 88, 95, 1000, 1120, 1240, 1360, 1480, 1600, 1720, 1840, 1960, 3, 9]
    

### 3.4.2 条件语句
**条件语句模式**

```python
if expression:
    statements
elif expression:
    statements
elif expression:
    pass
...
else:
    statements
```


```python
x=10
if x<0:
    print('It is negative.')
elif x==0:
    print('Equal to zero.')
elif 0<x<10:
    print('Positive but smaller than 10')
else:
    print('Positive and larger than or equal to 10.')
```

    Positive and larger than or equal to 10.
    

**条件的判断**

| 表达式         | 描述              |
|-------------|-----------------|
| x==y        | x等于y            |
| x<y         | x小于y            |
| x>y         |  x大于y           |
| x>=y        |  x大于等于y         |
| x<=y        |  x小于等于y         |
| x!=y        | x不等于y           |
| x is y      |  x和y是同一个对象      |
| x is not y  | x和y是不同的对象       |
| x in y      | x是y容器（例如列表）的成员  |
| x not in y  | x不是y容器（例如列表）的成员 |

相等运算符==与同一性运算符is


```python
x=y=[3,6,9]
z=[3,6,9]
print("x==y->{}".format(x==y))
print("x is y->{}".format(x is y))
print("x==z->{}".format(x==z))
print("x is z->{}".format(x is z))
print("x is not y->{}".format(x is not y))
print("x is not z->{}".format(x is not z))
print("id_x:{};id_y:{};id_z:{}".format(id(x),id(y),id(z))) #Memory Location

del x[2]
print("x={},y={},z={} after del x[2]".format(x,y,z))
```

    x==y->True
    x is y->True
    x==z->True
    x is z->False
    x is not y->False
    x is not z->True
    id_x:2036884737152;id_y:2036884737152;id_z:2036881713600
    x=[3, 6],y=[3, 6],z=[3, 6, 9] after del x[2]
    

成员资格运算符，in与not in


```python
x=[3,6,9]
print("3 in x->{}".format(3 in x))
print("0 in x->{}".format(0 in x))
print("3 not in x->{}".format(3 not in x))
print("0 not in x->{}".format(0 not in x))
```

    3 in x->True
    0 in x->False
    3 not in x->False
    0 not in x->True
    

布尔运算符


```python
a,b,c=0,10,15
if c>a and c<b:
    print('a<c<b')
else: print('a<c<b kidding!!!')
```

    a<c<b kidding!!!
    

## 3.5 函数 function
### 3.5.1 定义函数
```
def function(arguments):
    statement
    ...
    return values
```


```python
def simple_func(x,y):
    z=pow(x,2)+y
    return z
```


```python
print("simple_func(3,7)->{}".format(simple_func(3,7)))
print("simple_func(7,3)->{}".format(simple_func(7,3)))
print("simple_func(y=7,x=3)->{}".format(simple_func(y=7,x=3)))
```

    simple_func(3,7)->16
    simple_func(7,3)->52
    simple_func(y=7,x=3)->16
    

* 定义斐波那契数列(Successione di Fibonacci)函数

<a href=""><img src="./imgs_pyd/011.jpg" height="auto" width="auto" title="caDesign"></a>  

数学定义：
```
F_0=0
F_1=1
F_n=F_{n-1}+ F_{n-2}（n≧2）
```

**0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233……**


```python
def fibonacci(s,count): #定义fib函数的方法较笨
    fib_lst=[0,1]
    fib_part=[]
    if s==0 or s==1:
        fib_part[:]=fib_lst
        for i in range(count-2):
            fib_part.append(fib_part[-1]+fib_part[-2])
    else:
        while True:
            fib_lst[:]=[fib_lst[1],fib_lst[0]+fib_lst[1]]
            #print(fib_lst)
            if fib_lst[1]-s>=0:break
        fib_part[:]=fib_lst
        if abs(fib_lst[0]-s)>=abs(fib_lst[1]-s):
            for i in range(count-1):
                fib_part.append(fib_part[-1]+fib_part[-2])
            fib_part.pop(0)
        else:
            for i in range(count-2):
                fib_part.append(fib_part[-1]+fib_part[-2])
    return fib_part

print("fibonacci(6,9)->{}".format(fibonacci(6,9)))
print("fibonacci(7,9)->{}".format(fibonacci(7,9)))
```

    fibonacci(6,9)->[5, 8, 13, 21, 34, 55, 89, 144, 233]
    fibonacci(7,9)->[8, 13, 21, 34, 55, 89, 144, 233, 377]
    

### 3.5.2 递归
递归recursion-在数学与计算机科学中，是指在函数的定义中使用函数自身的方法。一般来讲递归包含两个部分：

• 当函数直接返回值时有基本实例（最小可能性问题）；

• 递归实例，包括一个或者多个问题最小部分的递归调用；

这里的关键就是将问题分成为小部分，递归不能永远继续下去，因为它总是以最小可能性问题结束，而这些问题又存贮在基本实例中。每次函数被调用时，针对这个调用的新命名空间会被创建，意味着当函数调用“自身”时，实际上运行的是两个不同的函数（或者说是同一个函数具有两个不同的命名空间）。

**用递归定义阶乘 factorial**

<a href=""><img src="./imgs_pyd/013.png" height="auto" width="auto" title="caDesign"></a>  



```python
def factorial(n):
    if n==1:
        return 1
    else:
        return n*factorial(n-1)
print(factorial(7))
```

    5040
    

## 3.6 类 class
### 3.6.1 定义类

事物认知过程中，总是习惯将世界中存在的对象进行分类，例如鸟类、鱼类、花草等，在Rhinoceros几何形式中则有点、线、曲面、格网等。每个分类之所以能够归为一类，是因为该类的对象具有共同的属性特征，例如鸟类一般都能够飞翔，具有翅膀，身披羽毛等，而鱼类则生活在水中，用腮呼吸，用鳍辅助身体平衡与运动；而点具有坐标，线具有曲率、长度、方向，曲面具有表面积、截面线等。每一类都有每一类的属性特点，也具有该类的一些行为方法，但是每一个类下又有很多子类，例如鸟类又分雨燕目、 鸽形目、 雁形目等，而雨燕目之下由有科，科下则为具体的对象例如黑雨燕、珍雨燕、小雨燕等。

对于人类认知世界的分类系统，在Python语言中有与之类似的程序编写方法-类。例如创建一个鸟的类：

<a href=""><img src="./imgs_pyd/014.png" height="auto" width="auto" title="caDesign"></a>  



```python
class Bird:
    fly='Whirring' #美 /wɜːr/ 
    def __init__(self):
        self.hungry=True
    def eat(self):
        if self.hungry:
            print('Aaaah...')
            self.hungry=False
        else:
            print('No.Thanks!')

class Apodidae(Bird):  #/'æpədədi:/
    def __init__(self):
        super(Apodidae,self).__init__()
        self.sound='Squawk!' #美 /skwɔːk/ 
    def sing(self):
        print(self.sound)
```


```python
swift=Apodidae()
print("swift.fly->{}".format(swift.fly))
print("swift.eat()->")
swift.eat()
print("swift.eat()->")
swift.eat()
print("swift.sing()->")
swift.sing()
```

    swift.fly->Whirring
    swift.eat()->
    Aaaah...
    swift.eat()->
    No.Thanks!
    swift.sing()->
    Squawk!
    

* 类与类的实例


```python
blackswift=Apodidae()
scarceswift=Apodidae()
print("blackswift.sing()->")
blackswift.sing()
print("scarceswift.sing()->")
scarceswift.sing()
```

    blackswift.sing()->
    Squawk!
    scarceswift.sing()->
    Squawk!
    

* 类的属性


```python
print("blackswift.fly->{}".format(blackswift.fly))
blackswift.fly='humming' #重新赋予实例的属性
print("blackswift.fly after redefining the blackswif's attribute->{}".format(blackswift.fly))
print("scarceswift.fly->{}".format(scarceswift.fly))
```

    blackswift.fly->Whirring
    blackswift.fly after redefining the blackswif's attribute->humming
    scarceswift.fly->Whirring
    

* 类的方法

` object.method() `


```python
blackswift.sing()
```

    Squawk!
    

* 超类+子类


```python
help(Bird)
```

    Help on class Bird in module __main__:
    
    class Bird(builtins.object)
     |  Methods defined here:
     |  
     |  __init__(self)
     |      Initialize self.  See help(type(self)) for accurate signature.
     |  
     |  eat(self)
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors defined here:
     |  
     |  __dict__
     |      dictionary for instance variables (if defined)
     |  
     |  __weakref__
     |      list of weak references to the object (if defined)
     |  
     |  ----------------------------------------------------------------------
     |  Data and other attributes defined here:
     |  
     |  fly = 'Whirring'
    
    


```python
help(Apodidae)
```

    Help on class Apodidae in module __main__:
    
    class Apodidae(Bird)
     |  Method resolution order:
     |      Apodidae
     |      Bird
     |      builtins.object
     |  
     |  Methods defined here:
     |  
     |  __init__(self)
     |      Initialize self.  See help(type(self)) for accurate signature.
     |  
     |  sing(self)
     |  
     |  ----------------------------------------------------------------------
     |  Methods inherited from Bird:
     |  
     |  eat(self)
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors inherited from Bird:
     |  
     |  __dict__
     |      dictionary for instance variables (if defined)
     |  
     |  __weakref__
     |      list of weak references to the object (if defined)
     |  
     |  ----------------------------------------------------------------------
     |  Data and other attributes inherited from Bird:
     |  
     |  fly = 'Whirring'
    
    

### 3.6.2 迭代+迭代器

> 迭代方法往往在机器/深度学习中用于读取数据。`import torch.utils.data as Data` `train_data_iter=Data.DataLoader(train_dataset,batch_size,shuffle=True) #随机读取小批量`


```python
class Fibs():
    def __init__(self):
        self.a=0
        self.b=1
    def next(self):  #实现迭代器的next方法
        self.a,self.b=self.b,self.a+self.b
        return self.a
    def __iter__(self): #实现迭代方法
        return self
```


```python
f=Fibs()
fa=[]
fb=[]
for i in range(9):
    fa.append(f.next())
print("fa={}".format(fa))
for i in range(5):
    fb.append(f.next())
print("fb={}".format(fb))
```

    fa=[1, 1, 2, 3, 5, 8, 13, 21, 34]
    fb=[55, 89, 144, 233, 377]
    


```python
lst_d=list(range(3,20,2))
print("lst_d={}".format(lst_d))
print("_"*50)
lst_iter=iter(lst)
print("next(lst_iter)->{}".format(next(lst_iter)))
print("next(lst_iter)->{}".format(next(lst_iter)))
for i in lst_iter:
    print(i)
next(lst_iter)
```

    lst_d=[3, 5, 7, 9, 11, 13, 15, 17, 19]
    __________________________________________________
    next(lst_iter)->5
    next(lst_iter)->8
    11
    14
    17
    


    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_38760/423625245.py in <module>
          7 for i in lst_iter:
          8     print(i)
    ----> 9 next(lst_iter)
    

    StopIteration: 


### 3.6.3 生成器(generator)

**一般方法**


```python
lst_e=[list(range(3,20,3)),[3,7,67,list(range(5)),89]]
print("lst_e={}".format(lst_e))
print("_"*50)
flatten_lst=[]
for v_1 in lst_e:
    try:
        for v_2 in v_1:
            try:
                for v_3 in v_2:
                    flatten_lst.append(v_3)
            except TypeError:
                flatten_lst.append(v_2)
    except TypeError:
        flatten_lst.append(v_1)
print("flatten_lst={}".format(flatten_lst))
```

    lst_e=[[3, 6, 9, 12, 15, 18], [3, 7, 67, [0, 1, 2, 3, 4], 89]]
    __________________________________________________
    flatten_lst=[3, 6, 9, 12, 15, 18, 3, 7, 67, 0, 1, 2, 3, 4, 89]
    

**生成器方法**

使用for循环展平列表，语句表达繁琐，如果存在更多的嵌套或者不可知层次的嵌套列表，需要求助于生成器，一种普遍的函数语法定义的迭代器。


```python
def flatten(lst): #定义生成器（包含yield语句的函数）
    try: #使用语句try\except捕捉异常        
        for n_lst in lst:  #循环列表            
            for m in flatten(n_lst): #使用递归的方法循环子列表                
                yield m  #使用yield语句，每次产生多个值，当返回一个值时函数就会被冻结，当再次激活时，从停止的那点开始执行
    except TypeError:  #当函数被告知展开一个元素时，引发TypeError异常，生成器返回一个值
        yield lst #生成器返回引发异常的一个值        
```


```python
print("list(flatten(lst_e))->{}".format(list(flatten(lst_e))))
```

    list(flatten(lst_e))->[3, 6, 9, 12, 15, 18, 3, 7, 67, 0, 1, 2, 3, 4, 89]
    


```python
flatten_lst=lambda lst:[m for n_lst in lst for m in flatten_lst(n_lst)] if type(lst) is list else [lst] #lambda 生成器方法
```

* 建立无穷列表


```python
def infinite():
    n=0
    while True:
        yield 'num#'+str(n)
        n+=1
```


```python
n=infinite()
print("next(n)->{}".format(next(n)))
print("next(n)->{}".format(next(n)))
print("next(n)->{}".format(next(n)))
print("[next(n) for i in range(5)]->{}".format([next(n) for i in range(5)]))
```

    next(n)->num#0
    next(n)->num#1
    next(n)->num#2
    [next(n) for i in range(5)]->['num#3', 'num#4', 'num#5', 'num#6', 'num#7']
    

**生成器表达式(generator expression)**


```python
n=3
print("[[(2*pi*(2*(u/(n-1))-1),2*pi*(2*(v/(n-1))-1)) for u in range(n)] for v in range(n)]->{}".format([[(2*pi*(2*(u/(n-1))-1),2*pi*(2*(v/(n-1))-1)) for u in range(n)] for v in range(n)]))
print("_"*50)
print("([(2*pi*(2*(u/(n-1))-1),2*pi*(2*(v/(n-1))-1)) for u in range(n)] for v in range(n))->{}".format(([(2*pi*(2*(u/(n-1))-1),2*pi*(2*(v/(n-1))-1)) for u in range(n)] for v in range(n))))
gen=([(2*pi*(2*(u/(n-1))-1),2*pi*(2*(v/(n-1))-1)) for u in range(n)] for v in range(n))
print("next(gen)->{}".format(next(gen)))
print("next(gen)->{}".format(next(gen)))
```

    [[(2*pi*(2*(u/(n-1))-1),2*pi*(2*(v/(n-1))-1)) for u in range(n)] for v in range(n)]->[[(-6.283185307179586, -6.283185307179586), (0.0, -6.283185307179586), (6.283185307179586, -6.283185307179586)], [(-6.283185307179586, 0.0), (0.0, 0.0), (6.283185307179586, 0.0)], [(-6.283185307179586, 6.283185307179586), (0.0, 6.283185307179586), (6.283185307179586, 6.283185307179586)]]
    __________________________________________________
    ([(2*pi*(2*(u/(n-1))-1),2*pi*(2*(v/(n-1))-1)) for u in range(n)] for v in range(n))-><generator object <genexpr> at 0x000001DA3FD1D2E0>
    next(gen)->[(-6.283185307179586, -6.283185307179586), (0.0, -6.283185307179586), (6.283185307179586, -6.283185307179586)]
    next(gen)->[(-6.283185307179586, 0.0), (0.0, 0.0), (6.283185307179586, 0.0)]
    

**[itertools](https://docs.python.org/3/library/itertools.html) — Functions creating iterators for efficient looping**

## 3.7 异常 try exception
### 3.7.1 try/except捕捉异常
使用except语句时如果不待任何异常类型，将会捕捉所有异常，但是会捕捉到很多不必要的异常，应该尽量避免使用

```python
try:
    do something
except:
    do something
```


```python
def f_convert_a(x):
    try:
        return float(x)
    except:
        return x
print("f_convert_a('3.1415')->{}".format(f_convert_a('3.1415')))    
print("f_convert_a('string')->{}".format(f_convert_a('string')))  
print("f_convert_a(3,6,7)->{}".format(f_convert_a((3,6,7))))  
```

    f_convert_a('3.1415')->3.1415
    f_convert_a('string')->string
    f_convert_a(3,6,7)->(3, 6, 7)
    

```python
try:
    do something
except ValueError:
    do something
```


```python
def f_convert_b(x):
    try:
        return float(x)
    except ValueError:
        return x
print("f_convert_b('3.1415')->{}".format(f_convert_b('3.1415')))    
print("f_convert_b('string')->{}".format(f_convert_b('string')))  
print("f_convert_b(3,6,7)->{}".format(f_convert_b((3,6,7))))  
```

    f_convert_b('3.1415')->3.1415
    f_convert_b('string')->string
    


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_38760/4203039468.py in <module>
          6 print("f_convert_b('3.1415')->{}".format(f_convert_b('3.1415')))
          7 print("f_convert_b('string')->{}".format(f_convert_b('string')))
    ----> 8 print("f_convert_b(3,6,7)->{}".format(f_convert_b((3,6,7))))
    

    ~\AppData\Local\Temp/ipykernel_38760/4203039468.py in f_convert_b(x)
          1 def f_convert_b(x):
          2     try:
    ----> 3         return float(x)
          4     except ValueError:
          5         return x
    

    TypeError: float() argument must be a string or a number, not 'tuple'


使用多条except子句可以指定多个异常处理代码块：

```python
try:
    do something
except TypeError:
    do something
except ValueError:
    do something
```


```python
def f_convert_c(x):
    try:
        return float(x)
    except ValueError:
        return x
    except TypeError:
        print(x,'TypeError')
print("f_convert_c('3.1415')->{}".format(f_convert_c('3.1415')))    
print("f_convert_c('string')->{}".format(f_convert_c('string')))  
print("f_convert_c(3,6,7)->{}".format(f_convert_c((3,6,7))))         
```

    f_convert_c('3.1415')->3.1415
    f_convert_c('string')->string
    (3, 6, 7) TypeError
    f_convert_c(3,6,7)->None
    

处理程序也可以捕捉多种类型的异常

```python
try:
    do something
except (TypeError, ValueError ):
    do something
```


```python
def f_convert_d(x):
    try:
        return float(x)
    except (ValueError,TypeError):
        print(x,'ValueError or TypeError')
print("f_convert_d('3.1415')->{}".format(f_convert_d('3.1415')))    
print("f_convert_d('string')->{}".format(f_convert_d('string')))  
print("f_convert_d(3,6,7)->{}".format(f_convert_d((3,6,7)))) 
```

    f_convert_d('3.1415')->3.1415
    string ValueError or TypeError
    f_convert_d('string')->None
    (3, 6, 7) ValueError or TypeError
    f_convert_d(3,6,7)->None
    

```python
try:
    do something
except (TypeError, ValueError ) as e:
    do something
```


```python
def f_convert_e(x):
    try:
        return float(x)
    except (ValueError,TypeError) as e:
        return print(x,e)
print("f_convert_e('3.1415')->{}".format(f_convert_e('3.1415')))    
print("f_convert_e('string')->{}".format(f_convert_e('string')))  
print("f_convert_e(3,6,7)->{}".format(f_convert_e((3,6,7))))    
```

    f_convert_e('3.1415')->3.1415
    string could not convert string to float: 'string'
    f_convert_e('string')->None
    (3, 6, 7) float() argument must be a string or a number, not 'tuple'
    f_convert_e(3,6,7)->None
    

使用pass语句可以忽略异常

```python
def f_convert_f(x):
try:
    return float(x)
except ValueError as e:
    pass
```


```python
def f_convert_f(x):
    try:
        return float(x)
    except (ValueError,TypeError) as e:
        pass
print("f_convert_f('3.1415')->{}".format(f_convert_f('3.1415')))    
print("f_convert_f('string')->{}".format(f_convert_f('string')))  
print("f_convert_f(3,6,7)->{}".format(f_convert_f((3,6,7))))     
```

    f_convert_f('3.1415')->3.1415
    f_convert_f('string')->None
    f_convert_f(3,6,7)->None
    

try语句也支持else子句，但是不需跟在最后一个except子句的后面。如果try代码块中的程序没有引发异常，将会执行else子句中的程序：

```python
def f_open_a(fp):
try:
    do something
except IOError as e:
    do something
else:
    do something
```


```python
def f_open_a(fp):
    try:
        f=open(fp,'r')
    except IOError as e:
        print('Unable to open',fp,':%s\n' %e)
    else:
        data=f.read()
        f.close
        return data
tryException_content=f_open_a("./data/tryException.txt")   
print("tryException_content->{}".format(tryException_content))
f_open_a("./data/tryException_.txt")
```

    tryException_content->Hello Python!
    Unable to open ./data/tryException_.txt :[Errno 2] No such file or directory: './data/tryException_.txt'
    
    

finally语句为try代码块中的代码定义了结束操作，finally子句不是用于捕捉错误，而是提供一些程序代码，无论程序是否出错误都需要执行该代码。如果没有引发异常，finally子句的代码将在try代码块中的代码执行完毕后立即执行；如果引发了异常，控制权首先传递给finally子句的第一条语句。这段代码执行完毕后，将重新引发异常然后交由另一个异常处理程序进行处理。

```python
def f_open_a(fp):
try:
    do something
except IOError as e:
    do something
else:
    do something
finally:
    do something
```


```python
def f_open_b(fp):
    try:
        f=open(fp,'r')
    except IOError as e:
        print('Unable to open',fp,':%s\n' %e)
    else:
        data=f.read()
        f.close()
        return data
    finally:
        print('done!')
f_open_b("./data/tryException.txt")        
```

    done!
    




    'Hello Python!'



### 3.7.2 python内置异常

<a href=""><img src="./imgs_pyd/015.jpg" height="auto" width="600" title="caDesign"></a>  

**raise 引发异常**


```python
raise Exception #没有任何有关错误信息的普通异常
```


    ---------------------------------------------------------------------------

    Exception                                 Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_38760/3275756190.py in <module>
    ----> 1 raise Exception
    

    Exception: 



```python
raise Exception('Computer says no!') #添加错误信息的异常
```


    ---------------------------------------------------------------------------

    Exception                                 Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_38760/602727432.py in <module>
    ----> 1 raise Exception('Computer says no!') #添加错误信息的异常
    

    Exception: Computer says no!



```python
class some_custom_exception(Exception):pass #
raise some_custom_exception
```


    ---------------------------------------------------------------------------

    some_custom_exception                     Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_38760/2391698703.py in <module>
          1 class some_custom_exception(Exception):pass #
    ----> 2 raise some_custom_exception
    

    some_custom_exception: 


**assert 断言**


```python
x=20
assert x>0 #为真则不做任何事情
```


```python
assert x<0 #为假则引发AssertionError异常
```


    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_38760/2309557825.py in <module>
    ----> 1 assert x<0 #为假则引发AssertionError异常
    

    AssertionError: 


<a href=""><img src="./imgs_pyd/016.jpg" height="auto" width="auto" title="caDesign"></a>  
