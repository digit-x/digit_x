> Created on Sat Sep  3 17:23:57 2022 @author: Richie Bao-caDesign设计(cadesign.cn)

<style>
  code {
    white-space : pre-wrap !important;
    word-break: break-word;
  }
</style>

# Python Cheat Sheet-8. 类(OOP)_Classes_定义，继承，__init__()构造方法，私有变量/方法

<span style = "color:Teal;background-color:;font-size:20.0pt">PCS_8</span>

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

__8.1__ OOP(Object-oriented programming)_面向对象编程，及Classes定义和`__init__()`构造方法

</td>
<td>


* OOP与定义类

面向对象编程（Object-oriented programming，OOP）是一种计算机编程模型，是围绕数据或对象而不是功能和逻辑来组织软件设计，更专注于对象与对象之间的交互，对象涉及的方法（methods）和属性(attributes)都在对象内部。其中类（Classes）是相同种类对象的抽象，是该类对象的公共属性；将该类实例化一个或多个对象（instance objects），即为该对象的实例。例如，定义一个类为鸟类，所有的鸟类都具有觅食的行为，可以将该行为定义为类的函数（成员函数，member functions），称为类的方法；可以将鸟类的飞翔动作定义为类的变量（成员变量），称为类的属性。鸟类通常会根据鸟类特征划分很多子类，例如雨燕目、 鸽形目、 雁形目等，不同子类的羽毛颜色、鸟鸣声都会不同，因此可以基于已有鸟类（父类，superclass），定义继承父类属性方法的子类（subclass）。

python类提供了面向对象编程的所有标准特性：类的继承机制（inheritance mechanism）允许多个基类（basse classes/superclasses）；一个派生类（derived class/subclass）可以覆盖基类或任何类的方法；一个方法可以调用（call/invoke）其基类的方法；对象可以包含任意数量和种类的数据；类具有动态特性（dynamic nature），可以在运行时添加或者删除属性方法等。

类定义的语法，可简单表示为：

```python
class ClassName:
    <statement-1>
    .
    .
    .
    <statement-N>
```

通常类的定义名首字母大写（共识非标准），其中语句一般为函数定义（允许其它类型的语句，例如定义变量赋值语句等）。下面引入[python官网的几个小案例](https://docs.python.org/3/tutorial/classes.html)，稍许调整,辅助理解类定义和调用的方式。

下述类定义的类名称为`SayHello`，该类之下的开始行，函数定义之前，定义了一行赋值语句`name_default='Who'`；定义的函数`helloWorld(self,name)`，其中`self`参数为类方法定义的第一个必要参数（本身的意思），在方法调用时指向类的实例。同时该函数也传入了另一个参数`name`，指定默认值为变量`name_default`。同时需要注意，通过该函数内`print(SayHello.name_default)`语句可以发现，通过`Class.attribute`的方式可以调用函数外类内的变量。`x=SayHello()`即实例化类对象语句，将类实例化为变量`x`，则该实例`x`具有了该类的所有属性和方法。`x.name_default`为调用类属性的方式；`x.helloWorld()`和`x.helloWorld('Swift')`为调用类方法（函数）的方式。

类可以使用内置方法`__doc__`读取类下的注释语句。


</td>
<td>

```python
class SayHello:
    """A simple example class"""
    name_default='Who'

    def helloWorld(self,name=name_default):        
        print(SayHello.name_default) #可以通过Class.attribute的方式获取函数外的变量值
        return '%s says hello, world!'%name
    
x=SayHello()
print(x.name_default)
print(x.helloWorld())
print(x.helloWorld('Swift'))

print('-'*50)
print(SayHello.__doc__)
```

</td>
<td>

    Who
    Who
    Who says hello, world!
    Who
    Swift says hello, world!
    --------------------------------------------------
    A simple example class

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


通过`help()`可以查看定义类的内容，这包括开始行的注释语句，方法（Methods）和属性（attributes），及定义的数据描述（data description defined）等。其中`__dict__`是存储对象属性的一个字典，键为属性名，而值为属性值。通过`__dict__`可以查看所有类的属性和方法。


</td>
<td>

```python
print(help(SayHello))
print(SayHello.__dict__)
print('-'*50)
print(SayHello.__dict__['name_default'])
print(x.__dict__)


```

</td>
<td>

    Help on class SayHello in module __main__:
    
    class SayHello(builtins.object)
     |  A simple example class
     |  
     |  Methods defined here:
     |  
     |  helloWorld(self, name='Who')
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
     |  name_default = 'Who'
    
    None
    {'__module__': '__main__', '__doc__': 'A simple example class', 'name_default': 'Who', 'helloWorld': <function SayHello.helloWorld at 0x00000152FBEC9CA0>, '__dict__': <attribute '__dict__' of 'SayHello' objects>, '__weakref__': <attribute '__weakref__' of 'SayHello' objects>}
    --------------------------------------------------
    Who
    {}

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

* `'__init__()'`构造方法与可变对象

如果类实例化时要传入参数（初始化参数），则可以定义类时，定义一个名为`__init__()`的特殊方法（constructor method，构造方法），下述案例为构造方法传入了一个`name`参数。在实例化时，如果该参数未指定默认值，则需要传入该参数值，如`d = Dog('Fido')`。类的实例化会自动掉调用`__init__()`来处理新创建的类的实例，通过` self.name = name`，初始化对应的参数，则`self.name`变量的作用域为整个类，内部的所有函数均可以调用`self.name`变量，外部实例化对象也可以通过`d.name`访问。也可以在该构造函数内书写其它代码完成相关的任务，例如定义新的变量，或相关计算。

下述引用官网的案例在说明`__init__()`构造方法初始化参数值和相关运算，也阐述在类下函数外定义的变量，如果为可变对象（mutable objects）例如`tricks = [] `，的变量`tricks`为一个列表，是可变对象，则实例化多个对象，执行类的`add_trick()`方法时，新的值都会被追加到`tricks`列表中，即各个实例化对象的`tricks`属性值相同，为共同追加的值列表。如果要将其分开，则需要在初始化时，通过`self.tricks = []`方法定义可变变量，`self`指向各个实例化对象自身，而不是类本身，因此在各各个实例化对象（`d`和`e`）执行`add_trick()`方法时，仅会将值追加在各自的`self.tricks`列表中。

在`SayHello`案例中，因为没有以`self`引入的变量，因此`print(x.__dict__)`时为空。在`Dog`案例中，因为存在变量`self.name`，因此`print(d.__dict__)`时，会返回`{'name': 'Fido'}`。


</td>
<td>

```python
class Dog:

    tricks = []             # mistaken use of a class variable

    def __init__(self, name):
        self.name = name

    def add_trick(self, trick):
        self.tricks.append(trick)
        
d = Dog('Fido')
print(d.name)
e = Dog('Buddy')
d.add_trick('roll over')
e.add_trick('play dead')
print(d.tricks)
print(e.tricks)

print("-"*50)
print(d.__dict__)
```

</td>
<td>

    Fido
    ['roll over', 'play dead']
    ['roll over', 'play dead']
    --------------------------------------------------
    {'name': 'Fido'}
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
class Dog:

    def __init__(self, name):
        self.name = name
        self.tricks = []    # creates a new empty list for each dog

    def add_trick(self, trick):
        self.tricks.append(trick)

d = Dog('Fido')
e = Dog('Buddy')
d.add_trick('roll over')
e.add_trick('play dead')
print(d.tricks)
print(e.tricks)
```

</td>
<td>

    ['roll over']
    ['play dead']

</td>
<td>
</td>
</tr>


<tr>
<td> 

__8.2__ Inheritance（继承）——superclasses（父类）和subclasses（子类）

</td>
<td>


继承主要作用是实现代码的复用，使子类拥有父类的方法和属性，在已有类的继承上扩展子类，可以大幅度降低代码书写量，并避免重复；同时，可以更加方便的理清类之间、子类父类之间的关系，这使得代码逻辑更加清晰；配置类的组合关系，则使得代码书写具有更好的弹性。

继承的语法为：

```python
class DerivedClassName(BaseClassName):
    <statement-1>
    .
    .
    .
    <statement-N>
```

如果父类和子类不在同一个模块（module）中（即不在同一个文件），则可以用下述方法：

```python
class DerivedClassName(modname.BaseClassName):
```

python的类支持多重继承（multiple inheritance），即可以同时继承多个父类，语法如下：

```python
class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    .
    .
    .
    <statement-N>
```

* 继承搜索

具有继承关系类的属性和方法搜索，基本路径是自下而上，自左而右。例如上述多重继承，首先在子类自身`DerivedClassName`中搜索，如果搜索不到，则向上在父类中搜索。首先在`Base1`中搜索，如果仍搜索不到，则继续搜索`Base2`和`Base3`，直至锁定，或者找不到属性和方法，则引发异常。

* 继承的第1个案例——1个父类+1个子类

下述案例定义了父类`Bird`，拥有的属性有：`fly`和`hungry`；拥有的方法有：`eat()`。定义的子类`Apodidae(Bird)`，继承父类`Bird`的所有属性和方法，同时增加了自身的属性有：`sound`；方法有：`sing()`。通过`super(Apodidae,self).__init__(hungry)`语句，为调用父类的初始化方法来初始化子类，也可以在子类中重新定义初始化方法来覆盖父类初始化的属性结果。


</td>
<td>


```python
class Bird:
    fly='Whirring' #美 /wɜːr/ 
    def __init__(self,hungry=True):
        self.hungry=hungry
    def eat(self):
        if self.hungry:
            print('Aaaah...')
            self.hungry=False
        else:
            print('No.Thanks!')

class Apodidae(Bird):  #/'æpədədi:/
    def __init__(self,sound='Squawk!',hungry=True):
        super(Apodidae,self).__init__(hungry)
        self.sound=sound #美 /skwɔːk/ 
    def sing(self):
        print(self.sound)
```


</td>
<td>



</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


将子类实例化为两个实例对象，为`swift`和`blackswift`，实例化时，传入的初始化参数不同，对于`swift`使用了默认参数值，即`(Squawk,True)`，而`blackswift`传入的参数为`('hooting',False)`。子类具有自身定义的属性（`sound`）和方法(`sing()`)，也继承了父类的属性（`hungry`,`fly`）和方法(`eat()`)。


</td>
<td>


```python
swift=Apodidae()
print("swift.fly->{}".format(swift.fly))
print("swift.hungry->{}".format(swift.hungry))
swift.eat()
print("swift.hungry->{}".format(swift.hungry))
swift.eat()
print("swift.sing->{}".format(swift.sound))
swift.sing()

print("-"*50)
blackswift=Apodidae('hooting',False)
print("blackswift.fly->{}".format(blackswift.fly))
print("blackswift.hungry->{}".format(blackswift.hungry))
blackswift.eat()
print("blackswift.hungry->{}".format(blackswift.hungry))
print("blackswift.sing->{}".format(blackswift.sound))
blackswift.sing()
```


</td>
<td>

    swift.fly->Whirring
    swift.hungry->True
    Aaaah...
    swift.hungry->False
    No.Thanks!
    swift.sing->Squawk!
    Squawk!
    --------------------------------------------------
    blackswift.fly->Whirring
    blackswift.hungry->False
    No.Thanks!
    blackswift.hungry->False
    blackswift.sing->hooting
    hooting
    

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

如果将父类`Bird`实例化为对象`crow`，则`crow`仅具有`Bird`类的属性和方法，而不具有子类的属性和方法。


</td>
<td>


```python
crow=Bird()
print("crow.fly->{}".format(crow.fly))
print("crow.hungry->{}".format(crow.hungry))
crow.eat()
print("crow.hungry->{}".format(crow.hungry))
print("-"*50)
crow.sing()
```

</td>
<td>


    crow.fly->Whirring
    crow.hungry->True
    Aaaah...
    crow.hungry->False
    --------------------------------------------------
    


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Input In [20], in <cell line: 7>()
          5 print("crow.hungry->{}".format(crow.hungry))
          6 print("-"*50)
    ----> 7 crow.sing()
    

    AttributeError: 'Bird' object has no attribute 'sing'


</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


* 继承的第2个案例——连续继承与`__repr__()`重写（overload）

该案例引自`Mark Lutz. (2013). Learning python(5th Edition).O'Reilly Media, Inc, USA`中关于类阐述的章节。其中`AttriDisplay`是`TopTest`的父类，`TopTest`是`SubTest`的父类。因为`__dict__`可以获取类的所有属性和方法的键值对，而`getattr()`内置函数用于一个对象的属性值，因此`AttrDisplay`方法`gatherAttrs()`提取了`self`（实例化对象）具有的属性，并存储在`attrs`列表中。

`repr()`通过`__repr__()`这个特殊方法来得到一个对象的字符串表示形式，方便查看。如果对象没有实现`__repr__()`，返回的字符串则如下述的`<__main__.Example object at 0x00000152FBE9E190>`。在`AttriDisplay`类定义中，则定义了`__repr__()`方法，该方法返回值必须是字符串对象。因此，当打印实例化对象时，则会执行`__repr__()`方法，返回对应格式化后的字符串。


</td>
<td>


```python
string='example'
print(repr(string))

class Example:pass
print(repr(Example()))
```

</td>
<td>

    <__main__.Example object at 0x00000152FBE9E190>
    'example'

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
class AttrDisplay:
    """
    Provides an inheritable display overload method that shows
    instances with their class names and a name=value pair for
    each attribute stored on the instance itself (but not attrs
    inherited from its classes). Can be mixed into any class,
    and will work on any instance.
    """
    def gatherAttrs(self):
        attrs = []
        for key in sorted(self.__dict__):
            attrs.append('%s=%s' % (key, getattr(self, key)))
        return ', '.join(attrs)
    def __repr__(self):
        return '[%s: %s]' % (self.__class__.__name__, self.gatherAttrs())
    
class TopTest(AttrDisplay):
    count = 0
    def __init__(self):
        self.attr1 = TopTest.count
        self.attr2 = TopTest.count+1
        TopTest.count += 2
class SubTest(TopTest):
    pass        

print(AttrDisplay()) #父类AttriDisplay没有self引导的属性
X, Y = TopTest(), SubTest() 
print(X)   
print(Y)    


print("-"*50)
print(getattr(X,'count'))
```


</td>
<td>



    [AttrDisplay: ]
    [TopTest: attr1=0, attr2=1]
    [SubTest: attr1=2, attr2=3]
    --------------------------------------------------
    4
 

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


* 继承的第3个案例——[shelve](https://docs.python.org/3/library/shelve.html)数据录入、更新、删除、与读取

`shelve`，是python数据储存的一种方式，类似key-value数据库存储形式，便于保存python对象。shelve只有一个open（）函数，用来打开指定的文件（字典），返回一个类似字典的对象。为了方便shelve存储方式的操作，编写下述`DB_Shelve`类，实现读、写、更新和删除4类基本操作。同时继承了`AttrDisplay`类，方便查看属性对象，当调试完后，可以移除该父类。

写入(`write()`)与更新(`update`)数据的方法使用了关键字(`**kwargs`)的形式。同时为了方便查看操作结果，增加了`feedback()`方法，可以在读写更新等方法中调用，反馈操作结果等信息。


</td>
<td>


```python
import shelve

class DB_Shelve(AttrDisplay):    
    '''
    为方便shelve库方式的数据存储，编写数据写入、读取、更新和删除等操作，方便调用。
    '''
    
    def __init__(self,db_fn,flag='c'):
        '''
        初始化读写shelve数据库（存储文件）的基本参数

        Parameters
        ----------
        db_fn : string
            存储文件路径名.
        flag : string, optional
            读写方式。包括r-只读；
            w-可读写;
            n-每次调用open()都重新创建一个空的文件，可读写. 
            The default is 'c'-如果数据文件不存在，就创建，允许读写.

        Returns
        -------
        None.

        '''
        self.db_fn=db_fn
        self.flag=flag
        
    def feedback(self,process=None,msg='OK.',data=None):
        '''
        用于反馈读写信息

        Parameters
        ----------
        process : string, optional
            标识操作，例如write, read,update或trash等. The default is None.
        msg : string, optional
            返回信息，表述运行是否成功等. The default is 'OK.'.
        data : any python built-in types, optional
            打印数据. The default is None.

        Returns
        -------
        dict
            反馈操作结果信息.

        '''
        if data:
            return {"process":process,'msg':msg,'data':data}
        else:
            return  {"process":process,'msg':msg}
    
    def write(self,**kwargs):
        '''
        向存储文件中写入数据

        Parameters
        ----------
        **kwargs : dict/kwargs
            以字典形式（加**dict）或关键字参数方式输入数据.

        Returns
        -------
        dict
            反馈操作结果.

        '''
        with shelve.open(filename=self.db_fn,flag=self.flag) as db:
            for k,v in kwargs.items():
                db[k]=v
        return self.feedback('write')
    
    def read(self,keys_selection=[]):
        '''
        从存储文件中读取数据

        Parameters
        ----------
        keys_selection : list, optional
            待读取存储文件数据的键. The default is [].

        Returns
        -------
        dict
            反馈操作结果.

        '''
        with shelve.open(filename=self.db_fn,flag=self.flag) as db:
            keys=list(db.keys())
            data={}
            if keys_selection:
                for k in keys_selection:
                    data[k]=db[k]
            else:
                for k in keys:
                    data[k]=db[k]                
        return self.feedback('read',data=data)
    
    def trash(self,trash_keys):
        '''
        根据指定的键，删除存储文件对应键值数据

        Parameters
        ----------
        trash_keys : list
            待删除的存储文件键列表.

        Returns
        -------
        dict
             反馈操作结果.

        '''
        with shelve.open(filename=self.db_fn,flag=self.flag) as db:
            keys=set(db.keys())
            if set(trash_keys).issubset(keys):
                for t_k in trash_keys:
                    del db[t_k]
                return self.feedback('trash')
            else:
                return self.feedback('trash','failed, %s not exist.'%trash_keys)
    
    def update(self,clear_all=False,**kwargs):
        '''
        给定键，更新存储文件对应键的值

        Parameters
        ----------
        clear_all : bool, optional
            是否清空存储文件所有数据. The default is False.
        **kwargs : dict/kwargs
             以字典形式（加**dict）或关键字参数方式输入更新数据.

        Returns
        -------
        dict
            反馈操作结果.

        '''
        with shelve.open(filename=self.db_fn,flag=self.flag) as db:
            if clear_all:
                keys=list(db.keys())
                for k in keys:
                    del db[k]
            if len(kwargs)>0:
                for k,v in kwargs.items():
                    db[k]=v
            return self.feedback('update')
        
db_fn='./database/shelve_db'        
db=DB_Shelve(db_fn)        
print(db)

print("-"*50)
fb_w=db.write(**{'commodity_name':'muskmelon','sales':1100})
print(fb_w)
fb_r_all=db.read()
print(fb_r_all)
fb_r_selection=db.read(['sales'])
print(fb_r_selection)
db.write(**{'exporting_country_name':'peru'})

print("-"*50)
print(db.read())
fb_t=db.trash(['sales'])
print(fb_t)
print(db.read())

print("-"*50)
db.update(sales=1100)
db.update(commodity_code=101,idx=1101)
```

</td>
<td>


    [DB_Shelve: db_fn=./database/shelve_db, flag=c]
    --------------------------------------------------
    {'process': 'write', 'msg': 'OK.'}
    {'process': 'read', 'msg': 'OK.', 'data': {'commodity_name': 'muskmelon', 'exporting_country_name': 'peru', 'commodity_code': 101, 'idx': 1101, 'commodity_dic': {'commodity_code': 103, 'commodity_name': 'muskmelon'}, 'sales': 1100}}
    {'process': 'read', 'msg': 'OK.', 'data': {'sales': 1100}}
    --------------------------------------------------
    {'process': 'read', 'msg': 'OK.', 'data': {'commodity_name': 'muskmelon', 'exporting_country_name': 'peru', 'commodity_code': 101, 'idx': 1101, 'commodity_dic': {'commodity_code': 103, 'commodity_name': 'muskmelon'}, 'sales': 1100}}
    {'process': 'trash', 'msg': 'OK.'}
    {'process': 'read', 'msg': 'OK.', 'data': {'commodity_name': 'muskmelon', 'exporting_country_name': 'peru', 'commodity_code': 101, 'idx': 1101, 'commodity_dic': {'commodity_code': 103, 'commodity_name': 'muskmelon'}}}
    --------------------------------------------------
    




    {'process': 'update', 'msg': 'OK.'}


</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


`DB_Shelve`类是对shelve数据存储的基本操作，如果要拓展列表追加或字典更新等值的存储类型，可以以子类的方式增加功能。同时，也可以对原有父类的某些方法重新。这里仅增加了`update_lstExtend(()`和`update_dict`两种方法，未重写父类方法。注意，要列表追加或更新字典，需要配置`shelve.open(file_name, flag='', writeback=True\False)`中的`writeback`为`True`。


</td>
<td>


```python
class DB_Shelve_Extension(DB_Shelve):
    '''
    拓展DB_Shelve操作shelve数据存储的方式，包括值类型的，列表形式追加和字典更新
    '''
    def __init__(self,db_fn,flag='c',writeback=True):
        '''
        初始化读写shelve数据库（存储文件）的基本参数，较之DB_Shelve类，增加了writeback参数

        Parameters
        ----------
        db_fn : string
            存储文件路径名.
        flag : string, optional
            读写方式。包括r-只读；
            w-可读写;
            n-每次调用open()都重新创建一个空的文件，可读写. 
            The default is 'c'-如果数据文件不存在，就创建，允许读写.
        writeback : bool, optional
            当设置为True以后，shelve对象为所有访问过的条目保留缓存并在close()或sync()时将它们写回到DB. The default is True.

        Returns
        -------
        None.

        '''
        super(DB_Shelve_Extension,self).__init__(db_fn,flag)
        self.writeback=writeback
        
    def update_lstExtend(self,**kwargs):
        '''
        对值为列表形式的数据，追加新的数据

        Parameters
        ----------
        **kwargs : dict/kwargs
             以字典形式（加**dict）或关键字参数方式输入更新数据，值为列表.

        Returns
        -------
        dict
            反馈操作结果.

        '''
        with shelve.open(filename=self.db_fn,flag=self.flag,writeback=self.writeback) as db:
            if len(kwargs)>0:
                for k,v in kwargs.items():
                    #print('-'*30,k,v)
                    #print(db[k])
                    db[k].extend(v)      
                    #print(db[k])
        return self.feedback('update_lstExtend')
    
    def update_dict(self,**kwargs):
        '''
        对值为字典形式的数据，根据新的字典数据更新值

        Parameters
        ----------
        **kwargs : dict/kwargs
             以字典形式（加**dict）或关键字参数方式输入更新数据，值为字典.

        Returns
        -------
        dict
            反馈操作结果.

        '''
        with shelve.open(filename=self.db_fn,flag=self.flag,writeback=self.writeback) as db:
            if len(kwargs)>0:
                for k,v in kwargs.items():
                    #print(db[k])
                    db[k].update(v)
                    #print(db[k])
        return self.feedback('update_dict')              
    
db_fn='./database/shelve_db'  
db_ex=DB_Shelve_Extension(db_fn)     
print(db_ex)

print("-"*50)
print(db_ex.read())

print("-"*50)
db_ex.update(commodity_name=['muskmelon','strawberry','apple'])
print(db_ex.read())
db_ex.update_lstExtend(commodity_name=['lemon'])
print(db_ex.read(keys_selection=['commodity_name']))

print("-"*50)
db_ex.write(commodity_dic={'commodity_code':101,'commodity_name':'muskmelon'})
print(db_ex.read())

print("-"*50)
db_ex.update_dict(commodity_dic={'commodity_code':103})
print(db_ex.read())
```

</td>
<td>


    [DB_Shelve_Extension: db_fn=./database/shelve_db, flag=c, writeback=True]
    --------------------------------------------------
    {'process': 'read', 'msg': 'OK.', 'data': {'commodity_name': ['muskmelon', 'strawberry', 'apple', 'lemon'], 'exporting_country_name': 'peru', 'commodity_code': 101, 'idx': 1101, 'commodity_dic': {'commodity_code': 103, 'commodity_name': 'muskmelon'}, 'sales': 1100}}
    --------------------------------------------------
    {'process': 'read', 'msg': 'OK.', 'data': {'commodity_name': ['muskmelon', 'strawberry', 'apple'], 'exporting_country_name': 'peru', 'commodity_code': 101, 'idx': 1101, 'commodity_dic': {'commodity_code': 103, 'commodity_name': 'muskmelon'}, 'sales': 1100}}
    {'process': 'read', 'msg': 'OK.', 'data': {'commodity_name': ['muskmelon', 'strawberry', 'apple', 'lemon']}}
    --------------------------------------------------
    {'process': 'read', 'msg': 'OK.', 'data': {'commodity_name': ['muskmelon', 'strawberry', 'apple', 'lemon'], 'exporting_country_name': 'peru', 'commodity_code': 101, 'idx': 1101, 'commodity_dic': {'commodity_code': 101, 'commodity_name': 'muskmelon'}, 'sales': 1100}}
    --------------------------------------------------
    {'process': 'read', 'msg': 'OK.', 'data': {'commodity_name': ['muskmelon', 'strawberry', 'apple', 'lemon'], 'exporting_country_name': 'peru', 'commodity_code': 101, 'idx': 1101, 'commodity_dic': {'commodity_code': 103, 'commodity_name': 'muskmelon'}, 'sales': 1100}}
 

</td>
<td>
</td>
</tr>


<tr>
<td> 

__8.3__ private variables(私有变量/成员)和private methods(私有方法)

</td>
<td>


python实际上没有类似`public`，`private`等关键字修饰的成员变量和方法，通常在变量名或方法名前增加`__`两个下划线，将公有变量和方法转换为私有变量和方法，实际上私有变量和方法是可以在外部访问的，只需要将例如`__data`的私有变量，`__say()`或`__subPrintData()`的私有方法，替换为`_PA__data`，`_PA__say()`和`_PB__subPrintData()`（类名之前一个下划线，私有变量和方法前两个下划线），因此python中并没有真正的私有变量和方法。私有变量和方法仅在类内访问，其子类和父类无法直接访问。


</td>
<td>


```python
class PA:
    def __init__(self):
        self.__data=[] #私有变量，实际被替换为：self._PA__data=[]
        self.__name='Guido van Rossum' #私有变量，实际被替换为：self._PA__name='Guido van Rossum'
    def add(self,item):
        self.__data.append(item) #实际被替换为：self._PA__data.append(item)
    def printData(self):
        print(self.__data) #实际被替换为：self._PA__data
    def __say(self): #私有方法，实际被替换为：def _PA__say(self)
        print("%s created python."%self.__name) #实际被替换为：self._PA__name
        
class PB(PA):
    def printSuperclassAttri(self):
        print(self._PA__data) #self.__data无法访问，属于父类的私有变量，需按替换语句操作  
    def __subPrintData(self):
        print('This is a private method of the subclass.')
        
instance=PB()
for i in ['c','c#','C++','python']:instance.add(i)
instance.printData()
print(instance._PA__data) #访问私有变量，需按替换语句操作
print(instance._PA__say()) #调用私有方法，需按替换语句操作

print("-"*50)
instance.printSuperclassAttri()
instance._PB__subPrintData()
```

</td>
<td>


    ['c', 'c#', 'C++', 'python']
    ['c', 'c#', 'C++', 'python']
    Guido van Rossum created python.
    None
    --------------------------------------------------
    ['c', 'c#', 'C++', 'python']
    This is a private method of the subclass.
 

</td>
<td>
</td>
</tr>


<tr>
<td> 

__8.4__ Operator Overloading（运算符/操作符重载） 与类属性格式"字典"

</td>
<td>

* Operator Overloading（操作符重载） 

操作符重载理解为，如果在类的内部重写了python内置操作（build-in operations），类的实例化时会自动调用重写的方法，由重写的方法计算的结果作为返回值。对于操作符重载： 即重写的方法拦截了python正常的内置操作方法；类可以重载所有的python表达式操作；类也可以重载python内置操作，例如打印（printing），函数调用（function calls）和属性访问（attribute access）等； 重载使类的实例表现得更像内置类型；重载是通过在一个类中提供特别命名的方法来实现的。

例如下述案例在类里重写了python的运算符加号`+`，即`__add__`方法。为了区别正常的加法运算，这里在两个数相加后，再加10。下述类重写加法运算符直接调用了重写的方法运算方式，同时调用类进行计算时类似于常规的内置类型运算方式，直接使用`+`进行计算，而不必按照类调用方法的途径实现。

</td>
<td>


```python
class OO_Add:
    def __init__(self,data):
        self.data=data
    def __add__(self,y):
        return OO_Add(self.data+y+10)

x=OO_Add(3)
y=x+9
print(y.data)

print("-"*50)
class OO_Add_:
    def __init__(self,data):
        self.data=data
    def __add__(self,y):
        return self.data+y+10
print(OO_Add_(3)+9)
```
</td>
<td>

    22
    --------------------------------------------------
    22

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

常见可用于操作符重载方法的内置运算符：

|  Method | Implements  | Called for  |
|---|---|---|
|  `__init__` | Constructor 构造函数  |  Object creation: X=Class(args) |
| `__del__`  | Destructor 析构函数 | Object deletion of X  |
|  `__add__` | Operator `+`  加法| X+Y, X+=Y if no `__iadd__`  |
|  `__or__` |  Operator `\|`(bitwise OR) 运算符`\|`|  X\|Y, X\|=Y if no `__ior__` |
| `__repr__`, `__str__`  | Printing, conversions 打印／转换 | print(X), repr(X),str(X)  |
| `__call__`  | Function calls 函数调用 | X(*args, **kwargs)  |
|  `__getattr__` | Attribute fetch  属性引用| X.undefined  |
|  `__setattr__` | Attribute assignment  属性赋值| X.attribute=value  |
| `__delattr__`  |  Attribute deletion 属性删除| del X.attribute  |
| `__getattribute__`  | Attribute fetch  属性获取 |  X.any |
| `__getitem__`  |  Indexing, slicing, iteration 索引运算| X[Key], X[i:j], for loops and other iterations if no `__iter__`  |
| `__setitem__`  | Index and slice assignment  索引赋值 | X[Key]=value, X[i:j]=iterable  |
| `__delitem__`  | Index and slice deletion  索引和分片删除| del X[Key],del X[i:j]  |
| `__len__`  | Length  长度| len(X), truth tests if no `__bool__`  |
| `__bool__`  | Boolean tests  布尔测试| bool(X), truth tests(named `__nonzero__` in 2.X)  |
|  `__lt__`, `__gt__` , `__le__`,`__ge__`,`__eq__`,`__ne__`| Comparisons 特定的比较 | X<Y, X>Y, X<=Y, X>=Y, X==Y, X!=Y  |
|  `__radd__` | Right-side operators   右侧加法| Other+X  |
| `__iadd__`  | In-place augmented operators 实地（增强的）加法 | X+=Y (or else `__add__`)  |
| `__iter__`, `__next__`  | Iteration contexts  迭代| I=iter(X), next(I); for loops, in if no `__contains__`, all comprehensions, map(F,X), others(`__next__` is named next in 2.X)  |
|  `__contains__` | Membership test  成员关系测试| item in X(any iterable)  |
|  `__index__` | Integer value  整数值| hex(X), bin(X), oct(X), O[X],O[X:]   |
| `__enter__`, `__exit__`  | Context manager 环境管理器 | with obj as var:  |
| `__get__`,`__set__`,`__delete__`  | Descriptor attributes 描述符属性 | X.attr, X.attr=value, del X.attr  |
| `_new__`  | Creation  创建| Object creation, before `__init__`  |

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

* 类属性格式"字典"

</td>
<td>


如果以`dict.key`获取类属性的形式替代字典`dict[key]`的形式，在代码书写上相对简单。要达到这样的目的，可以建立一个空的类，通过赋予属性的方式实现，例如下述案例中的`class Sales:pass`；`Sales`类方法在赋值时需要逐一赋值，也不能够将已有的字典转换为属性字典的形式，因此定义类`class AttrDict_simple(dict)`，该种方法继承了父类`dict`内置字典类，并以父类的方式`super(AttrDict_simple,self).__init__(*args,**kwargs)`初始化参数值，并通过`self.__dict__=self`将实例化对象本身赋予给`__dict__`对象（可以获取类的所有属性和方法的键值对），因此可以通过属性方式访问字典成员；`AttrDict_simple`类方式虽然可以直接将字典转换为属性字典的格式，但是缺少编辑功能，例如增减成员，因此定义`class AttriDict(dict)`类，重写（overload）了部分父类`dict`的方法，实现增加键值对（属性值）的方法。

> 类似 `__xx__` 或 `__xx__()` 前后都有2个下划线的变量或方法，通常是python中内置的特殊变量属性或方法的标识，在书写代码时应尽量避免用该类方式自定义变量或方法。


</td>
<td>

```python
help(dict)
```

</td>
<td>


    Help on class dict in module builtins:
    
    class dict(object)
     |  dict() -> new empty dictionary
     |  dict(mapping) -> new dictionary initialized from a mapping object's
     |      (key, value) pairs
     |  dict(iterable) -> new dictionary initialized as if via:
     |      d = {}
     |      for k, v in iterable:
     |          d[k] = v
     |  dict(**kwargs) -> new dictionary initialized with the name=value pairs
     |      in the keyword argument list.  For example:  dict(one=1, two=2)
     |  
     |  Built-in subclasses:
     |      StgDict
     |  
     |  Methods defined here:
     |  
     |  __contains__(self, key, /)
     |      True if the dictionary has the specified key, else False.
     |  
     |  __delitem__(self, key, /)
     |      Delete self[key].
     |  
     |  __eq__(self, value, /)
     |      Return self==value.
     |  
     |  __ge__(self, value, /)
     |      Return self>=value.
     |  
     |  __getattribute__(self, name, /)
     |      Return getattr(self, name).
     |  
     |  __getitem__(...)
     |      x.__getitem__(y) <==> x[y]
     |  
     |  __gt__(self, value, /)
     |      Return self>value.
     |  
     |  __init__(self, /, *args, **kwargs)
     |      Initialize self.  See help(type(self)) for accurate signature.
     |  
     |  __iter__(self, /)
     |      Implement iter(self).
     |  
     |  __le__(self, value, /)
     |      Return self<=value.
     |  
     |  __len__(self, /)
     |      Return len(self).
     |  
     |  __lt__(self, value, /)
     |      Return self<value.
     |  
     |  __ne__(self, value, /)
     |      Return self!=value.
     |  
     |  __repr__(self, /)
     |      Return repr(self).
     |  
     |  __reversed__(self, /)
     |      Return a reverse iterator over the dict keys.
     |  
     |  __setitem__(self, key, value, /)
     |      Set self[key] to value.
     |  
     |  __sizeof__(...)
     |      D.__sizeof__() -> size of D in memory, in bytes
     |  
     |  clear(...)
     |      D.clear() -> None.  Remove all items from D.
     |  
     |  copy(...)
     |      D.copy() -> a shallow copy of D
     |  
     |  get(self, key, default=None, /)
     |      Return the value for key if key is in the dictionary, else default.
     |  
     |  items(...)
     |      D.items() -> a set-like object providing a view on D's items
     |  
     |  keys(...)
     |      D.keys() -> a set-like object providing a view on D's keys
     |  
     |  pop(...)
     |      D.pop(k[,d]) -> v, remove specified key and return the corresponding value.
     |      If key is not found, d is returned if given, otherwise KeyError is raised
     |  
     |  popitem(self, /)
     |      Remove and return a (key, value) pair as a 2-tuple.
     |      
     |      Pairs are returned in LIFO (last-in, first-out) order.
     |      Raises KeyError if the dict is empty.
     |  
     |  setdefault(self, key, default=None, /)
     |      Insert key with a value of default if key is not in the dictionary.
     |      
     |      Return the value for key if key is in the dictionary, else default.
     |  
     |  update(...)
     |      D.update([E, ]**F) -> None.  Update D from dict/iterable E and F.
     |      If E is present and has a .keys() method, then does:  for k in E: D[k] = E[k]
     |      If E is present and lacks a .keys() method, then does:  for k, v in E: D[k] = v
     |      In either case, this is followed by: for k in F:  D[k] = F[k]
     |  
     |  values(...)
     |      D.values() -> an object providing a view on D's values
     |  
     |  ----------------------------------------------------------------------
     |  Class methods defined here:
     |  
     |  fromkeys(iterable, value=None, /) from builtins.type
     |      Create a new dictionary with keys from iterable and values set to value.
     |  
     |  ----------------------------------------------------------------------
     |  Static methods defined here:
     |  
     |  __new__(*args, **kwargs) from builtins.type
     |      Create and return a new object.  See help(type) for accurate signature.
     |  
     |  ----------------------------------------------------------------------
     |  Data and other attributes defined here:
     |  
     |  __hash__ = None
 

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
from datetime import datetime
date=datetime.strptime('2020-3-30','%Y-%m-%d').date()

sales_dict={'idx':1101,'date':date,'exporting_country':'peru','sales':2500,'commodity_name':'lemon'}
print(sales_dict['commodity_name'])

print("-"*50)
class Sales:pass
sales_rec=Sales()
sales_rec.idx=1101
sales_rec.date=date
sales_rec.exporting_country='peru'
sales_rec.sales=2500
sales_rec.commodity_name='lemon'
print(sales_rec.commodity_name)

print("-"*50)
class AttrDict_simple(dict):
    def __init__(self,*args,**kwargs):
        super(AttrDict_simple,self).__init__(*args,**kwargs)
        self.__dict__=self
        
sales_attrDict_simple=AttrDict_simple(sales_dict)
print(sales_attrDict_simple.commodity_name)
```

</td>
<td>

    lemon
    --------------------------------------------------
    lemon
    --------------------------------------------------
    lemon

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
class AttriDict(dict):
    def __init__(self, *args, **kwargs):
        super(AttriDict, self).__init__(*args, **kwargs)
        for arg in args:
            if isinstance(arg, dict):
                for k, v in arg.items():
                    self[k]=v
        if kwargs:
            for k, v in kwargs.items():
                self[k]=v     
                
    def __getattr__(self, attr):
        return self.get(attr)    
    
    def __setattr__(self, key, value):
        self.__setitem__(key, value)
        
    def __setitem__(self, key, value):
        super(AttriDict, self).__setitem__(key, value)
        self.__dict__.update({key: value})
    
    def __delattr__(self, item):
        self.__delitem__(item)   

    def __delitem__(self, key):
        super(AttriDict, self).__delitem__(key)
        del self.__dict__[key]        

sales_attrDict=AttriDict(sales_dict)        
print(sales_attrDict)
print(sales_attrDict.commodity_name)
sales_attrDict.commodity_code=102
print(sales_attrDict)
sales_attrDict['exporting_country_ID']=25
print(sales_attrDict)
del sales_attrDict.idx  #或用：del sales_attrDict['idx']
print(sales_attrDict)
```

</td>
<td>


    {'idx': 1101, 'date': datetime.date(2020, 3, 30), 'exporting_country': 'peru', 'sales': 2500, 'commodity_name': 'lemon'}
    lemon
    {'idx': 1101, 'date': datetime.date(2020, 3, 30), 'exporting_country': 'peru', 'sales': 2500, 'commodity_name': 'lemon', 'commodity_code': 102}
    {'idx': 1101, 'date': datetime.date(2020, 3, 30), 'exporting_country': 'peru', 'sales': 2500, 'commodity_name': 'lemon', 'commodity_code': 102, 'exporting_country_ID': 25}
    {'date': datetime.date(2020, 3, 30), 'exporting_country': 'peru', 'sales': 2500, 'commodity_name': 'lemon', 'commodity_code': 102, 'exporting_country_ID': 25}


</td>
<td>
</td>
</tr>











</table>

<span style = "color:Teal;background-color:;font-size:20.0pt">是否完成PCS_8(&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)</span>