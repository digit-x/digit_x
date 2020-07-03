🐞 图文排版：*赵虎宸* &nbsp;&nbsp; 图文来源：*徐隆双  赵虎宸*&nbsp;&nbsp; 校对：*王垚*&nbsp;&nbsp;📅 May 31, 2019
# Digit-X | 基于无人机-Agisoft-GIS-Rhino-Flowdesign/Phoenics的场地物理环境分析
 数字营造学社 術字怪兽 2019-05-31

大家在课程设计时遇到基地数据不完整难以进行场地分析时是不是非常头疼呢

这篇文章将解决你的问题！

本文承接上期《带上无人机去看世界》将**无人机结合GIS，ladybug，Phoenics等软件**，在实际场地应用中建立一套较完整的无人机数据基地分析工作流。

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/1.gif" height="auto" width="auto"  title="digit-x">

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/2.webp" height="auto" width="auto"  title="digit-x">

本次研究区域是安徽省合肥市矾山镇

我们先来欣赏一下基地的风光吧

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/3.gif" height="auto" width="auto"  title="digit-x">


该项目是西安建筑科技大学风景园林系四年级城市设计课的几个STUDIO之一。要求同学完成**基地总体城市设计框架**，并在此基础上共同完成一套包括**重点地段城市设计、重要建筑或建筑群设计、重点空间节点（或段落）景观设计在内的互为依托、互为支撑、互为补充的综合城市设计方案**。

通过对矾山老街及周边用地保护与更新的城市设计课程训练，让学生认识当前存量规划、旧城更新的、城市建设的必要性及对地域文化、当地居民意愿尊重的重要意义，了解发展与保护的辩证关系，学习在复杂城市环境中进行物质空间环境规划设计的基本原则和手法，掌握基本的城市景观设计要求与方法。

## 场地物理环境分析

### 场地信息

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/4.webp" height="auto" width="auto"  title="digit-x">

矾山镇位于安徽省合肥市庐江县东南部，地处庐江、无为、枞阳三县交界。距县城27公里，距合肥市区70公里。自唐代就因盛产明矾著称于世，因矾得名，因矿成镇，“矾山镇”。

矾山老街系原采矿工人住宿区，与矾矿生产区一河之隔，老街两边至今还保留了一部分明清时期砖石结构的老房子，街道路面全由青石板铺就，每块青石板上都有几道深深的车辙印痕，深处足有3厘米，见证了矾山千年采矾人手推独轮车劳作的艰辛。如今走在老街上，古井、石墙、檐角、石板 ……无一不散发着古老的历史气息，眼前还能感受到当年车水马龙、商贾云集的繁华盛况。

庐江矾矿的发展史是我国近代工业文明的活化石，见证了我国矿山企业从古代、近代直至现代的艰难发展演变历程和从农业文明到工业文明、从计划经济向市场经济的艰难转型。

#### 一. 无人机应用部分

##### （1）生成正射投影图

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/5.webp" height="auto" width="auto"  title="digit-x">


##### (2) 生成三维模型

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/6.webp" height="auto" width="auto"  title="digit-x">

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/7.jpg" height="auto" width="auto"  title="digit-x">

##### （3) 生成DEM通过gis/rhino生成等高线

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/8.webp" height="auto" width="auto"  title="digit-x">

#### 二 高程、坡度、排水分析

##### 坡度分析

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/9.webp" height="auto" width="auto"  title="digit-x">


矾山镇已建成区坡度在0~8之间，是适宜的建设区。南侧有梯田，坡度较低坡度基本位于0~5度之间。南侧为矿山和东山，坡度较大。

##### 排水分析

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/10.webp" height="auto" width="auto"  title="digit-x">

山体上的排水主要是往东山和矿山的周围排水，几乎不会对城区造成影响。城区的排水主要是从南侧源头（东山和矿山汇聚而成）排向城区。城区河道从南向北，还汇聚东西两侧地形排水。

##### 坡向分析

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/11.webp" height="auto" width="auto"  title="digit-x">


矾山镇朝向西北侧区域主要是矿山区域，朝东南侧主要集中在东山与城区。坡向主要影响建筑的排布方式与朝向，坡向也影响植物生产和人们对景观的观赏方式。

##### 高程分析

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/12.webp" height="auto" width="auto"  title="digit-x">

矾山城区高差较大，属于大别山外围丘陵，双顶山、天光山和钟子山三足鼎立。地面海拔高程在70~410m，相对高程50~300m。最高点在矿山顶，高度为287.5m。北侧为高坡，南侧为缓坡，高程变化不大。南高北低，两山环绕。

#### 基于Flowdesign/Phoenics风环境分析

##### (1) 风环境模拟

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/13.webp" height="auto" width="200"  title="digit-x">

季节风向示意图

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/14.webp" height="auto" width="auto"  title="digit-x">

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/15.webp" height="auto" width="auto"  title="digit-x">

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/16.webp" height="auto" width="auto"  title="digit-x">


矾山冬夏季风环境模拟评价


夏季风能够快速的通过城镇中心区，冬季风由于受到山体的阻隔，导致城区风速较低。冬季以季风为主导成恨风环境，夏季由于山体的阻挡，导致城镇主要处于低风速风压区。城镇主要通风方式是“冬季通风，夏季暖风”。



风场剖面图

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/17.webp" height="auto" width="auto"  title="digit-x">

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/18.webp" height="auto" width="auto"  title="digit-x">


城区典型片区风环境模拟

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/19.webp" height="auto" width="auto"  title="digit-x">


##### （2）日照模拟

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/20.webp" height="auto" width="auto"  title="digit-x">

大寒日日照模拟

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/21.webp" height="auto" width="auto"  title="digit-x">


日照模拟全年光照最不充足的一天（1月21日）城区由于建筑是多层，阴影面积较大，但不影响其他建筑日照时间，建筑采光影响不大。


夏至日日照模拟

<img src="./imgs_/physical_envir_analysis_basedOn_uavAgisoft/22.webp" height="auto" width="auto"  title="digit-x">

日照模拟全年光照最充足大的一天，山体对建筑影响很小。


本文主要讲述了使用无人机和其他分析软件对基地进行分析，使我们对场地大的物理环境有了初步的认识，有了这些数据，可以指导我们在城市尺度的基础上进行城市规划或绿地系统的设计。

那么下一期我们将为大家介绍小尺度环境下设计怎样协同基地小环境条件，敬请期待。

****

图文来源：*徐隆双  赵虎宸*

图文排版：*赵虎宸*

校正：*王垚*