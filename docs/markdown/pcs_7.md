> Created on Sun Aug 21 14:57:02 2022 @author: Richie Bao-caDesign设计(cadesign.cn)

<style>
  code {
    white-space : pre-wrap !important;
    word-break: break-word;
  }
</style>

# Python Cheat Sheet-7. 模块与包（module and package），及pypi发布（distribution）

<span style = "color:Teal;background-color:;font-size:20.0pt">PCS_7</span>

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


</td>
<td>


一个单独的`.py`文件即为一个模块（module），包含定义的函数、类或变量；当模块的数量不断增加，最好将其分类于不同的文件夹中，这种有效组织模块的方式就是创建一个包（package），包通常为一个含有`__init__.py`文件的文件夹，可以包含子模块或子包(子目录)。通常把一个项目组织为一个包，然后可以使用[setuptools](https://pypi.org/project/setuptools/)工具创建和分发包，从而像标准库或扩展库一样安装包和调用包，使用包内的工具（即定义的函数或类等）。

下述为一个包的文件夹结构。整个项目位于`PCS_7_package_example`文件夹下，其中包放置于`src`文件夹下，其中`toolkit4beginner`文件夹是创建的包，包括两个子包`graph`和`utility`文件夹，其中`graph`子包组织图表绘制类的工具，例如自定义样式的打印箱型图和四象限图（4 quadrant diagram），包含在`graph.py`文件中；`utility`子包组织通用的工具，例如展平列表，计算时间长度等都位于`general_tools.py`文件中，而描述性统计和获取最近值的函数都位于`stats_tools.py`文件中。`test`文件夹包含用于测试包的模块，确保运行正确，如果出现错误，则需要进行调试。

在包中，都有一个`__init__.py`文件，确保包能够正确导入及用`setuptools`打包时，可以找到包的位置，该文件通常为空，也可以像常规的模块写入代码，代码会在python第1次导入一个目录时自动运行，因此主要作为执行（软件）包所需初始化步骤的钩子（hook），控制定义包级别的变量。因为python文件都是按目录（子包）组织模块，python会通过搜索该目录下的文件导入相关模块。而不是搜索所有目录，只搜索目录下包含有`__init__.py`文件的目录，此时这个目录当作一个包目录，进而搜索包内的模块。如果未含有`__init__.py`文件，调用该目录下的模块时会引发未找到等错误提示。

> 如果在python 3.3及以上的版本，如果目录中不含有`__init__.py`文件，也可以作为单目录命名空间包实现（single-directory namespace packages），正确导入模块。但是也意味着，没有自定义初始化运行时加载控制变量的代码。

```
PCS_7_package_example/
├── docs/
├── pyproject.toml
├── README.md
├── setup.py
├── src/
│   └── toolkit4beginner/
│       ├── __init__.py
│       ├── graph/
│       │   ├── __init__.py
│       │   └── graph.py
│       └── utility/
│           ├── __init__.py
│           ├── general_tools.py
│           └── stats_tools.py
└── tests/
    ├── __init__.py
    ├── displayablePath.py
    ├── pypi_published_test.py
    └── test.py
```

上述文件夹目录结构中，`setup.py`文件是[setuptools](https://pypi.org/project/setuptools/)创建和分发包的配置文件；`README.md`文件是在`setup.py`文件中读取，用于配置`long_description`参数，方便编辑；`pyproject.toml`文件是从[PEP 621](https://peps.python.org/pep-0621/)开始，Python 社区选择 `pyproject.toml` 作为指定项目元数据的标准方式。 `Setuptools` 采用了此标准，使用此文件中包含的信息作为构建过程的输入（与`setup.py`作用同，但格式不同，此包中可以移除该文件）。创建分发包有多种打包方式，例如在anaconda下所建立环境的终端（terminal）执行`python setup.py sdist`，为打包成源码包（即压缩包，`.zip`，`.tar`，`.gz`等，如果不指定参数`--formats=gztar`，按当前平台默认格式; 执行`python setup.py bdist_wheel`， 为打包成二进制包（需要先安装[wheel](https://pypi.org/project/wheel/)）。打包完成后，会在文件夹下生成相应的文件，目录结构变化为：

```
PCS_7_package_example/
├── build/
│   ├── bdist.win-amd64/
│   └── lib/
│       └── toolkit4beginner/
│           ├── __init__.py
│           ├── graph/
│           │   ├── __init__.py
│           │   └── graph.py
│           └── utility/
│               ├── __init__.py
│               ├── general_tools.py
│               └── stats_tools.py
├── dist/
│   ├── toolkit4beginner-0.3.3-py3-none-any.whl
│   └── toolkit4beginner-0.3.3.tar.gz
├── docs/
├── pyproject.toml
├── README.md
├── setup.py
├── src/
│   ├── toolkit4beginner/
│   │   ├── __init__.py
│   │   ├── graph/
│   │   │   ├── __init__.py
│   │   │   └── graph.py
│   │   └── utility/
│   │       ├── __init__.py
│   │       ├── general_tools.py
│   │       └── stats_tools.py
│   └── toolkit4beginner.egg-info/
│       ├── dependency_links.txt
│       ├── PKG-INFO
│       ├── requires.txt
│       ├── SOURCES.txt
│       └── top_level.txt
└── tests/
    ├── __init__.py
    ├── displayablePath.py
    ├── pypi_published_test.py
    └── test.py
```

打包的二进制文件或源码文件为新生成的`dist`文件夹下的`toolkit4beginner-0.3.3-py3-none-any.whl`和`toolkit4beginner-0.3.3.tar.gz`。安装分发包同一般库的安装，可以在终端定位到该文件夹后，直接执行`pip install XXX.whl`或源码包执行`pip install xxx.tar.gz`，完成安装。如果要在[pypi](https://pypi.org/)上发布，可以提供给更多人使用，则可以注册该网站，将生成的源码或者二进制文件通过`twine upload dist/...`方式推送至自己的注册地址（需要安装[twine](https://pypi.org/project/twine/)工具，推送时提示输入pypi的注册用户名和密码）。上传成成功后（pypi可能不支持代理上传），则可以在`pypi`网站注册地址的`your projects`下找到发布的包（因为推送的包不能有重名，如果名字已存在，需要修改后，重新打包推送），在[view](https://pypi.org/project/toolkit4beginner/0.3.3/)下查看安装代码为`pip install toolkit4beginner==0.3.3`（或存在之前版本，则可以用`pip install toolkit4beginner -U`方法更新）。 此时该包同任何第三方库一样，可以直接在终端根目录安装，并且不需要源文件，可以为任何人安装使用。

建立一个项目，如果希望最终发布为一个分发包提供给更多人使用，则在建立项目的时候最好就按照包的文件目录结构布局，根据项目要求初步构思好基本的分类，然后在书写代码时，将函数、类和变量按类置于不同的模块下，并将模块置于对应的分类目录下，如果开始粗略的分类不满足要求，则实时调整目录结构，只要保持一个包合理的目录结构就可。最终配置`setup.py`文件，打包推送发布。

本节的案例是将之前PCS1-6中部分函数分类置于不同的模块和目录下，并增加了一个新的函数`boxplot_custom(data_dict,**args)`位于`graph/graph.py`模块下。

每一模块都使用`if __name__=="__main__":`语句进行模块内的测试。如果构建包，在直接使用模块做数据分析时，通常也将函数、类和全局性的变量置于该语句之前，而在该语句之下（缩进），执行调用计算的代码，该语句的目的是避免其它模块调用该模块内的工具时，执行了测试或者用于直接计算部分的代码。
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

</td>
<td>


* toolkit/setup.py

该配置文件不在交互式解释器中运行，需要在终端中定位到该文件目录后，执行`python setup.py sdist`或`python setup.py bdist_wheel`，完成打包。通常确定包名`name`之后，每次模块发生更新时，修改`version`参数后，重新打包上传至pypi。pypi会将包置于同一个项目下，其下包含每次包更新的版本。参数配置时主要包括两大部分，一部分是项目(版权、管理、版本)信息，例如name, version,license, author,author_email,description,long_description,long_description_content_type和url等；另一部分是打包要求、依赖和包位置等胚子信息，定位包所在目录的packages参数，对python版本有要求的python_requires参数，安装依赖库的 install_requires参数，是否包含额外文件的include_package_data参数等。更多参数可以查看[setuptools documentation](https://setuptools.pypa.io/en/latest/)，其中包括[pyproject.toml](https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html)配置说明。


</td>
<td>


```python
# -*- coding: utf-8 -*-
"""
Created on Sun Aug 21 14:57:02 2022

@author: Richie Bao-caDesign设计(cadesign.cn)
"""
from setuptools import setup,find_packages,find_namespace_packages

with open("README.md", "r", encoding="utf-8") as fh:
    long_description = fh.read()

setup(name='toolkit4beginner', #应用名，即包名
    version='0.3.3', #版本号
    license="MIT", #版权声明，BSD,MIT
    author='Richie Bao-caDesign设计(cadesign.cn)', #作者名
    author_email='richiebao@outlook.com', #作者邮箱
    description='模块、包和分发文件目录组织结构说明', #描述
    long_description=long_description,
    long_description_content_type="text/markdown",
    url='https://richiebao.github.io/USDA_CH_final',  #项目主页 
    #----------------------------------------------------------------  
    package_dir={"": "src"},
    packages=find_packages(where='src'),#包括安装包内的python包；find_namespace_packages()，和find_packages() ['toolkit4beginner']
    python_requires='>=3.6', #pyton版本控制
    platforms='any',
    install_requires=['matplotlib','statistics','numpy'] #自动安装依赖包（库）
    # include_package_data=True, #如果配置有MANIFEST.in，包含数据文件或额外其它文，该参数配置为True，则一同打包
    )
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

* toolkit4beginner/graph/graph.py

自定义样式图表打印模块。

</td>
<td>


```python
# -*- coding: utf-8 -*-
"""
Created on Sun Aug 21 15:08:21 2022

@author: Richie Bao-caDesign设计(cadesign.cn)
"""
def boxplot_custom(data_dict,**args):
    '''
    根据matplotlib库的箱型图打印方法，自定义箱型图可调整的打印样式。 

    Parameters
    ----------
    data_dict : dict(list,numerical)
        字典结构形式的数据，键为横坐分类数据，值为数值列表.
    **args : keyword arguments
        可调整的箱型图样式参数包括['figsize',  'fontsize',  'frameOn',  'xlabel',  'ylabel',  'labelsize',  'tick_length',  'tick_width',  'tick_color',  'tick_direction',  'notch',  'sym',  'whisker_linestyle',  'whisker_linewidth',  'median_linewidth',  'median_capstyle'].

    Returns
    -------
    paras : dict
        样式更新后的参数值.

    '''
    import matplotlib.pyplot as plt
    
    #计算值提取
    data_keys=list(data_dict.keys())
    data_values=list(data_dict.values())     
    
    #配置与更新参数
    paras={'figsize':(10,10),
           'fontsize':15,
           'frameOn':['top','right','bottom','left'],
           'xlabel':None,
           'ylabel':None,
           'labelsize':15,
           'tick_length':7,
           'tick_width':3,
           'tick_color':'b',
           'tick_direction':'in',
           'notch':0,
           'sym':'b+',
           'whisker_linestyle':None,
           'whisker_linewidth':None,
           'median_linewidth':None,
           'median_capstyle':'butt'}
    
    # print(paras)
    paras.update(args)
    # print(paras)
    
    #根据参数调整打印图表样式
    plt.rcParams.update({'font.size': paras['fontsize']})
    frameOff=set(['top','right','bottom','left'])-set(paras['frameOn'])
   
 
    #图表打印
    fig, ax=plt.subplots(figsize=paras['figsize'])
    ax.boxplot(data_values,
               notch=paras['notch'],
               sym=paras['sym'],
               whiskerprops=dict(linestyle=paras['whisker_linestyle'],linewidth=paras['whisker_linewidth']),
               medianprops={"linewidth": paras['median_linewidth'],"solid_capstyle": paras['median_capstyle']})
    
    ax.set_xticklabels(data_keys) #配置X轴刻度标签
    for f in frameOff:
        ax.spines[f].set_visible(False) #配置边框是否显示
    
    #配置X和Y轴标签
    ax.set_xlabel(paras['xlabel'])
    ax.set_ylabel(paras['ylabel'])
    
    #配置X和Y轴标签字体大小
    ax.xaxis.label.set_size(paras['labelsize'])
    ax.yaxis.label.set_size(paras['labelsize'])
    
    #配置轴刻度样式
    ax.tick_params(length=paras['tick_length'],
                   width=paras['tick_width'],
                   color=paras['tick_color'],
                   direction=paras['tick_direction'])

    plt.show()    
    return paras

#%%
def four_quadrant_diagram(data_dict,method='mean',**args):
    '''
    绘制四象限图。

    Parameters
    ----------
    data_dict : dict(list)
        字典格式数据，包括两组键值.其中键名将用作轴标签
    method : string, optional
        按照均值（mean）或中位数(median)方式划分四象限. The default is 'mean'.
    **args : keyword arguments
        可调整的图表样式参数包括：['figsize', 'fontsize', 'frameOn', 'crosshair_color', 'crosshair_linstyle', 'crosshair_linewidth', 'labelsize', 'tick_length', 'tick_width', 'tick_color', 'tick_direction', 'dot_color', 'dot_size', 'annotation_position_finetune', 'annotation_fontsize', 'annotation_lable', 'annotation_color'].

    Returns
    -------
    None.

    '''
    import matplotlib as mpl 
    import matplotlib.pyplot as plt
    from statistics import mean,median
    
    #解决中文乱字符的问题
    mpl.rcParams['font.sans-serif']=['SimHei']
    mpl.rcParams['font.serif']=['SimHei']  
    
    #计算值提取
    (key_x,x),(key_y,y)=data_dict.items()    

    #配置与更新参数
    paras={'figsize':(10,10),
           'fontsize':15,
           'frameOn':['top','right','bottom','left'],
           'crosshair_color':'red',
           'crosshair_linstyle':'--',
           'crosshair_linewidth':3,
           'labelsize':15,
           'tick_length':7,
           'tick_width':3,
           'tick_color':'b',
           'tick_direction':'in',
           'dot_color':'k',
           'dot_size':50,
           'annotation_position_finetune':0.5,
           'annotation_fontsize':10,
           'annotation_lable':None,
           'annotation_color':'gray',
           }  
    paras.update(args)
    
    #根据参数调整打印图表样式
    plt.rcParams.update({'font.size': paras['fontsize']})
    frameOff=set(['top','right','bottom','left'])-set(paras['frameOn'])    
    
    #图表打印
    fig, ax=plt.subplots(figsize=paras['figsize'])    
    ax.scatter(x,y,c=paras['dot_color'],s=paras['dot_size'])
    
    crosshair_color=paras['crosshair_color']
    crosshair_linestyle=paras['crosshair_linstyle']
    crosshair_linewidth=paras['crosshair_linewidth']
    if method=='mean':
        ax.axhline(y=mean(y), color=crosshair_color, linestyle=crosshair_linestyle, linewidth=crosshair_linewidth)           
        ax.axvline(x=mean(x), color=crosshair_color, linestyle=crosshair_linestyle, linewidth=crosshair_linewidth)     
    elif method=='median':
        plt.axhline(y=median(y), color=crosshair_color, linestyle=crosshair_linestyle, linewidth=crosshair_linewidth)           
        plt.axvline(x=median(x), color=crosshair_color, linestyle=crosshair_linestyle, linewidth=crosshair_linewidth)     

    for f in frameOff:
        ax.spines[f].set_visible(False) #配置边框是否显示  
        
    #标注点    
    if paras['annotation_lable']:
        annotations=paras['annotation_lable']
    else:
        annotations=list(range(len(x)))
        
    annotation_position_finetune=paras['annotation_position_finetune']
    for label,x,y in zip(annotations,x,y):
        ax.annotate(label,(x+annotation_position_finetune,y+annotation_position_finetune),
                    fontsize=paras['annotation_fontsize'],
                    color=paras['annotation_color'])
        
    #配置X和Y轴标签
    ax.set_xlabel(key_x)
    ax.set_ylabel(key_y)  

    #配置X和Y轴标签字体大小
    ax.xaxis.label.set_size(paras['labelsize'])
    ax.yaxis.label.set_size(paras['labelsize'])
    
    #配置轴刻度样式
    ax.tick_params(length=paras['tick_length'],
                   width=paras['tick_width'],
                   color=paras['tick_color'],
                   direction=paras['tick_direction'])      
    
    plt.tight_layout()
    plt.show() 

# four_quadrant_diagram(test_EC_scores,
#                       # method='median',
#                       figsize=(15,15),
#                       fontsize=23,
#                       frameOn=['bottom','left'],
#                       labelsize='30',
#                       dot_size=200,
#                       annotation_fontsize=30,
#                       annotation_lable=test_names,
#                       )      
#%%    

if __name__=="__main__":
    #模块内测试
    #A_测试-boxplot_custom(data_dict,**args)
    #%%
    test_score_lst_dic={'English': [90, 81, 73, 97, 85, 60, 74, 64, 72, 67, 87, 78, 85, 96, 77, 100, 92, 86], 
                        'Chinese': [71, 90, 79, 70, 67, 66, 60, 83, 57, 85, 93, 89, 78, 74, 65, 78, 53, 80], 
                        'history': [73, 61, 74, 47, 49, 87, 69, 65, 36, 7, 53, 100, 57, 45, 56, 34, 37, 70], 
                        'biology': [59, 73, 47, 38, 63, 56, 75, 53, 80, 50, 41, 62, 44, 26, 91, 35, 53, 68]}
    _=boxplot_custom(test_score_lst_dic,
                   figsize=(15,10),
                   fontsize=23,
                   frameOn=['bottom','left'],
                   xlabel='subject',
                   ylabel='score',
                   labelsize='30',
                   tick_color='r',
                   notch=1,
                   sym='rs',
                   whisker_linestyle='--',
                   whisker_linewidth=5,
                   median_linewidth=5
                  )
    #%%
    #B_测试-four_quadrant_diagram(data_dict,method='mean',**args)
    test_EC_scores={'English': [90, 81, 73, 97, 85, 60, 74, 64, 72, 67, 87, 78, 85, 96, 77, 100, 92, 86], 
                   'Chinese': [71, 90, 79, 70, 67, 66, 60, 83, 57, 85, 93, 89, 78, 74, 65, 78, 53, 80], }
    test_names=['Mason', 'Reece', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P']
    four_quadrant_diagram(test_EC_scores,
                          # method='median',
                          figsize=(15,15),
                          fontsize=23,
                          frameOn=['bottom','left'],
                          labelsize='30',
                          dot_size=200,
                          annotation_fontsize=30,
                          annotation_lable=test_names,
                          )   
    #%%
```


</td>
<td>

<img src="./imgs/pcs_7_01.png" height='auto' width='auto' title="caDesign">

<img src="./imgs/pcs_7_02.png" height='auto' width='auto' title="caDesign">

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

* toolkit4beginner/utility/general_tools.py

一般常用工具模块。

</td>
<td>


```python
# -*- coding: utf-8 -*-
"""
Created on Sun Aug 21 18:35:24 2022

@author: Richie Bao-caDesign设计(cadesign.cn)
"""
#展平嵌套列表，返回列表
flatten_lst=lambda lst: [m for n_lst in lst for m in flatten_lst(n_lst)] if type(lst) is list else [lst]

def flatten_lst_generator(lst):
    '''
    展平嵌套列表，返回迭代对象。

    Parameters
    ----------
    lst : list
        嵌套列表.

    Yields
    ------
    iterable object
        嵌套列表展平后的迭代对象.

    '''
    try: #使用语句try/except捕捉异常
        for n_lst  in lst: 
             for m in flatten_lst_generator(n_lst):
                yield m
               
    except:  
        yield lst

def infinite_generator():
    '''
    无穷整数生成器。

    Yields
    ------
    i : int
        整数值.

    '''
    i=0
    while True:
        i+=1
        yield i
        
def recursive_factorial(n):
    '''
    阶乘计算。

    Parameters
    ----------
    n : int
        阶乘计算的末尾值.

    Returns
    -------
    int
        阶乘计算结果.

    '''
    if n==1:
        return 1
    else:
        return n*recursive_factorial(n-1)

def start_time():
    '''
    获取当前时间

    Returns
    -------
    start_time : datetime
        返回当前时间.

    '''
    import datetime
    
    start_time=datetime.datetime.now()
    print("start time:",start_time)
    return start_time

def duration(start_time):
    '''
    配合start_time()使用。计算时间长度。

    Parameters
    ----------
    start_time : datetime
        用于计算时间长度的开会时间.

    Returns
    -------
    None.

    '''
    import datetime
    
    end_time=datetime.datetime.now()
    print("end time:",end_time)
    duration=(end_time-start_time).seconds/60
    print("Total time spend:%.2f minutes"%duration)            

if __name__=="__main__":
    #模块内测试
    #A_测试-flatten_lst
    #%%
    nested_lst=[['A','B',['C','D'],'E'],[6,[7,8,[9]]]]  
    print(flatten_lst(nested_lst))       
    #%%
    #B_测试-flatten_lst_generator(lst)
    fl=flatten_lst_generator(nested_lst)
    print(list(fl))
    #%%
    #C_测试-infinite_generator()
    inf=infinite_generator()
    print(next(inf))    
    print(next(inf)) 
    #%%
    #D_测试-recursive_factorial(n)
    print(recursive_factorial(6))
    #%%
    #E_测试-start_time();duration(start_time)
    s_t=start_time()
    for i in range(10**8):value=i
    duration(s_t)    
    #%%

```

</td>
<td>

    ['A', 'B', 'C', 'D', 'E', 6, 7, 8, 9]
    ['A', 'B', 'C', 'D', 'E', 6, 7, 8, 9]
    1
    2
    720
    start time: 2022-08-21 23:16:51.631636
    end time: 2022-08-21 23:17:04.243942
    Total time spend:0.20 minutes
 

</td>
<td>
</td>
</tr>


<tr>
<td> 

</td>
<td>

* toolkit4beginner/utility/stats_tools.py

统计类工具模块。


</td>
<td>


```python
# -*- coding: utf-8 -*-
"""
Created on Sun Aug 21 18:56:45 2022

@author: Richie Bao-caDesign设计(cadesign.cn)
"""

def descriptive_statistics(data,measure=None,decimals=2):
    '''
    计算给定数值列表的描述性统计值，包括数量、均值、标准差、方差、中位数、众数、最小值和最大值。
    
    
    Parameters
    ----------
    data : list(numerical)
        待统计的数值列表.
    measure : str, optional
        包括：'count', 'mean', 'std', 'variance', 'median', 'mode', 'min', 'max'. The default is None.
    decimals : int, optional
        小数位数. The default is 2.

    Returns
    -------
    dict
        如果不给定参数measure，则以字典形式返回所有值；否则返回给定measure对应值的表述字符串.

    '''
    import statistics
    
    d_s={
        'count':len(data), #样本数
        'mean':round(statistics.mean(data),decimals), #均值
        'std':round(statistics.stdev(data),decimals), #标准差
        'variance': round(statistics.variance(data),decimals), #方差 
        'median':statistics.median(data), #中位数
        'mode':statistics.mode(data), #众数
        'min':min(data), #最小值
        'max':max(data), #最大值        
        }
    
    if measure:
        return '{}={}'.format(measure,d_s[measure])
    else:
        return d_s
    
def value2values_comparison(x,lst):
    '''
    判断给定的一个值是否在一个列表中，如果在，则返回给定值；如果不在，则返回列表中与其差值绝对值最小的值。

    Parameters
    ----------
    x : numerical
        数值.
    lst : list(numerical)
        用于寻找给定值的参考列表.

    Returns
    -------
    numerical
        列表中最接近给定值的值.

    '''
    import numpy as np
    
    lst_mean=np.mean(lst)
    abs_difference_func=lambda value:abs(x)
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
    
    
if __name__=='__main__':  
    #%%
    #A_测试-descriptive_statistics(data,measure=None,decimals=2)
    ranmen_price_lst=[700,850,600,650,980,750,500,890,880,
                     700,890,720,680,650,790,670,680,900,
                     880,720,850,700,780,850,750,780,590,
                     650,580,750,800,550,750,700,600,800,
                     800,880,790,790,780,600,690,680,650,
                     890,930,650,777,700]
    d_s_1=descriptive_statistics(ranmen_price_lst)
    print(d_s_1)
    print('--'*30)
    d_s_2=descriptive_statistics(ranmen_price_lst,'std')
    print(d_s_2)
    d_s_3=descriptive_statistics(ranmen_price_lst,measure='mean',decimals=1)
    print(d_s_3)
    #%%
    #B_测试-value2values_comparison(x,lst)
    ranmen_price_lst=[700,850,600,650,980,750,500,890,880,700,890,720,680,650,790,670,680,900,880,720,850,700,780,850,750,
     80,590,650,580,750,800,550,750,700,600,800,800,880,790,790,780,600,690,680,650,890,930,650,777,700]
    price_x=200.68    
    price_x_closestValue=value2values_comparison(price_x,ranmen_price_lst)    
    print(price_x_closestValue)
    #%%

```

</td>
<td>

    {'count': 50, 'mean': 743.34, 'std': 108.26, 'variance': 11720.64, 'median': 750.0, 'mode': 700, 'min': 500, 'max': 980}
    ------------------------------------------------------------
    std=108.26
    mean=743.3
    200.680 is not in the list.
    the nearest value to 200.68 is 700.
    __________________________________________________
    x is lower than the average 729.34 of the list.
    700
    


</td>
<td>
</td>
</tr>

<tr>
<td> 

</td>
<td>

* toolkit4beginner/test/test.py

如果打包推送至[pypi](https://pypi.org/)上发布，并通过`pip install toolkit4beginner==0.3.3`或`pip install toolkit4beginner -U`安装，则可以使用该包的工具。首先是通过一行调入语句`import toolkit4beginner`简单的测试，是否包已经正确安装，如果没有提示错误，则已正确安装并调入，否则需要确认之前大安装步骤是否正确，或者源码和打包的整个流程中是否有疏漏，从新安装或者调试后重新打包，以新版本（配置`version`参数）发布后再安装或更新包。


</td>
<td>

```python
import toolkit4beginner
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

下述测试代码是对具体模块的测试。在模块调入方式上，选择了不同的调入方式，包括`from toolkit4beginner.graph import graph`，从子包（子目录）中调入一个模块；`from toolkit4beginner.utility.general_tools import flatten_lst`，从子包的模块中调入一个方法（函数）；`from toolkit4beginner.utility.stats_tools import *`，一次性调入一个模块中的所有方法。也可以使用`import toolkit4beginner as t4b`，或者`from toolkit4beginner.utility.general_tools import flatten_lst as FL`等方式为调入的对象重新命名，这尤其适合于有同名的文件或方法，避免同名冲突。


</td>
<td>


```python
# -*- coding: utf-8 -*-
"""
Created on Sun Aug 21 15:23:51 2022

@author: Richie Bao-caDesign设计(cadesign.cn)
"""
#测试
#%%
from toolkit4beginner.graph import graph

test_score_lst_dic={'English': [90, 81, 73, 97, 85, 60, 74, 64, 72, 67, 87, 78, 85, 96, 77, 100, 92, 86], 
                    'Chinese': [71, 90, 79, 70, 67, 66, 60, 83, 57, 85, 93, 89, 78, 74, 65, 78, 53, 80], 
                    'history': [73, 61, 74, 47, 49, 87, 69, 65, 36, 7, 53, 100, 57, 45, 56, 34, 37, 70], 
                    'biology': [59, 73, 47, 38, 63, 56, 75, 53, 80, 50, 41, 62, 44, 26, 91, 35, 53, 68]}
_=graph.boxplot_custom(test_score_lst_dic,
               figsize=(15,10),
               fontsize=23,
               frameOn=['bottom','left'],
               xlabel='subject',
               ylabel='score',
               labelsize='30',
               tick_color='r',
               notch=1,
               sym='rs',
               whisker_linestyle='--',
               whisker_linewidth=5,
               median_linewidth=5
              )
#%%
test_EC_scores={'English': [90, 81, 73, 97, 85, 60, 74, 64, 72, 67, 87, 78, 85, 96, 77, 100, 92, 86], 
               'Chinese': [71, 90, 79, 70, 67, 66, 60, 83, 57, 85, 93, 89, 78, 74, 65, 78, 53, 80], }
test_names=['Mason', 'Reece', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P']
graph.four_quadrant_diagram(test_EC_scores,
                      # method='median',
                      figsize=(15,15),
                      fontsize=23,
                      frameOn=['bottom','left'],
                      labelsize='30',
                      dot_size=200,
                      annotation_fontsize=30,
                      annotation_lable=test_names,
                      ) 
#%%
from toolkit4beginner.utility.general_tools import flatten_lst

nested_lst=[['A','B',['C','D'],'E'],[6,[7,8,[9]]]]  
print(flatten_lst(nested_lst))  
#%%
from toolkit4beginner.utility.stats_tools import *

ranmen_price_lst=[700,850,600,650,980,750,500,890,880,700,890,720,680,650,790,670,680,900,880,720,850,700,780,850,750,
 80,590,650,580,750,800,550,750,700,600,800,800,880,790,790,780,600,690,680,650,890,930,650,777,700]
price_x=200.68    
price_x_closestValue=value2values_comparison(price_x,ranmen_price_lst)    
print(price_x_closestValue)
#------------------------------------------------------------------------------
ranmen_price_lst=[700,850,600,650,980,750,500,890,880,
                 700,890,720,680,650,790,670,680,900,
                 880,720,850,700,780,850,750,780,590,
                 650,580,750,800,550,750,700,600,800,
                 800,880,790,790,780,600,690,680,650,
                 890,930,650,777,700]
d_s_1=descriptive_statistics(ranmen_price_lst)
print(d_s_1)
print('--'*30)
d_s_2=descriptive_statistics(ranmen_price_lst,'std')
print(d_s_2)
d_s_3=descriptive_statistics(ranmen_price_lst,measure='mean',decimals=1)
print(d_s_3)
#%%
```

</td>
<td>

<img src="./imgs/pcs_7_03.png" height='auto' width='auto' title="caDesign">

<img src="./imgs/pcs_7_04.png" height='auto' width='auto' title="caDesign">

    ['A', 'B', 'C', 'D', 'E', 6, 7, 8, 9]
    200.680 is not in the list.
    the nearest value to 200.68 is 700.
    __________________________________________________
    x is lower than the average 729.34 of the list.
    700
    {'count': 50, 'mean': 743.34, 'std': 108.26, 'variance': 11720.64, 'median': 750.0, 'mode': 700, 'min': 500, 'max': 980}
    ------------------------------------------------------------
    std=108.26
    mean=743.3


</td>
<td>
</td>
</tr>




</table>

<span style = "color:Teal;background-color:;font-size:20.0pt">是否完成PCS_7(&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)</span>