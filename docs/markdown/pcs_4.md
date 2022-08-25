> Created on Tue Jul 26 13:39:22 2022 @author: Richie Bao-caDesign设计(cadesign.cn)

<style>
  code {
    white-space : pre-wrap !important;
    word-break: break-word;
  }
</style>

# Python Cheat Sheet-4. 基本语句_条件语句（if,elif,else）、循环语句(for, while)、列表推导式（comprehension）

<span style = "color:Teal;background-color:;font-size:20.0pt">PCS_4</span>

<table style="width:100%">
<tr>
<th style="width:10%"> 知识点 </th>
<th style="width:30%"> 描述 </th>
<th style="width:30%"> 代码段 </th> 
<th style="width:20%"> 运算结果 </th>
<th style="width:10%"> 备注</th> 
</tr>

<tr>
<td> 

__4.1__ 缩进（indentation）和代码（语句）块（code block）

</td>
<td>

代码语句的书写上，python不同于C、C++等语言最大的不同是python强制缩进，这样的好处是任何人书写的python代码都具有统一的‘格式’，无需另行规定基本的代码书写规范，方便代码传播和复用；同时，缩进的方式使得代码易读，不用特意去寻找语句块结束的标志，通过段落就可以轻易判断。


可以把一行代码看作一个基本单元，即为一个可执行的语句。语句通常是从上至下逐行执行，当定义函数和类之后，调用函数或方法则可以跳转执行语句，但跳转后仍是从上至下执行语句，结束调用方法后从之前调用的语句部分从新向下执行；或者，遇到条件语句，需要根据条件判断将要执行哪部分语句；或者，遇到循环语句和递归算法，将从循环位置反复执行同一语句或语句块，直至遇到结束条件；代码也可以不在同一个文件中，通过调用其它文件中的代码，语句的执行顺序也会跳转。

代码块是一段可以作为一个单元执行的一行或多行语句（程序文本），例如一个条件或循环的代码段，一个函数、一个类等；一个代码块也可以包含其它代码块，或调用执行其它代码块，因此对于代码块的定义相对比较宽泛，可以根据是否执行了一个任务来确定，不了任务的大小。

</td>
<td>

</td>
<td>

</td>
<td>
</td>
</tr>

<tr>
<td> 

__4.2__ 条件语句

</td>
<td>

条件语句的基本语法如下：

```python
if test1:
    statement1
elif test2:
    statement2
elif test3:
    statement3
...
else:
    statements
```

同一条件代码块，`if`通常只用一次，`elif`可以执行0次或多次，`else`为不满足上述所有条件后，执行的语句，也可以不调用，但最好通过`else`表明其它情况如何处理。

下面应用了《漫画统计学》’美味拉面畅销前50‘上刊载的拉面馆的拉面价格数据，并将其存储在了`ranmen_price_lst`列表中。这里给了一个`input()`内置函数来在外部交互输入指令，这里的指令就是条件语句中的`test`部分，如果输入指令满足`if`或`elif`后的要求，则对应执行该语句缩进后的代码。如果都不满足则执行`else`后的语句，提示"Please enter the correct command:("。

数据分析时很少用到`input()`函数来外部输入参数值，而通常使用交互图表，一般选择，[tkinter](https://docs.python.org/3/library/tkinter.html)GUI(Graphical User Inteface)工具包，[plotly](https://plotly.com/python/#controls)自定义控件，[pygame](https://www.pygame.org/news)游戏编程模块，[gradio](https://gradio.app/)以Web界面演示机器学习模型等既有成熟完善的库来处理。

</td>
<td>

```python
import numpy as np
ranmen_price_lst=[700,850,600,650,980,750,500,890,880,700,890,720,680,650,790,670,680,900,880,720,850,700,780,850,750,
     80,590,650,580,750,800,550,750,700,600,800,800,880,790,790,780,600,690,680,650,890,930,650,777,700]

command=input("Enter your command('mean,std,max,min,median'):")

if command=='mean':
    print(np.mean(ranmen_price_lst))
elif command=='std':
    print(np.std(ranmen_price_lst))
elif command=='max':
    print(np.max(ranmen_price_lst))
elif command=='min':
    print(np.min(ranmen_price_lst))   
elif command=='median':
    print(np.median(ranmen_price_lst))
else:
    print("Please enter the correct command:(") 
```

</td>
<td>

Enter your command('mean,std,max,min,median'):

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


* 嵌套条件语句（Nested if statements）

一个条件下可以再嵌套多个条件，例如下述代码外层条件语句是判断变量`price_x`值是否属于列表`ranmen_price_lst`，如果属于则打印该价格，并执行嵌套条件语句块，判断该值是否大于或者小于等于平均价格；回到外层条件，如果`price_x`值不属于列表`ranmen_price_lst`，则寻找最近值，这里使用了一个`lambda`匿名函数计算绝对值的功能，并将其作为`min(iterable, *[, default=obj, key=func])`函数的`key`参数值，即比较的是匿名函数所定义返回值（差值的绝对值）的最小值，并返回对应绝对值最小的价格列表中的值。并打印该值，同时执行嵌套条件，与`if`下嵌套条件一样来判断大于或者小于等于价格均值。

</td>
<td>


```python
import numpy as np
ranmen_price_lst=[700,850,600,650,980,750,500,890,880,700,890,720,680,650,790,670,680,900,880,720,850,700,780,850,750,
     80,590,650,580,750,800,550,750,700,600,800,800,880,790,790,780,600,690,680,650,890,930,650,777,700]

price_x=200.68

abs_difference_func=lambda value:abs(value-price_x)
if price_x in ranmen_price_lst:
    print(price_x)
    if price_x>np.mean(ranmen_price_lst):
        print('The price is higher than the average price.')
    else:
        print('The price is lower than the average price.')    
else:
    print('%.3f is no in ranmen_price_lst.'%price_x)
    closest_value=min(ranmen_price_lst,key=abs_difference_func)
    print('the nearest value to %s is %s.'%(price_x,closest_value))
    #定义了与if中同样的功能代码块，不过将price_X替换为closest_value
    price_mean=np.mean(ranmen_price_lst)
    if closest_value>price_mean:
        print('The price is higher than the average price.')
    else:
        print('The price is lower than the average price %s.'%price_mean)      
```

</td>
<td>

    200.680 is no in ranmen_price_lst.
    the nearest value to 200.68 is 80.
    The price is lower than the average price 729.34.

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


\+ 尝试下定义函数的优势（下一PCS预热）

如果要重复比较不同值和不同列表值的关系，返回列表最近值，那么上述的代码使用起来不方便，还会很繁琐，也很难分享，不易被其它程序调用（代码复用），因此需要将这一功能代码块定义为函数形式。从下述转换为函数后的代码可以观察到几个需要注意的点：

1. 关于变量名和函数名的命名，可以发现下述的变量名并没有延续上一代码段定义的各类名称，这包括变量名、函数名，及参数名。因为该函数代码的主要功能是比较一个值和一个列表中的值的关系，给的数据不一定是拉面价格，因此函数中各个名称的定义应该尽量通用化，主要表述和反应定义函数所要解决的内容或问题；

2. 对于重复的代码段或变量，通常不会重复书写，例如上述代码中内层的两个判断与均值大小的条件语句块重复书写，因此将其定义为单独的匿名函数`comparisonOF2values`方便调用。也可以看到列表均值的计算`np.mean(ranmen_price_lst)`被书写了两次，可以将该计算语句放置于条件代码块之外赋值给单独变量名，之后只需要用该变量就可，避免重复较长语句的书写；

3. 函数内的打印语句文字，同变量名的定义一样应通用化。

</td>
<td>


```python
def value2values_comparison(x,lst):
    import numpy as np
    
    lst_mean=np.mean(ranmen_price_lst)
    abs_difference_func=lambda value:abs(value-price_x)
    comparisonOF2values=lambda v1,v2:print('x is higher than the average %s of the list.'%lst_mean) if v1>v2 else print('x is lower than the average %s of the list.'%lst_mean)    
        
    if x in lst:
        print("%s in the given list."%x)
        comparisonOF2values(x,lst_mean)   
        return x
    else:
        print('%.3f is not in the list.'%x)
        closest_value=min(lst,key=abs_difference_func)
        print('the nearest value to %s is %s.'%(x,closest_value))
        print("_"*50)
        comparisonOF2values(closest_value,lst_mean)    
        return closest_value
    
ranmen_price_lst=[700,850,600,650,980,750,500,890,880,700,890,720,680,650,790,670,680,900,880,720,850,700,780,850,750,
     80,590,650,580,750,800,550,750,700,600,800,800,880,790,790,780,600,690,680,650,890,930,650,777,700]
price_x=200.68    
price_x_closestValue=value2values_comparison(price_x,ranmen_price_lst)    
print(price_x_closestValue)
```

</td>
<td>

    200.680 is not in the list.
    the nearest value to 200.68 is 80.
    __________________________________________________
    x is lower than the average 729.34 of the list.
    80

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


</td>
<td>

```python
price_x_closestValue=value2values_comparison(890,ranmen_price_lst)    
print(price_x_closestValue)
```

</td>
<td>

    890 in the given list.
    x is higher than the average 729.34 of the list.
    890 

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


</td>
<td>

```python
price_x_closestValue=value2values_comparison(78,[3,4,5,733,66,22,99,88,11])    
print(price_x_closestValue)
```

</td>
<td>

    78.000 is not in the list.
    the nearest value to 78 is 99.
    __________________________________________________
    x is lower than the average 729.34 of the list.
    99

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


* 三元表达式（Ternary Expression）

形如`variable=v1 if test else v2`的语句即为三元表达式，该语句等同于：

```python
if test:
    variable=v1
else:
    variable=v2
```

三元表达式通常用于较简单的条件语句，因为用一行表述较之多行书写更为便捷；但是对于较长，较复杂的条件语句则建议按常规缩进书写。

</td>
<td>

```python
v1=33.5
v2=78.3
max_v1Nv2=v1 if v1>v2 else v2
print(max_v1Nv2)
```

</td>
<td>

    78.3

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


* 用`;`连接简单的语句为一行

如果语句非常的简单，则可以使用`;`将其连接置于一行。下述示例还包括了一个简单的三元表达。


</td>
<td>

```python
x=3.5;y=7.8;print(x if x>y else y)
```

</td>
<td>

    7.8

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


* 条件语句与比较运算符、逻辑运算符和成员运算符

条件语句通常会用逻辑运算符连接多个比较运算符或其他条件，实现条件判断的目的。


</td>
<td>


```python
a,b,c=23,57,68
lst=[23,77,96]

if a<b and c>b:
    print('a is less than c.')
if a<b or b>c:
    print('b is not sure greater than c.')
    
if a not in lst:
    print('a not in list')
else:
    print('a in list')
    
if a in lst:
    print('a in list')
else:
    print('a not in list')    
```


</td>
<td>

    a is less than c.
    b is not sure greater than c.
    a in list
    a in list

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

很多变量可以直接用于条件之后，简化条件书写，例如下述是否为空列表的判断，一个直接使用变量，一个则计算列表的长度来判断是否为0，从而证实是否为空列表。

</td>
<td>


```python
if 1:
    print('return true.')
if 0:
    print('This statement will not be executed.')
else:
    print('It is 0.')
    
if True:
    print('This statement is executed!')

if '':
    print('Empty string...')
else:
    print('This test is an empty string.')    

empty_lst=[]
if not empty_lst:
    print('This is an empty list!!!')
if len(empty_lst)==0:
    print('This is an empty list!!!')
    
lst=[3,4,5]
if lst:
    print('This is not an empty list!!!')
```


</td>
<td>

    return true.
    It is 0.
    This statement is executed!
    This test is an empty string.
    This is an empty list!!!
    This is an empty list!!!
    This is not an empty list!!!

</td>
<td>
</td>
</tr>

<tr>
<td> 

__4.3__ 循环语句_for loops 模式

</td>
<td>


for循环的基本语法为：

```python
for target in object:
    statements
else: #可选部分
    statements #如果for循环没有被终断（break）
```

`object` 为序列或者任何可迭代的对象，例如strings, lists, truples, dict和其它内置可迭代（iterable）对象，如`zip()`，`map()`等返回的可迭代对象。

* 循环列表与`enumerate()`

在解释循环语句时，使用了[The Cityscapes Dataset](https://www.cityscapes-dataset.com/)的标签数据（Cityscapes数据集集中于城市街道场景的语义解释（semantic understanding），如图像语义分割、对象检测等深度学习模型的训练，这非常适用于对城市空间内容的分析）。为了方便数据的处理，将标签数据存储为`namedtuple`数据格式（结构），`namedtuple`是由python内置库[collections](https://docs.python.org/3/library/collections.html)提供，一般翻译为具名元组。python内置数据结构tuple(元组), 不能像表格抬头（例如pandas的DataFrame数据结构）一样为数据指定字段名（列名），因此不能够很好的管理数据，这包括对于数据的存储更新和提取，因此`collections.namedtuple`类型的数据结构就解决了这个问题。`namedtuple(typename, field_names, *, rename=False, defaults=None, module=None)`，定义`namedtuple`的输入参数中`typename`为元组的名称，`filed_names`为元组中元素的名称，`rename`为如果元素名称含有python的关键字，则必须配置该参数为`rename=True`。使用`namedtuple`首先定义一个`namedtuple`对象，例如示例中的`Label`对象，然后应用该对象定义不同的`namedtuple`变量存储数据，例如`label_building`和`label_caravan`。可以通过类属性值（`object.attribute`）的途径读取字段值，及更新字段值。


> [collections](https://docs.python.org/3/library/collections.html)库提供有专门的容器数据类型（container datatype），即数据结构，为dcit, list, set 和tuple提供了可替代数据存储管理方式。 将会有专门的PCS阐释该库。

</td>
<td>


```python
from collections import namedtuple

Label=namedtuple('label',['name','id','trainID','category','categoryID','hasInstances','igoreInEval','color'])
print(Label)
print("_"*50)
label_building=Label( 'building',11,2,'construction',2,False,False, ( 70, 70, 70))
print(label)
print(label_building._fields)

print("_"*50)
print(label_building.name)
print(label_building.id)
print(label_building.category)
print(label_building.color)

print("_"*50)
caravan_lst=['caravan', 29,255,'vehicle',7,True,True, (  0,  0, 90)]
label_caravan=Label._make(caravan_lst)
print(label_caravan.name)
print(label_caravan.id)
print(label_caravan.category)
print(label_caravan.color)

label_caravan=label_caravan._replace(category='schooner',color=(30,30,60)) #替换属性值
print(label_caravan.category)
print(label_caravan.color)

print("_"*50)
caravan_dict=label_caravan._asdict() #将nametuple转换为dict
print(caravan_dict)
```

</td>
<td>


    <class '__main__.label'>
    __________________________________________________
    label(name='building', id=11, trainID=2, category='construction', categoryID=2, hasInstances=False, igoreInEval=False, color=(70, 70, 70))
    ('name', 'id', 'trainID', 'category', 'categoryID', 'hasInstances', 'igoreInEval', 'color')
    __________________________________________________
    building
    11
    construction
    (70, 70, 70)
    __________________________________________________
    caravan
    29
    vehicle
    (0, 0, 90)
    schooner
    (30, 30, 60)
    __________________________________________________
    {'name': 'caravan', 'id': 29, 'trainID': 255, 'category': 'schooner', 'categoryID': 7, 'hasInstances': True, 'igoreInEval': True, 'color': (30, 30, 60)}


</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


cityscapes的标签数据以namedtuple列表形式存储，列表中的每一个值就为一个namedtuple对象，具有相同的字段名称。通过namedtuple读取值的方法，并配合列表推导式很容易提取各个字段名为单独的列表，或两个到多个字段名提取为字典的模式，建立不同字段之间的映射。为了清晰的观察数据，在输入数据时，有意识的将其各列对其，每一列就为一个具有名称的元素，例如`name`字段列对其方便观察名称。注意，这里修改了`color`字段的值，使用了[ANSI Escape Sequences/Codes。ANSI code](https://en.wikipedia.org/wiki/ANSI_escape_code)，可翻译为ANSI转义序列/代码，ANSI code用于控制光标位置、颜色和字体样式，也包括视频文本终端或终端仿真器。某些字节序列（大多数以 ASCII 转义字符和括号字符开头）被嵌入到文本中。 终端将这些序列解释为命令，而不是逐字显示的文本。

在python解释器中显示字体的颜色，包括16的模式（8个字体颜色和8个背景颜色）和256色模式。16色字符串格式化的模式示例为`print('\033[2;31;43m CHEESY \033[0;0m')`，或`print('\x1b[2;31;43m CHEESY \x1b[0;0m')`，其中`\033[0;0m')`是重置终端打印颜色为默认，防止继续打印设置的颜色，各字符含义如图：<img src="./imgs/pc_4_01.jpg" height='auto' width='700' title="caDesign">


256色打印方式例如`print("\033[48;5;236m\033[38;5;231mStack \033[38;5;208mAbuse\033[0;0m")`, 各字符含义如图：<img src="./imgs/pc_4_02.jpg" height='auto' width='700' title="caDesign">

> 参考[How to Print Colored Text in Python](https://stackabuse.com/how-to-print-colored-text-in-python/)

[American National Standards Institute , ANSI](https://www.ansi.org/)

</td>
<td>


```python
Label=namedtuple('label',['name','id','trainId','category','catId','hasInstances','igoreInEval','color'])

labels = [
    #       name                     id    trainId   category            catId     hasInstances   ignoreInEval   color
    Label(  'unlabeled'            ,  0 ,      255 , 'void'            , 0       , False        , True         , (0, 30,  47) ),
    Label(  'ego_vehicle'          ,  1 ,      255 , 'void'            , 0       , False        , True         , (0, 31,  46) ),
    Label(  'rectification_border' ,  2 ,      255 , 'void'            , 0       , False        , True         , (0, 32,  45) ),
    Label(  'out_of_roi'           ,  3 ,      255 , 'void'            , 0       , False        , True         , (0, 33,  44) ),
    Label(  'static'               ,  4 ,      255 , 'void'            , 0       , False        , True         , (0, 34,  43) ),
    Label(  'dynamic'              ,  5 ,      255 , 'void'            , 0       , False        , True         , (1, 35,  42) ),
    Label(  'ground'               ,  6 ,      255 , 'void'            , 0       , False        , True         , (1, 36,  41) ),
    Label(  'road'                 ,  7 ,        0 , 'flat'            , 1       , False        , False        , (1, 37,  40) ),
    Label(  'sidewalk'             ,  8 ,        1 , 'flat'            , 1       , False        , False        , (1, 30,  41) ),
    Label(  'parking'              ,  9 ,      255 , 'flat'            , 1       , False        , True         , (1, 31,  42) ),
    Label(  'rail_track'           , 10 ,      255 , 'flat'            , 1       , False        , True         , (2, 32,  43) ),
    Label(  'building'             , 11 ,        2 , 'construction'    , 2       , False        , False        , (2, 33,  44) ),
    Label(  'wall'                 , 12 ,        3 , 'construction'    , 2       , False        , False        , (2, 34,  45) ),
    Label(  'fence'                , 13 ,        4 , 'construction'    , 2       , False        , False        , (2, 35,  46) ),
    Label(  'guard_rail'           , 14 ,      255 , 'construction'    , 2       , False        , True         , (2, 36,  47) ),
    Label(  'bridge'               , 15 ,      255 , 'construction'    , 2       , False        , True         , (3, 37,  40) ),
    Label(  'tunnel'               , 16 ,      255 , 'construction'    , 2       , False        , True         , (3, 30,  42) ),
    Label(  'pole'                 , 17 ,        5 , 'object'          , 3       , False        , False        , (3, 31,  43) ),
    Label(  'polegroup'            , 18 ,      255 , 'object'          , 3       , False        , True         , (3, 32,  44) ),
    Label(  'traffic_light'        , 19 ,        6 , 'object'          , 3       , False        , False        , (3, 33,  45) ),
    Label(  'traffic_sign'         , 20 ,        7 , 'object'          , 3       , False        , False        , (4, 34,  46) ),
    Label(  'vegetation'           , 21 ,        8 , 'nature'          , 4       , False        , False        , (4, 35,  47) ),
    Label(  'terrain'              , 22 ,        9 , 'nature'          , 4       , False        , False        , (4, 36,  40) ),
    Label(  'sky'                  , 23 ,       10 , 'sky'             , 5       , False        , False        , (4, 37,  41) ),
    Label(  'person'               , 24 ,       11 , 'human'           , 6       , True         , False        , (4, 30,  43) ),
    Label(  'rider'                , 25 ,       12 , 'human'           , 6       , True         , False        , (5, 31,  44) ),
    Label(  'car'                  , 26 ,       13 , 'vehicle'         , 7       , True         , False        , (5, 32,  45) ),
    Label(  'truck'                , 27 ,       14 , 'vehicle'         , 7       , True         , False        , (5, 33,  46) ),
    Label(  'bus'                  , 28 ,       15 , 'vehicle'         , 7       , True         , False        , (5, 34,  47) ),
    Label(  'caravan'              , 29 ,      255 , 'vehicle'         , 7       , True         , True         , (5, 35,  42) ),
    Label(  'trailer'              , 30 ,      255 , 'vehicle'         , 7       , True         , True         , (5, 36,  41) ),
    Label(  'train'                , 31 ,       16 , 'vehicle'         , 7       , True         , False        , (0, 37,  40) ),
    Label(  'motorcycle'           , 32 ,       17 , 'vehicle'         , 7       , True         , False        , (1, 30,  44) ),
    Label(  'bicycle'              , 33 ,       18 , 'vehicle'         , 7       , True         , False        , (2, 32,  45) ),
    Label(  'license_plate'        , -1 ,       -1 , 'vehicle'         , 7       , False        , True         , (3, 33,  47) ),
]

print(labels[:3])
```


</td>
<td>

    [label(name='unlabeled', id=0, trainId=255, category='void', catId=0, hasInstances=False, igoreInEval=True, color=(0, 30, 47)), label(name='ego_vehicle', id=1, trainId=255, category='void', catId=0, hasInstances=False, igoreInEval=True, color=(0, 31, 46)), label(name='rectification_border', id=2, trainId=255, category='void', catId=0, hasInstances=False, igoreInEval=True, color=(0, 32, 45))]
    


</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>



</td>
<td>

```python
print('\x1b[2;31;43m CHEESY \x1b[0;0m')
print("\033[48;5;236m\033[38;5;231mStack \033[38;5;208mAbuse\033[0;0m")
```

</td>
<td>

<img src="./imgs/pc_4_03.jpg" height='auto' width='auto' title="caDesign">

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


`color_lst`为提取的ANSI code格式颜色数据列表，每一元组值对应text styles(字体类型，包括normal/0, bold/1, light/2, italicized/3, underlined/4, blink/5)，foreground(Text)color（字体颜色，包括black/30, red/31, green/32, yellow/33, blue/34, purple/35, cyan/36, white/37计8个颜色），及background color（字体的背景色，颜色同字体色，但是编号为40-47）。每次循环配置打印颜色值，并以颜色值为打印的字符串。

需要注意对于循环语句，通常包括多个值，甚至千万个待循环值，因此在书写代码时需要增加终止循环的代码`if i==5:break`，当变量`i`每次循环自增1到5时，调用`break`终止语句，跳出循环，待调试一次或几次循环无误后，再循环所有的值，避免等待运算时间，尤其需要花费10分钟以上，甚至多到几个小时或几天才能运算完的代码段。

下述示例代码保留了调试代码，除变量`i`和终止条件语句行外，调试时，要不断用`print()`函数查看变量值，从而确定变量值是否正确，及确认变量值结构，从而知晓后续代码行应用该变量的方式，或者通过后续要求的数据结构来处理数据为后续所用结构的类型（通常是用后者的方式判断和书写代码）。例如示例中通过`print(color)`来查看通过`color=';'.join([str(i) for i in c])`语句编写满足ANSI code要求的颜色格式，例如`0;30;47`，注意这里的数字为使用`str()`转换数字为字符串，满足使用`%s`格式化符号的要求，也可以先转换为字符串，而是使用`%d`的方式直接格式化。


</td>
<td>

```python
color_lst=[label.color for label in labels]
print(color_lst)

#i=0 #调试用
for c in color_lst:    
    color=';'.join([str(i) for i in c])
    #print(color) #调试用
    s='\x1b[%sm %s \x1b[0m' % (color,color)
    print(s)
    #if i==5:break #调试用
    #i+=1  #调试用
    
```

</td>
<td>

    [(0, 30, 47), (0, 31, 46), (0, 32, 45), (0, 33, 44), (0, 34, 43), (1, 35, 42), (1, 36, 41), (1, 37, 40), (1, 30, 41), (1, 31, 42), (2, 32, 43), (2, 33, 44), (2, 34, 45), (2, 35, 46), (2, 36, 47), (3, 37, 40), (3, 30, 42), (3, 31, 43), (3, 32, 44), (3, 33, 45), (4, 34, 46), (4, 35, 47), (4, 36, 40), (4, 37, 41), (4, 30, 43), (5, 31, 44), (5, 32, 45), (5, 33, 46), (5, 34, 47), (5, 35, 42), (5, 36, 41), (0, 37, 40), (1, 30, 44), (2, 32, 45), (3, 33, 47)]

<img src="./imgs/pc_4_04.jpg" height='auto' width='auto' title="caDesign">    

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


`enumerate(iterable, start=0)`同时成对返回计数值和列表值，为一个枚举对象（return an enumerate object）。在对列表执行`enumerate()`之后，在使用循环语句时可以将成对的计数值和元素值分别赋予给两个变量，如`idx`和`c`。如果并不在`for`循环中直接序列解包，赋予了一个变量，如`i`，则其如`(0, (0, 30, 47))`，仍然需要索引方式或序列解包方式（idx,c=i）提取值。

在打印字符时，如果不换行，可以增加参数`end=''`来避免起新行。


</td>
<td>


```python
for idx,c in enumerate(color_lst):    
    color=';'.join([str(i) for i in c])
    s='\x1b[%sm %s \x1b[0m' % (color,idx)
    print(s,end='')
    
print('\n',"_"*50,'\n')    
for i in enumerate(color_lst):    
    #print(i)
    idx,c=i
    color=';'.join([str(i) for i in c])
    s='\x1b[%sm %s \x1b[0m' % (color,idx)
    print(s,end='')
        
```

</td>
<td>

<img src="./imgs/pc_4_05.jpg" height='auto' width='auto' title="caDesign">   

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


* 循环字典——键值对形式

以键值对形式循环字典是经常使用到的一种方式，可以很方便的同时提键名和元素值，并在每一次循环中同时处理键名和元素值，再成对输出。例如建立了一个空字典`name2NewColor_dict`，在每次成对循环原有字典值时，修改了颜色值（+1），并按键名和新颜色值成对存储在新建的字典中。

对于新键字典也在终端打印具有色彩的字符串，因为ANSI code格式颜色有值域，如果超出范围则可以看到对应部分不会发生颜色变化。

这里在调试代码时，直接使用了`break`语句，没有结合条件语句，因此该循环在调试时只执行一次循环。


</td>
<td>


```python
name2color_dict={label.name:label.color for label in labels}
print(name2color_dict)

name2NewColor_dict={}
for name,color in name2color_dict.items():
    c=';'.join([str(i) for i in color])
    s='\x1b[%sm %s \x1b[0m' % (c,name)
    print(s,end='')   
    
    name2NewColor_dict[name]=(i+1 for i in color)
    #break
print('\n',"_"*50,'\n')  
for name,color in name2NewColor_dict.items():
    c=';'.join([str(i) for i in color])
    s='\x1b[%sm %s \x1b[0m' % (c,name)
    print(s,end='') 
```

</td>
<td>

    {'unlabeled': (0, 30, 47), 'ego_vehicle': (0, 31, 46), 'rectification_border': (0, 32, 45), 'out_of_roi': (0, 33, 44), 'static': (0, 34, 43), 'dynamic': (1, 35, 42), 'ground': (1, 36, 41), 'road': (1, 37, 40), 'sidewalk': (1, 30, 41), 'parking': (1, 31, 42), 'rail_track': (2, 32, 43), 'building': (2, 33, 44), 'wall': (2, 34, 45), 'fence': (2, 35, 46), 'guard_rail': (2, 36, 47), 'bridge': (3, 37, 40), 'tunnel': (3, 30, 42), 'pole': (3, 31, 43), 'polegroup': (3, 32, 44), 'traffic_light': (3, 33, 45), 'traffic_sign': (4, 34, 46), 'vegetation': (4, 35, 47), 'terrain': (4, 36, 40), 'sky': (4, 37, 41), 'person': (4, 30, 43), 'rider': (5, 31, 44), 'car': (5, 32, 45), 'truck': (5, 33, 46), 'bus': (5, 34, 47), 'caravan': (5, 35, 42), 'trailer': (5, 36, 41), 'train': (0, 37, 40), 'motorcycle': (1, 30, 44), 'bicycle': (2, 32, 45), 'license_plate': (3, 33, 47)}

<img src="./imgs/pc_4_06.jpg" height='auto' width='auto' title="caDesign">   

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

* zip()和map()

`zip(*iterables)`将多个列表（序列）返回为成对的值，`for Loops`可以逐个成对循环。下述示例使用了ANSI code格式颜色256模式，用`catId`作为背景颜色，未配置字体颜色，同时字符串之间增加了一个空格，断开名称。


</td>
<td>


```python
name_lst=[label.name for label in labels]
catId_lst=[label.catId for label in labels]

for name,catId in zip(name_lst,catId_lst):
    s='\033[48;5;%dm%s\033[0;0m '%(catId,name)
    print(s,end='')
    #break
```

</td>
<td>

<img src="./imgs/pc_4_07.jpg" height='auto' width='auto' title="caDesign">

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

`map(func, *iterables)`输入参数`func`，自定义为`lambda`（ˈlamdə）函数，并给了两个输入参数`x`和`n`，返回一个元组，其中一个值保持不变（即name名称），另一个值加1（即用作颜色值的catId加1）。用`for Loops`可以逐个循环给定列表中的值经过`map()`中`func`参数函数的计算的返回值。


</td>
<td>

```python
xAdd33_Wname=lambda x,n:(n,x+33)
for name,catId_new in map(xAdd33_Wname,catId_lst,name_lst):
    #print( name,color)
    s='\033[48;5;%dm%s\033[0;0m '%(catId_new,name)
    print(s,end='')        
    #break
```

</td>
<td>

<img src="./imgs/pc_4_08.jpg" height='auto' width='auto' title="caDesign">

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

* 循环`range(len())`

在组织列表中数值之间的运算模式时，经常通过列表的不同索引组合规律相加，相乘，或者任何更为复杂的计算来获取新的符合某一规律的列表值。例如`regularAdd_1`为逐个计算列表中相邻两个值之和（即循环时为当前索引对应值和其后索引对应值之和）；`regularAdd_2`为循环时，计算当前索引之前所有值之和；而`regularAdd_3`则结合`slicing`，实现间隔相加的结果。


</td>
<td>


```python
for idx in range(len(name_lst)):
    print('%d-%s;'%(idx,name_lst[idx]),end='')

import random
lst=[random.randint(10,30) for i in range(10)]
print('\n',"_"*50)
print(lst)

regularAdd_1=[]
regularAdd_2=[]
for i in range(len(lst)-1):
    regularAdd_1.append(lst[i]+lst[i+1])
    regularAdd_2.append(sum([lst[i] for i in range(i+1)]))

regularAdd_3=[]
for i in range(1,len(lst)-1,2):
    print(i)
    regularAdd_3.append(lst[i]+lst[i+2])

print(regularAdd_1,'\n',regularAdd_2,'\n',regularAdd_3)
```

</td>
<td>

    0-unlabeled;1-ego_vehicle;2-rectification_border;3-out_of_roi;4-static;5-dynamic;6-ground;7-road;8-sidewalk;9-parking;10-rail_track;11-building;12-wall;13-fence;14-guard_rail;15-bridge;16-tunnel;17-pole;18-polegroup;19-traffic_light;20-traffic_sign;21-vegetation;22-terrain;23-sky;24-person;25-rider;26-car;27-truck;28-bus;29-caravan;30-trailer;31-train;32-motorcycle;33-bicycle;34-license_plate;
     __________________________________________________
    [28, 24, 16, 19, 29, 20, 18, 11, 26, 21]
    1
    3
    5
    7
    [52, 40, 35, 48, 49, 38, 29, 37, 47] 
     [28, 52, 68, 87, 116, 136, 154, 165, 191] 
     [43, 39, 31, 32]

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

* 嵌套循环（nested for loops）

如果要获取嵌套列表、嵌套字典或这任何包含多个嵌套关系的数据结构，或者需要多个序列值运算处理，通常需要用嵌套循环逐层的拆解或嵌套循环不同序列运算。下述示例对同一个列表执行嵌套循环，并相加。为了方便观察数据关系，使用列表推导式切分为嵌套列表，并循逐行环打印，可以看到构建了一个列表中每一个元素与该列表全部值相加的矩阵，这可用于计算多个点列表，两两点之间距离的成本矩阵（起点-目的地 (OD) 成本矩阵），用于城市交通等分析。


</td>
<td>



```python
lst=[random.randint(10,30) for i in range(10)]
print(lst)

regularAdd_4=[]
for i in range(len(lst)):
    for j in range(len(lst)):
        regularAdd_4.append(lst[i]+lst[j])
n=len(lst)

regularAdd_4_chunks=[regularAdd_4[i:i+n] for i in range(0,len(regularAdd_4),n)]   

for sub_lst in regularAdd_4_chunks:
    print(sub_lst)
```


</td>
<td>

    [12, 18, 28, 24, 20, 14, 27, 25, 18, 23]
    [24, 30, 40, 36, 32, 26, 39, 37, 30, 35]
    [30, 36, 46, 42, 38, 32, 45, 43, 36, 41]
    [40, 46, 56, 52, 48, 42, 55, 53, 46, 51]
    [36, 42, 52, 48, 44, 38, 51, 49, 42, 47]
    [32, 38, 48, 44, 40, 34, 47, 45, 38, 43]
    [26, 32, 42, 38, 34, 28, 41, 39, 32, 37]
    [39, 45, 55, 51, 47, 41, 54, 52, 45, 50]
    [37, 43, 53, 49, 45, 39, 52, 50, 43, 48]
    [30, 36, 46, 42, 38, 32, 45, 43, 36, 41]
    [35, 41, 51, 47, 43, 37, 50, 48, 41, 46]
    


</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


* `for Loops`中的星号`*`（asterisk）

python中`*`和`**`，除了作为运算符或者字符串中的特殊字符外，通常作为前缀运算符（prefix operators），即在变量之前使用`*`和`**`运算符。`*`和`**`运算符具有丰富的用法，在单独的PCS中加以解释。下述应用到循环中的示例是用`*`来收集多个元素值。


</td>
<td>

```python
for a,*b,c in [(1,2,3,4,5),(5,6,7,8)]:
    print(a,b,c)
```


</td>
<td>

    1 [2, 3, 4] 5
    5 [6, 7] 8

</td>
<td>
</td>
</tr>


<tr>
<td> 

__4.4__ 循环语句_while 模式

</td>
<td>


基于语法为：

```python
while test:
    statements
else:
    statements
```
或
```python
while test:
    statements
    if test:break
    if test: continue
else:
    statement
```

`while`的关键是处理循环停止的条件，一种是在`while`之后给出停止条件，例如`while x<=10:`，只要不满足条件就会停止循环，这时给出的条件变量通常在`while`代码块中参与执行相关的运算并会因为变化而不满足给出的条件后跳出循环，例如`x+=1`；或者在`while`代码块内给出条件语句，配合使用`break`停止循环，类似于`for`循环中`break`。


</td>
<td>


```python
x=1
while x<=10:
    print(x,';',end='')
    x+=1    
    
print('\n',"_"*50)
x=1
while True:
    print(x,';',end='')
    x+=1
    if x>10:break
    
print('\n',"_"*50)
x=1
for i in range(1000):
    print(x,';',end='')
    x+=1
    if x>10:break
```


</td>
<td>

    1 ;2 ;3 ;4 ;5 ;6 ;7 ;8 ;9 ;10 ;
     __________________________________________________
    1 ;2 ;3 ;4 ;5 ;6 ;7 ;8 ;9 ;10 ;
     __________________________________________________
    1 ;2 ;3 ;4 ;5 ;6 ;7 ;8 ;9 ;10 ;

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

将`while True/break`用于`input()`函数。

</td>
<td>


```python
while True:
    value=input('To calculate the square root. Enter a number:(enter "stop" to stop the operation)')
    if value=='stop':break
    import math
    print("The square root of \033[1;31;40m%s\033[0m is \033[1;31;40m%.2f\033[0m."%(value, math.sqrt(float(value))))
    
```


</td>
<td>

<img src="./imgs/pc_4_09.jpg" height='auto' width='auto' title="caDesign">



</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

可以给定不同的要求，使用多个`elif`的方式分别计算。不过对于这种在终端交互的方式通常是使用既有的交互图表库。

</td>
<td>


```python
while True:
    import math
    command=input('sqrt, power,sum, or stop:')
    if command=='sqrt':
        number=input('Enter 1 number:')
        print('square root=%.2f'%math.sqrt(float(number)))
    elif command=='power':
        number=input('Enter 1 number:')
        print('power=%.2f'%math.pow(float(number),2))
    elif command=='sum':
        numbers=input('Enter 2 numbers, separated by commas:')
        print('sum=%.2f'%sum([float(v) for v in numbers.split(",")]))
    elif command=='stop':break
```


</td>
<td>


    sqrt, power,sum, or stop: sqrt
    Enter 1 number: 5
    

    square root=2.24
    

    sqrt, power,sum, or stop: power
    Enter 1 number: 5
    

    power=25.00
    

    sqrt, power,sum, or stop: sum
    Enter 2 numbers, separated by commas: 5,6
    

    sum=11.00
    

    sqrt, power,sum, or stop: stop


</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

将`while True/break`用于文件读取。注意在书写代码时是将`poi_lst.append(POI._make(line.split(",")))`语句写于`if not line:break`中断语句之后，先判断是否已到行末尾（读取的是否为空行），否则会提示运行错误，因为空行不能执行`line.split(",")`运算。



</td>
<td>


```python
xian_poi_fn='./data/xian_poi.csv' #存储又西安POI, point of interesting兴趣点数据
f=open(xian_poi_fn,'r', encoding="utf-8") #

from collections import namedtuple
POI=namedtuple('POI',['idx','unknown','name','lat','lon','category','score','price','x','y'])
poi_lst=[]
while True:
    line=f.readline()   
    if not line:break
    poi_lst.append(POI._make(line.split(",")))
    #break #调试用
f.close()
print('The length of the poi list is %d.'%len(poi_lst))
print("_"*50)
for i in poi_lst[:3]:print(i,'\n')    
```


</td>
<td>

    The length of the poi list is 13732.
    __________________________________________________
    POI(idx='1', unknown='0101000020897F000008D599CB26E312418530CECF16EB4C41', name='美香源', lat='34.23709808337344', lon='108.93100212046282', category='美食;中餐厅', score='0', price='', x='309449.6988290106', y='3790381.623479905\n') 
    
    POI(idx='2', unknown='0101000020897F0000B038F4CF9BE31241389AD606BFEC4C41', name='雷记澄城水盆羊肉(红樱路店)', lat='34.244750060429915', lon='108.93113243785623', category='美食;中餐厅', score='3.9', price='20.0', x='309478.95308006834', y='3791230.053424146\n') 
    
    POI(idx='3', unknown='0101000020897F00005C24B17F97E3124160F86A81E5EC4C41', name='段府农家菠菜面(红缨路店)', lat='34.245443456204875', lon='108.93110375582032', category='美食;中餐厅', score='4', price='', x='309477.8746991807', y='3791307.011076972\n') 
  

</td>
<td>
</td>
</tr>

<tr>
<td> 

__4.5__ 列表推导式（comprehension）

</td>
<td>

print
`print('Hello World!') `代码。

</td>
<td>

```python
print('Hello World!') 
```

</td>
<td>

Hello World! 

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

因为使用`for loops`会占据多行，而只要不是复杂的循环，使用列表推导式是优先选择，不仅会简化代码行，同时书写起来相对要便捷的多。其语法为`newlist=[expression for item in iterable if condition == True]`，或者`newlist=[expression1 if condition == True else expression2 for item in iterable]`


</td>
<td>


```python
lst=[9,8,7,6,5,4,3]
for i in range(len(lst)):
    lst[i]+=10
print(lst)

lst=[9,8,7,6,5,4,3]
lst_A=[i+10 for i  in lst]
print(lst_A)
```

</td>
<td>

    [19, 18, 17, 16, 15, 14, 13]
    [19, 18, 17, 16, 15, 14, 13]

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

列表推导式也可以转换具有条件语句的`for loops`。

</td>
<td>

```python
lst=[9,8,7,6,5,4,3]
for i in range(len(lst)):
    if i%2==0:lst[i]+=10
    else:lst[i]+=100
print(lst)

lst=[9,8,7,6,5,4,3]
lst_B=[lst[i]+10 if i%2==0 else lst[i]+100 for i  in range(len(lst))]
print(lst_B)
```

</td>
<td>

    [19, 108, 17, 106, 15, 104, 13]
    [19, 108, 17, 106, 15, 104, 13]

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

将嵌套循环转换为列表推导式计算。

</td>
<td>

```python
monogram_lst=[]
for i in 'abcd':
    for j in 'hijk':
        monogram_lst.append('%s-%s'%(i,j)) 
print(monogram_lst)        

monogram_lst_A=['%s-%s'%(i,j) for i in 'abcd' for j in 'hijk']
print(monogram_lst_A)
```

</td>
<td>

    ['a-h', 'a-i', 'a-j', 'a-k', 'b-h', 'b-i', 'b-j', 'b-k', 'c-h', 'c-i', 'c-j', 'c-k', 'd-h', 'd-i', 'd-j', 'd-k']
    ['a-h', 'a-i', 'a-j', 'a-k', 'b-h', 'b-i', 'b-j', 'b-k', 'c-h', 'c-i', 'c-j', 'c-k', 'd-h', 'd-i', 'd-j', 'd-k']

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

列表推导式用于dict数据结构。将`hasInstances`字段的布尔值转换为0或1。

</td>
<td>

```python
name2hasInstances_dict={label.name:label.hasInstances for label in labels}
print(name2hasInstances_dict)
print({k:int(v) for k,v in name2hasInstances_dict.items()})

```

</td>
<td>

    {'unlabeled': False, 'ego_vehicle': False, 'rectification_border': False, 'out_of_roi': False, 'static': False, 'dynamic': False, 'ground': False, 'road': False, 'sidewalk': False, 'parking': False, 'rail_track': False, 'building': False, 'wall': False, 'fence': False, 'guard_rail': False, 'bridge': False, 'tunnel': False, 'pole': False, 'polegroup': False, 'traffic_light': False, 'traffic_sign': False, 'vegetation': False, 'terrain': False, 'sky': False, 'person': True, 'rider': True, 'car': True, 'truck': True, 'bus': True, 'caravan': True, 'trailer': True, 'train': True, 'motorcycle': True, 'bicycle': True, 'license_plate': False}
    {'unlabeled': 0, 'ego_vehicle': 0, 'rectification_border': 0, 'out_of_roi': 0, 'static': 0, 'dynamic': 0, 'ground': 0, 'road': 0, 'sidewalk': 0, 'parking': 0, 'rail_track': 0, 'building': 0, 'wall': 0, 'fence': 0, 'guard_rail': 0, 'bridge': 0, 'tunnel': 0, 'pole': 0, 'polegroup': 0, 'traffic_light': 0, 'traffic_sign': 0, 'vegetation': 0, 'terrain': 0, 'sky': 0, 'person': 1, 'rider': 1, 'car': 1, 'truck': 1, 'bus': 1, 'caravan': 1, 'trailer': 1, 'train': 1, 'motorcycle': 1, 'bicycle': 1, 'license_plate': 0}

</td>
<td>
</td>
</tr>




</table>

<span style = "color:Teal;background-color:;font-size:20.0pt">是否完成PCS_4(&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)</span>