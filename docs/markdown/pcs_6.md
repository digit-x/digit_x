> Created on Thu Aug 18 20:42:00 2022 @author: Richie Bao-caDesign设计(cadesign.cn)

<style>
  code {
    white-space : pre-wrap !important;
    word-break: break-word;
  }
</style>

# Python Cheat Sheet-6. recursion(递归)、lambda(Anonymous Function, 匿名函数)、generator(生成器)

<span style = "color:Teal;background-color:;font-size:20.0pt">PCS_6</span>

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

__6.1__ 递归

</td>
<td>


通过简单的案例查看每一个循环，满足条件代码块运行变量值和返回值的变化是理解递归最好的途径。递归的核心是调用自身，重复同一个代码逻辑过程，实现返回值满足某一变化特征的叠加；并且需要有结束条件，以结束递归，即结束调用自身的循环；同时，给结束条件一个返回值，为最后一次调用自身的返回值。

下述两个案例一个为递归求和，一个为递归阶乘，通常用此来解释递归函数的方法。

* 递归求和

第1个解释案例是递归求和，其过程如图。用具体的语句和值解释上一段话为：开始输入值（开始值）为`lst=[1,2,3,4,5] `, 不满足结束条件`if not lst`，执行`else`语句块，返回值语句为`return lst[0]+recursive_sum(lst[1:])`，将值代入语句（表达式），为`1+recursive_sum([2,3,4,5])`，其中包含调用自身语句`recursive_sum(lst[1:]`，因此从函数名定义行重新开始执行该函数；因为输入参数值`[2,3,4,5]`不满足结束条件，再次执行`else`语句块，将值代入语句（表达式），为`2+recursive_sum([3,4,5])`，继续调用自身，此时的输入值为`[3,4,5]`；以次类推，不断迭代，直至执行到`5+recursive_sum([])`时，可以发现调用递归的输入值为`[]`，满足结束条件，因此执行`if not lst`语句块，返回值为0，结束递归迭代。当结束递归后，将每一次的返回值代入上一次迭代的返回值，计算获得最终结果为`1+2+3+4+5+0=15`。

从上述递归过程的描述中，可以确定递归的基本逻辑结构，无外乎调用自身即返回值计算逻辑和该逻辑能够产生结束条件，因此具有递归属性的问题都可以此点切入设计代码。

<img src="./imgs/pc_6_01.jpg" height='auto' width=100% title="caDesign">


</td>
<td>

```python
def recursive_sum(lst):
    if not lst:
        print("if not lst:{}".format(lst))
        return 0
    else:
        print(lst[1:])
        return lst[0]+recursive_sum(lst[1:])
    
lst=[1,2,3,4,5]    
lst_sum=recursive_sum(lst)
print("--"*30)
print(lst_sum)
```

</td>
<td>

    [2, 3, 4, 5]
    [3, 4, 5]
    [4, 5]
    [5]
    []
    if not lst:[]
    ------------------------------------------------------------
    15

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

可以通过小的语句核实确定某些语句的运行结果，辅助理解程序。

</td>
<td>

```python
print(not [])
print(not [5])
print([5][1:])
```

</td>
<td>

    True
    False
    []

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

返回值是需要代入到上次迭代调用自身的部分，因此只要返回值满足计算要求，可以根据需要返回任何值。下述代码将结束条件返回值配置为入`1000`时，计算结果为`1015`。


</td>
<td>

```python
def recursive_sum(lst):
    if not lst:
        print("if not lst:{}".format(lst))
        return 1000
    else:
        print(lst[1:])
        return lst[0]+recursive_sum(lst[1:])
    
lst=[1,2,3,4,5]    
lst_sum=recursive_sum(lst)
print("--"*30)
print(lst_sum)
```

</td>
<td>

    [2, 3, 4, 5]
    [3, 4, 5]
    [4, 5]
    [5]
    []
    if not lst:[]
    ------------------------------------------------------------
    1015

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

* 递归阶乘

直接可以替换递归求和图中对应的值，来表达递归阶乘的过程。可以解释为：开始输入值（开始值）为`6`, 不满足结束条件`if n==1`，执行`else`语句块，返回值语句为`return n*recursive_factorial(n-1)`，将值代入语句（表达式），为`6×recursive_factorial(5)`，其中包含调用自身语句`recursive_factorial(5)`，因此从函数名定义行重新开始执行该函数；因为输入参数值`5`不满足结束条件，再次执行`else`语句块，将值代入语句（表达式），为`5×recursive_factorial(4)`，继续调用自身，此时的输入值为`4`；以次类推，不断迭代，直至执行到`2×recursive_factorial(1)`时，可以发现调用递归的输入值为`1`，满足结束条件，因此执行`if n==1`语句块，返回值为1，结束递归迭代。当结束递归后，将每一次的返回值代入上一次迭代的返回值，计算获得最终结果为`6×5×4×3×2×1=720`。

<img src="./imgs/pc_6_02.jpg" height='auto' width=100% title="caDesign">


</td>
<td>

```python
def recursive_factorial(n):
    if n==1:
        return 1
    else:
        return n*recursive_factorial(n-1)

fatorial_result=recursive_factorial(6)
print(fatorial_result)
```

</td>
<td>

    720

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

* 用循环语句替换递归的方法

递归的方法可以用循环的方式解决，例如下述代码改写的递归求和及阶乘。虽然循环的方式更加易读，但是递归的方法似乎也符合python的宗旨，保持简单，除非它必须复杂！因此，究竟是使用递归还是循环，可以由代码的作者自己决定。



</td>
<td>

```python
lst=[1,2,3,4,5]  
lst_sum=0
while lst:
    lst_sum+=lst[0]
    lst=lst[1:]
print(lst_sum)

n=6
fatorial_result=1
while n:
    if n==1:break
    else:
        fatorial_result*=n
        n=n-1
print(fatorial_result)
```

</td>
<td>

    15
    720

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

* 递归列表展平

下述递归嵌套列表展平的代码，虽然代码行数不多，但是相对递归求和和阶乘要复杂。这里需要注意两个问题，一个是存在有三个条件，`if`是结束条件。`elif`和`else`分别针对两种不同情况采取的措施。结束条件不是像递归求和和阶乘最后执行一次结束，是对`elif`和`else`下不断调用自身，最终达到各个分枝下满足结束条件的输入参数，逐个结束各个分枝；另一个是，返回值中存在`return flatten_lst(lst[0])+flatten_lst(lst[1:])`，为调用自身的函数求和，需要从左到右一次计算，先返回` flatten_lst(lst[0])`部分，待这部分递归完毕后，在执行`flatten_lst(lst[1:])`部分，而各自部分同样有满足`elif isinstance(lst[0],list)`条件的对象，因此还会产生调用自身两个函数相加的情况，依旧从左到右依次计算，至递归结束。具体的过程可以从图中观察，一个是正序过程，由`k-i,j`的方式标注执行顺序；粉色部分则是逆序逐步代入值的过程。比较清晰的表述了该递归的整个过程。

> 当很难通过代码直接理解算法时，必然需要通过`print()`，打印各个变化值查看整个过程。如果打印的对象比较多时，尤其类似递归这样要不断循环的算法，最好给出些辅助标识，定位每一轮次迭代，方便查看每次迭代对应变量的变化，从而找出规律，理解算法。

<img src="./imgs/pc_6_03.jpg" height='auto' width=100% title="caDesign">

</td>
<td>

```python
i=0
j=0
k=0
def flatten_lst(lst):
    global k,i,j
    
    print("#"*30,'%d-%d-%d'%(k,i,j))
    print("lst:",lst)
    if lst==[]:
        print('_'*30,'if')
        k+=1
        return lst
    
    elif isinstance(lst[0],list):
        print('_'*30,"elif")
        print(lst[0],';',lst[1:])
        i+=1
        return flatten_lst(lst[0])+flatten_lst(lst[1:])
    
    else:
        print('_'*30,'else')
        print(lst[:1],";",lst[1:])
        j+=1
        return lst[:1]+flatten_lst(lst[1:])
    
nested_lst=[['A','B',['C','D'],'E'],[6,[7,8,[9]]]]   
flatten_nested_lst=flatten_lst(nested_lst)
print("--"*30)
print(flatten_nested_lst)    
```

</td>
<td>


    ############################## 0-0-0
    lst: [['A', 'B', ['C', 'D'], 'E'], [6, [7, 8, [9]]]]
    ______________________________ elif
    ['A', 'B', ['C', 'D'], 'E'] ; [[6, [7, 8, [9]]]]
    ############################## 0-1-0
    lst: ['A', 'B', ['C', 'D'], 'E']
    ______________________________ else
    ['A'] ; ['B', ['C', 'D'], 'E']
    ############################## 0-1-1
    lst: ['B', ['C', 'D'], 'E']
    ______________________________ else
    ['B'] ; [['C', 'D'], 'E']
    ############################## 0-1-2
    lst: [['C', 'D'], 'E']
    ______________________________ elif
    ['C', 'D'] ; ['E']
    ############################## 0-2-2
    lst: ['C', 'D']
    ______________________________ else
    ['C'] ; ['D']
    ############################## 0-2-3
    lst: ['D']
    ______________________________ else
    ['D'] ; []
    ############################## 0-2-4
    lst: []
    ______________________________ if
    ############################## 1-2-4
    lst: ['E']
    ______________________________ else
    ['E'] ; []
    ############################## 1-2-5
    lst: []
    ______________________________ if
    ############################## 2-2-5
    lst: [[6, [7, 8, [9]]]]
    ______________________________ elif
    [6, [7, 8, [9]]] ; []
    ############################## 2-3-5
    lst: [6, [7, 8, [9]]]
    ______________________________ else
    [6] ; [[7, 8, [9]]]
    ############################## 2-3-6
    lst: [[7, 8, [9]]]
    ______________________________ elif
    [7, 8, [9]] ; []
    ############################## 2-4-6
    lst: [7, 8, [9]]
    ______________________________ else
    [7] ; [8, [9]]
    ############################## 2-4-7
    lst: [8, [9]]
    ______________________________ else
    [8] ; [[9]]
    ############################## 2-4-8
    lst: [[9]]
    ______________________________ elif
    [9] ; []
    ############################## 2-5-8
    lst: [9]
    ______________________________ else
    [9] ; []
    ############################## 2-5-9
    lst: []
    ______________________________ if
    ############################## 3-5-9
    lst: []
    ______________________________ if
    ############################## 4-5-9
    lst: []
    ______________________________ if
    ############################## 5-5-9
    lst: []
    ______________________________ if
    ------------------------------------------------------------
    ['A', 'B', 'C', 'D', 'E', 6, 7, 8, 9]


</td>
<td>
</td>
</tr>


<tr>
<td> 

__6.2__ 匿名函数（lambda）

</td>
<td>


lambda的基本语法为`lambda argument1,argument2,argument3,...,argumentN:expression using arguments`，需要注意的是lambda使用的是表达式（expression）不是语句（statement），返回一个函数表达式，不需要定义函数名（因此称为匿名函数）。lambda的表达式中可以加入`if`条件语句，语法为`lambda <arguments>:<return value if condition is True> if <condition> else <return value if condition is False>`；如果包含类似`elif`的结构，则语法为`lambda <args>:<return value> if <condition> else (return value if <condition> else <return value>)`。

对于较为简单的函数定义，使用lambda为内联函数（inline function），可以简化代码，而`def`方法则会稍显繁琐。


</td>
<td>


```python
def sum_func(x,y,z):
    return x+y+z
lst=[1,2,3]
print(sum_func(*lst))

sum_lambda=lambda x,y,z:x+y+z
print(sum_lambda(*lst))

print("-"*60)
comparison_operation_A=lambda x:True if x>10 else False #只有一个条件
print(comparison_operation_A(20))

CompOpera_B=lambda x:True if (x>10 and x<20) else False #包含多个条件，需要括起
print(CompOpera_B(18))

CompOpera_C=lambda x:x>10 and x<20 #不使用if else的方式
print(CompOpera_C(18))

print("-"*60)      
CompOpera_D=lambda x:1 if x<10 else (2 if x<20 else 0) #类似elif
print(CompOpera_D(7))
print(CompOpera_D(15))
print(CompOpera_D(30))
```

</td>
<td>

    6
    6
    ------------------------------------------------------------
    True
    True
    True
    ------------------------------------------------------------
    1
    2
    0

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

* 嵌套的匿名函数

与嵌套函数类似。


</td>
<td>

```python
nested_sum=lambda x:lambda y:x+y
sum_instance=nested_sum(10) #输入值对应参数x
print(sum_instance(20)) #输入值对应参数y
print(sum_instance(50))
```

</td>
<td>

    30
    60

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

* lambda 与filter(), map(),reduce(),sorted

lambda经常配合其它以函数作为输入参数的函数，可以非常便捷的以`inline function`方式行内完成代码。通过`help()`非常方便的确定`filter(), map(),reduce(),sorted`输入参数包含使用函数作为参数的部分。



</td>
<td>



```python
print(help(filter))
print("+"*60)  
print(help(map))
print("+"*60)  
from functools import reduce
print(help(reduce))
print("+"*60)  
print(help(sorted))
```

</td>
<td>


    Help on class filter in module builtins:
    
    class filter(object)
     |  filter(function or None, iterable) --> filter object
     |  
     |  Return an iterator yielding those items of iterable for which function(item)
     |  is true. If function is None, return the items that are true.
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
    
    None
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
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
    
    None
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    Help on built-in function reduce in module _functools:
    
    reduce(...)
        reduce(function, sequence[, initial]) -> value
        
        Apply a function of two arguments cumulatively to the items of a sequence,
        from left to right, so as to reduce the sequence to a single value.
        For example, reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) calculates
        ((((1+2)+3)+4)+5).  If initial is present, it is placed before the items
        of the sequence in the calculation, and serves as a default when the
        sequence is empty.
    
    None
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    Help on built-in function sorted in module builtins:
    
    sorted(iterable, /, *, key=None, reverse=False)
        Return a new list containing all items from the iterable in ascending order.
        
        A custom key function can be supplied to customize the sort order, and the
        reverse flag can be set to request the result in descending order.
    
    None


</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


如果某一函数或者方法的输入参数有函数参数，则首先考虑使用`lambda`的方式，这经常用于[pandas](https://pandas.pydata.org/)下配合方法`apply`，根据行数据增加新的列数据。

下述案例将以字典保存的保龄球大赛得分转换为`DataFrame`数据格式，并用`apply`方法，以`lambda`方式计算标准计分（Standard Score，z_score，代表原始数值和平均值之间的距离，并以标准差为单位计算，即z-score是从感兴趣的点到均值之间有多少个标准差，这样就可以在不同组数据间比较某一数值的重要程度。）

> 浅试pandas的DataFrame数据结构。pandas数据处理方法异常丰富，是数据处理的核心应用库，可以查看pandas手册，或相关说明。


</td>
<td>


```python
vals=[2,3,4,10,15,17,19,30,50]
filtered_vals=filter(lambda x:x>10, vals) #返回的是一个迭代器（iterator）
print(filtered_vals)
print(list(filtered_vals))

print("-"*60)  
mapped_vals=map(lambda x:x**2,vals)
print(list(mapped_vals))

print("-"*60)  
reduced_vals=reduce(lambda x,y:x+y, vals) #相当于求和
print(reduced_vals)

print("-"*60)  
scores=[('Mason',90),('Reece',81),('A',73),('B',97),('C',85)]
sorted_vals=sorted(scores,key=lambda score:score[1])
print(sorted_vals)
```

</td>
<td>


    <filter object at 0x0000020982DF1730>
    [15, 17, 19, 30, 50]
    ------------------------------------------------------------
    [4, 9, 16, 100, 225, 289, 361, 900, 2500]
    ------------------------------------------------------------
    150
    ------------------------------------------------------------
    [('A', 73), ('Reece', 81), ('C', 85), ('Mason', 90), ('B', 97)]
 

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
bowlingContest_scores_dic={'Barney':86,'Harold':73,'Chris':124,'Neil':111,'Tony':90,'Simon':38,
                           'Jo':84,'Dina':71,'Graham':103,'Joe':85,'Alan':90,'Billy':89,
                           'Gordon':229,'Wade':77,'Cliff':59,'Arthur':95,'David':70,'Charles':88}
                          
print(bowlingContest_scores_dic)

import pandas as pd
bowlingContest_scores_df=pd.DataFrame.from_dict(bowlingContest_scores_dic,orient='index',columns=['score'])
print(bowlingContest_scores_df)
scores_mean=bowlingContest_scores_df.score.mean()
scores_std=bowlingContest_scores_df.score.std()
print("-"*60)  
print('scores_mean=%.2f,scores_std=%.2f'%(scores_mean,scores_std))
bowlingContest_scores_df['z_score']=bowlingContest_scores_df.score.apply(lambda score:round((score-scores_mean)/scores_std,3))
print(bowlingContest_scores_df)
```

</td>
<td>


    {'Barney': 86, 'Harold': 73, 'Chris': 124, 'Neil': 111, 'Tony': 90, 'Simon': 38, 'Jo': 84, 'Dina': 71, 'Graham': 103, 'Joe': 85, 'Alan': 90, 'Billy': 89, 'Gordon': 229, 'Wade': 77, 'Cliff': 59, 'Arthur': 95, 'David': 70, 'Charles': 88}
             score
    Barney      86
    Harold      73
    Chris      124
    Neil       111
    Tony        90
    Simon       38
    Jo          84
    Dina        71
    Graham     103
    Joe         85
    Alan        90
    Billy       89
    Gordon     229
    Wade        77
    Cliff       59
    Arthur      95
    David       70
    Charles     88
    ------------------------------------------------------------
    scores_mean=92.33,scores_std=39.09
             score  z_score
    Barney      86   -0.162
    Harold      73   -0.495
    Chris      124    0.810
    Neil       111    0.477
    Tony        90   -0.060
    Simon       38   -1.390
    Jo          84   -0.213
    Dina        71   -0.546
    Graham     103    0.273
    Joe         85   -0.188
    Alan        90   -0.060
    Billy       89   -0.085
    Gordon     229    3.496
    Wade        77   -0.392
    Cliff       59   -0.853
    Arthur      95    0.068
    David       70   -0.571
    Charles     88   -0.111
 

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


* 用lambda定义递归展平嵌套列表

前述递归嵌套列表是根据索引为0的项值是否为列表给出`elif`和`else`的处理路径，如果列表为空则执行`if`，其为终止条件。此次给出的方法是使用lambda函数，lambda函数组合了列表推导式的方法，递归lambda自身完成嵌套列表的展平。直接看包括列表推导式和递归的lambda函数理解递归的过程，因为无法`print()`变量，很难查看整个计算流程，因此需要将lambda转换为`def`的形式，并将列表推导式转换为`for`循环。

图解释了整个计算流程。对循环的子列表执行递归操作，通过判断该列表是否为列表，执行`if`或者`else`语句块。`else`语句块为终止条件，为列表项值不再是子列表，而是单个值时的情况，则返回包含该一个值的列表，通过`extend`的方法，将其追加到变量`lst_collection`中。注意，追加的过程，是根据循环从左到右，从外到内（子列表）迭代过程的逆序，例如先`['A','B',['C','D'],'E']` 子列表，然后`[6,[7,8,[9]]]`子列表；前者子列表仍旧从左到右，先`A`，再`B`，在调用自身位置追加到列表为`['A','B']`；到`['C','D']`时，先执行嵌套子列表，从左到右，在调用到自身位置处追加到列表为`['C','D']`；在`A`,`B`和`['C','D']`齐平的位置合并列表为`['A','B','C','D','E']`，依次类推。

终止条件的返回值是返回递归调用自身的位置，例如满足终止条件的`A`，返回值为`['A']`，该值对应到递归调用的位置`lst_collection.extend(flatten_lst_loop(n_lst))`，而被追加到` lst_collection`列表中，因为返回的是含一个值的列表，因此使用`extend`方法。

思考：如果将`else`终止条件返回值改为`lst`，而不是`[lst]`，并将调用位置语句改为`lst_collection.append(flatten_lst_loop(n_lst))`，即将`extend`改为`append`，为什么不可以？

<img src="./imgs/pc_6_04.jpg" height='auto' width=100% title="caDesign">



</td>
<td>


```python
flatten_lst=lambda lst: [m for n_lst in lst for m in flatten_lst(n_lst)] if type(lst) is list else [lst]

nested_lst=[['A','B',['C','D'],'E'],[6,[7,8,[9]]]]  
print(flatten_lst(nested_lst))
```

</td>
<td>

    ['A', 'B', 'C', 'D', 'E', 6, 7, 8, 9]

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

将lambda转换为`def`定义的形式，并拆解列表推导式，方便通过`print`方式查看变量值的变化。

</td>
<td>


```python
i=0
def flatten_lst_loop(lst):  
    global i
    
    print("-"*50,i)
    lst_collection=[]
    if type(lst) is list:
        i+=1
        print('**',lst)
        for n_lst in lst:
            print("##",n_lst)            
            lst_collection.extend(flatten_lst_loop(n_lst))
            print(':',lst_collection)
    else:
        i+=1
        print('++',lst)
        return [lst]
    return lst_collection
            
flattened_nestedLst=flatten_lst_loop(nested_lst)
print("-"*60)
print(flattened_nestedLst)
```

</td>
<td>


    -------------------------------------------------- 0
    ** [['A', 'B', ['C', 'D'], 'E'], [6, [7, 8, [9]]]]
    ## ['A', 'B', ['C', 'D'], 'E']
    -------------------------------------------------- 1
    ** ['A', 'B', ['C', 'D'], 'E']
    ## A
    -------------------------------------------------- 2
    ++ A
    : ['A']
    ## B
    -------------------------------------------------- 3
    ++ B
    : ['A', 'B']
    ## ['C', 'D']
    -------------------------------------------------- 4
    ** ['C', 'D']
    ## C
    -------------------------------------------------- 5
    ++ C
    : ['C']
    ## D
    -------------------------------------------------- 6
    ++ D
    : ['C', 'D']
    : ['A', 'B', 'C', 'D']
    ## E
    -------------------------------------------------- 7
    ++ E
    : ['A', 'B', 'C', 'D', 'E']
    : ['A', 'B', 'C', 'D', 'E']
    ## [6, [7, 8, [9]]]
    -------------------------------------------------- 8
    ** [6, [7, 8, [9]]]
    ## 6
    -------------------------------------------------- 9
    ++ 6
    : [6]
    ## [7, 8, [9]]
    -------------------------------------------------- 10
    ** [7, 8, [9]]
    ## 7
    -------------------------------------------------- 11
    ++ 7
    : [7]
    ## 8
    -------------------------------------------------- 12
    ++ 8
    : [7, 8]
    ## [9]
    -------------------------------------------------- 13
    ** [9]
    ## 9
    -------------------------------------------------- 14
    ++ 9
    : [9]
    : [7, 8, 9]
    : [6, 7, 8, 9]
    : ['A', 'B', 'C', 'D', 'E', 6, 7, 8, 9]
    ------------------------------------------------------------
    ['A', 'B', 'C', 'D', 'E', 6, 7, 8, 9]
    


</td>
<td>
</td>
</tr>

<tr>
<td> 

__6.3__ 生成器（generator）

</td>
<td>


生成器包括生成器函数和生成器表达式。生成器不会一次性计算所有结果，例如以列表形式返回所有计算值（如果为海量数据，列表形式会非常耗内存），而是根据需要提取数值，这样可以增加计算的效率，节约内存空间。例如`filter()`，`map()`，`zip()`等函数的返回值均为迭代器（iterator）（可迭代对象），即生成器返回迭代器（对象）。

生成器函数，就是在`def`定义函数时，返回值以`yield`的方式一次返回一个结果；生成器表达式，就是将列表推导式的`[]`改为`()`就可。或者使用`iter()`函数将可迭代对象转换为迭代器（iterator）。

通过`help()`查看生成器说明文件，有`__next__`方法，即`next(iterable)`实现逐个读取生成器返回的迭代对象。

* 生成器函数

</td>
<td>


```python
def squared_generator(range_start,range_stop,range_step=1):
    for i in range(range_start,range_stop,range_step):
        yield i**2
squared_iterable=squared_generator(1,5)
print(squared_iterable)

for i in squared_iterable:print(i)
print("-"*60)
for i in squared_iterable:print(i) #当迭代完毕后，在执行则为空，需重新调用生成器函数
```

</td>
<td>

    <generator object squared_generator at 0x000001E013AF4430>
    1
    4
    9
    16
    ------------------------------------------------------------
  

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
squared_iterable=squared_generator(1,5)
for i in squared_iterable:print(i)

```

</td>
<td>

    1
    4
    9
    16

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
def computing_generator(x):
    x+=1
    print('Performed addition')
    yield x
    
    x*=2
    print('Performed multiplication')
    yield x

computing_iterable=computing_generator(5)
print(next(computing_iterable))
print(next(computing_iterable))
```

</td>
<td>

    Performed addition
    6
    Performed multiplication
    12

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
def infinite_generator():
    i=0
    while True:
        i+=1
        yield i
infinite_iterable=infinite()        
print(next(infinite_iterable))
print(next(infinite_iterable))
print(next(infinite_iterable))

help(infinite_iterable)

```

</td>
<td>


    1
    2
    3
    Help on generator object:
    
    infinite = class generator(object)
     |  Methods defined here:
     |  
     |  __del__(...)
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
     |  __repr__(self, /)
     |      Return repr(self).
     |  
     |  close(...)
     |      close() -> raise GeneratorExit inside generator.
     |  
     |  send(...)
     |      send(arg) -> send 'arg' into generator,
     |      return next yielded value or raise StopIteration.
     |  
     |  throw(...)
     |      throw(typ[,val[,tb]]) -> raise exception in generator,
     |      return next yielded value or raise StopIteration.
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors defined here:
     |  
     |  gi_code
     |  
     |  gi_frame
     |  
     |  gi_running
     |  
     |  gi_yieldfrom
     |      object being iterated by yield from, or None
    
 

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

* 迭代对象的`send()`方法

`send()`方法可以向`yield`表达式传递参数，作为`yield`表达式的值。


</td>
<td>


```python
def double_inputs():
    while True:
        x=yield
        print("-"*10,x)
        yield x*2
        
di_gen= double_inputs()
print(next(di_gen))
print(di_gen.send(5))
next(di_gen)
print(di_gen.send(7))
```

</td>
<td>


    None
    ---------- 5
    10
    ---------- 7
    14


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
def squared_generator():
    for i in range(1,10):
        print("-"*10,i)
        yield i**2
sg=squared_generator()
print(next(sg))
print(next(sg))
print(next(sg))
print("-"*60)
print(sg.send(7))
print(sg.send(9))
```

</td>
<td>

    ---------- 1
    1
    ---------- 2
    4
    ---------- 3
    9
    ------------------------------------------------------------
    ---------- 4
    16
    ---------- 5
    25

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

需要注意的是，下述计数求和代码，需要先运行`next()`，然后执行`send()`，否则提示`TypeError: can't send non-None value to a just-started generator`错误。`send()`将值传递给的是`yield`表达式，因此可以看到下述代码，在`send(1)`之后，先运行` delta=yield countNtotal_nt(count,total)`语句，此时`delta`对象的值为1，执行`if`语句块，然后，依次迭代。



</td>
<td>


```python
from collections import namedtuple

countNtotal_nt=namedtuple('countNsum',['count','total'])

def countNtotal(count=0,total=0):
    i=0
    while True:
        print('-'*10,i)
        delta=yield countNtotal_nt(count,total)
        print("###",delta)
        if delta:
            print('+'*10,i)
            count+=1
            total+=delta
            i+=1
            
vals=[1,2,3,4,None,7,8,9]
countNtotal_gen=countNtotal()
print(next(countNtotal_gen))
print("-"*60)
for v in vals:print(countNtotal_gen.send(v))
```

</td>
<td>


    ---------- 0
    countNsum(count=0, total=0)
    ------------------------------------------------------------
    ### 1
    ++++++++++ 0
    ---------- 1
    countNsum(count=1, total=1)
    ### 2
    ++++++++++ 1
    ---------- 2
    countNsum(count=2, total=3)
    ### 3
    ++++++++++ 2
    ---------- 3
    countNsum(count=3, total=6)
    ### 4
    ++++++++++ 3
    ---------- 4
    countNsum(count=4, total=10)
    ### None
    ---------- 4
    countNsum(count=4, total=10)
    ### 7
    ++++++++++ 4
    ---------- 5
    countNsum(count=5, total=17)
    ### 8
    ++++++++++ 5
    ---------- 6
    countNsum(count=6, total=25)
    ### 9
    ++++++++++ 6
    ---------- 7
    countNsum(count=7, total=34)
    


</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

* 生成器表达式

</td>
<td>


```python
plus10_generator=(i+10 for i  in [1,2,3,4,5])
print(plus10_generator)
print(list(plus10_generator))

print("-"*60)
lst_iter=iter([1,2,3,4,5])
print(lst_iter)
print(next(lst_iter))
print(next(lst_iter))
```

</td>
<td>

    <generator object <genexpr> at 0x000001E013AE7430>
    [11, 12, 13, 14, 15]
    ------------------------------------------------------------
    <list_iterator object at 0x000001E013BB9670>
    1
    2
    

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


* 用生成器递归展平列表

该方法逻辑同`用lambda定义递归展平嵌套列表`，只是这里有些处理方法的替换，1是，将`if type(lst) is list:`判断条件用`try,except`异常处理替换，当`lst`变量不为列表时，就会引发异常，从而执行`except`语句块，为结束条件；2是，用`yield`替换`lst_collection.extend(flatten_lst_loop(n_lst))`值追加到列表中，而是直接构建生成器，产生迭代对象。


</td>
<td>


```python
def flatten_lst_generator(lst):
    try: #使用语句try/except捕捉异常
        for n_lst  in lst: 
             for m in flatten_lst_generator(n_lst):
                yield m
               
    except:  
        yield lst
        
nested_lst=[['A','B',['C','D'],'E'],[6,[7,8,[9]]]]  
flatten_nestedLst=flatten_lst_generator(nested_lst)
print(list(flatten_nestedLst))               
```


</td>
<td>

    ['A', 'B', 'C', 'D', 'E', 6, 7, 8, 9]
</td>
<td>
</td>
</tr>


<tr>
<td> 

__6.4__  [itertools](https://docs.python.org/3/library/itertools.html)库

</td>
<td>


[itertools](https://docs.python.org/3/library/itertools.html)库可以实现一系列迭代器（iterator），可以简洁而高效的完成相关的算法，而避免自行重新编写相关的工具。理解与善用`itertools`库，可以提高代码书写效率，及高效利用内存。对`itertools`可以直接查看官网。下述摘抄了官网说明表格，记录如下，方便查阅。通过表格给出的参数，结果和示例很容易推断工具的使用方法和算法目的。

* Infinite iterators（无穷迭代器）:

| Iterator | Arguments     | Results                                        | Example                               |
|----------|---------------|------------------------------------------------|---------------------------------------|
| count()  | start, [step] | start, start+step, start+2*step, …             | count(10) --> 10 11 12 13 14 ...      |
| cycle()  | p             | p0, p1, … plast, p0, p1, …                     | cycle('ABCD') --> A B C D A B C D ... |
| repeat() | elem [,n]     | elem, elem, elem, … endlessly or up to n times | repeat(10, 3) --> 10 10 10            |

* Iterators terminating on the shortest input sequence（根据最短输入序列长度停止的迭代器）:

| accumulate()          | p [,func]                   | p0, p0+p1, p0+p1+p2, …                     | accumulate([1,2,3,4,5]) --> 1 3 6 10 15                  |
|-----------------------|-----------------------------|--------------------------------------------|----------------------------------------------------------|
| chain()               | p, q, …                     | p0, p1, … plast, q0, q1, …                 | chain('ABC', 'DEF') --> A B C D E F                      |
| chain.from_iterable() | iterable                    | p0, p1, … plast, q0, q1, …                 | chain.from_iterable(['ABC', 'DEF']) --> A B C D E F      |
| compress()            | data, selectors             | (d[0] if s[0]), (d[1] if s[1]), …          | compress('ABCDEF', [1,0,1,0,1,1]) --> A C E F            |
| dropwhile()           | pred, seq                   | seq[n], seq[n+1], starting when pred fails | dropwhile(lambda x: x<5, [1,4,6,4,1]) --> 6 4 1          |
| filterfalse()         | pred, seq                   | elements of seq where pred(elem) is false  | filterfalse(lambda x: x%2, range(10)) --> 0 2 4 6 8      |
| groupby()             | iterable[, key]             | sub-iterators grouped by value of key(v)   |                                                          |
| islice()              | seq, [start,] stop [, step] | elements from seq[start:stop:step]         | islice('ABCDEFG', 2, None) --> C D E F G                 |
| pairwise()            | iterable                    | (p[0], p[1]), (p[1], p[2])                 | pairwise('ABCDEFG') --> AB BC CD DE EF FG                |
| starmap()             | func, seq                   | func(*seq[0]), func(*seq[1]), …            | starmap(pow, [(2,5), (3,2), (10,3)]) --> 32 9 1000       |
| takewhile()           | pred, seq                   | seq[0], seq[1], until pred fails           | takewhile(lambda x: x<5, [1,4,6,4,1]) --> 1 4            |
| tee()                 | it, n                       | it1, it2, … itn splits one iterator into n |                                                          |
| zip_longest()         | p, q, …                     | (p[0], q[0]), (p[1], q[1]), …              | zip_longest('ABCD', 'xy', fillvalue='-') --> Ax By C- D- |

* Combinatoric iterators（排列组合迭代器）:

| Iterator                        | Arguments          | Results                                                       |
|---------------------------------|--------------------|---------------------------------------------------------------|
| product()                       | p, q, … [repeat=1] | cartesian product, equivalent to a nested for-loop            |
| permutations()                  | p[, r]             | r-length tuples, all possible orderings, no repeated elements |
| combinations()                  | p, r               | r-length tuples, in sorted order, no repeated elements        |
| combinations_with_replacement() | p, r               | r-length tuples, in sorted order, with repeated elements      |

</td>
<td>



</td>
<td>


</td>
<td>
</td>
</tr>







</table>

<span style = "color:Teal;background-color:;font-size:20.0pt">是否完成PCS_6(&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)</span>