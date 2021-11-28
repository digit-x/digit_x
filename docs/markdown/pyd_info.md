> Created on Sun Oct 10 20\36\01 2021 @author: Richie Bao-python_code_archi_la_design_method_study 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# PYTHON 编程景观|建筑 设计方法  python coding Archi|LA design method study

### 高校课程信息
**课程名称**：Python数字设计编程基础 （以后课程名称可能修改为：PYTHON数字化设计方法）

**学分**：1.5

**总学时/课内实践学时**：24

**课程性质**：选修课程

**课程类型**：通识核心课程

**适应对象**：建筑、风景、规划


### 组队
每6人一组，如不能归整，余数分别自由组到已有组别。每组自行推选一个组长，组名自选任何一种动物名。不同专业尽量混合。

推选课程代表2-3人，负责课程事宜。

### 记笔记
使用markdown（[在Visual Studio Code下编辑](https://code.visualstudio.com/)）/JupyterLab([使用Anaconda下的JupyterLab](https://www.anaconda.com/))记笔记。

### 考核

* 平时成绩

1. 小组实验，组内成员贡献度由小组根据个人贡献大小共同确定（需诚实守信）， 百分比计算，综合为100%。
2. 个人笔记记录情况（存储于个人GitHub）。
3. 签到，由课代表及组长负责（需诚实守信）。

* 期末成绩

### 代码和笔记托管

个人申请[GitHub](https://github.com/)账号，代码和笔记推送至此，用于教师在线查看评定。

小组实验的代码和笔记置于组长的GitHub账户下，单独建立代码仓库repository，各组统一命名为:PYD_Experiment，每次实验均推送至该仓库下，并位于单独的文件夹下（每次实验的单独文件夹名，由教师给定）。

### 关于英语
编写程序时，可以打开翻译软件，通过函数名，变量名以及注释，使得程序代码易读。同时提升英语能力（包括读音锻炼）。

### 小组名单
由课代表完成汇总，格式为（Excel编写）：

* 表1

| 序号  |  组名 | 组员名 |学号 | 角色  |  个人GitHub仓库链接地址 |签到-1 |签到-2 |... |签到-12 | 签到成绩 |小组实验个人贡献-1|...|小组实验个人贡献-n|小组实验个人贡献成绩|期末实验个人贡献度|总成绩|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1  | Kitty  | 张三  |   | 负责组长 |   |  1 |   |   |   |   |   |   |   |   |   |   |
| 2  |   | 李四  |   |   |   |  1 |   |   |   |   |   |   |   |   |   |   |
| 3  |   | 王五  |   | 负责班长  |   |  0 |   |   |   |   |   |   |   |   |   |   |
| 4  |   | .  |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
| 5  |   | .  |   |  |   |   |   |   |   |   |   |   |   |   |   |   |
| 6  |   | .  |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
| 7  | Squirrel  | 赵六 |   | 负责组长  |   | 1  |   |   |   |   |   |   |   |   |   |   |
| 8  |   | 孙七  |   |   |   |  2 |   |   |   |   |   |   |   |   |   |   |
| ...  |   | .  |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
| n  |   |  . |   |   |   |   |   |   |   |   |   |   |   |   |   |   |

* 表2

| 序号  | 组名   | 小组实验-1  | ...  |  小组实验-n |期末实验|
|---|---|---|---|---|---|
|  1 | Kitty  |   |   |   |   |
|  2 | Squirrel  |   |   |   |   |
| ...  |  . |   |   |   |   |
| n  |  . |   |   |   |   |


> 签到，1为到，0为未到，2为请假（需假条）。最后汇总，由课代表在python中读取Excel文件编写代码，计算最终签到成绩，小组实验成绩。

> 小组实验个人贡献计算方式，值为0-1，所有成员个人贡献值总和为1，精度为0.1。如果为个人实验，所有人的值为1。

### 小组实验

在Github中的个人账户中，定义Repository（仓库），Repository统一命名为`python_AR_LA_PL_course`，并将Repositories设置为"public"，以便教师浏览给出成绩。之后的实验均使用[GitHub Desktop](https://desktop.github.com/)，push（推送）至该repository中。

#### 实验-1
个人实验：在Ananconda下的JupyterLab中，完成[**设计师与PYTHON | PYTHON基础速学**](https://digit-x.github.io/digit_x/#/./markdown/py_designer_and_python_tutorial_basic)章节**PYTHON基础速学**部分代码，命名为`ex_01_python_quick_tutorial.ipynb`，并上传至GitHub中的个人Repository(`python_AR_LA_PL_course`)中。

> 第1次个人实验评分粗略标准：1. 全，即应包括所有的速学部分内容；2，变化，不要照抄源代码，可以自行发挥变化实验内容，以自修学习为目的；3，注释，通过搜索查阅，给出关键注释，以及自己需要注意的部分。(4.empty;5.似乎复制；6.文件未能打开)

> 实验-1的方式，提交的结果不易评判和控制。1.有直接复制的情况；2.有代做的情况；3.自修能力待提升；5.本科生课程之间的时间协调，不能按时完成实验。


#### 实验-2
小组实验：从各自专业领域选择解决一个问题的任一知识点，应用python语言定义一个泛化通用的函数，能够解决满足此类逻辑不同输入条件参数的问题。例如：

建筑专业：例如解决一注真题_建筑结构[2001-6]的此类问题：

<a href=""><img src="./imgs_pyd/058.jpg" height='auto' width='auto' title="caDesign"></a> 

规划专业：例如计算技术经济指标：容积率，人口毛密度，住宅建筑面积净密度，住宅建筑净密度，住宅平均层数，绿地率，容积率，建筑密度等。下述问题引自网络：

<a href=""><img src="./imgs_pyd/059.jpg" height='auto' width='auto' title="caDesign"></a> 

景观专业：例如求定额工程量。下述问题引自网络：

<a href=""><img src="./imgs_pyd/060.jpg" height='auto' width='auto' title="caDesign"></a> 

> 每组自行选题，各组之间选题不能相同，如果相同则均以最低分计算。

代码文件统一命名为`ex_02_function.ipynb`，并由组长上传至组长的GitHub账户，单独建立代码仓库repository，各组统一命名为:｀PYD_Experiment｀。

文件中，需说明函数的意图，输入参数，和返回值等信息。并应用该函数实际运算演示。

> 第2次个人实验评分粗略标准：1.是否定义了泛化通用的函数； 2.执行代码提示错误；3.未提交；4.变量名过于随意；5.注释不充分；6.无法打开;7.代码不流畅。

#### 实验-3
个人实验：完成[pandas](https://digit-x.github.io/digit_x/#/./markdown/pyd_SQLite?id=pandas-geopandas)与[numpy](https://digit-x.github.io/digit_x/#/./markdown/pyd_sympy_regression?id=numpy)库的自修。要求同实验-1，命名为`ex_03_pd_np.ipynb`。
 


#### 实验-4
小组实验：依据课程“数据库_SQlite及与grasshopper的数据交换”，每组从OSM下载任一城市区域的数据（各组不能相同），将数据处理后写入SQLite数据库，并在GH中编写GHPython组件调入显示数据。同时，根据所选择城市，查找任一类型数据，同样处理，写入数据库，并调入GH。代码由Spyder编写，并命名为`ex_04_OSM.py`，SQLite数据库文件命名为`ex_04_OSM.sqlite`。将成果组织以markdown形式写入README.md。

#### 实验-5
小组实验：依据课程[机器学习-回归](https://digit-x.github.io/digit_x/#/./markdown/pyd_sympy_regression?id=_2-%e6%9c%ba%e5%99%a8%e5%ad%a6%e4%b9%a0-%e5%9b%9e%e5%bd%92)部分，从各自专业领域选择一组具有“因果”关系的数据，通过[相关性分析](https://richiebao.github.io/Urban-Spatial-Data-Analysis_python/#/./notebook_code/correlation?id=_12-%e7%9b%b8%e5%85%b3%e6%80%a7)确定解释变量(自变量-input)和结果变量(因变量-output)之间的相关强度，并用机器学习库[scikit-learn](https://scikit-learn.org/stable/index.html)，将数据集切分为训练和测试数据集，训练模型及精度评估。代码由Spyder编写，并命名为`ex_05_regression.py`，同时将成果组织以markdown形式写入README.md。

例如：获取城市[公共健康数据](https://richiebao.github.io/Urban-Spatial-Data-Analysis_python/#/./notebook_code/regression_publicHeath_grad),分析疾病与拥挤的住房之间的相关性，并建立回归模型；或者，结构分析中由不同初始条件和常规公式计算结果对应到解释变量和结果变量，建立回归模型等。

#### 实验-6 无


### 期末（考试 $∨$ 考察）

#### 考试途径（闭卷）：

* __试题主要内容包括：__
1. [PYTHON基础速学](https://digit-x.github.io/digit_x/#/./markdown/py_designer_and_python_tutorial_basic)
2. [pandas + GeoPandas](https://digit-x.github.io/digit_x/#/./markdown/pyd_SQLite?id=pandas-geopandas)
3. [Numpy](https://digit-x.github.io/digit_x/#/./markdown/pyd_sympy_regression?id=numpy)
4. [数据库](https://digit-x.github.io/digit_x/#/./markdown/pyd_SQLite)、机器学习-[回归](https://digit-x.github.io/digit_x/#/./markdown/pyd_sympy_regression)-[聚类](https://digit-x.github.io/digit_x/#/./markdown/pyd_clustering)、[深度学习](https://digit-x.github.io/digit_x/#/./markdown/pyd_deeplearning_basis)的基础内容。
5. 不确定的拓展内容。

* __试题形式：__
1. 选择
2. 程序阅读题
3. 程序填空题
4. 编程题

#### 考察途径（综合实验<小组>+个人实验）：

> 2021年秋季，考察内容

**题目：** 城市地块调研与空间数据分析

##### 1. 调研区域

* 西安城区内，自行选择区域，但调查街区不小于$2.0 km^{2} $。

##### 2. 数据

包括数据的获取，数据处理，以及数据可视化

2.1  手机调研拍摄图像（含GPS信息，和时间戳）。

* python读取图像，提取GPS经纬度坐标，使用Plotly库提供的go方法调用地图，绘制调研路径，并将GPS位置信息与时间戳写入SQLite数据库。参考课程[调研路径与图像](https://digit-x.github.io/digit_x/#/./markdown/pyd_clustering?id=_3-%e8%b0%83%e7%a0%94%e8%b7%af%e5%be%84%e4%b8%8e%e5%9b%be%e5%83%8f)


* 将GPS经纬度和提取的图像时间戳信息，处理为DataFrame格式数据，并转换为GeoDataFrame(使用geopandas库)，通过epsg(crs)配置投影（西安的epsg=32749，为WGS 84 / UTM zone 49S）。参考课程[OSM（OpenStreetMap）数据处理](https://digit-x.github.io/digit_x/#/./markdown/pyd_SQLite)

2.2 下载[POI（西安）](https://github.com/richieBao/Urban-Spatial-Data-Analysis_python/tree/master/notebook/BaiduMapPOIcollection_ipynb/data/xianPOI_36)数据，读取所有数据，转换为GeoDataFrame数据格式（需配置投影），并根据各组调研区域提取所在区域的POI数据。参考文件[批量转换.csv格式数据为GeoDataFrame](https://richiebao.github.io/Urban-Spatial-Data-Analysis_python/#/./notebook_code/BaiduMapPOI_collection_multipleClassification?id=_12-%e6%89%b9%e9%87%8f%e8%bd%ac%e6%8d%a2csv%e6%a0%bc%e5%bc%8f%e6%95%b0%e6%8d%ae%e4%b8%bageodataframe)

2.3 下载[.shp格式的建筑高度数据（西安）](https://github.com/richieBao/python_code_archi_la_design_method_study/tree/main/notebook/data/xianBuildingHeight)，使用geopandas读取为GeoDataFrame数据格式，并根据各组调研区域提取所在区域的建筑高度数据。

2.4 任何可以自行获取的各组调研区域数据。

> 通过GeoDataFrame.plot()方法或其它方法查看提取的数据。

##### 3. 分析

* 调研路径数据分析

3.1 提取调研路径邻近的POI数据，统计分析调研路径业态分布情况，例如聚类POI点，统计簇数量，以及不同业态的分布比例，结构或频数等，并图表表述。（用到[shaply库](通过GeoDataFrame.plot()方法或其它方法查看提取的数据。)）。

3.2 提取调研路径邻近的建筑高度数据，分析延调研路径的建筑高度变化，以及建筑密度变化情况。

* 选择区域数据分析

3.3 区域内的POI数据分析，和建筑分布和高度分析，自行拟定分析内容。

> 可自行分析不同或更多内容。

* __自行获取区域内的任何数据的任何类型分析。（为个人实验）__

##### 4. 讨论

根据上述分析内容，文字总结调研区域和调研路径下，城市空间的特点。

##### 5. 表达

5.1 所有数据，分析过程数据，以及结果，均push到GitHub仓库（repository）中。仓库的文件结构要合理，例如包括data，graph，process_data，database等。

5.2 使用[docsify](https://docsify.js.org/#/)，将综合实验内容，用markdown编辑，发布为在线文档形式，例如[数字营造学社官网](https://digit-x.github.io/digit_x/#/)。综合实验内容的报告文件，包括调研概述，区域，数据，分析和讨论几个环节。需图文并茂，阐述结构合理。<同时，将平时小组实验和个人实验，写入在线文档中>

5.3 将在线报告，打印为PDF，push到GitHub的repository中。

##### 综合实验评分标准

1. 完成度：全部完成包括上述5部分，以及各部分内容；

2. 分析深度：给出的分析内容，可以根据具体情况，深入分析，甚至可以提出新的角度；

3. 数据图表表达：结合图表表述分析内容，图表清晰合理；

4. 报告文件：需使用docsify完成在线文档形式，markdown图文并茂编辑，阐释清晰。（基于GitHub的repository使用docsify布局在线文档，为自学）

> 评分标准应能够反应，学习者代码的自修能力，自主解决问题的能力，团队合作的能力，以及创新能力。

---

* 期末成绩评定占比：

| 课程总评成绩  | 成绩组成  |成绩占比（%）   |   
|---|---|---|
|   | 期末考核成绩  | 50%  |   
|   |  出勤 | 10%  |   
|   | 课堂讨论（分配给平时个人实验） |  15% |   
|   | 随堂测验（分配给平时小组实验）  |  25% |   
| 总计  |   |  100% |   


---

### 附：分组代码

> 因为多个班级，班级间同学可能并不十分熟识，随机分组会增加磨合时间和配合度，因此改为自行结组。

```python
import pandas as pd
requirements_fp=r'./data/python_lesson_roll_book_2021.xlsx'
pylesson_roll=pd.read_excel(requirements_fp,sheet_name='roll',header=[0],engine='openpyxl')
pylesson_roll=pylesson_roll[pylesson_roll.filter(regex='^(?!Unnamed)').columns]
pylesson_roll
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>N</th>
      <th>ID</th>
      <th>name</th>
      <th>major</th>
      <th>class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2001050101</td>
      <td>程佳怡</td>
      <td>建筑</td>
      <td>建筑2001</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2001050102</td>
      <td>王雨晨</td>
      <td>建筑</td>
      <td>建筑2001</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2001050104</td>
      <td>武长乐</td>
      <td>建筑</td>
      <td>建筑2001</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2001050105</td>
      <td>杨昱萱</td>
      <td>建筑</td>
      <td>建筑2001</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2001050106</td>
      <td>李玉洁</td>
      <td>建筑</td>
      <td>建筑2001</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>91</th>
      <td>92</td>
      <td>1904020502</td>
      <td>马萍坪</td>
      <td>建筑</td>
      <td>风景2001</td>
    </tr>
    <tr>
      <th>92</th>
      <td>93</td>
      <td>2001030101</td>
      <td>马好好</td>
      <td>建筑</td>
      <td>风景2001</td>
    </tr>
    <tr>
      <th>93</th>
      <td>94</td>
      <td>2001030115</td>
      <td>凌雨翔</td>
      <td>建筑</td>
      <td>风景2001</td>
    </tr>
    <tr>
      <th>94</th>
      <td>95</td>
      <td>2001030123</td>
      <td>张卜予</td>
      <td>建筑</td>
      <td>风景2001</td>
    </tr>
    <tr>
      <th>95</th>
      <td>96</td>
      <td>2001030220</td>
      <td>王光辉</td>
      <td>建筑</td>
      <td>风景2002</td>
    </tr>
  </tbody>
</table>
<p>96 rows × 5 columns</p>
</div>




```python
from sklearn.utils import shuffle
pylesson_roll=shuffle(pylesson_roll,random_state=10) 
pylesson_roll
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>N</th>
      <th>ID</th>
      <th>name</th>
      <th>major</th>
      <th>class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <td>35</td>
      <td>2001010220</td>
      <td>潘洪转</td>
      <td>建筑</td>
      <td>建筑2002</td>
    </tr>
    <tr>
      <th>58</th>
      <td>59</td>
      <td>2001010331</td>
      <td>任长帅</td>
      <td>建筑</td>
      <td>建筑2003</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2001050105</td>
      <td>杨昱萱</td>
      <td>建筑</td>
      <td>建筑2001</td>
    </tr>
    <tr>
      <th>35</th>
      <td>36</td>
      <td>2001010223</td>
      <td>李飞翔</td>
      <td>建筑</td>
      <td>建筑2002</td>
    </tr>
    <tr>
      <th>19</th>
      <td>20</td>
      <td>2001050128</td>
      <td>刘恒</td>
      <td>建筑</td>
      <td>建筑2001</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>89</th>
      <td>90</td>
      <td>2008050205</td>
      <td>刘茜</td>
      <td>建筑</td>
      <td>城规2102</td>
    </tr>
    <tr>
      <th>28</th>
      <td>29</td>
      <td>2001010211</td>
      <td>尹若寒</td>
      <td>建筑</td>
      <td>建筑2002</td>
    </tr>
    <tr>
      <th>64</th>
      <td>65</td>
      <td>2001010412</td>
      <td>吴博雅</td>
      <td>建筑</td>
      <td>建筑2004</td>
    </tr>
    <tr>
      <th>15</th>
      <td>16</td>
      <td>2001050122</td>
      <td>李凯</td>
      <td>建筑</td>
      <td>建筑2001</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>2001050116</td>
      <td>薛嘉玮</td>
      <td>建筑</td>
      <td>建筑2001</td>
    </tr>
  </tbody>
</table>
<p>96 rows × 5 columns</p>
</div>




```python
import math,itertools

rows=len(pylesson_roll)
member_num=6
group_num=math.floor(rows/member_num)
remainder=rows%member_num
print("group_num={},remainder={}".format(group_num,remainder))
group_label=[list(itertools.repeat(i+1,member_num)) for i in range(group_num)]
print(group_label)
print("_"*50)
if remainder !=0:
    group_label=[group_label[i]+[i+1] if i <remainder else group_label[i] for i in range(group_num) ]
print(group_label)    
print("_"*50)
flatten_lst=lambda lst: [m for n_lst in lst for m in flatten_lst(n_lst)] if type(lst) is list else [lst]
group_label_flatten=flatten_lst(group_label)
print(group_label_flatten)
print("_"*50)
pd.options.mode.chained_assignment = None  # default='warn'
pylesson_roll['group_label']=group_label_flatten
print(pylesson_roll)
pylesson_roll.to_excel('./data/python_lesson_roll_book_2021_group.xlsx')
```

    group_num=16,remainder=0
    [[1, 1, 1, 1, 1, 1], [2, 2, 2, 2, 2, 2], [3, 3, 3, 3, 3, 3], [4, 4, 4, 4, 4, 4], [5, 5, 5, 5, 5, 5], [6, 6, 6, 6, 6, 6], [7, 7, 7, 7, 7, 7], [8, 8, 8, 8, 8, 8], [9, 9, 9, 9, 9, 9], [10, 10, 10, 10, 10, 10], [11, 11, 11, 11, 11, 11], [12, 12, 12, 12, 12, 12], [13, 13, 13, 13, 13, 13], [14, 14, 14, 14, 14, 14], [15, 15, 15, 15, 15, 15], [16, 16, 16, 16, 16, 16]]
    __________________________________________________
    [[1, 1, 1, 1, 1, 1], [2, 2, 2, 2, 2, 2], [3, 3, 3, 3, 3, 3], [4, 4, 4, 4, 4, 4], [5, 5, 5, 5, 5, 5], [6, 6, 6, 6, 6, 6], [7, 7, 7, 7, 7, 7], [8, 8, 8, 8, 8, 8], [9, 9, 9, 9, 9, 9], [10, 10, 10, 10, 10, 10], [11, 11, 11, 11, 11, 11], [12, 12, 12, 12, 12, 12], [13, 13, 13, 13, 13, 13], [14, 14, 14, 14, 14, 14], [15, 15, 15, 15, 15, 15], [16, 16, 16, 16, 16, 16]]
    __________________________________________________
    [1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 3, 3, 4, 4, 4, 4, 4, 4, 5, 5, 5, 5, 5, 5, 6, 6, 6, 6, 6, 6, 7, 7, 7, 7, 7, 7, 8, 8, 8, 8, 8, 8, 9, 9, 9, 9, 9, 9, 10, 10, 10, 10, 10, 10, 11, 11, 11, 11, 11, 11, 12, 12, 12, 12, 12, 12, 13, 13, 13, 13, 13, 13, 14, 14, 14, 14, 14, 14, 15, 15, 15, 15, 15, 15, 16, 16, 16, 16, 16, 16]
    __________________________________________________
         N          ID name major   class  group_label
    34  35  2001010220  潘洪转    建筑  建筑2002            1
    58  59  2001010331  任长帅    建筑  建筑2003            1
    3    4  2001050105  杨昱萱    建筑  建筑2001            1
    35  36  2001010223  李飞翔    建筑  建筑2002            1
    19  20  2001050128   刘恒    建筑  建筑2001            1
    ..  ..         ...  ...   ...     ...          ...
    89  90  2008050205   刘茜    建筑  城规2102           16
    28  29  2001010211  尹若寒    建筑  建筑2002           16
    64  65  2001010412  吴博雅    建筑  建筑2004           16
    15  16  2001050122   李凯    建筑  建筑2001           16
    9   10  2001050116  薛嘉玮    建筑  建筑2001           16
    
    [96 rows x 6 columns]
    


```python
def roll_group(member_num,random_state=None,save_path=None,**kwargs):
    import pandas as pd
    pd.options.mode.chained_assignment = None  # default='warn'
    import math,itertools
    from sklearn.utils import shuffle    
    
    '''
    function - 指定每组人数，随机分组，未尽分组人分配到其他组
    
    paras:
    member_num - 每组成员数
    random_state - 随机状态。默认每次均变化，给定固定值，则每次一样
    save_path - 设置保存路径，如果不设置则不保存
    kwargs - keyword arguments。包括：file_path - excel文件路径； sheet_name - 表名称； header - column 名称为第几行

    return:
    roll - DataFrame格式，返回随机分配好的成员名单
    '''

    roll=pd.read_excel(kwargs["file_path"],kwargs["sheet_name"],header=kwargs["header"],engine='openpyxl')    
    roll=shuffle(roll,random_state=random_state) 
    #print(roll)
    
    member_num=member_num
    rows=len(pylesson_roll)
    group_num=math.floor(rows/member_num)
    remainder=rows%member_num
    group_label=[list(itertools.repeat(i+1,member_num)) for i in range(group_num)]
    if remainder !=0:
        group_label=[group_label[i]+[i+1] if i <remainder else group_label[i] for i in range(group_num) ]    
    flatten_lst=lambda lst: [m for n_lst in lst for m in flatten_lst(n_lst)] if type(lst) is list else [lst]
    group_label_flatten=flatten_lst(group_label)   
    roll['group_label']=group_label_flatten
    #print(roll)
    
    if save_path:
        roll.to_excel(save_path)
        print("The group excel has been saved to the path:{}".format(save_path))
    
    return roll    
    
exel_read_info={"file_path":r'./data/python_lesson_roll_book_2021.xlsx',"sheet_name":'roll',"header":[0]}
roll_group=roll_group(member_num=6,random_state=10,save_path='./data/python_lesson_roll_book_2021_group.xlsx',**exel_read_info)    
```

    The group excel has been saved to the path:./data/python_lesson_roll_book_2021_group.xlsx
    


```python
roll_group
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>N</th>
      <th>ID</th>
      <th>name</th>
      <th>major</th>
      <th>class</th>
      <th>group_label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <td>35</td>
      <td>2001010220</td>
      <td>潘洪转</td>
      <td>建筑</td>
      <td>建筑2002</td>
      <td>1</td>
    </tr>
    <tr>
      <th>58</th>
      <td>59</td>
      <td>2001010331</td>
      <td>任长帅</td>
      <td>建筑</td>
      <td>建筑2003</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2001050105</td>
      <td>杨昱萱</td>
      <td>建筑</td>
      <td>建筑2001</td>
      <td>1</td>
    </tr>
    <tr>
      <th>35</th>
      <td>36</td>
      <td>2001010223</td>
      <td>李飞翔</td>
      <td>建筑</td>
      <td>建筑2002</td>
      <td>1</td>
    </tr>
    <tr>
      <th>19</th>
      <td>20</td>
      <td>2001050128</td>
      <td>刘恒</td>
      <td>建筑</td>
      <td>建筑2001</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>89</th>
      <td>90</td>
      <td>2008050205</td>
      <td>刘茜</td>
      <td>建筑</td>
      <td>城规2102</td>
      <td>16</td>
    </tr>
    <tr>
      <th>28</th>
      <td>29</td>
      <td>2001010211</td>
      <td>尹若寒</td>
      <td>建筑</td>
      <td>建筑2002</td>
      <td>16</td>
    </tr>
    <tr>
      <th>64</th>
      <td>65</td>
      <td>2001010412</td>
      <td>吴博雅</td>
      <td>建筑</td>
      <td>建筑2004</td>
      <td>16</td>
    </tr>
    <tr>
      <th>15</th>
      <td>16</td>
      <td>2001050122</td>
      <td>李凯</td>
      <td>建筑</td>
      <td>建筑2001</td>
      <td>16</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>2001050116</td>
      <td>薛嘉玮</td>
      <td>建筑</td>
      <td>建筑2001</td>
      <td>16</td>
    </tr>
  </tbody>
</table>
<p>96 rows × 6 columns</p>
</div>

### [指南](https://richiebao.github.io/Urban-Spatial-Data-Analysis_python/#/./markdown/instruction)

[课程所有原始文件链接]（https://github.com/richieBao/python_code_archi_la_design_method_study/tree/main/notebook）

