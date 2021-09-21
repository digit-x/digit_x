> Created on Tue Sep 21 03\41\46 2021 @author: Richie Bao-parameterize_archi_la_design_and_data_analysis 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# 建筑物理环境——生态分析

<img src="./imgs_parae/248.jpg" height="auto" width="auto"  title="digit-x">

## 1. 气象数据(逐时气象数据)

### 1.1 获取，读取与绘制查看逐时气象数据

<a href="https://energyplus.net/"><img src="./imgs_parae/203.png" height="auto" width="auto"  title="digit-x"></a>

[获取气象数据](https://energyplus.net/weather)

<img src="./imgs_parae/qrcode_energyplus.net.png" height="auto" width="300"  title="digit-x">

**CHN_Shaanxi.Xian.570360_CSWD.epw**

<img src="./imgs_parae/205.png" height="auto" width="auto"  title="digit-x">

* .epw 气象文件

* .ddy 区域设计条件文件

* .stat 数据摘要

.epw文件内容
```
LOCATION,Xian,Shaanxi,CHN,CSWD,570360,34.30,108.93,8.0,397.5
DESIGN CONDITIONS,1,Climate Design Data 2009 ASHRAE Handbook,,Heating,1,-6.3,-4.8,-18.1,0.8,1.6,-15.3,1,1.7,7.3,3,6.3,1.8,1.1,70,Cooling,7,9.2,35.9,23.3,34.4,23.1,33,22.9,26.4,32,25.5,30.9,24.8,30,2.9,70,25,21,29.9,24,19.9,28.9,23.2,18.9,28.1,85.5,32,81.5,31.1,78.2,30.1,730,Extremes,7.8,6.5,5.4,29.7,-9.9,38.4,2.5,1.6,-11.7,39.6,-13.2,40.6,-14.6,41.5,-16.5,42.6
TYPICAL/EXTREME PERIODS,6,Summer - Week Nearest Max Temperature For Period,Extreme,7/27,8/ 2,Summer - Week Nearest Average Temperature For Period,Typical,7/13,7/19,Winter - Week Nearest Min Temperature For Period,Extreme,1/ 6,1/12,Winter - Week Nearest Average Temperature For Period,Typical,12/22,1/ 5,Autumn - Week Nearest Average Temperature For Period,Typical,10/ 6,10/12,Spring - Week Nearest Average Temperature For Period,Typical,4/19,4/25
GROUND TEMPERATURES,3,.5,,,,2.95,1.86,3.80,6.83,14.77,20.89,25.02,26.28,24.14,19.38,13.03,7.13,2,,,,6.97,5.03,5.42,7.04,12.35,17.15,20.98,23.04,22.61,19.91,15.53,10.89,4,,,,10.30,8.31,7.87,8.47,11.48,14.71,17.68,19.74,20.22,19.05,16.48,13.34
HOLIDAYS/DAYLIGHT SAVINGS,No,0,0,0
COMMENTS 1,Custom/User Format -- WMO#570360; China Standard Weather Data for Analyzing Building Thermal Conditions May 2010; China Meteorological Bureau-Climate Information Center-Climate Data Office and Tsinghua University-Department of Building Science and Technology
COMMENTS 2, -- Ground temps produced with a standard soil diffusivity of 2.3225760E-03 {m**2/day}
DATA PERIODS,1,1,Data,Sunday, 1/ 1,12/31
```

<img src="./imgs_parae/206.png" height="auto" width="auto"  title="digit-x">

使用[ladybug](https://www.food4rhino.com/en/app/ladybug-tools)读取与绘制逐时气象数据

|   |   |
|---|---|
| <img src="./imgs_parae/208.jpg" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/207.png" height="auto" width="auto"  title="digit-x">  |

**《中国建筑热环境分析专用气象数据集》**

<img src="./imgs_parae/209.png" height="auto" width="auto"  title="digit-x">

### 1.2 气象数据可视化与相关分析-Ladybug 

|   |   |
|---|---|
| <a href="https://www.ladybug.tools/"><img src="./imgs_parae/ladybug-large.png" height="auto" width="200"  title="digit-x"></a> |  <img src="./imgs_parae/210.jpg" height="auto" width="auto"  title="digit-x">  |

#### 1.2.1 日晷/sunpath+数据

|   |   |
|---|---|
| <img src="./imgs_parae/211.jpg" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/212.jpg" height="auto" width="auto"  title="digit-x">  |
|<img src="./imgs_parae/215.jpg" height="auto" width="auto"  title="digit-x"> |<img src="./imgs_parae/216.jpg" height="auto" width="auto"  title="digit-x"> |
|<img src="./imgs_parae/217.jpg" height="auto" width="auto"  title="digit-x"> |<img src="./imgs_parae/218.jpg" height="auto" width="auto"  title="digit-x"> |

| wind rose  |   |
|---|---|
| <img src="./imgs_parae/233.jpg" height="auto" width="auto"  title="digit-x">   |  <img src="./imgs_parae/232.jpg" height="auto" width="auto"  title="digit-x">  |

#### 1.2.2 日照分析/Direct sun hours(小时数)

|   |   |
|---|---|
| <img src="./imgs_parae/213.jpg" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/214.jpg" height="auto" width="auto"  title="digit-x">  |

#### 1.2.3 年舒适性>日晷

**焓湿图**

|   |   |
|---|---|
| <img src="./imgs_parae/221.jpg" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/219.jpg" height="auto" width="auto"  title="digit-x">  |

<img src="./imgs_parae/220.jpg" height="auto" width="auto"  title="digit-x"> 

* 影响人体热舒适性的因素/个体差异

热舒适的瞬感现象； 服装调节；性别差异； 个体状况； 适宜性差异； 民族差异； 年龄差异； 恒定与变化

* 相关指标判定

**风速**

|   |   |
|---|---|
| <img src="./imgs_parae/222.png" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/223.png" height="auto" width="auto"  title="digit-x">  |

**代谢率**

|   |   |   |
|---|---|---|
|   <img src="./imgs_parae/224.png" height="auto" width="auto"  title="digit-x">  |   <img src="./imgs_parae/225.png" height="auto" width="auto"  title="digit-x">  |   <img src="./imgs_parae/226.png" height="auto" width="auto"  title="digit-x">  |

**穿衣指数**

<img src="./imgs_parae/227.png" height="auto" width="600"  title="digit-x"> 

* 被动式策略分析

<img src="./imgs_parae/228.png" height="auto" width="500"  title="digit-x"> 

#### 1.2.4 室外热舒适度/通用热气候指标UTCI

* 热舒适指标+MRT与舒适度分析

PMV-Predicted Mean Vote-预测平均投票; PET-Physiological Equivalent Temperature-生理学等价温度; UTCI-Universal Thermal Climate Index-通用热气候指标

|   |   |
|---|---|
| <img src="./imgs_parae/230.jpg" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/229.jpg" height="auto" width="auto"  title="digit-x">  |

UTCI Assessment Scale: UTCI  categorized in terms of thermal stress

<img src="./imgs_parae/231.png" height="auto" width="200"  title="digit-x">

* 影响人体热舒适性的因素-环境物理状况

室内气温: 冬季室内气温一般应在16～22℃；夏季空调房间的气温多规定为24～28℃

室内空气湿度: 一般认为最适宜的相对湿度应为50％～60％。在多数情况下，即气温在16～25℃时，相对湿度在30％～70％范围内变化，对人体的热感觉影响不大

室内风速: 对人体舒适的气流速度应小于0.3 m/s；但在夏季利用自然通风的房间，由于室温较高，舒适的气流速度也应较大；

MRT-Mean Radiant Temperature-平均辐射温度: 如果平均辐射温度（MRT）与气温相差很多的话，就必须考虑MRT的作用。在冬季面南的窗户，因为阳光的辐射导致MRT增高，会使人体感觉到热，即使此时的环境温度在舒适的24℃；当夜晚，情况则相反，会感觉到冷。同时，必须注意到人体皮肤和衣物的平均温度约在29℃左右，这个温度决定了人体与周围环境的辐射交换状态；

> The mean radiant temperature (MRT) is defined as the uniform temperature of an imaginary enclosure in which the radiant heat transfer from the human body is equal to the radiant heat transfer in the actual non-uniform enclosure 假想的等温围合面的表面温度，与人体的辐射热交换量等于人体周围实际的非等温围合面与人体间的辐射热交换量。

#### 1.2.5 实时辐射分析

**太阳辐射**

太阳辐射强度(太阳辐射通量密度）：单位时间内投射到单位面积上的太阳辐射能量。单位：W·m-2；

到达地面的太阳辐射强度:

太阳直接辐射强度：单位时间内以平行光形式投射到地表单位水平面积上的太阳辐射能。

天空散射辐射强度(D)：阳光被大气散射后，单位时间内以散射光形式到达地表单位水平面积上的太阳辐射能。

太阳总辐射强度：到达地面的太阳总辐射强度是太阳直接辐射强度和天空辐射强度的总和。

**影响因子**

•太阳高度角(h)：太阳总辐射与太阳高度呈正相关关系。

•大气透明度(P)：大气透明度差，到达地面的太阳直接辐射减少，故太阳总辐射减少。

•大气质量(m)：大气质量m愈大，到达地面的太阳总辐射愈少。

•纬度：纬度愈高，太阳总辐射愈少。

•海拔：海拔愈高，地面接受的太阳总辐射愈强。

•坡度坡向：北半球北回归线(23.5°N)以北地区，纬度愈高，愈是表现出南坡向阳、北坡背阴，冬季比夏季显著。

•云：一般云愈厚、愈多，太阳直接辐射愈弱，散射辐射的比例增大。

**相关**

•(与植物)植物对不同光照强度的生态适应

•(与建筑围护)太阳辐射可以穿过建筑的围护结构进入到室内，一般认为当围护结构每小时的太阳辐射Solar Radiation低于250w/m2时，没有必要对窗户遮阳，当高于600w/m2时，对非窗体的墙体维护结构遮阳也是必要的，或者增加隔热材料。

•(与光电能)比较建筑不同位置放置集电器所收集太阳能的多少以确定安置位置

|   |   ||
|---|---|---|
| <img src="./imgs_parae/234.jpg" height="auto" width="auto"  title="digit-x">   | <img src="./imgs_parae/235.jpg" height="auto" width="auto"  title="digit-x">   |<img src="./imgs_parae/236.jpg" height="auto" width="auto"  title="digit-x">   |

#### 1.2.6 给定位置计算舒适度- UTCI

<img src="./imgs_parae/237.jpg" height="auto" width="auto"  title="digit-x">  

<img src="./imgs_parae/238.jpg" height="auto" width="500"  title="digit-x">  

* radiation benefit on sunpath

<img src="./imgs_parae/239.jpg" height="auto" width="500"  title="digit-x">  

### 1.3 energy+radiance-Honeybee 

<a href="https://www.ladybug.tools/honeybee.html"><img src="./imgs_parae/honeybee-large.png" height="auto" width="200"  title="digit-x"></a> 


|   |   |
|---|---|
| [Radiance](https://www.radiance-online.org/)  | <a href="https://www.radiance-online.org/"><img src="./imgs_parae/240.png" height="auto" width="auto"  title="digit-x"></a>   |
| [therm finite element simulation](https://windows.lbl.gov/tools/therm/software-download)  |  <a href="https://windows.lbl.gov/tools/therm/software-download"><img src="./imgs_parae/241.png" height="auto" width="auto"  title="digit-x"></a>  |
| [openstudio](https://www.openstudio.net/)  |   <a href="https://www.openstudio.net/"><img src="./imgs_parae/242.png" height="auto" width="auto"  title="digit-x"></a> OpenStudio是由美国能源部可再生能源实验室（NationalRenewableEnergyLaboratory,NREL）领导下开发的集成EnergyPlus和Radiance的建筑能耗模拟软件。OpenStudio使用EnergyPlus模拟建筑的能耗，也可以将OpenStudio视为EnergyPlus的一种可视化用户界面。OpenStudio既能进行能耗分析，也可以进行采光分析。（SketchUp三维的建模；设定建筑的构造和材料；建筑人员、照明和其他负荷；可视化供暖空调通风建模；Radiance自然采光模块；气象参数设定；经济成本分析模块；导入导出模块；数据库；参数分析模块）|

<img src="./imgs_parae/243.jpg" height="auto" width="auto"  title="digit-x">  

**涉及内容**

1. illuminance studies/照度研究
2. annual daylight studies/年度日光研究
3. annual sun exposure(ASE)/年度阳光照射
4. glare analysis/眩光分析
5. advanced solar radiation/高级太阳辐射
6. electric light modeling/电光建模
7. heating energy usage/电热
8. cooling energy  usage/制冷
9. HVAC sizing/暖通空调尺寸
10. color zones w/energy results/能量结果的颜色域
11. energy balance visualization/能量平衡可视化
12. indoor thermal comfort/室内热舒适
13. microclimate mapping/微气候图
14. passive strategies/被动策略
15. active strategies/主动策略
16. energy shade benefit/能量阴影效益
17. envelope heat flow modeling/包络热流建模
18. condensation risk studies/冷凝风险研究

#### 1.3.1 光照模拟 viewbased

> 三维模型有问题，计算结果不准确，待调整。

<img src="./imgs_parae/244.jpg" height="auto" width="auto"  title="digit-x"> 

#### 1.3.2 采光系数 daylight factor
 
> 三维模型有问题，待解决。


* LEED： 领先能源与环境设计（LEED）是美国绿建筑协会在2000年设立的一项绿建筑评分认证系统，用以评估建筑绩效是否能符合永续性。这套标准逐步修正，目前适用版本为2009版本。适用建物类型包含;新建案、既有建筑物、商业建筑内部设计、学校、租屋与住家等。对于新建案（LEEDNC），评分项目包括7大指标;永续性建址（SustainableSite）、用水效率（WaterEfficiency）、能源和大气（EnergyandAtmosphere）、材料和资源（Materials andResources）、室内环境品质（IndoorEnvironmentalQuality）、革新和设计过程（InnovationandDesignProcess）、区域优先性（RegionalPriority）。评分系统中，总分为110分。申请LEED的建筑物，如评分达40-49，则该建筑物被LEED认证级（Certified）；评分达50-59，则该建筑物达到LEED银级认证（Silver）；如评分达60-79，则该建筑物达到LEED金级认证；如评分达80分以上，则该建筑物达到LEED白金级认证（Platinum）。

* 光环境：自然采光是指通过建筑围护结构上的各种孔洞将直射、散射的自然光引入室内为使用者提供照明的过程。具有光效高、显色性好和节能人工光源的自然光，在减少能耗的同时，可以减少由于人工照明引起的空调能耗，对于商业和教育科研建筑，良好的自然光环境设计大约可以减少30%～40%的能耗。

**评估**

|   |   |   
|---|---|
|  <img src="./imgs_parae/245.png" height="auto" width="auto"  title="digit-x">  |  <img src="./imgs_parae/246.png" height="auto" width="auto"  title="digit-x">  |  

<img src="./imgs_parae/247.png" height="auto" width="auto"  title="digit-x">  

### 1.4 生态协同（待）

### 未来潜在探索的方向（敬请关注）
１. 进一步阐述建筑生态的光，热，声，风在参数化环境中协助设计的方法；

２. 建筑生态相关标准写入参数化数据库；

３. 完成生态协同设计部分。