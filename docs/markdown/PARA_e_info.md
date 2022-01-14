> Created on Sun Oct 10 22\24\40 2021 @author: Richie Bao-python_code_archi_la_design_method_study 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# 参数化景观|建筑 设计方法 parameterize archi|la design and data analysis

### 高校课程信息
**课程名称**：软件基础CAAD与创新设计 （日后课程名称可能修改为：参数化景观设计方法）

**学分**：1.5

**总学时/课内实践学时**：24

**课程性质**：选修课程

**课程类型**：通识核心课程

**适应对象**：建筑、风景、规划

### 组队
每6人一组，如不能归整，余数分别自由组到已有组别。每组自行推选一个组长，组名自选任何一种动物名。

推选课程代表1-2人，负责课程事宜。

### 考核

* 平时成绩

1. 小组实验，组内成员贡献度由小组根据个人贡献大小共同确定（需诚实守信）， 百分比计算，综合为100%。
2. 签到，由课代表及组长负责（需诚实守信）。

* 期末成绩

### 小组名单
由课代表完成汇总，格式为（Excel编写）：

* 表1

| 序号  |  组名 | 组员名 |学号 | 组长  |  备注 |签到-1 |签到-2 |... |签到-12 | 签到成绩 |小组实验个人贡献-1|...|小组实验个人贡献-n|小组实验个人贡献成绩|期末实验个人贡献度|总成绩|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1  | Kitty  | 张三  |   | 是  |   |  1 |   |   |   |   |   |   |   |   |   |   |
| 2  |   | 李四  |   |   |   |  1 |   |   |   |   |   |   |   |   |   |   |
| 3  |   | 王五  |   |   |   |  0 |   |   |   |   |   |   |   |   |   |   |
| 4  |   | .  |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
| 5  |   | .  |   |  |   |   |   |   |   |   |   |   |   |   |   |   |
| 6  |   | .  |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
| 7  | Squirrel  | 赵六 |   | 是  |   | 1  |   |   |   |   |   |   |   |   |   |   |
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


> 签到，1为到，0为未到，2为请假（需假条）。最后汇总，由课代表在grasshopper中读取Excel文件编写代码，计算最终签到成绩，小组实验成绩。

### 小组实验
#### 实验-1
个人实验。自修完[参数化逻辑构建过程](http://cadesign.cn/course/2010_2020/3_gh_parameterized_logical_build_process/), 完成时间为第5次课前。练习的.gh及.3dm文件单独存储为独立的一个文件夹内，文件夹统一命名为“Experi_01_姓名_学号”。最后提交图文并茂A4若干张的一个.pdf自修报告文件，文件命名为`Experi_01_姓名_学号.pdf`。

评分粗略标准：

1. 不要完全照抄源代码，建议可以自行变化实验，并反应到报告文件中；
2. 报告文件内容包括自修课程自己练习的参数化设计图表达及代码，和理解注释等。另排版合理，美观；
3. 是否全部完成;
4. 与给定学习内容不符；

#### 实验-2
个人实验。对应完成**风景园林工程与技术**场地三维地形参数化模型的建立。文件夹统一命名为“Experi_02_姓名_学号”,文件命名为`Experi_02_topography.3dm`，以及`Experi_02_topography.gh`。并任一设置一个场地，随机生成一个地形，参数化标注高程等信息，文件命名为`Experi_02_topography_generation.gh`。最后提交图文并茂A4若干张的一个.pdf报告文件，文件命名为`Experi_02_姓名_学号.pdf`。


#### 实验-3
小组实验。阅读《图解木构造》，从中选择一种建筑形式，或者根据内容讲解，自行设计。全部设计均由参数化实现（不能在RH空间中增加辅助线），最后提交整体的参数化设计程序模型，以及任意3组参数化节点构造详图。文件命名为`Experi_03_design_of_wood_小组名.gh`。并提交图文并茂A4若干张的一个.pdf报告文件，文件命名为`Experi_03_小组名.pdf`。

评分粗略标准：
1. 未按指定要求作业；
2. 没有.pdf报告文件;

#### 实验-4(deprecated)


#### 实验-5(deprecated)


#### 实验-6(deprecated)


### 期末实验
基于“风景园林工程与技术”课程场地，以参数化方式，从新完成所有设计。提交材料：

1. 一个.gh文件，一个.3dm文件；
2. A4大小，5-8页一个.pdf设计说明文件。

（已确定，需要打印纸质版，批分见红并提交至教务处存档。因此打印后汇总到课程负责人处）

> Created on Fri Jan 14 14:42:51 2022 @author: Richie Bao

## 评分程序


```python
fp="./data/2021Fall_GH_experi_grade_note.xlsx"
```

### 1. 考勤 -分值计算（10）


```python
import pandas as pd

#1.数据预处理
grade_attendance=pd.read_excel(fp,sheet_name="attendance",header=0)
unnamed_columns=[i for i in  grade_attendance.columns.to_list() if i.split(":")[0]=="Unnamed"]
attendance_column_name_mapping={k:i+2 for i,k in enumerate(unnamed_columns)}
attendance_column_name_mapping.update({"考勤记录":1})
grade_attendance.rename(columns=attendance_column_name_mapping,inplace=True)

def string_processing(df,str_columns):
    for sc in str_columns:
        df[sc]=df[sc].astype(str)
        df[sc].str.strip()    
    return df
str_columns=["学号","姓名"]
grade_attendance=string_processing(grade_attendance,str_columns)

#2.请假值(2)替换值：0.9
day_off=0.9 
grade_attendance.replace({2:0.9},inplace=True) 

#3.求和与映射到10分制区间（出勤占比10%）,公式：(score/10)*10
grade_attendance["score_sum"]=grade_attendance[list(attendance_column_name_mapping.values())].sum(axis=1)
grade_attendance["score_attendance_10"]=grade_attendance.score_sum.apply(lambda row:row*10/10)

grade_attendance.head()
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
      <th>序号</th>
      <th>组名</th>
      <th>学号</th>
      <th>姓名</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>score_sum</th>
      <th>score_attendance_10</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>alpha</td>
      <td>1901030126</td>
      <td>周子涵</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>10.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1810010215</td>
      <td>聂川林</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>10.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1901030117</td>
      <td>丁睿思</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>9.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1901030119</td>
      <td>金恽卓</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>9.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1901030204</td>
      <td>邵雨</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>10.0</td>
      <td>10.0</td>
    </tr>
  </tbody>
</table>
</div>



### 2. 个人实验（实验1）-分值计算（15）


```python
grade_experi=pd.read_excel(fp,sheet_name="experi-class",header=0)
str_columns=["学号","姓名"]
grade_experi=string_processing(grade_experi,str_columns)

def grade_updated(df,grade_updated_columns,name):
    df_grades=df[grade_updated_columns].T
    df_grades.fillna(method="ffill",inplace=True)
    df_grades_final=df_grades.T[grade_updated_columns[-1]]
    df_grades_final.fillna(method="ffill",inplace=True)
    df_grades_final.replace({"?":"E"},inplace=True) #处理？部分，即没有收到实验
    print("{}:\n{}".format(name,df_grades_final.to_list()))        
    df_grades_final.fillna("D",inplace=True) #配置空值评级为D
    df[name]=df_grades_final 
    
    return df    

grade_updated_expri_1=["experi-1","experi-1-updated",]
grade_experi=grade_updated(grade_experi,grade_updated_expri_1,"experi_1_grade_final")

grade2score_mapping={"A+":99,"A":95,"A-":91,"B+":89,"B":85,"B-":81,"C+":79,"C":75,"C-":71,"D+":69,"D":65,"D-":61,"E":55}
grade_experi["experi_1_100"]=grade_experi["experi_1_grade_final"].map(grade2score_mapping)
grade_experi["experi_1_15"]=grade_experi.experi_1_100.apply(lambda row:row*15/100)

print(grade_experi.columns)
grade_experi.head()
```

    experi_1_grade_final:
    ['A-', 'B-', 'A', 'A-', 'B+', 'B+', 'A', 'A-', 'A+', 'A-', 'B', 'A-', 'B+', 'A', 'A-', 'A+', 'C', 'D', 'C-', 'D', 'B+', 'D', 'B', 'C-', 'A', 'B-', 'C', 'C+', 'B+', 'C', 'A-', 'B', 'B', 'B+', 'B-', 'B+', 'A', 'A', 'A+', 'A+', 'B+', 'B', 'C-', 'C-']
    Index(['序号', '组名', '学号', '姓名', 'experi-1', 'experi-1-note', 'experi-1-updated',
           'experi-3', 'experi-3-note', 'experi-3-note-updated', 'final',
           'final-note', 'final-updated', 'experi_1_grade_final', 'experi_1_100',
           'experi_1_15'],
          dtype='object')
    




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
      <th>序号</th>
      <th>组名</th>
      <th>学号</th>
      <th>姓名</th>
      <th>experi-1</th>
      <th>experi-1-note</th>
      <th>experi-1-updated</th>
      <th>experi-3</th>
      <th>experi-3-note</th>
      <th>experi-3-note-updated</th>
      <th>final</th>
      <th>final-note</th>
      <th>final-updated</th>
      <th>experi_1_grade_final</th>
      <th>experi_1_100</th>
      <th>experi_1_15</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>alpha</td>
      <td>1901030126</td>
      <td>周子涵</td>
      <td>A-</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A-</td>
      <td>未见参数化人模型辅助设计推敲，部分节点设计缺少与地形的参数化关联，部分节点为课程示范案例。</td>
      <td>NaN</td>
      <td>A-</td>
      <td>91</td>
      <td>13.65</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1810010215</td>
      <td>聂川林</td>
      <td>B-</td>
      <td>2,3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>C</td>
      <td>部分节点为课程演示模型，节点参数化设计脱离地形环境。报告文件粗糙。</td>
      <td>NaN</td>
      <td>B-</td>
      <td>81</td>
      <td>12.15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1901030117</td>
      <td>丁睿思</td>
      <td>A</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B+</td>
      <td>部分节点为课程示范，未见参数化人模型辅助推敲。</td>
      <td>NaN</td>
      <td>A</td>
      <td>95</td>
      <td>14.25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1901030119</td>
      <td>金恽卓</td>
      <td>A-</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B+</td>
      <td>能够将节点设计与地形结合起来，未见参数化人模型辅助推敲，部分节点为复制课程节点。</td>
      <td>NaN</td>
      <td>A-</td>
      <td>91</td>
      <td>13.65</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1901030204</td>
      <td>邵雨</td>
      <td>B+</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A-</td>
      <td>能够结合地形环境从事节点部分的设计，但是未见人模型辅助尺度设计。部分代码编写过于繁琐，缺少对...</td>
      <td>A</td>
      <td>B+</td>
      <td>89</td>
      <td>13.35</td>
    </tr>
  </tbody>
</table>
</div>



### 3. 小组实验（实验3）-分值计算（25）


```python
grade_updated_expri_3=['experi-3','experi-3-note-updated']
grade_experi=grade_updated(grade_experi,grade_updated_expri_3,"experi_3_grade_final")
grade_experi["experi_3_100"]=grade_experi["experi_3_grade_final"].map(grade2score_mapping)
grade_experi["experi_3_25"]=grade_experi.experi_3_100.apply(lambda row:row*25/100)

grade_experi.head()
```

    experi_3_grade_final:
    ['A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'C', 'C', 'C', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A', 'A+', 'A+', 'A+', 'A+', 'B+', 'B+', 'B+', 'B+']
    




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
      <th>序号</th>
      <th>组名</th>
      <th>学号</th>
      <th>姓名</th>
      <th>experi-1</th>
      <th>experi-1-note</th>
      <th>experi-1-updated</th>
      <th>experi-3</th>
      <th>experi-3-note</th>
      <th>experi-3-note-updated</th>
      <th>final</th>
      <th>final-note</th>
      <th>final-updated</th>
      <th>experi_1_grade_final</th>
      <th>experi_1_100</th>
      <th>experi_1_15</th>
      <th>experi_3_grade_final</th>
      <th>experi_3_100</th>
      <th>experi_3_25</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>alpha</td>
      <td>1901030126</td>
      <td>周子涵</td>
      <td>A-</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A-</td>
      <td>未见参数化人模型辅助设计推敲，部分节点设计缺少与地形的参数化关联，部分节点为课程示范案例。</td>
      <td>NaN</td>
      <td>A-</td>
      <td>91</td>
      <td>13.65</td>
      <td>A</td>
      <td>95</td>
      <td>23.75</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1810010215</td>
      <td>聂川林</td>
      <td>B-</td>
      <td>2,3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>C</td>
      <td>部分节点为课程演示模型，节点参数化设计脱离地形环境。报告文件粗糙。</td>
      <td>NaN</td>
      <td>B-</td>
      <td>81</td>
      <td>12.15</td>
      <td>A</td>
      <td>95</td>
      <td>23.75</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1901030117</td>
      <td>丁睿思</td>
      <td>A</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B+</td>
      <td>部分节点为课程示范，未见参数化人模型辅助推敲。</td>
      <td>NaN</td>
      <td>A</td>
      <td>95</td>
      <td>14.25</td>
      <td>A</td>
      <td>95</td>
      <td>23.75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1901030119</td>
      <td>金恽卓</td>
      <td>A-</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B+</td>
      <td>能够将节点设计与地形结合起来，未见参数化人模型辅助推敲，部分节点为复制课程节点。</td>
      <td>NaN</td>
      <td>A-</td>
      <td>91</td>
      <td>13.65</td>
      <td>A</td>
      <td>95</td>
      <td>23.75</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1901030204</td>
      <td>邵雨</td>
      <td>B+</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A-</td>
      <td>能够结合地形环境从事节点部分的设计，但是未见人模型辅助尺度设计。部分代码编写过于繁琐，缺少对...</td>
      <td>A</td>
      <td>B+</td>
      <td>89</td>
      <td>13.35</td>
      <td>A</td>
      <td>95</td>
      <td>23.75</td>
    </tr>
  </tbody>
</table>
</div>



### 4. 期末实验-分值计算（50）


```python
grade_updated_expri_final=['final','final-updated']
grade_experi=grade_updated(grade_experi,grade_updated_expri_final,"final_grade")
grade_experi["final_100"]=grade_experi["final_grade"].map(grade2score_mapping)
grade_experi["final_50"]=grade_experi.final_100.apply(lambda row:row*50/100)

grade_experi.head()
```

    final_grade:
    ['A-', 'C', 'B+', 'B+', 'A', 'A-', 'A-', 'B+', 'A-', 'B-', 'C', 'A-', 'B+', 'B+', 'C', 'B+', 'C-', 'D-', 'D-', 'D-', 'E', 'C+', 'A-', 'D', 'B+', 'B-', 'B+', 'B', 'B', 'C+', 'B+', 'A', 'B', 'B', 'B', 'B+', 'A+', 'A-', 'A-', 'A', 'B-', 'B+', 'B-', 'B+']
    




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
      <th>序号</th>
      <th>组名</th>
      <th>学号</th>
      <th>姓名</th>
      <th>experi-1</th>
      <th>experi-1-note</th>
      <th>experi-1-updated</th>
      <th>experi-3</th>
      <th>experi-3-note</th>
      <th>experi-3-note-updated</th>
      <th>...</th>
      <th>final-updated</th>
      <th>experi_1_grade_final</th>
      <th>experi_1_100</th>
      <th>experi_1_15</th>
      <th>experi_3_grade_final</th>
      <th>experi_3_100</th>
      <th>experi_3_25</th>
      <th>final_grade</th>
      <th>final_100</th>
      <th>final_50</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>alpha</td>
      <td>1901030126</td>
      <td>周子涵</td>
      <td>A-</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>A-</td>
      <td>91</td>
      <td>13.65</td>
      <td>A</td>
      <td>95</td>
      <td>23.75</td>
      <td>A-</td>
      <td>91</td>
      <td>45.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1810010215</td>
      <td>聂川林</td>
      <td>B-</td>
      <td>2,3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>B-</td>
      <td>81</td>
      <td>12.15</td>
      <td>A</td>
      <td>95</td>
      <td>23.75</td>
      <td>C</td>
      <td>75</td>
      <td>37.5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1901030117</td>
      <td>丁睿思</td>
      <td>A</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>A</td>
      <td>95</td>
      <td>14.25</td>
      <td>A</td>
      <td>95</td>
      <td>23.75</td>
      <td>B+</td>
      <td>89</td>
      <td>44.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1901030119</td>
      <td>金恽卓</td>
      <td>A-</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>A-</td>
      <td>91</td>
      <td>13.65</td>
      <td>A</td>
      <td>95</td>
      <td>23.75</td>
      <td>B+</td>
      <td>89</td>
      <td>44.5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1901030204</td>
      <td>邵雨</td>
      <td>B+</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>A</td>
      <td>B+</td>
      <td>89</td>
      <td>13.35</td>
      <td>A</td>
      <td>95</td>
      <td>23.75</td>
      <td>A</td>
      <td>95</td>
      <td>47.5</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



### 总分数计算（100）


```python
import pandas as pd
from functools import reduce
score_attendance=grade_attendance[["姓名","学号","score_attendance_10"]]
score_experi=grade_experi[["学号","experi_1_15","experi_3_25","final_50"]]
df_list=[score_attendance,score_experi]
overall_score=reduce(lambda left,right:pd.merge(left,right,on=["学号"],how="outer"),df_list)
column_name_list=["score_attendance_10","experi_1_15","experi_3_25","final_50"]
overall_score['overall_score']=overall_score[column_name_list].sum(axis=1)
overall_score.to_excel("./data/overall score.xlsx",sheet_name="overall_score")

overall_score.head()
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
      <th>姓名</th>
      <th>学号</th>
      <th>score_attendance_10</th>
      <th>experi_1_15</th>
      <th>experi_3_25</th>
      <th>final_50</th>
      <th>overall_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>周子涵</td>
      <td>1901030126</td>
      <td>10.0</td>
      <td>13.65</td>
      <td>23.75</td>
      <td>45.5</td>
      <td>92.9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>聂川林</td>
      <td>1810010215</td>
      <td>10.0</td>
      <td>12.15</td>
      <td>23.75</td>
      <td>37.5</td>
      <td>83.4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>丁睿思</td>
      <td>1901030117</td>
      <td>9.0</td>
      <td>14.25</td>
      <td>23.75</td>
      <td>44.5</td>
      <td>91.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>金恽卓</td>
      <td>1901030119</td>
      <td>9.0</td>
      <td>13.65</td>
      <td>23.75</td>
      <td>44.5</td>
      <td>90.9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>邵雨</td>
      <td>1901030204</td>
      <td>10.0</td>
      <td>13.35</td>
      <td>23.75</td>
      <td>47.5</td>
      <td>94.6</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
