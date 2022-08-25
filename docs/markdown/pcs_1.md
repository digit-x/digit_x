> Created on Thu Jun 23 18:21:05 2022 @author: Richie Bao-caDesign设计(cadesign.cn)

<style>
  code {
    white-space : pre-wrap !important;
    word-break: break-word;
  }
</style>

# Python Cheat Sheet-1. 善用print()，基础运算，变量及赋值

<span style = "color:Teal;background-color:;font-size:20.0pt">PCS_1</span>

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

__1.1__ 不知道运行结果，就很难写代码——善用print()；第一个python代码

</td>
<td>

print()是python语言中使用最为频繁的语句，在代码编写、调试过程中，要不断的用该语句来查看数据的值、数据的变化、数据的结构、变量所代表的内容、监测程序进程、及显示结果等。通过`print()`实时查看数据反馈，才可知道代码编写的目的是否达到，并做出反馈。善用print()是用python写代码的基础。

`print('Hello World!') `代码（打印显示一行字符串）基本成为所有类型编程语言第一行代码的标配，标志着正式开启代码学习的篇章。

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

用print()查看当前python版本。这里调入了python的一个[标准库](https://docs.python.org/3/library/index.html)[platform](https://docs.python.org/3/library/platform.html?highlight=platform#module-platform)的`python_version`方法查看python版本。调入库的方法，使用`import`， 如果仅调入库中的一个方法，则可以使用`from 'library name' import 'method'`。

</td>
<td>

```python
from platform import python_version
print(python_version())
```

</td>
<td>

3.8.13

</td>
<td></td>
</tr>

<tr>
<td></td>
<td>

`print("_"*50)`，这里对字符`"_"`乘以了一个数字，则复制该字符多少个; 对于字符串可以使用双引号，也可以使用单引号。但是希望内部字符包括单引号时，则外部使用双引号，而内部使用单引号。如果内容包括双引号时，则需要借助转义字符（escape character）`\`实现转义，即将python的特殊字符，例如表征字符串的双引号转换为普通字符串使用。当然，也可以配合使用三引号；如果，语句位于同一行，直接可以用`;`号分割，但是通常不会这么做，因为这使得代码的可读性变弱；backslash(\)可以将长文本切为多段输入，输出字符串不断行。

</td>
<td>

```python
print("Hello Python!")
print("_"*50)
print("编程让设计更具'创造力！'");print("Everybody should learn how to code a computer, because it teaches you how to think, and allows designers more creative!")
print("成为工具的\"建构者！\"")
print("""You must "type" each of these excercises in, mannually. \
If you copy and paste, you might as well as not even do them.""")
```

</td>
<td>

    Hello Python!
    __________________________________________________
    编程让设计更具'创造力！'
    Everybody should learn how to code a computer, because it teaches you how to think, and allows designers more creative!
    成为工具的"建构者！"
    You must "type" each of these excercises in, mannually. If you copy and paste, you might as well as not even do them.

</td>
<td></td>
</tr>

<tr>
<td>

__1.2__ 增加注释的必要性

</td>
<td>

注释包括单行注释，使用（hash）#开头；多行注释，使用''' comments '''，或者""" comments """。注释并不会被执行，解释器将忽略注释的所有内容。注释的目的，1-为作者的注解，方便日后查看已经写过的代码含义，避免重新解读（尤其对于复杂或不易理解的逻辑和算法）；2-方便交流，他人阅读该代码时，可以快速的知道代码书写的目的或逻辑；3-传递代码书写作者、日期、版权等辅助信息；4-书写函数时，以注释的方式说明函数的功用，输入参数和返回变量的数据类型及说明等。

> 注：用于函数说明时，如果是使用`spyder`交互式解释器编写代码，函数名行后回车，会提示是否书写函数说明，并自动配置下述格式，作者仅需要输入必要信息。 

</td>
<td>

```python
# 1-作者备忘注释，及说明方便交流
data_path='./data' #配置数据存储位置

# 2-辅助信息

"""
Created on Tue Feb 15 09:58:38 2022

@author: Richie Bao-caDesign设计(cadesign.cn)
"""

# 3- 用于函数说明
def cfg_load_yaml(ymlf_fp):
    '''
    读取 yaml 格式的配置文件

    Parameters
    ----------
    ymlf_fp : string
        配置文件路径.

    Returns
    -------
    cfg : yaml-dict
        读取到python中的配置信息.
    '''
    import yaml
    with open (ymlf_fp,'r') as ymlfile:
        cfg=yaml.safe_load(ymlfile)   
    return cfg
```

</td>
<td></td>
<td></td>
</tr>
<tr>
<td>

__1.3__ 基本的数据类型（Basic Data Types）及运算（Operations）

</td>
<td>

代码处理的对象就是数据，基本的数据类型包括整数(Integer,int)，实数（浮点型）（Real numbers, float），复数（Complex numbers，complex），字符（String, str）和布尔(Boolean，bool)。各种数据类型，都可以通过python内置函数（方法）`type`查看数据类型。

> 注：内置函数为可以直接调用的函数，直接使用而无需导入库（模块）。

</td>
<td>

```python
print(type(7))
print(type(3.1415926))
print(type(3+6j))
print(type('Small is Beautiful'))
print(type(True),type(False))
```

</td>
<td>

    <class 'int'>
    <class 'float'>
    <class 'complex'>
    <class 'str'>
    <class 'bool'> <class 'bool'>
    
</td>
<td></td>
</tr>
<tr>
<td>

* 变换数据类型

</td>
<td>

`int(value,base)`，其中`base`基数默认为10。`float(value)`只有一个输入参数。可已用内置函数转二进制、十进制和十六进制，其计算结果类型表述中`0b`代表二进制，`0o`代表八进制，`ox`代表十六进制。

</td>
<td>

```python
print(int(3.1415926))
print(int(2.7182818)) #直接使用int()会自动向下取整
print(int("255",10)) #字符串转整数。如果字符串内容为浮点数，则会提示错误

print(bin(12)) #转二进制（binary）
print(oct(12)) #转十进制（octal）
print(hex(12)) #转十六进制（hexadecimal）

print("_"*50)
print(float(64))
print(float("1.618034"))

print("_"*50)
print(complex(10))
print(complex("10+3j"))

print("_"*50)
print(bool(0))
print(bool(1))
print(bool())
print(bool(""))
print(bool("values"))

print("_"*50)
print(str(3.1415926),":",type(str(3.1415926)))
```

</td>
<td>

    3
    2
    255
    0b1100
    0o14
    0xc
    __________________________________________________
    64.0
    1.618034
    __________________________________________________
    (10+0j)
    (10+3j)
    __________________________________________________
    False
    True
    False
    False
    True
    __________________________________________________
    3.1415926 : <class 'str'>

</td>
<td></td>
</tr>
<tr>
<td>

* 运算类型（Types of Operators）

</td>
<td>

`6+7=13`中，数值（numerical values）`6`和`7`为操作数（operands）；`+`为运算符/操作符（operators）。

1. 算数运算符

| 运算（Syntax）  | 说明 (Description) |
|---|---|
| a+b | a加b (Addition)  |
| a-b  | a减b (Subtraction)  |
| a*b  | a乘以b  (Multiplication)|
| a/b  | a除以b (Division) |
| a//b  | a除以b后向下取整  (Floor Divisiont)|
| a**b  | a的b次方  (Exponential/Power)|
|a%b| 模运算（Modulus）。取模运算是计算两个数相除之后的余数|

</td>
<td>

```python
print(15//7)
print(15%7)
```

</td>
<td>

    2
    1

</td>
<td></td>
</tr>

<tr>
<td></td>

<td>

2. 比较运算符（Comparison/Relational Operators）

比较运算结果为布尔值（True 或False）。

| 运算（Syntax）  | 说明 (Description) |
|---|---|
| a>b、a>=b | 如果a大于(或大于等于)b，则结果为True (Greater than, Greater than or equal to)  |
| a<b、a<=b | 如果a小于等于(或小于)b，则结果为True  (Lesser than, Lesser than or equal to)|
| a==b  | 如果a等于b，则结果为True (Equal to)|
| a!=b  | 如果a不等于b，则结果为True (Not equals to)|

</td>
<td>

```python
print(6!=7)
print(6==7)

print("_"*50)
print("six"!="seven")
print("six"=="six")

print("_"*50)
print(2.718==2.718000)
```

</td>
<td>

    True
    False
    __________________________________________________
    True
    True
    __________________________________________________
    True

</td>
<td></td>
</tr>
<tr>
<td></td>
<td>

3. 赋值运算符（Assignment Operators）

赋值运算符相当于将等号右边的值按运算符计算到等号左边值，此时a为变量，而不是具体的值，计算后的值再赋值给变量a。

| 运算（Syntax）  | 等价于（Syntax Equivalence）|
|---|---|
| a+=b | a=a+b  |
| a-=b | a=a-b|
| a*=b  | a=a*b|
| a/=b  | a=a/b)|
|a//=b|a=a//b|
|a**=b|a=a**b|
|a%=b|a=a%b|

</td>
<td>

```python
i=0
i+=1
print(i)
i+=1
print(i)
```

</td>
<td>

    1
    2

</td>
<td></td>
</tr>
<tr>
<td></td>
<td>

4. 逻辑运算符 （Logical Operators）

| 运算（Syntax）  | 说明 (Description) |
|---|---|
| a and b | 都为True时，返回True  |
| a or b | 至少一个为True时，返回True|
| not a  |为True时返回False，为False时返回True |

</td>
<td>

```python
print(True and True)
print(True and False)
print(True or True)
print(True or False)
print(False and False)
print(not True)
print(not False)
```

</td>
<td>

    True
    False
    True
    True
    False
    False
    True

</td>
<td></td>
</tr>
<tr>
<td></td>
<td>

5. 按位运算符（Bitwise Operators）

按位运算符通常用于嵌入式系统，多个输入输出端口（高低电平）表示的命令操作中，在数据分析领域使用暂不常见。但`&`和`|`可以替代`and`和`or`逻辑运算符使用。

| 运算（Syntax）  | 说明 (Description) |
|---|---|
| a & b | 如果a和b均为True，则结果为True。对于整数（二进制），执行按位与操作。(Bitwise AND)|
| a \| b | 如果a和b任意一个为True，返回True。对于整数（二进制），执行按位或操作。(Bitwise OR)|
| a^b  |为True时返回False，为False时返回True。对于布尔值，如果a或b为True(但不都为True)，则结果为True。对于整数（二进制），执行按位异或操作。 (Bitwise XOR)|
|~a|对于整数（二进制），执行按位取反操作。(Bitwiese NOT)|
|a<<b|对于整数（二进制），对a执行按位左移b个位操作。(Bitwise left shift)|
|a>>b|对于整数（二进制），对a执行按位右移b个位操作。(Bitwise right shift)|

</td>
<td>

```python
print(True & True)
print(True & False)
print(True | True)
print(True | False)
print(False | False)

print("_"*50)
print(bin(7))
print(bin(0b0111))
print(0b0111) #会自动转换为十进制

print("_"*50)
print(bin(~0b0111))

print("_"*50)
print(bin(0b0111<<1))
print(bin(0b0111>>1))
```

</td>
<td>

    True
    False
    True
    True
    False
    __________________________________________________
    0b111
    0b111
    7
    __________________________________________________
    -0b1000
    __________________________________________________
    0b1110
    0b11

</td>
<td></td>
</tr>
<tr>
<td></td>
<td>

6. 成员运算幅 （Membership Operators）

用于判断一个对象是否在python序列中（例如，string(字符串), list(列表)，tuple(元组)，array(数组)等）。

| 运算（Syntax）  | 说明 (Description) |
|---|---|
| a in b | 如果a在序列b中，则为True |
| a not in b | 如果a不在序列b中，则为True|

</td>
<td>

```python
lst=[1,3,6,7,9]
string="python supports two membership operators, in and not in."

print(2 in lst)
print(3 in lst)
print("in" in string)
print("is not" not in string)
```

</td>
<td>

    False
    True
    True
    True

</td>
<td></td>
</tr>
<tr>
<td></td>
<td>

7. 同一运算符 （Identity Operators）

用于判断两个对象（例如变量）是否使用同一位置索引内存。可用内置函数`id()`查看对象唯一标识，即获取对象的内存地址。

| 运算（Syntax）  | 说明 (Description) |
|---|---|
| a is b | 如果变量a和b指向同一个python对象，则结果为True|
|a is not b | 	如果变量a和b指向不同的python对象，则结果为True|

</td>
<td>

```python
a=7
b=a
c=7
d=9

print(b is a)
print(c is a)
print(d is a)
print(id(a),id(b),id(c),id(d))
```

</td>
<td>

    True
    True
    False
    140730917330880 140730917330880 140730917330880 140730917330944

</td>
<td></td>
</tr>
<tr>
<td></td>
<td>

* 运算符优先级 （Precedence and Associativity Rule of Operators）

包括多个运算符时，优先顺序如下表。L2R表示Left to right（从左到右）；R2L表示Right to left(从右到左)

|运算符（Operator）   |  说明（Description） | 结合性（Associativity）  |
|---|---|---|
| ()<br /> **  | 圆括号（Parentheses）<br />幂（Exponential/Power）  |  L2R <br />R2L|
| +x,-x,~x <br />*,/,//,%  | 一元加（Unary Addition），一元减（Unary Subtraction）, 按位取反（Bitwise NOT）<br />乘（Multiplication），除(Division), 向下取整除（Floor Division）,取模运算（Modulus） | L2R<br />L2R  |
|  +,- | 算数加（Arithmetic addition）,算数减（Arithmetic subtraction）  | L2R  |
| <<,>>  | 按位左移（Bitwise shift left）, 按位右移（Bitwise shift right）  |  L2R |
| &  | 按位与（Bitwise AND）  | L2R  |
| ^  | 按位或（Bitwise OR）  | L2R  |
|  \|<br />==,!=,>,>=,<,<= |按位异或（Bitwise XOR）<br />比较运算符（Relational operators）   | L2R<br />L2R  |
| =,+=,-=,*=,/=,//=,**=,%= <br /> in, not in, is, is not | 赋值运算符（Assignment and Augmented assignment operators））<br />成员，同一运算符（Membership, Identity operators）  | R2L<br />L2R   |
|  Not <br /> And | 逻辑非（Logical NOT） <br />逻辑与（Logical AND） | L2R<br />L2R  |
|Or | 逻辑或（Logical OR）  |  L2R |

</td>
<td>

```python
x=7
print((x**2-2*x-3)/2)
```

</td>
<td>

    16.0

</td>
<td></td>
</tr>
<tr>
<td>

__1.4__ 变量及赋值 (Variables and assignment)

</td>
<td>

代码读起来应该像流畅的英语散文，而不是加密的密码。好的易读的变量名的定义正是让代码变的流畅的基础。变量名不能以数字和特殊字符为开头，也不可以内置的函数名定义，也不存在空格，如果由几个单词或数字组成变量名，通常由下划线连接，或者每一新单词首字母大写（通常尽量保持一种风格）。变量名定义不符合规范时，解释器会提示错误。

</td>
<td>

```python
func=2*y+1 #当程序逐行从上至下运行时，注意变量定义的顺序
y=5
print(func)
```

</td>
<td>

    11

</td>
<td></td>
</tr>
<tr>
<td></td>
<td>

如果想把变量值作为字符串的一部分打印出来，可以使用字符串格式化方法，例如`%`形式，或者`'{}'.format(variable)`方式。

</td>
<td>

```python
x=5.0
monadic_equation=2*x+1
print("monadic_equation=",monadic_equation)
print("monadic_equation=%.2f"%monadic_equation) #%字符串格式化方法
print("monadic_equation={:.2f}".format(monadic_equation)) #format()字符串格式化方法
```

</td>
<td>

    monadic_equation= 11.0
    monadic_equation=11.00
    monadic_equation=11.00

</td>
<td></td>
</tr>
<tr>
<td></td><td></td>
<td>

```python
city_name="Xi'an"
coordinate_longitude=108.942292
coordiante_latitude=34.261013
print("The longitude of the Xi'an coordinate is {lon:.2f}, and the latitude is {lat}.".format(lon=coordinate_longitude,lat=coordiante_latitude))
```

</td>
<td>

    The longitude of the Xi'an coordinate is 108.94, and the latitude is 34.261013.

</td>
<td></td>
</tr>
<tr>
<td></td><td></td>
<td>

```python
x,y,b=2,5,7 #序列解包（unpacking）。尝试，x,y,*z=0,1,2,3,4,5,6; x,y,*z=0,1; (x,y),(a,b)=(0,1),(2,3)
func_2=2*x+3*y+b
print("func_2={}".format(x,y,b,func_2))
```

</td>
<td>

    func_2=2

</td>
</tr>
<tr>
<td></td>
<td>

当代码变量不断的增多时，可以在尽量保持风格统一条件下，综合下划线和首字母大写，及专业术语缩写等方式表达。

</td>
<td>

```python
landuseName='General_Industrial'
landuseID=3
GIndustrial_area=5700
GIndustrial_greenArea=3214 
GIndustrial_GSR=GIndustrial_greenArea/GIndustrial_area*100 #green space ratio(GSR)
print("GIndustrial_GSR={:.3f}%".format(GIndustrial_GSR))
```

</td>
<td>

    GIndustrial_GSR=56.386%

</td>
<td></td>
</tr>
</table>
  
<span style = "color:Teal;background-color:;font-size:20.0pt">完成PCS_1(&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)</span>
