> Created on Mon Sep 27 14\17\47 2021 @author: Richie Bao-python_code_archi_la_design_method_study 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# 网络关系->空间模型-agent基于代理模型(mesa)

> 参考： 《2018年全国一级注册建筑师执业资格考试——历年真题解析与模拟试卷：建筑方案设计（作图题）》-2017年真题

## 1. 关系查看
### 1.1 网络关系数据组织与读取


```python
import pandas as pd

requirements_fp=r'./data/2017_exam_paper_network.xlsx'
floor_1=pd.read_excel(requirements_fp,sheet_name='floor_1',header=[1],engine='openpyxl')
floor_1=floor_1[floor_1.filter(regex='^(?!Unnamed)').columns]
print(floor_1.columns)
```

    Index(['空间名称', '分区名称', '房间名称', '建筑面积', '间数', '备注'], dtype='object')
    


```python
floor_1
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
      <th>空间名称</th>
      <th>分区名称</th>
      <th>房间名称</th>
      <th>建筑面积</th>
      <th>间数</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>公共部分</td>
      <td>旅馆大堂区</td>
      <td>大堂</td>
      <td>400</td>
      <td>1.0</td>
      <td>含前台办公40m2、行季间20m2、库房10m2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>大堂吧</td>
      <td>260</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>商店</td>
      <td>90</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>商务中心</td>
      <td>45</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>次出入口门厅</td>
      <td>130</td>
      <td>1.0</td>
      <td>含2台客悌、1部楼梯，通往二层宴会（会议）区</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>客房电梯厅</td>
      <td>70</td>
      <td>1.0</td>
      <td>含2台客梯、1部楼梯可结合大堂布置适当扩大面枳</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>客房货梯厅</td>
      <td>40</td>
      <td>1.0</td>
      <td>含1台货梯（兼消防电梯）、1部楼梯</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>大堂公共卫生间</td>
      <td>55</td>
      <td>1.0</td>
      <td>男女各25m2,无障碍卫生间5m2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>餐饮区</td>
      <td>中餐厅</td>
      <td>600</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>西餐厅</td>
      <td>260</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>餐厅公共卫生间</td>
      <td>85</td>
      <td>4.0</td>
      <td>男女各35m2，无障诗卫生间5m2，清洁间10m2</td>
    </tr>
    <tr>
      <th>11</th>
      <td>NaN</td>
      <td>健身娱乐区</td>
      <td>休息厅</td>
      <td>80</td>
      <td>1.0</td>
      <td>含接待服务台</td>
    </tr>
    <tr>
      <th>12</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>健身房</td>
      <td>260</td>
      <td>1.0</td>
      <td>含男、女更衣各30m2（含卫生间）</td>
    </tr>
    <tr>
      <th>13</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>台球室</td>
      <td>130</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>辅助部分</td>
      <td>厨房共用区</td>
      <td>货物门厅</td>
      <td>55</td>
      <td>1.0</td>
      <td>含1台货梯</td>
    </tr>
    <tr>
      <th>15</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>收验间</td>
      <td>25</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>垃圾电梯厅</td>
      <td>20</td>
      <td>1.0</td>
      <td>含1台垃圾电梯,并直接对外开门</td>
    </tr>
    <tr>
      <th>17</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>垃圾间</td>
      <td>15</td>
      <td>1.0</td>
      <td>与垃圾电梯厅相邻</td>
    </tr>
    <tr>
      <th>18</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>员工门厅</td>
      <td>30</td>
      <td>1.0</td>
      <td>含1部专用楼梯</td>
    </tr>
    <tr>
      <th>19</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>员工更衣室</td>
      <td>90</td>
      <td>1.0</td>
      <td>男女各45m2 （含卫生间）</td>
    </tr>
    <tr>
      <th>20</th>
      <td>NaN</td>
      <td>中餐厨房区</td>
      <td>中餐加工制作间</td>
      <td>180</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>中餐备餐间</td>
      <td>40</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>中餐洗琬间</td>
      <td>30</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>中餐库房</td>
      <td>80</td>
      <td>2.0</td>
      <td>每间40m2,与加工制作间相邻</td>
    </tr>
    <tr>
      <th>24</th>
      <td>NaN</td>
      <td>西餐厨房区</td>
      <td>西餐加工制作间</td>
      <td>120</td>
      <td>1.0</td>
      <td>每间40m2,与加工制作间相邻</td>
    </tr>
    <tr>
      <th>25</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>西餐备餐间</td>
      <td>30</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>西餐洗碗间</td>
      <td>30</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>27</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>西餐库房</td>
      <td>50</td>
      <td>2.0</td>
      <td>每间25m2.与加工制作间相邻</td>
    </tr>
    <tr>
      <th>28</th>
      <td>其他</td>
      <td>交通</td>
      <td>走道、楼梯等</td>
      <td>800</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
floor_1_ffill_columns=['空间名称', '分区名称']
floor_1[floor_1_ffill_columns]=floor_1[floor_1_ffill_columns].fillna(method='ffill')
floor_1
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
      <th>空间名称</th>
      <th>分区名称</th>
      <th>房间名称</th>
      <th>建筑面积</th>
      <th>间数</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>公共部分</td>
      <td>旅馆大堂区</td>
      <td>大堂</td>
      <td>400</td>
      <td>1.0</td>
      <td>含前台办公40m2、行季间20m2、库房10m2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>公共部分</td>
      <td>旅馆大堂区</td>
      <td>大堂吧</td>
      <td>260</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>公共部分</td>
      <td>旅馆大堂区</td>
      <td>商店</td>
      <td>90</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>公共部分</td>
      <td>旅馆大堂区</td>
      <td>商务中心</td>
      <td>45</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>公共部分</td>
      <td>旅馆大堂区</td>
      <td>次出入口门厅</td>
      <td>130</td>
      <td>1.0</td>
      <td>含2台客悌、1部楼梯，通往二层宴会（会议）区</td>
    </tr>
    <tr>
      <th>5</th>
      <td>公共部分</td>
      <td>旅馆大堂区</td>
      <td>客房电梯厅</td>
      <td>70</td>
      <td>1.0</td>
      <td>含2台客梯、1部楼梯可结合大堂布置适当扩大面枳</td>
    </tr>
    <tr>
      <th>6</th>
      <td>公共部分</td>
      <td>旅馆大堂区</td>
      <td>客房货梯厅</td>
      <td>40</td>
      <td>1.0</td>
      <td>含1台货梯（兼消防电梯）、1部楼梯</td>
    </tr>
    <tr>
      <th>7</th>
      <td>公共部分</td>
      <td>旅馆大堂区</td>
      <td>大堂公共卫生间</td>
      <td>55</td>
      <td>1.0</td>
      <td>男女各25m2,无障碍卫生间5m2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>公共部分</td>
      <td>餐饮区</td>
      <td>中餐厅</td>
      <td>600</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>公共部分</td>
      <td>餐饮区</td>
      <td>西餐厅</td>
      <td>260</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>公共部分</td>
      <td>餐饮区</td>
      <td>餐厅公共卫生间</td>
      <td>85</td>
      <td>4.0</td>
      <td>男女各35m2，无障诗卫生间5m2，清洁间10m2</td>
    </tr>
    <tr>
      <th>11</th>
      <td>公共部分</td>
      <td>健身娱乐区</td>
      <td>休息厅</td>
      <td>80</td>
      <td>1.0</td>
      <td>含接待服务台</td>
    </tr>
    <tr>
      <th>12</th>
      <td>公共部分</td>
      <td>健身娱乐区</td>
      <td>健身房</td>
      <td>260</td>
      <td>1.0</td>
      <td>含男、女更衣各30m2（含卫生间）</td>
    </tr>
    <tr>
      <th>13</th>
      <td>公共部分</td>
      <td>健身娱乐区</td>
      <td>台球室</td>
      <td>130</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>辅助部分</td>
      <td>厨房共用区</td>
      <td>货物门厅</td>
      <td>55</td>
      <td>1.0</td>
      <td>含1台货梯</td>
    </tr>
    <tr>
      <th>15</th>
      <td>辅助部分</td>
      <td>厨房共用区</td>
      <td>收验间</td>
      <td>25</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>辅助部分</td>
      <td>厨房共用区</td>
      <td>垃圾电梯厅</td>
      <td>20</td>
      <td>1.0</td>
      <td>含1台垃圾电梯,并直接对外开门</td>
    </tr>
    <tr>
      <th>17</th>
      <td>辅助部分</td>
      <td>厨房共用区</td>
      <td>垃圾间</td>
      <td>15</td>
      <td>1.0</td>
      <td>与垃圾电梯厅相邻</td>
    </tr>
    <tr>
      <th>18</th>
      <td>辅助部分</td>
      <td>厨房共用区</td>
      <td>员工门厅</td>
      <td>30</td>
      <td>1.0</td>
      <td>含1部专用楼梯</td>
    </tr>
    <tr>
      <th>19</th>
      <td>辅助部分</td>
      <td>厨房共用区</td>
      <td>员工更衣室</td>
      <td>90</td>
      <td>1.0</td>
      <td>男女各45m2 （含卫生间）</td>
    </tr>
    <tr>
      <th>20</th>
      <td>辅助部分</td>
      <td>中餐厨房区</td>
      <td>中餐加工制作间</td>
      <td>180</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>辅助部分</td>
      <td>中餐厨房区</td>
      <td>中餐备餐间</td>
      <td>40</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>辅助部分</td>
      <td>中餐厨房区</td>
      <td>中餐洗琬间</td>
      <td>30</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>辅助部分</td>
      <td>中餐厨房区</td>
      <td>中餐库房</td>
      <td>80</td>
      <td>2.0</td>
      <td>每间40m2,与加工制作间相邻</td>
    </tr>
    <tr>
      <th>24</th>
      <td>辅助部分</td>
      <td>西餐厨房区</td>
      <td>西餐加工制作间</td>
      <td>120</td>
      <td>1.0</td>
      <td>每间40m2,与加工制作间相邻</td>
    </tr>
    <tr>
      <th>25</th>
      <td>辅助部分</td>
      <td>西餐厨房区</td>
      <td>西餐备餐间</td>
      <td>30</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>辅助部分</td>
      <td>西餐厨房区</td>
      <td>西餐洗碗间</td>
      <td>30</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>27</th>
      <td>辅助部分</td>
      <td>西餐厨房区</td>
      <td>西餐库房</td>
      <td>50</td>
      <td>2.0</td>
      <td>每间25m2.与加工制作间相邻</td>
    </tr>
    <tr>
      <th>28</th>
      <td>其他</td>
      <td>交通</td>
      <td>走道、楼梯等</td>
      <td>800</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
fr_1=pd.read_excel(requirements_fp,sheet_name='fr_1',header=[1],engine='openpyxl')
fr_1=fr_1[fr_1.filter(regex='^(?!Unnamed)').columns]
print(fr_1.columns)
fr_1_ffill_columns=['A']
fr_1[fr_1_ffill_columns]=fr_1[fr_1_ffill_columns].fillna(method='ffill')
fr_1
```

    Index(['A', 'B', '备注'], dtype='object')
    




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
      <th>A</th>
      <th>B</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>厨房共用区</td>
      <td>中餐厨房区</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>厨房共用区</td>
      <td>西餐厨房区</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>厨房共用区</td>
      <td>员工出入口</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>厨房共用区</td>
      <td>货物出入口</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>厨房共用区</td>
      <td>垃圾出口</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>大堂</td>
      <td>中餐厅</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>大堂</td>
      <td>西餐厅</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>大堂</td>
      <td>大堂吧</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>大堂</td>
      <td>健身娱乐区</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>大堂</td>
      <td>客房电梯厅</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>大堂</td>
      <td>次出入口门厅</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>中餐厅</td>
      <td>中餐备餐间</td>
      <td>中餐厨房</td>
    </tr>
    <tr>
      <th>12</th>
      <td>西餐厅</td>
      <td>西餐备餐间</td>
      <td>西餐厨房</td>
    </tr>
    <tr>
      <th>13</th>
      <td>大堂吧</td>
      <td>次出入口门厅</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### 1.2 [Sankey图](https://plotly.com/python/sankey-diagram/)

Sankey图 -> [Plotly Python](https://plotly.com/python/basic-charts/)


```python
floor_1_classi=['空间名称', '分区名称', '房间名称']
floor_1['edges']=floor_1.apply(lambda row:[(row[i],row[i+1]) for i in range(len(floor_1_classi)-1)],axis=1)
fr_1_classi=['A', 'B']
fr_1['edges']=fr_1.apply(lambda row:[(row[i],row[i+1]) for i in range(len(fr_1_classi)-1)],axis=1)

print(floor_1.edges)
print("-"*50)
print(fr_1.edges)
```

    0          [(公共部分, 旅馆大堂区), (旅馆大堂区, 大堂)]
    1         [(公共部分, 旅馆大堂区), (旅馆大堂区, 大堂吧)]
    2          [(公共部分, 旅馆大堂区), (旅馆大堂区, 商店)]
    3        [(公共部分, 旅馆大堂区), (旅馆大堂区, 商务中心)]
    4      [(公共部分, 旅馆大堂区), (旅馆大堂区, 次出入口门厅)]
    5       [(公共部分, 旅馆大堂区), (旅馆大堂区, 客房电梯厅)]
    6       [(公共部分, 旅馆大堂区), (旅馆大堂区, 客房货梯厅)]
    7     [(公共部分, 旅馆大堂区), (旅馆大堂区, 大堂公共卫生间)]
    8             [(公共部分, 餐饮区), (餐饮区, 中餐厅)]
    9             [(公共部分, 餐饮区), (餐饮区, 西餐厅)]
    10        [(公共部分, 餐饮区), (餐饮区, 餐厅公共卫生间)]
    11        [(公共部分, 健身娱乐区), (健身娱乐区, 休息厅)]
    12        [(公共部分, 健身娱乐区), (健身娱乐区, 健身房)]
    13        [(公共部分, 健身娱乐区), (健身娱乐区, 台球室)]
    14       [(辅助部分, 厨房共用区), (厨房共用区, 货物门厅)]
    15        [(辅助部分, 厨房共用区), (厨房共用区, 收验间)]
    16      [(辅助部分, 厨房共用区), (厨房共用区, 垃圾电梯厅)]
    17        [(辅助部分, 厨房共用区), (厨房共用区, 垃圾间)]
    18       [(辅助部分, 厨房共用区), (厨房共用区, 员工门厅)]
    19      [(辅助部分, 厨房共用区), (厨房共用区, 员工更衣室)]
    20    [(辅助部分, 中餐厨房区), (中餐厨房区, 中餐加工制作间)]
    21      [(辅助部分, 中餐厨房区), (中餐厨房区, 中餐备餐间)]
    22      [(辅助部分, 中餐厨房区), (中餐厨房区, 中餐洗琬间)]
    23       [(辅助部分, 中餐厨房区), (中餐厨房区, 中餐库房)]
    24    [(辅助部分, 西餐厨房区), (西餐厨房区, 西餐加工制作间)]
    25      [(辅助部分, 西餐厨房区), (西餐厨房区, 西餐备餐间)]
    26      [(辅助部分, 西餐厨房区), (西餐厨房区, 西餐洗碗间)]
    27       [(辅助部分, 西餐厨房区), (西餐厨房区, 西餐库房)]
    28             [(其他, 交通), (交通, 走道、楼梯等)]
    Name: edges, dtype: object
    --------------------------------------------------
    0     [(厨房共用区, 中餐厨房区)]
    1     [(厨房共用区, 西餐厨房区)]
    2     [(厨房共用区, 员工出入口)]
    3     [(厨房共用区, 货物出入口)]
    4      [(厨房共用区, 垃圾出口)]
    5          [(大堂, 中餐厅)]
    6          [(大堂, 西餐厅)]
    7          [(大堂, 大堂吧)]
    8        [(大堂, 健身娱乐区)]
    9        [(大堂, 客房电梯厅)]
    10      [(大堂, 次出入口门厅)]
    11      [(中餐厅, 中餐备餐间)]
    12      [(西餐厅, 西餐备餐间)]
    13     [(大堂吧, 次出入口门厅)]
    Name: edges, dtype: object
    


```python
flatten_lst=lambda lst: [m for n_lst in lst for m in flatten_lst(n_lst)] if type(lst) is list else [lst]

nodes_floor_1=flatten_lst([pd.unique(floor_1[nodes_column]).tolist() for nodes_column in floor_1_classi])
nodes_fr_1=flatten_lst([pd.unique(fr_1[nodes_column]).tolist() for nodes_column in fr_1_classi])
nodes=nodes_floor_1+nodes_fr_1

nodes_idx_mapping={v:i for i,v in enumerate(nodes)}
print(nodes)
```

    ['公共部分', '辅助部分', '其他', '旅馆大堂区', '餐饮区', '健身娱乐区', '厨房共用区', '中餐厨房区', '西餐厨房区', '交通', '大堂', '大堂吧', '商店', '商务中心', '次出入口门厅', '客房电梯厅', '客房货梯厅', '大堂公共卫生间', '中餐厅', '西餐厅', '餐厅公共卫生间', '休息厅', '健身房', '台球室', '货物门厅', '收验间', '垃圾电梯厅', '垃圾间', '员工门厅', '员工更衣室', '中餐加工制作间', '中餐备餐间', '中餐洗琬间', '中餐库房', '西餐加工制作间', '西餐备餐间', '西餐洗碗间', '西餐库房', '走道、楼梯等', '厨房共用区', '大堂', '中餐厅', '西餐厅', '大堂吧', '中餐厨房区', '西餐厨房区', '员工出入口', '货物出入口', '垃圾出口', '中餐厅', '西餐厅', '大堂吧', '健身娱乐区', '客房电梯厅', '次出入口门厅', '中餐备餐间', '西餐备餐间']
    


```python
import matplotlib.pyplot as plt
import numpy as np
import random

opacity=0.4
cmap=plt.get_cmap('gist_ncar') #'gnuplot'  'gist_ncar'  'gist_earth'  'gist_stern' 'twilight_shifted'
list_itemReplace=lambda lst,idx,v:[lst[i] if i!=idx else v for i in range(len(lst)) ]
nodes_colors=['rgba'+str(tuple(list_itemReplace([int(j*255) for j in cmap(i)],3,opacity))) for i in np.linspace(0, 1, len(nodes))]
random.shuffle(nodes_colors)
print(nodes_colors)
```

    ['rgba(0, 0, 128, 0.4)', 'rgba(44, 236, 0, 0.4)', 'rgba(199, 27, 255, 0.4)', 'rgba(0, 210, 255, 0.4)', 'rgba(251, 229, 251, 0.4)', 'rgba(255, 213, 2, 0.4)', 'rgba(0, 127, 255, 0.4)', 'rgba(255, 0, 70, 0.4)', 'rgba(236, 255, 15, 0.4)', 'rgba(0, 253, 52, 0.4)', 'rgba(19, 251, 0, 0.4)', 'rgba(255, 87, 2, 0.4)', 'rgba(0, 192, 255, 0.4)', 'rgba(255, 235, 0, 0.4)', 'rgba(173, 255, 47, 0.4)', 'rgba(132, 254, 11, 0.4)', 'rgba(0, 33, 155, 0.4)', 'rgba(103, 212, 0, 0.4)', 'rgba(0, 61, 88, 0.4)', 'rgba(255, 0, 230, 0.4)', 'rgba(121, 241, 0, 0.4)', 'rgba(111, 225, 0, 0.4)', 'rgba(255, 14, 0, 0.4)', 'rgba(0, 250, 241, 0.4)', 'rgba(255, 200, 7, 0.4)', 'rgba(0, 70, 255, 0.4)', 'rgba(254, 247, 254, 0.4)', 'rgba(255, 188, 12, 0.4)', 'rgba(154, 255, 31, 0.4)', 'rgba(247, 206, 248, 0.4)', 'rgba(76, 217, 0, 0.4)', 'rgba(0, 0, 238, 0.4)', 'rgba(0, 250, 176, 0.4)', 'rgba(255, 38, 0, 0.4)', 'rgba(205, 97, 244, 0.4)', 'rgba(241, 164, 243, 0.4)', 'rgba(255, 0, 141, 0.4)', 'rgba(255, 222, 0, 0.4)', 'rgba(255, 245, 0, 0.4)', 'rgba(0, 253, 209, 0.4)', 'rgba(195, 255, 51, 0.4)', 'rgba(255, 57, 0, 0.4)', 'rgba(255, 120, 6, 0.4)', 'rgba(172, 41, 255, 0.4)', 'rgba(234, 10, 255, 0.4)', 'rgba(244, 187, 246, 0.4)', 'rgba(238, 146, 240, 0.4)', 'rgba(229, 121, 239, 0.4)', 'rgba(176, 68, 250, 0.4)', 'rgba(0, 94, 5, 0.4)', 'rgba(0, 65, 43, 0.4)', 'rgba(255, 161, 11, 0.4)', 'rgba(0, 250, 146, 0.4)', 'rgba(0, 251, 93, 0.4)', 'rgba(218, 255, 31, 0.4)', 'rgba(0, 29, 90, 0.4)', 'rgba(0, 232, 255, 0.4)']
    


```python
import itertools

edge_floor_1=pd.unique(list(itertools.chain(*floor_1['edges'].to_list()))).tolist()
edge_fr_1=pd.unique(list(itertools.chain(*fr_1['edges'].to_list()))).tolist()
edges_mapping=edge_floor_1+edge_fr_1
print(edges_mapping)
```

    [('公共部分', '旅馆大堂区'), ('旅馆大堂区', '大堂'), ('旅馆大堂区', '大堂吧'), ('旅馆大堂区', '商店'), ('旅馆大堂区', '商务中心'), ('旅馆大堂区', '次出入口门厅'), ('旅馆大堂区', '客房电梯厅'), ('旅馆大堂区', '客房货梯厅'), ('旅馆大堂区', '大堂公共卫生间'), ('公共部分', '餐饮区'), ('餐饮区', '中餐厅'), ('餐饮区', '西餐厅'), ('餐饮区', '餐厅公共卫生间'), ('公共部分', '健身娱乐区'), ('健身娱乐区', '休息厅'), ('健身娱乐区', '健身房'), ('健身娱乐区', '台球室'), ('辅助部分', '厨房共用区'), ('厨房共用区', '货物门厅'), ('厨房共用区', '收验间'), ('厨房共用区', '垃圾电梯厅'), ('厨房共用区', '垃圾间'), ('厨房共用区', '员工门厅'), ('厨房共用区', '员工更衣室'), ('辅助部分', '中餐厨房区'), ('中餐厨房区', '中餐加工制作间'), ('中餐厨房区', '中餐备餐间'), ('中餐厨房区', '中餐洗琬间'), ('中餐厨房区', '中餐库房'), ('辅助部分', '西餐厨房区'), ('西餐厨房区', '西餐加工制作间'), ('西餐厨房区', '西餐备餐间'), ('西餐厨房区', '西餐洗碗间'), ('西餐厨房区', '西餐库房'), ('其他', '交通'), ('交通', '走道、楼梯等'), ('厨房共用区', '中餐厨房区'), ('厨房共用区', '西餐厨房区'), ('厨房共用区', '员工出入口'), ('厨房共用区', '货物出入口'), ('厨房共用区', '垃圾出口'), ('大堂', '中餐厅'), ('大堂', '西餐厅'), ('大堂', '大堂吧'), ('大堂', '健身娱乐区'), ('大堂', '客房电梯厅'), ('大堂', '次出入口门厅'), ('中餐厅', '中餐备餐间'), ('西餐厅', '西餐备餐间'), ('大堂吧', '次出入口门厅')]
    


```python
edge_source=[nodes_idx_mapping[i[0]] for i in edges_mapping]
edge_target=[nodes_idx_mapping[i[1]] for i in edges_mapping]

link_num=len(edge_source)
#link_value=[1]*link_num #further adjust

opacity_edge=0.2
link_color_rgba=[nodes_colors[src].replace(str(opacity),str(opacity_edge)) for src in edge_target]
link_label=[""]*link_num
```


```python
area_partition=floor_1.groupby('分区名称')['建筑面积'].sum().to_dict()
area_space=floor_1.groupby('空间名称')['建筑面积'].sum().to_dict()
area_room=floor_1.set_index('房间名称').to_dict()['建筑面积']
areas={**area_partition,**area_space,**area_room}

nodes_idx_mapping_exchage=dict((v,k) for k,v in nodes_idx_mapping.items())
link_value=[areas[nodes_idx_mapping_exchage[i]] if nodes_idx_mapping_exchage[i] in list(areas.keys()) else 1 for i in edge_target]
```


```python
import plotly.graph_objects as go

fig = go.Figure(data=[go.Sankey(
    valueformat = ".0f",
    valuesuffix = "TWh",
    # Define nodes
    node = dict(
      pad = 15,
      thickness = 15,
      line = dict(color = "black", width = 0.5),
      label =nodes,
      color =nodes_colors
    ),
    # Add links
    link = dict(
      source =edge_source,
      target =edge_target,
      value=link_value,
      label=link_label,
      color =link_color_rgba
))])


title_text="建筑一层房间网络关系"
font_size=13
height=800
fig.update_layout(title_text=title_text,font_size=font_size,height=height)
fig.show()

html_savePath='./html/floor_1_connection.html' #_m
fig.write_html("%s"%html_savePath)
```

<a href=""><img src="./imgs_pyd/044.png" height="auto" width="auto" title="digit-x"></a>  


### 1.3 [房间关联](https://networkx.org/documentation/stable/auto_examples/algorithms/plot_snap.html#sphx-glr-auto-examples-algorithms-plot-snap-py)+[network](https://networkx.org/)

network -> [NetworkX](https://networkx.org/)


```python
room_conn=pd.read_excel(requirements_fp,sheet_name='room_conn',header=[0],engine='openpyxl')
room_conn
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
      <th>房间名称</th>
      <th>大堂</th>
      <th>大堂吧</th>
      <th>商店</th>
      <th>商务中心</th>
      <th>次出入口门厅</th>
      <th>客房电梯厅</th>
      <th>客房货梯厅</th>
      <th>大堂公共卫生间</th>
      <th>中餐厅</th>
      <th>...</th>
      <th>员工更衣室</th>
      <th>中餐加工制作间</th>
      <th>中餐备餐间</th>
      <th>中餐洗琬间</th>
      <th>中餐库房</th>
      <th>西餐加工制作间</th>
      <th>西餐备餐间</th>
      <th>西餐洗碗间</th>
      <th>西餐库房</th>
      <th>走道、楼梯等</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>大堂</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>10.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>大堂吧</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>商店</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>商务中心</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>次出入口门厅</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>客房电梯厅</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>客房货梯厅</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>大堂公共卫生间</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>中餐厅</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>西餐厅</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>餐厅公共卫生间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>休息厅</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>健身房</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13</th>
      <td>台球室</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>货物门厅</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>15</th>
      <td>收验间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>垃圾电梯厅</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17</th>
      <td>垃圾间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18</th>
      <td>员工门厅</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>员工更衣室</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>中餐加工制作间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>中餐备餐间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>中餐洗琬间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>中餐库房</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>西餐加工制作间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25</th>
      <td>西餐备餐间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>西餐洗碗间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>27</th>
      <td>西餐库房</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28</th>
      <td>走道、楼梯等</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>29 rows × 30 columns</p>
</div>




```python
room_partition=floor_1.groupby('房间名称')['分区名称'].sum().to_dict()
cmap=plt.get_cmap('twilight_shifted') #'gnuplot'  'gist_ncar'  'gist_earth'  'gist_stern' 'twilight_shifted'
partition_list=pd.unique(list(room_partition.values())).tolist()
colors_list=[cmap(i) for i in np.linspace(0, 1, len(partition_list))]
node_colors_rc={k:v for k,v in zip(partition_list,colors_list)}
room_names=room_conn['房间名称'].to_list()
nodes_rc={n:dict(color=node_colors_rc[room_partition[n]],area=area_room[n]) for n in room_names}
print(nodes_rc)
```

    {'大堂': {'color': (0.775907907306857, 0.5355421788246119, 0.42413367909988375, 1.0), 'area': 400}, '大堂吧': {'color': (0.775907907306857, 0.5355421788246119, 0.42413367909988375, 1.0), 'area': 260}, '商店': {'color': (0.775907907306857, 0.5355421788246119, 0.42413367909988375, 1.0), 'area': 90}, '商务中心': {'color': (0.775907907306857, 0.5355421788246119, 0.42413367909988375, 1.0), 'area': 45}, '次出入口门厅': {'color': (0.775907907306857, 0.5355421788246119, 0.42413367909988375, 1.0), 'area': 130}, '客房电梯厅': {'color': (0.775907907306857, 0.5355421788246119, 0.42413367909988375, 1.0), 'area': 70}, '客房货梯厅': {'color': (0.775907907306857, 0.5355421788246119, 0.42413367909988375, 1.0), 'area': 40}, '大堂公共卫生间': {'color': (0.775907907306857, 0.5355421788246119, 0.42413367909988375, 1.0), 'area': 55}, '中餐厅': {'color': (0.36717513650699446, 0.26883085667326956, 0.6495927229789225, 1.0), 'area': 600}, '西餐厅': {'color': (0.36717513650699446, 0.26883085667326956, 0.6495927229789225, 1.0), 'area': 260}, '餐厅公共卫生间': {'color': (0.36717513650699446, 0.26883085667326956, 0.6495927229789225, 1.0), 'area': 85}, '休息厅': {'color': (0.4867359599430568, 0.6341664625110051, 0.7625578008592404, 1.0), 'area': 80}, '健身房': {'color': (0.4867359599430568, 0.6341664625110051, 0.7625578008592404, 1.0), 'area': 260}, '台球室': {'color': (0.4867359599430568, 0.6341664625110051, 0.7625578008592404, 1.0), 'area': 130}, '货物门厅': {'color': (0.8857115512284565, 0.8500218611585632, 0.8857253899008712, 1.0), 'area': 55}, '收验间': {'color': (0.8857115512284565, 0.8500218611585632, 0.8857253899008712, 1.0), 'area': 25}, '垃圾电梯厅': {'color': (0.8857115512284565, 0.8500218611585632, 0.8857253899008712, 1.0), 'area': 20}, '垃圾间': {'color': (0.8857115512284565, 0.8500218611585632, 0.8857253899008712, 1.0), 'area': 15}, '员工门厅': {'color': (0.8857115512284565, 0.8500218611585632, 0.8857253899008712, 1.0), 'area': 30}, '员工更衣室': {'color': (0.8857115512284565, 0.8500218611585632, 0.8857253899008712, 1.0), 'area': 90}, '中餐加工制作间': {'color': (0.18739228342697645, 0.07710209689958833, 0.21618875376309582, 1.0), 'area': 180}, '中餐备餐间': {'color': (0.18739228342697645, 0.07710209689958833, 0.21618875376309582, 1.0), 'area': 40}, '中餐洗琬间': {'color': (0.18739228342697645, 0.07710209689958833, 0.21618875376309582, 1.0), 'area': 30}, '中餐库房': {'color': (0.18739228342697645, 0.07710209689958833, 0.21618875376309582, 1.0), 'area': 80}, '西餐加工制作间': {'color': (0.5566322903496934, 0.17269677158182117, 0.31423043195101424, 1.0), 'area': 120}, '西餐备餐间': {'color': (0.5566322903496934, 0.17269677158182117, 0.31423043195101424, 1.0), 'area': 30}, '西餐洗碗间': {'color': (0.5566322903496934, 0.17269677158182117, 0.31423043195101424, 1.0), 'area': 30}, '西餐库房': {'color': (0.5566322903496934, 0.17269677158182117, 0.31423043195101424, 1.0), 'area': 50}, '走道、楼梯等': {'color': (0.18488035509396164, 0.07942573027972388, 0.21307651648984993, 1.0), 'area': 800}}
    


```python
room_conn['edges']=room_conn.apply(lambda row:[(row['房间名称'],i,row[i]) for i in room_names],axis=1)
room_conn
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
      <th>房间名称</th>
      <th>大堂</th>
      <th>大堂吧</th>
      <th>商店</th>
      <th>商务中心</th>
      <th>次出入口门厅</th>
      <th>客房电梯厅</th>
      <th>客房货梯厅</th>
      <th>大堂公共卫生间</th>
      <th>中餐厅</th>
      <th>...</th>
      <th>中餐加工制作间</th>
      <th>中餐备餐间</th>
      <th>中餐洗琬间</th>
      <th>中餐库房</th>
      <th>西餐加工制作间</th>
      <th>西餐备餐间</th>
      <th>西餐洗碗间</th>
      <th>西餐库房</th>
      <th>走道、楼梯等</th>
      <th>edges</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>大堂</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>10.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(大堂, 大堂, nan), (大堂, 大堂吧, 10.0), (大堂, 商店, 1.0)...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>大堂吧</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(大堂吧, 大堂, 1.0), (大堂吧, 大堂吧, nan), (大堂吧, 商店, 1....</td>
    </tr>
    <tr>
      <th>2</th>
      <td>商店</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(商店, 大堂, 1.0), (商店, 大堂吧, 1.0), (商店, 商店, nan),...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>商务中心</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(商务中心, 大堂, 1.0), (商务中心, 大堂吧, 1.0), (商务中心, 商店,...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>次出入口门厅</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(次出入口门厅, 大堂, 1.0), (次出入口门厅, 大堂吧, 1.0), (次出入口门...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>客房电梯厅</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(客房电梯厅, 大堂, 1.0), (客房电梯厅, 大堂吧, 1.0), (客房电梯厅, ...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>客房货梯厅</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(客房货梯厅, 大堂, 1.0), (客房货梯厅, 大堂吧, 1.0), (客房货梯厅, ...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>大堂公共卫生间</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(大堂公共卫生间, 大堂, 1.0), (大堂公共卫生间, 大堂吧, 1.0), (大堂公...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>中餐厅</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(中餐厅, 大堂, nan), (中餐厅, 大堂吧, nan), (中餐厅, 商店, na...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>西餐厅</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(西餐厅, 大堂, nan), (西餐厅, 大堂吧, nan), (西餐厅, 商店, na...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>餐厅公共卫生间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(餐厅公共卫生间, 大堂, nan), (餐厅公共卫生间, 大堂吧, nan), (餐厅公...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>休息厅</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(休息厅, 大堂, nan), (休息厅, 大堂吧, nan), (休息厅, 商店, na...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>健身房</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(健身房, 大堂, nan), (健身房, 大堂吧, nan), (健身房, 商店, na...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>台球室</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(台球室, 大堂, nan), (台球室, 大堂吧, nan), (台球室, 商店, na...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>货物门厅</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(货物门厅, 大堂, nan), (货物门厅, 大堂吧, nan), (货物门厅, 商店,...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>收验间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(收验间, 大堂, nan), (收验间, 大堂吧, nan), (收验间, 商店, na...</td>
    </tr>
    <tr>
      <th>16</th>
      <td>垃圾电梯厅</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(垃圾电梯厅, 大堂, nan), (垃圾电梯厅, 大堂吧, nan), (垃圾电梯厅, ...</td>
    </tr>
    <tr>
      <th>17</th>
      <td>垃圾间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(垃圾间, 大堂, nan), (垃圾间, 大堂吧, nan), (垃圾间, 商店, na...</td>
    </tr>
    <tr>
      <th>18</th>
      <td>员工门厅</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(员工门厅, 大堂, nan), (员工门厅, 大堂吧, nan), (员工门厅, 商店,...</td>
    </tr>
    <tr>
      <th>19</th>
      <td>员工更衣室</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(员工更衣室, 大堂, nan), (员工更衣室, 大堂吧, nan), (员工更衣室, ...</td>
    </tr>
    <tr>
      <th>20</th>
      <td>中餐加工制作间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(中餐加工制作间, 大堂, nan), (中餐加工制作间, 大堂吧, nan), (中餐加...</td>
    </tr>
    <tr>
      <th>21</th>
      <td>中餐备餐间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(中餐备餐间, 大堂, nan), (中餐备餐间, 大堂吧, nan), (中餐备餐间, ...</td>
    </tr>
    <tr>
      <th>22</th>
      <td>中餐洗琬间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(中餐洗琬间, 大堂, nan), (中餐洗琬间, 大堂吧, nan), (中餐洗琬间, ...</td>
    </tr>
    <tr>
      <th>23</th>
      <td>中餐库房</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(中餐库房, 大堂, nan), (中餐库房, 大堂吧, nan), (中餐库房, 商店,...</td>
    </tr>
    <tr>
      <th>24</th>
      <td>西餐加工制作间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>[(西餐加工制作间, 大堂, nan), (西餐加工制作间, 大堂吧, nan), (西餐加...</td>
    </tr>
    <tr>
      <th>25</th>
      <td>西餐备餐间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>[(西餐备餐间, 大堂, nan), (西餐备餐间, 大堂吧, nan), (西餐备餐间, ...</td>
    </tr>
    <tr>
      <th>26</th>
      <td>西餐洗碗间</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>[(西餐洗碗间, 大堂, nan), (西餐洗碗间, 大堂吧, nan), (西餐洗碗间, ...</td>
    </tr>
    <tr>
      <th>27</th>
      <td>西餐库房</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(西餐库房, 大堂, nan), (西餐库房, 大堂吧, nan), (西餐库房, 商店,...</td>
    </tr>
    <tr>
      <th>28</th>
      <td>走道、楼梯等</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>[(走道、楼梯等, 大堂, nan), (走道、楼梯等, 大堂吧, nan), (走道、楼梯...</td>
    </tr>
  </tbody>
</table>
<p>29 rows × 31 columns</p>
</div>




```python
edges_rc=pd.unique(list(itertools.chain(*room_conn['edges'].to_list()))).tolist()
edges_rc=[i for i in edges_rc if ~np.isnan(i[2])]
print(edges_rc[:10])
```

    [('大堂', '大堂吧', 10.0), ('大堂', '商店', 1.0), ('大堂', '商务中心', 1.0), ('大堂', '次出入口门厅', 10.0), ('大堂', '客房电梯厅', 10.0), ('大堂', '客房货梯厅', 1.0), ('大堂', '大堂公共卫生间', 1.0), ('大堂', '中餐厅', 10.0), ('大堂', '西餐厅', 10.0), ('大堂', '休息厅', 10.0)]
    


```python
import networkx as nx

G_room_conn=nx.Graph()
G_room_conn.add_nodes_from(n for n in nodes_rc.items())
G_room_conn.add_weighted_edges_from((u, v, weight) for u, v, weight in edges_rc)
```


```python
#解决中文显示问题
plt.rcParams['font.sans-serif'] = ['DengXian'] # 指定默认字体 'KaiTi','SimHei'
plt.rcParams['axes.unicode_minus'] = False # 解决保存图像是负号'-'显示为方块的问题

fig=plt.figure(figsize=(40,20))

plt.suptitle("房间关联")
base_options=dict(with_labels=True, edgecolors="black", node_size=[i*10 for i in list(nx.get_node_attributes(G_room_conn,'area').values())]) #1500
ax1=fig.add_subplot(1, 2, 1)
plt.title(
    "Original (%s nodes, %s edges)"
    % (G_room_conn.number_of_nodes(), G_room_conn.number_of_edges())
)
pos=nx.spring_layout(G_room_conn, k=1) #seed=7482934
node_colors=[d["color"] for _, d in G_room_conn.nodes(data=True)]
edge_weights=[d["weight"] for _, _, d in G_room_conn.edges(data=True)] 

nx.draw_networkx(G_room_conn, pos=pos, node_color=node_colors, width=edge_weights, **base_options)
node_attributes=("color",)
edge_attributes=("weight",)

summary_graph=nx.snap_aggregation(G_room_conn, node_attributes, edge_attributes,prefix="S-")

for k,v in node_colors_rc.items():
    plt.scatter([],[], c=[v], label=k)

plt.legend()
plt.show()
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_33392/1219552898.py in <module>
         20 edge_attributes=("weight",)
         21 
    ---> 22 summary_graph=nx.snap_aggregation(G_room_conn, node_attributes, edge_attributes,prefix="S-")
         23 
         24 for k,v in node_colors_rc.items():
    

    AttributeError: module 'networkx' has no attribute 'snap_aggregation'



    
<a href=""><img src="./imgs_pyd/045.png" height="auto" width="auto" title="digit-x"></a>  
    


## 2. [agent-based modeling](https://mesa.readthedocs.io/en/stable/#)

[mesa GitHub](https://github.com/projectmesa/mesa)

[NetLogo](https://ccl.northwestern.edu/netlogo/)


### 2.1 开始实验-逐步尝试


```python
from mesa import Agent, Model
from mesa.time import RandomActivation
from mesa.space import MultiGrid

class roomAgent(Agent):
    """An agent with fixed initial wealth."""
    def __init__(self, unique_id,area,conn,partition, model):
        super().__init__(unique_id, model) #area,conn,
        self.unique_id=unique_id
        self.area=area
        self.conn=conn
        self.partition=partition
        
    def step(self):
        # The agent's step will go here.
        pass
        

class roomModel(Model):
    """A model with some number of agents."""
    def __init__(self, N,room_area_dict,room_conn_dict,room_partition,width, height):
        self.num_agents=N
        self.room_area_dict=room_area_dict
        self.room_conn_dict=room_conn_dict        
        self.room_names=list(room_area_dict.keys())
        
        self.grid = MultiGrid(width, height, True)
        self.schedule=RandomActivation(self)
        # Create agents
        for rn in self.room_names:
            unique_id=rn
            area=self.room_area_dict[rn]
            conn=self.room_conn_dict[rn]
            partition=room_partition[rn]
            a=roomAgent(unique_id,area,conn,partition,self)
            self.schedule.add(a)
            
            # Add the agent to a random grid cell
            x=self.random.randrange(self.grid.width)
            y=self.random.randrange(self.grid.height)
            self.grid.place_agent(a, (x, y))
            
    def step(self):
        '''Advance the model by one step.'''
        self.schedule.step()          
```


```python
room_conn_dict=room_conn[room_names].set_index([room_names]).to_dict()
room_area_dict=area_room
#print(room_conn_dict)
N=len(room_area_dict)
```


```python
room_model=roomModel(N,room_area_dict,room_conn_dict,room_partition,10,10)
room_model.schedule.agents[0].partition
```




    '旅馆大堂区'




```python
from mesa.visualization.modules import CanvasGrid
from mesa.visualization.ModularVisualization import ModularServer

def agent_portrayal(agent):
    portrayal={"Shape": "circle",
               "Filled": "true",
                "Layer": 0,
                "Color": "red",
                "r": 0.5}
    return portrayal

grid=CanvasGrid(agent_portrayal, 50, 50, 1000, 1000)
server=ModularServer(roomModel,
                    [grid],
                    "rooms Model",
                    {"N":N,"room_area_dict":room_area_dict,"room_conn_dict":room_area_dict,"room_partition":room_partition,"width":50, "height":50})
server.port=8521 # The default
server.launch()
```

    Interface starting at http://127.0.0.1:8521
    


    ---------------------------------------------------------------------------

    RuntimeError                              Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_33392/1276791346.py in <module>
         16                     {"N":N,"room_area_dict":room_area_dict,"room_conn_dict":room_area_dict,"room_partition":room_partition,"width":50, "height":50})
         17 server.port=8521 # The default
    ---> 18 server.launch()
    

    ~\anaconda3\envs\py_code_arla\lib\site-packages\mesa\visualization\ModularVisualization.py in launch(self, port, open_browser)
        335             webbrowser.open(url)
        336         tornado.autoreload.start()
    --> 337         tornado.ioloop.IOLoop.current().start()
    

    ~\anaconda3\envs\py_code_arla\lib\site-packages\tornado\platform\asyncio.py in start(self)
        197             self._setup_logging()
        198             asyncio.set_event_loop(self.asyncio_loop)
    --> 199             self.asyncio_loop.run_forever()
        200         finally:
        201             asyncio.set_event_loop(old_loop)
    

    ~\anaconda3\envs\py_code_arla\lib\asyncio\base_events.py in run_forever(self)
        558         """Run until stop() is called."""
        559         self._check_closed()
    --> 560         self._check_running()
        561         self._set_coroutine_origin_tracking(self._debug)
        562         self._thread_id = threading.get_ident()
    

    ~\anaconda3\envs\py_code_arla\lib\asyncio\base_events.py in _check_running(self)
        550     def _check_running(self):
        551         if self.is_running():
    --> 552             raise RuntimeError('This event loop is already running')
        553         if events._get_running_loop() is not None:
        554             raise RuntimeError(
    

    RuntimeError: This event loop is already running


    Socket opened!
    {"type":"reset"}
    {"type":"get_step","step":1}
    

> Jupyter比较适合于代码笔记、解释库功用和教学等途径。实际应用的代码是需要在Spyder等解释器中完成，以.py形式保存运行。

### 2.2 建筑功能空间布局的代理模型-agent-based modeling，还未完成的代码

<a href=""><img src="./imgs_pyd/043.gif" height="auto" width="auto" title="digit-x"></a>

**agents.py**


```python
# -*- coding: utf-8 -*-
"""
Created on Sat Oct  2 12:38:47 2021

@author: Richie Bao-python_code_archi_la_design_method_study 西安建筑科技大学-规划/建筑/景观本科数字化系列课程
"""
from mesa import Agent
import math

def get_distance(pos_1, pos_2):
    """Get the distance between two point

    Args:
        pos_1, pos_2: Coordinate tuples for both points.

    """
    x1, y1 = pos_1
    x2, y2 = pos_2
    dx = x1 - x2
    dy = y1 - y2
    return math.sqrt(dx ** 2 + dy ** 2)

flatten_lst=lambda lst: [m for n_lst in lst for m in flatten_lst(n_lst)] if type(lst) is list else [lst]

class roomAgent(Agent):
    """An agent with fixed initial wealth."""
    def __init__(self,pos, unique_id,area,conn,partition, vision,periphery_RAgents_list,model):
        super().__init__(unique_id, model) #area,conn,
        self.pos=pos
        self.unique_id=unique_id
        self.area=area
        self.conn=conn
        self.partition=partition
        self.vision=vision
        self.periphery_RAgents_list=periphery_RAgents_list
        
    def who_occupied(self,pos,agent_type,target=None):
        this_cell=self.model.grid.get_cell_list_contents([pos])
        if target:
            RAgents_lst=[i for i in this_cell if type(i) is agent_type and i.unique_id.split("_")[0]==target]
        else:
            RAgents_lst=[i for i in this_cell if type(i) is agent_type]
        # RAgents_lst=[i.unique_id for i in this_cell if type(i) is RAgents]

        return RAgents_lst
    
    def move_around(self):
        self.vision=self.random.randrange(1, 6)  
        self.random.shuffle(self.neighbors)
        self.model.grid.move_agent(self, self.neighbors[0])
                        
        
    def find_fellows(self):
        self.neighbors=[i for i in self.model.grid.get_neighborhood(self.pos, moore=True,include_center=False,radius=self.vision)]
        # print(neighbors)
        self.periphery_RAgents_list=flatten_lst([self.who_occupied(i,RAgents,self.unique_id) for i in self.neighbors])
        # print(self.unique_id)
        # print(periphery_RAgents_list)
        # print("+"*50)         
        # return neighbors,periphery_RAgents_list  
        
    
    def RAgent_gathering(self): #,periphery_RAgents_list
        # print(periphery_RAgents_list)
        # print([i.conn for i in periphery_RAgents_list if i is not None])
        # print("_"*50)
        for i in self.periphery_RAgents_list:
            if i is not None:
                RA_vision=self.random.randrange(1, 6*2)
                RA_neighbors=[i for i in self.model.grid.get_neighborhood(i.pos, moore=True,include_center=False,radius=RA_vision)]
                for neighborhood_loc in RA_neighbors:
                    conn_targets=self.who_occupied(neighborhood_loc,RAgents)
                    # print(conn_targets)
                    # print("*"*50)
                    for conn_target in conn_targets:
                        conn_val=i.conn[conn_target.unique_id.split("_")[0]]
                        # print(i.conn)    
                        # print(conn_target.unique_id.split("_")[0])
                        # print(conn_val)
                        # print("*"*50)
                        if not math.isnan(conn_val):
                            # print(type(conn_val))
                            # print("!!!!!!!!!!!")
                            x_,y_=conn_target.pos[0]-i.pos[0],conn_target.pos[1]-i.pos[1]
                            if x_>1:delta_x=1 
                            elif x_<-1: delta_x=-1
                            else:delta_x=0
                            if y_>1:delta_y=1
                            elif y_<-1:delta_y=-1
                            else:delta_y=0
                            delta_x=delta_x*conn_val
                            delta_y=delta_y*conn_val
                            # new_pos=(int(i.pos[0]+delta_x),int(i.pos[1]+delta_y))
                            
                            n_x=i.pos[0]+delta_x
                            n_y=i.pos[1]+delta_y
                            if n_x>90:n_x=90
                            if n_y>60:n_y=60
                            new_pos=(int(n_x),int(n_y))
                            
                            # print(new_pos)
                            # print("—"*50)
                            if not self.who_occupied(new_pos,RAgents,i.unique_id):  
                                self.model.grid.move_agent(i, new_pos) 
         
       
    def step(self):
        # The agent's step will go here.
        self.find_fellows() #neighbors,periphery_RAgents_list=
        self.move_around() #self.neighbors
        # self.RAgent_gathering() #self.periphery_RAgents_list
    

class conAgent(Agent):
    def __init__(self,pos,model,con_):
        super().__init__(pos,model)
        self.con=con_
    def step(self):
        # The agent's step will go here.
        pass        
    
class RAgents(Agent):
    def __init__(self,pos,unique_id,area,conn,partition, model):
        super().__init__(unique_id, model) 
        self.pos=pos
        self.unique_id=unique_id
        self.area=area
        self.conn=conn
        self.partition=partition   
        self.model=model 
   
    def who_occupied(self,pos,agent_type,target=None):
        this_cell=self.model.grid.get_cell_list_contents([pos])
        if target:
            RAgents_lst=[i for i in this_cell if type(i) is agent_type and i.unique_id.split("_")[0]==target]
        else:
            RAgents_lst=[i for i in this_cell if type(i) is agent_type]
        return RAgents_lst    

    def RAgent_gathering(self): #,periphery_RAgents_list
        RA_vision=self.random.randrange(1, 6*2)
        RA_neighbors=[i for i in self.model.grid.get_neighborhood(self.pos, moore=True,include_center=False,radius=RA_vision)]

        for neighborhood_loc in RA_neighbors:
            conn_targets=self.who_occupied(neighborhood_loc,RAgents)
            # print(conn_targets)
            # print("*"*50)
            for conn_target in conn_targets:
                conn_val=self.conn[conn_target.unique_id.split("_")[0]]
                # print(i.conn)    
                # print(conn_target.unique_id.split("_")[0])
                # print(conn_val)
                # print("*"*50)
                if not math.isnan(conn_val):
                    # print(type(conn_val))
                    # print("!!!!!!!!!!!")
                    x_,y_=conn_target.pos[0]-self.pos[0],conn_target.pos[1]-self.pos[1]
                    if x_>1:delta_x=1 
                    elif x_<-1: delta_x=-1
                    else:delta_x=0
                    if y_>1:delta_y=1
                    elif y_<-1:delta_y=-1
                    else:delta_y=0
                    delta_x=delta_x*conn_val
                    delta_y=delta_y*conn_val
                    # new_pos=(int(i.pos[0]+delta_x),int(i.pos[1]+delta_y))
                    
                    n_x=self.pos[0]+delta_x
                    n_y=self.pos[1]+delta_y
                    if n_x>90:n_x=90
                    if n_y>60:n_y=60
                    new_pos=(int(n_x),int(n_y))
                    
                    # print(new_pos)
                    # print("—"*50)
                    if not self.who_occupied(new_pos,RAgents,self.unique_id):  
                        # print("!!!")
                        self.model.grid.move_agent(self, new_pos)     
      
        
    def step(self):
        self.RAgent_gathering()
        
        '''
        i=0
        # self.RAgent_gathering()
        # width=self.model.grid.width
        # height=self.model.grid.height    
        # agent=self.model.grid.iter_cell_list_contents()
        for coordi in self.model.grid.coord_iter():
            agents_list=coordi[0]
            room_agent=[i for i in agents_list if type(i) is roomAgent]
            if len(room_agent)>0:
                self.periphery_RAgents_list=room_agent[0].periphery_RAgents_list 
                if self.periphery_RAgents_list is not None:
                    if len(self.periphery_RAgents_list)>0:
                        # print(self.periphery_RAgents_list)
                        self.RAgent_gathering()
                        print(i)
                        i+=1
        '''
        
```

**model.py**


```python
# -*- coding: utf-8 -*-
"""
Created on Sat Oct  2 12:44:19 2021

@author: Richie Bao-python_code_archi_la_design_method_study 西安建筑科技大学-规划/建筑/景观本科数字化系列课程
"""
from agents import roomAgent, conAgent,RAgents
from mesa import Model
from mesa.space import MultiGrid
from mesa.time import RandomActivation
import numpy as np
# from tqdm import tqdm

class roomModel(Model):
    """A model with some number of agents."""
    verbose=True  # Print-monitoring
    def __init__(self, N,room_area_dict,room_conn_dict,room_partition,width, height):
        self.num_agents=N
        self.room_area_dict=room_area_dict
        self.room_conn_dict=room_conn_dict        
        self.room_names=list(room_area_dict.keys())
        
        self.height=height
        self.width=width        
        
        self.grid=MultiGrid( self.height, self.width,torus=True) #torus=False
        self.schedule=RandomActivation(self)
        
        #create conAgent /condition of env
        condition_layout=np.genfromtxt("./mesa_condition.txt").T
        for _, x, y in self.grid.coord_iter():
            #print(x,y)
            con_=condition_layout[x,y]
            con_dis=conAgent((x,y),self,con_)
            self.grid.place_agent(con_dis, (x,y))
            self.schedule.add(con_dis)
        
        # Create roomAgent
        for rn in self.room_names:
            unique_id=rn
            area=self.room_area_dict[rn]
            conn=self.room_conn_dict[rn]
            partition=room_partition[rn]                       
            
            # Add the agent to a random grid cell
            x=self.random.randrange(self.grid.width)
            y=self.random.randrange(self.grid.height)
            
            vision=self.random.randrange(1, 6)
            periphery_RAgents_list=None
            a=roomAgent((x,y),unique_id,area,conn,partition,vision,periphery_RAgents_list,self) #periphery_RAgents_list,
            self.grid.place_agent(a, (x, y))               
            
            self.schedule.add(a)
            
        # create RAgents
        for rn in self.room_names:
            area=self.room_area_dict[rn]
            conn=self.room_conn_dict[rn]
            partition=room_partition[rn]
            for a in range(area):
                unique_id_RAs="{}_{}".format(rn,a)
                # print(unique_id_RAs)                
                                
                x=self.random.randrange(self.grid.width)
                y=self.random.randrange(self.grid.height)
                
                RA=RAgents((x,y),unique_id_RAs,area,conn,partition,self)
                self.grid.place_agent(RA, (x, y))
            
                self.schedule.add(RA)
            
        self.running=True    
            
    def step(self):
        '''Advance the model by one step.'''
        self.schedule.step() 

```

**server.py**


```python
# -*- coding: utf-8 -*-
"""
Created on Sat Oct  2 12:45:03 2021

@author: Richie Bao-python_code_archi_la_design_method_study 西安建筑科技大学-规划/建筑/景观本科数字化系列课程
"""
from model import roomModel

from mesa.visualization.modules import CanvasGrid
from mesa.visualization.ModularVisualization import ModularServer

#data preprocessing
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib import colors
import numpy as np


from agents import roomAgent, conAgent,RAgents


requirements_fp=r'./data/2017_exam_paper_network.xlsx'
floor_1=pd.read_excel(requirements_fp,sheet_name='floor_1',header=[1],engine='openpyxl')
floor_1=floor_1[floor_1.filter(regex='^(?!Unnamed)').columns]
floor_1_ffill_columns=['空间名称', '分区名称']
floor_1[floor_1_ffill_columns]=floor_1[floor_1_ffill_columns].fillna(method='ffill')
area_room=floor_1.set_index('房间名称').to_dict()['建筑面积']
room_conn=pd.read_excel(requirements_fp,sheet_name='room_conn',header=[0],engine='openpyxl')
room_partition=floor_1.groupby('房间名称')['分区名称'].sum().to_dict()
room_names=room_conn['房间名称'].to_list()
room_conn['edges']=room_conn.apply(lambda row:[(row['房间名称'],i,row[i]) for i in room_names],axis=1)

room_conn_dict=room_conn[room_names].set_index([room_names]).to_dict()
room_area_dict=area_room
#print(room_conn_dict)
N=len(room_area_dict)

condition_layout=np.genfromtxt("./mesa_condition.txt")
condition_layout_shape=condition_layout.shape
print("condition_layout_shape={}".format(condition_layout_shape))

condition_layout_unique=np.unique(condition_layout)
print("condition_layout_unique={}".format(condition_layout_unique))

cmap=plt.get_cmap('gist_ncar') #'gnuplot'  'gist_ncar'  'gist_earth'  'gist_stern' 'twilight_shifted'
colors_con_list=[cmap(i) for i in np.linspace(0, 1, len(condition_layout_unique))]
colors_con_list=[colors.to_hex(c) for c in colors_con_list]

colors_con_dic={k:v for k,v in zip(condition_layout_unique,colors_con_list)}

room_partition_unique=np.unique(list(room_partition.values()))
colors_room_list=[cmap(i) for i in np.linspace(0, 1, len(room_partition_unique))]
colors_room_list=[colors.to_hex(c) for c in colors_room_list]
colors_room_dic={k:v for k,v in zip(room_partition_unique,colors_room_list)}

def agent_portrayal(agent):    
    if agent is None:
        return    
    portrayal={}
    if type(agent) is roomAgent:
        # portrayal["Shape"] = "resources/ant.png"
        portrayal["Shape"]="circle"
        portrayal["Filled"]="true"
        # print(agent.partition)
        portrayal["Color"]=colors_room_dic[agent.partition]
        portrayal["r"]=1
        portrayal["scale"] = 0.9
        portrayal["Layer"] = 2   
        portrayal["text"]=agent.unique_id
        
    elif type(agent) is conAgent:
        # if agent.con != 0:
        #print(colors_con_dic[agent.con])
        portrayal["Color"]=colors_con_dic[agent.con]
        # else:
            # portrayal["Color"]="#D6F5D6"
        portrayal["Shape"]="rect"
        portrayal["Filled"]="true"
        portrayal["Layer"]=0
        portrayal["w"]=1
        portrayal["h"]=1
        
    elif type(agent) is RAgents:
        # print(agent.partition.split("_")[0])
        # if agent.partition.split("_")[0]=="旅馆大堂区":
            # print("+"*50)
            # portrayal["Shape"]="rect"
            # portrayal["Filled"]="true"

        portrayal["Shape"]="circle"
        portrayal["Color"]=colors_room_dic[agent.partition]
        # portrayal["Filled"]="true"
        portrayal["r"]=1
        portrayal["scale"] = 0.9
        portrayal["Layer"] = 1   
        # portrayal["text"]=agent.unique_id        
        
    return portrayal

canvas_element=CanvasGrid(agent_portrayal, 98, 69, 980,690, )
server=ModularServer(roomModel,
                    [canvas_element],
                    "rooms Model",
                    {"N":N,"room_area_dict":room_area_dict,"room_conn_dict":room_conn_dict,"room_partition":room_partition,"width":69, "height":98})
server.port=8521 # The default
server.launch()
```

<a href=""><img src="./icon/ant_04.jpg" height="auto" width="200" title="caDesign"></a>
