> Created on Mon Sep  5 12:45:39 2022 @author: Richie Bao-caDesign设计(cadesign.cn)

<style>
  code {
    white-space : pre-wrap !important;
    word-break: break-word;
  }
</style>

# Python Cheat Sheet-9. (OOP)_Classes_Decorators(装饰器)_Slots

<span style = "color:Teal;background-color:;font-size:20.0pt">PCS_9</span>

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

__9.1__ 装饰器-函数

</td>
<td>


__1. 函数调用另一个函数（函数作为参数）__

这里定义了3个函数，`say_hello(name)`和`be_awesome(name)`，传入的为常规参数（不是以函数作为参数），并进行了不同方式字符串格式化。而对于`greet_bob(greeter_func)`，从` greeter_func("Bob")`语句可以判断出函数参数`greeter_func`为一个函数。将`say_hello(name)`和`be_awesome(name)`函数作为参数传入到函数`greet_bob(greeter_func)`，可以对应将参数替换为参数函数思考代码运行机制，比较方便理解。

> 该部分参考[Primer on Python Decorators](https://realpython.com/primer-on-python-decorators/)


</td>
<td>


```python
def say_hello(name):
    return f"Hello {name}"  # f-string字符串格式化方法，Literal String Interpolation（文字字符串插值）

def be_awesome(name):
    return f"Yo {name}, together we are the awesomest!"

def greet_bob(greeter_func):
    return greeter_func("Bob")

print(greet_bob(say_hello))
print(greet_bob(be_awesome))
```

</td>
<td>


    Hello Bob
    Yo Bob, together we are the awesomest!


</td>
<td>
</td>
</tr>



<tr>
<td> 

</td>
<td>


__2.内置函数（inner functions）__

如果函数内部存在多个内置函数，函数定义的前后位置并不重要，主要由执行语句的顺序确定。同时，内部函数在调用父函数之前不会被定义，属于父函数`parent()`的局部作用域，仅在`parent()`内部作为局部变量存在。


</td>
<td>


```python
def parent():
    print("Printing from the parent() function")

    def first_child():
        print("Printing from the first_child() function")

    def second_child():
        print("Printing from the second_child() function")

    second_child()
    first_child()

parent()   
```

</td>
<td>


    Printing from the parent() function
    Printing from the second_child() function
    Printing from the first_child() function
 

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


__3.函数返回值为一个函数__

python允许使用函数作为返回值，下述`parent()`函数返回了一个内置函数。需要注意，返回函数时，为`return first_child`，是一个没有给`()`的函数名，意味返回`first_child`函数的引用。如果给了`()`，则是返回`first_child`函数的一个结果。当函数返回值为函数，则返回值（通常赋予于新的变量名）可以像普通函数一样调用（使用）。

</td>
<td>


```python
def parent(num):
    def first_child():
        return "Hi, I am Emma"

    def second_child():
        return "Call me Liam"

    if num == 1:
        return first_child
    else:
        return second_child

first=parent(1)
second=parent(2)

print(first)
print(first())
print(second())
```

</td>
<td>


    <function parent.<locals>.first_child at 0x000001B52F2933A0>
    Hi, I am Emma
    Call me Liam


</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


__4.简单的装饰器__

`say_whee = my_decorator(say_whee)`返回`my_decorator(func)`父函数内置函数`wrapper()`的引用，为`return wrapper`。该内置函数包含父类传入的一个函数参数`func`，并执行`func()`，即执行参数函数的计算结果。执行`say_whee()`时，即执行父类`my_decorator(func)`内的`wrapper()`内置函数，只是此时，该函数已经独立于父函数`my_decorator(func)`，并包含有执行`wrapper()`函数所需的所有参数，这里为参数函数`func`。因此，`say_whee = my_decorator(say_whee)`中的`say_whee`为一个闭包（Closure，或Lexical Closure），为一个结构体，存储了一个函数和与其关联的环境参数。

因为内置函数`wrapper()`，实际上对传入的参数函数`say_whee()`的功能进行了增加，即“装饰”，所以可以简单说，装饰器就是对一个函数进行包装，修改已有的功能。



</td>
<td>


```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

def say_whee():
    print("Whee!")

say_whee = my_decorator(say_whee)
say_whee()
```

</td>
<td>


    Something is happening before the function is called.
    Whee!
    Something is happening after the function is called.
  

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


`say_whee = not_during_the_night(say_whee)`装饰，则根据条件判断执行不同的操作，如果满足`7 <= datetime.now().hour < 22`，则执行外部函数`say_whee()`;否则，什么都不发生。

</td>
<td>


```python
from datetime import datetime

def not_during_the_night(func):
    def wrapper():
        if 7 <= datetime.now().hour < 22:
            func()
        else:
            pass  # Hush, the neighbors are asleep
    return wrapper

def say_whee():
    print("Whee!")

say_whee = not_during_the_night(say_whee)
say_whee()
```

</td>
<td>

    Whee!

</td>
<td>
</td>
</tr>



<tr>
<td> 

</td>
<td>


__5.语法糖（Syntactic Sugar）__

上面的装饰器方法笨拙，为了简化代码过程，python允许用`@symbol`， 方式使用装饰器，有时称为`pie`语法。下述案例与上述结果一致，但是通过`@my_decorator` `pie`方法，替代了`say_whee = not_during_the_night(say_whee)`代码，简化操作。

</td>
<td>


```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

@my_decorator
def say_whee():
    print("Whee!")

say_whee()
```

</td>
<td>


    Something is happening before the function is called.
    Whee!
    Something is happening after the function is called.


</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


__6.带参数的装饰器__

在`wrapper_do_twice(*args, **kwargs)`内置函数传入参数为`*args, **kwargs`， 接受任意数量的位置参数和关键字参数。并将其传入参数函数。


</td>
<td>


```python
def do_twice(func):
    def wrapper_do_twice(*args, **kwargs):
        func(*args, **kwargs)
        func(*args, **kwargs)
    return wrapper_do_twice


@do_twice
def greet(name):
    print(f"Hello {name}")
    
greet("World")    
```

</td>
<td>


    Hello World
    Hello World


</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


__7.装饰器的返回值__

如果装饰器要返回值，`do_twice(func)`内置函数` wrapper_do_twice(*args, **kwargs)`在调用参数函数`func`时，需要执行`return func(*args, **kwargs)` 返回参数函数的返回值。

</td>
<td>


```python
def do_twice(func):
    def wrapper_do_twice(*args, **kwargs):
        func(*args, **kwargs)
        return func(*args, **kwargs)
    return wrapper_do_twice

@do_twice
def return_greeting(name):
    print("Creating greeting")
    return f"Hi {name}"

hi_adam = return_greeting("Adam")
print("-"*50)
print(hi_adam)

print("-"*50)
print(return_greeting)
print(return_greeting.__name__)
print("-"*50)
print(help(return_greeting))
```

</td>
<td>

    Creating greeting
    Creating greeting
    --------------------------------------------------
    Hi Adam
    --------------------------------------------------
    <function do_twice.<locals>.wrapper_do_twice at 0x000001B52F293EE0>
    wrapper_do_twice
    --------------------------------------------------
    Help on function wrapper_do_twice in module __main__:
    
    wrapper_do_twice(*args, **kwargs)
    
    None
 

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


__8.保留原始函数的信息-自省（introspection）调整__

自省是指一个对象在运行时了解自己的属性的能力。例如，一个函数知道它自己的名字和文档。在上述示例中，通过`return_greeting.__name__`，`help(return_greeting)`等方式可以查看函数对象相关属性，但是，发现给出的是`wrapper_do_twice`的内置函数，而不是`return_greeting`函数，因此可以通过[functools](https://docs.python.org/3/library/functools.html)的` @functools.wraps(func)`方法解决这个问题，保留原始函数的信息。

</td>
<td>


```python
import functools

def do_twice(func):
    @functools.wraps(func)
    def wrapper_do_twice(*args, **kwargs):
        func(*args, **kwargs)
        return func(*args, **kwargs)
    return wrapper_do_twice

@do_twice
def return_greeting(name):
    print("Creating greeting")
    return f"Hi {name}"

hi_adam = return_greeting("Adam")
print("-"*50)
print(hi_adam)

print("-"*50)
print(return_greeting)
print(return_greeting.__name__)
print("-"*50)
print(help(return_greeting))
```

</td>
<td>


    Creating greeting
    Creating greeting
    --------------------------------------------------
    Hi Adam
    --------------------------------------------------
    <function return_greeting at 0x000001B530D988B0>
    return_greeting
    --------------------------------------------------
    Help on function return_greeting in module __main__:
    
    return_greeting(name)
    
    None
 

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


__9. 带参数的装饰器__

装饰器中可以带参数，例如`@repeat(num_times=3)`中`num_times=3`。此时，对装饰器函数做了调整，增加了一层嵌套内置函数，传递装饰器参数。


</td>
<td>


```python
def repeat(num_times):
    def decorator_repeat(func):
        @functools.wraps(func)
        def wrapper_repeat(*args, **kwargs):
            for _ in range(num_times):
                value = func(*args, **kwargs)
            return value
        return wrapper_repeat
    return decorator_repeat

@repeat(num_times=3)
def greet(name):
    print(f"Hello {name}")
    
greet("Galaxy")
```

</td>
<td>


    Hello Galaxy
    Hello Galaxy
    Hello Galaxy


</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


__10.多个装饰器装饰一个函数__

可以将多个装饰器堆叠在一起，应用在一个函数上，此时，执行的装饰器执行的顺序是从内到外，例如示例先执行`@decor`，返回值为20，而后再执行`@decor1`，返回值为400。

</td>
<td>


```python
# code for testing decorator chaining
def decor1(func):
    def inner():
        x = func()
        return x * x
    return inner
 
def decor(func):
    def inner():
        x = func()
        return 2 * x
    return inner
 
@decor1
@decor
def num():
    return 10
 
print(num())
```

</td>
<td>

    400

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


__11.[decorator模块](https://pypi.org/project/decorator/)简化装饰器__

使用[decorator模块](https://pypi.org/project/decorator/)库的`@decorator`装饰器装饰‘装饰函数’，可以简化装饰器定义。例如下述代码取消了内置函数，将原始函数和输入参数都在`do_print(func,*args, **kwargs)`，装饰函数中一起输入。



</td>
<td>

```python
from decorator import decorator

@decorator
def do_print(func,*args, **kwargs):
    print('Hi {}!'.format(*args,**kwargs))
    return func(*args, **kwargs)

@do_print
def greet(name):
    print(f"Hello {name}!")
    
greet("World")    
```

</td>
<td>

    Hi World!
    Hello World!
    


</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


__12.示例__

* 执行时间长度

这个装饰器存储函数开始运行前的时间`start_time = time.perf_counter()`, 和函数结束后的时间`end_time = time.perf_counter()`， 然后计算运行函数的时间，`run_time = end_time - start_time`, 并打印。


</td>
<td>


```python
import functools
import time

def timer(func):
    """Print the runtime of the decorated function"""
    @functools.wraps(func)
    def wrapper_timer(*args, **kwargs):
        start_time = time.perf_counter()    # 1
        value = func(*args, **kwargs)
        end_time = time.perf_counter()      # 2
        run_time = end_time - start_time    # 3
        print(f"Finished {func.__name__!r} in {run_time:.4f} secs")
        return value
    return wrapper_timer

@timer
def waste_some_time(num_times):
    for _ in range(num_times):
        sum([i**2 for i in range(10000)])

waste_some_time(999)
```

</td>
<td>

    Finished 'waste_some_time' in 4.1239 secs
 

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

* 减缓运行

对执行的函数进行运行速度的限制。

</td>
<td>


```python
import functools
import time

def slow_down(func):
    """Sleep 1 second before calling the function"""
    @functools.wraps(func)
    def wrapper_slow_down(*args, **kwargs):
        time.sleep(1)
        return func(*args, **kwargs)
    return wrapper_slow_down

@slow_down
def countdown(from_number):
    if from_number < 1:
        print("Liftoff!")
    else:
        print(from_number)
        countdown(from_number - 1)
        
countdown(3)
```

</td>
<td>


    3
    2
    1
    Liftoff!
 

</td>
<td>
</td>
</tr>


<tr>
<td> 

__9.2__ 装饰器-类

</td>
<td>

__1.`@property`__

`@property` 内置装饰器可以将类的方法转换为只能读取的属性，例如使用`andy.password`类属性操作模式，而不是`andy.password()`类方法操作模式。如果要修改或者删除属性，则需要重新实现属性的`setter`，`getter`和`deleter`方法，例如`@password.setter`和` @password.deleter`装饰器。


</td>
<td>


```python
class Bank_acount:
    def __init__(self):
        self._password = 'preset password: 0000'

    @property
    def password(self):
        return self._password

    @password.setter
    def password(self, value):
        self._password = value

    @password.deleter
    def password(self):
        del self._password
        print('del complete')
        
andy = Bank_acount()
print(andy.password) #getter
andy.password='1q2w3e' #setter
print(andy.password)
del andy.password #deleter
```

</td>
<td>

    preset password: 0000
    1q2w3e
    del complete
    

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


__2.`@classmethod`和`@staticmethod`__

类方法`@classmetho`和静态方法`@staticmethod` ，都可以直接通过`Class/Instance.method()`调用，可以不用实例化对象，直接由类直接调用，例如类方法的`Person.fromBirthYear('mayank', 1996)`和静态方法的`Person.isAdult(22)`。对于类方法，需要将`self`参数转换为`cls`；对于静态方法，则不需要`self`等任何参数。

> 示例迁移于[classmethod() in Python](https://www.geeksforgeeks.org/classmethod-in-python/)


</td>
<td>


```python
# Python program to demonstrate
# use of a class method and static method.
from datetime import date
  
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
  
    # a class method to create a Person object by birth year.
    @classmethod
    def fromBirthYear(cls, name, year):
        return cls(name, date.today().year - year)
  
    # a static method to check if a Person is adult or not.
    @staticmethod
    def isAdult(age):
        return age > 18
  
person1 = Person('mayank', 21)
person2 = Person.fromBirthYear('mayank', 1996)
  
print(person1.age)
print(person2.age)
  
# print the result
print(Person.isAdult(22))
print(person1.isAdult(18))
```

</td>
<td>

    21
    26
    True
    False

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


__3.@abstractmethod__

标准库[abc](https://docs.python.org/3/library/abc.html)提供有`@abstractmethod`抽象方法，当所在的类继承了`abc.ABC`， 并给需要抽象的实例方法添加装饰器`@abstractmethod`后，这个类就成为了抽象类，不能够被直接实例化，例如示例的`Animal`类，抽象方法为`info()`。如果要使用抽象类，必须继承该类并实现该类的所有抽象方法，例如`Bird`子类继承了抽象类`Animal`，并在子类`info()`中实现父类抽象类的`info()`方法。

</td>
<td>


```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def info(self):
        print("Animal")
        
class Bird(Animal):
    # 实现抽象方法
    def info(self):
        # 调用基类方法(即抽象方法)
        super().info()
        print("Bird")        
```


```python
animal = Animal()
```

</td>
<td>

    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Input In [65], in <cell line: 1>()
    ----> 1 animal = Animal()
    

    TypeError: Can't instantiate abstract class Animal with abstract methods info



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
bird = Bird()
bird.info()
```

</td>
<td>

    Animal
    Bird

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

__4.装饰整个类__

装饰器接收的是一个类，而不是一个函数。


</td>
<td>


```python
import functools
import time

def timer(func):
    """Print the runtime of the decorated function"""
    @functools.wraps(func)
    def wrapper_timer(*args, **kwargs):
        start_time = time.perf_counter()    # 1
        value = func(*args, **kwargs)
        end_time = time.perf_counter()      # 2
        run_time = end_time - start_time    # 3
        print(f"Finished {func.__name__!r} in {run_time:.4f} secs")
        return value
    return wrapper_timer


@timer
class TimeWaster:
    def __init__(self, max_num):
        self.max_num = max_num

    def waste_time(self, num_times):
        for _ in range(num_times):
            sum([i**2 for i in range(self.max_num)])
            
tw = TimeWaster(1000)
tw.waste_time(999)
```

</td>
<td>

    Finished 'TimeWaster' in 0.0000 secs


</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


__5.示例__

* 记录状态的装饰器

使用类作为装饰器，实现`__init__()`和`__call__`方法，完成函数运行状态的记录。


</td>
<td>


```python
import functools

class CountCalls:
    def __init__(self, func):
        functools.update_wrapper(self, func)
        self.func = func
        self.num_calls = 0

    def __call__(self, *args, **kwargs):
        self.num_calls += 1
        print(f"Call {self.num_calls} of {self.func.__name__!r}")
        return self.func(*args, **kwargs)

@CountCalls
def say_whee():
    print("Whee!")
    
say_whee()
say_whee()
say_whee()
```

</td>
<td>

    Call 1 of 'say_whee'
    Whee!
    Call 2 of 'say_whee'
    Whee!
    Call 3 of 'say_whee'
    Whee!

</td>
<td>
</td>
</tr>


<tr>
<td> 

__9.3__ `__slots__`

</td>
<td>


通过`__slots__`类属性分配一连串的字符串属性名称进行属性声明，从而限制类实例对象将拥有的合法属性集，达到优化内存，提高程序运行速度的作用。当为`__slots__ `分配一串字符串名称，则只有`__slots__ `列表中的那些名称可以被分配为实例属性，并在实例化时，阻止了为实例分配`__dict__`对象，除非在`__slots__ `中包含该对象。

下述案例类`IceTeaSales`中配置` __slots__`对象的属性名称包括`['temperature','iceTeaSales']`，因此当配置非该列表中所列的属性名，例如`iceTea.price`时，就会引发异常。

</td>
<td>


```python
class IceTeaSales:
    __slots__=['temperature','iceTeaSales']
    def __init__(self):
        self.temperature=0
        self.iceTeaSales=0
    
iceTea=IceTeaSales()
print(iceTea.temperature)
iceTea.temperature=29
setattr(iceTea,'iceTeaSales',77)
print(iceTea.iceTeaSales,iceTea.temperature)
print(getattr(iceTea,'temperature'))
iceTea.price
```

</td>
<td>


    0
    77 29
    29
    


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Input In [96], in <cell line: 13>()
         11 print(iceTea.iceTeaSales,iceTea.temperature)
         12 print(getattr(iceTea,'temperature'))
    ---> 13 iceTea.price
    

    AttributeError: 'IceTeaSales' object has no attribute 'price'


</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>


`__slots__`阻止了`__dict__`对象分配给实例，因此`iceTea.__dict__`会引发异常，提示实例化对象没有属性`__dict_`。

</td>
<td>

```python
iceTea.__dict__
```

</td>
<td>


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Input In [90], in <cell line: 1>()
    ----> 1 iceTea.__dict__
    

    AttributeError: 'IceTeaSales' object has no attribute '__dict__'

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

`dir()`收集整个类树中所有继承的名称。


</td>
<td>


```python
print(dir(iceTea))
print('temperature' in dir(iceTea))
```

</td>
<td>


    ['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__slots__', '__str__', '__subclasshook__', 'iceTeaSales', 'temperature']
    True

</td>
<td>
</td>
</tr>



<tr>
<td> 

</td>
<td>


`__init__`构造方法初始化参数，如果参数名不在`__slots__`列表中，也会引发异常。

</td>
<td>


```python
class IceTeaSales:
    __slots__=['temperature','iceTeaSales']
    def __init__(self):
        self.temperature=0
        self.iceTeaSales=0
        self.price=0
iceTea=IceTeaSales()
```

</td>
<td>


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Input In [97], in <cell line: 7>()
          5         self.iceTeaSales=0
          6         self.price=0
    ----> 7 iceTea=IceTeaSales()
    

    Input In [97], in IceTeaSales.__init__(self)
          4 self.temperature=0
          5 self.iceTeaSales=0
    ----> 6 self.price=0
    

    AttributeError: 'IceTeaSales' object has no attribute 'price'


</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


如果在`__slots__`列表中包含`__dict__`，则可以增加新的属性名，`__dict__`则会包含非`__slots__`列表中新增加的属性名键值对。

</td>
<td>


```python
class IceTeaSales:
    __slots__=['temperature','iceTeaSales', '__dict__']
    def __init__(self):
        self.temperature=0
        self.iceTeaSales=0
        self.price=0
iceTea=IceTeaSales()
print(iceTea.price)
iceTea.name='flower tea'
print(iceTea.name)
print(iceTea.__slots__)
print(iceTea.__dict__)
```

</td>
<td>


    0
    flower tea
    ['temperature', 'iceTeaSales', '__dict__']
    {'price': 0, 'name': 'flower tea'}
    

</td>
<td>
</td>
</tr>



<tr>
<td> 

</td>
<td>


* `Slot`应用规则：

如果存在子类，在用`__slots__`时则需要注意：1. 子类中有`__slots__`，但父类中未配置`__slots__`，则实例对象总可以访问`__dict__`属性，因此没有意义。父类中有`__slots__`，而子类没有，同上，也没有意义；2. 子类定义了与父类相同的`__slots__`， 只能从父类中的`__slots__`获取定义的属性名。


</td>
<td>


```python
class C:pass
class D(C):__slots__=['a']

X=D()
X.a=1;X.b=2
print(X.__dict__)
print(D.__dict__.keys())
```

</td>
<td>


    {'b': 2}
    dict_keys(['__module__', '__slots__', 'a', '__doc__'])
   

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>


* 内存使用量测试

使用[memory-profiler ](https://pypi.org/project/memory-profiler/)库，测量代码内存的使用率。该模块对python程序的内存消耗进行逐行分析，从而监控一个进程的内存消耗，该模块依赖[psutil](https://pypi.org/project/psutil/)库。

从计算结果来看，未使用`__slots__`，内存变化为16.7MiB;  使用`__slots__`，内存变化为5.8MiB，因此使用`__slots__`可以有效节约内存空间。

> JupyterLab中无法执行，需要在Spyder中运行（保存为模块）

__未使用`__slots__`:__


</td>
<td>


```python
from memory_profiler import profile

class A(object): 
    def __init__(self,x):
        self.x=x
 
@profile
def main():
    f=[A(523825) for i in range(100000)]
 
if __name__=='__main__':
    main()
```

</td>
<td>


Line #    Mem usage    Increment  Occurences   Line Contents
============================================================
     7    142.2 MiB    142.2 MiB           1   @profile
     8                                         def main():
     9    158.9 MiB     16.7 MiB      100003       f=[A(523825) for i in range(100000)]

</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

 
     
__使用`__slots__`:__     
     

</td>
<td>


```python
from memory_profiler import profile
      
class A(object):
    __slots__=('x')
    def __init__(self,x):
        self.x=x        
 
@profile
def main():
    f=[A(523825) for i in range(100000)]
 
if __name__=='__main__':
    main()
```

</td>
<td>

Line #    Mem usage    Increment  Occurences   Line Contents
============================================================
    12    142.1 MiB    142.1 MiB           1   @profile
    13                                         def main():
    14    147.9 MiB      5.8 MiB      100003       f=[A(523825) for i in range(100000)]
    


</td>
<td>
</td>
</tr>









</table>

<span style = "color:Teal;background-color:;font-size:20.0pt">是否完成PCS_9(&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)</span>