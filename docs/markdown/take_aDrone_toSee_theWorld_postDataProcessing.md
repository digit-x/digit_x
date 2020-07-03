🐞 图文排版： *肖景天* &nbsp;&nbsp; 图文来源：*文师鹏  黄昊霖*&nbsp;&nbsp; 校对：*王垚*&nbsp;&nbsp;📅 May 21, 2019
# Digit-X | 带上无人机去看世界——后期数据处理
 数字营造学社 術字怪兽 2019-05-21

叮铃铃

数字营造学社的无人机小课堂又开始了

上一期我们讲解了无人机的基础操作

这一期我们将介绍无人机数据的处理流程

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/1.gif" height="auto" width="auto"  title="digit-x">

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/2.gif" height="auto" width="auto"  title="digit-x">


<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/3.webp" height="auto" width="auto"  title="digit-x">


我们先来看看我们之前都用无人机做了什么吧

| <img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/4.gif" height="auto" width="auto"  title="digit-x">  </br> <em>**三维建模**</em>|<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/5.gif" height="auto" width="auto"  title="digit-x"> </br><em>**建筑外环境测绘**</em> |
|------------ | -------------|
|<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/6.webp" height="auto" width="auto"  title="digit-x">  </br><em>**平面测绘**</em> |<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/7.webp" height="auto" width="auto"  title="digit-x"></br><em>**高空摄影**</em>|
|<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/8.webp" height="auto" width="auto"  title="digit-x"></br><em>**全景摄影**</em>||



|浐灞湿地公园全景图|商洛市滨江路全景图|
|------------ | -------------|
|<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/9.webp" height="auto" width="auto"  title="digit-x">| <<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/10.webp" height="auto" width="auto"  title="digit-x">|

......


看到这些是不是很心动呢

快让我们一起来学习相关内容吧

## 无人机的后期数据处理

### ①相关知识介绍

正射投影的资源获取好之后，我们就可以对它进行初步的数据处理，目前可用的软件主有两个①Agisoft②pix4d，这两个都是目前比较容易接受的且容易上手操作的。将照片导入软件中，并按照步骤进行流程化的数据处理，我们可以得出飞行区域的密集点云信息、DEM图片以及DSM的制作。分别简述一下这三个的基本信息。

密集点云：在逆向工程中通过测量仪器得到的产品外观表面的点数据集合也称之为点云，通常使用三维坐标测量机所得到的点数量比较少，点与点的间距也比较大，叫稀疏点云；而使用三维激光扫描仪或照相式扫描仪得到的点云，点数量比较大并且比较密集，叫密集点云。密集点云的制作只是其中的一部分，后续可能还会用到密集点云的分类。

数字高程模型（Digital Elevation Model)，简称DEM，是通过有限的地形高程数据实现对地面地形的数字化模拟（即地形表面形态的数字化表达），它是用一组有序数值阵列形式表示地面高程的一种实体地面模型，是数字地形模型(Digital Terrain Model，简称DTM)的一个分支，其它各种地形特征值均可由此派生。

数据获取结束，我们就可以将整个过程进行梳理，获取所需要的部分，流程如下：

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/11.webp" height="auto" width="auto"  title="digit-x">

生成的正射投影是tif格式的图片，作为带有信息的图片，我们可以导入进Arcgis等GIS软件中，进行下一步的处理--获取等高线。

对于Arcgis不熟练的人推荐使用Globemapper进行等高线的处理。

作为数据处理这个系统来说，我们还会接触到一个软件-ENVI。

它是一个完整的遥感图像处理平台，应用汇集中的软件处理技术覆盖了图像数据的输入/输出、图像定标、图像增强、纠正、正射校正、镶嵌、数据融合以及各种变换、信息提取、图像分类、基于知识的决策树分类、与GIS的整合、DEM及地形信息提取、雷达数据处理、三维立体显示分析。

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/12.jpg" height="auto" width="auto"  title="digit-x">

这个软件于Arcgis之间的联系是通过数据的交互，可以把Arcgis中处理好的数据与Envi进行叠加，从而进行宏观的观察，也是栅格数据和矢量数据的结合，两个软件使用同一个数据库管理系统，方便数据的管理和处理。

### ②无人机正射平面图制作流程

本次我们通过无人机航拍图像来合成正射平面图，用到的软件为Agisoft。

本次操作的流程如下：

1·导入影像数据并对齐图像。

2·建立密集点云。

3·通过密集点云的数据来生成DEM模型的高程数据。

4·通过DEM高程数据生成正射影像。

5·导出正射影像

#### 一、导入影像数据并对齐图像

首先打开软件的主界面，如下图。（有可能第一次安装后界面显示为英文，可以依次点击上方菜单栏：Tools→Preferences→Language选择Chinese即可。）

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/13.webp" height="auto" width="auto"  title="digit-x">


导入照片之后会如下图所示，这时候需要对它进行对齐照片操作才可以进行下一步。

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/14.webp" height="auto" width="auto"  title="digit-x">

注意在上一步操作当中要选择合适的精度，一般为中等以上，不然会像下面这样：

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/15.webp" height="auto" width="auto"  title="digit-x">

对齐完成之后如下图，这时候可以进行下一步了！

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/16.webp" height="auto" width="auto"  title="digit-x">

#### 二、建立密集点云

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/17.webp" height="auto" width="auto"  title="digit-x">

这里生成的质量也尽量选择中，如果电脑配置不错可以试着生成高质量。（内存一定要在8G及以上！）

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/18.webp" height="auto" width="auto"  title="digit-x">


#### 三、通过密集点云的数据来生成DEM高程数据


设置默认即可。（进行这步之前要进行保存）

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/19.webp" height="auto" width="auto"  title="digit-x">


生成的DEM如下图，可以导入GIS等软件深入处理。

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/20.webp" height="auto" width="auto"  title="digit-x">

#### 四、通过DEM高程数据生成正射影像

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/21.webp" height="auto" width="auto"  title="digit-x">

这就是生成的正射影像图了，不过还要进行最后一步导出才可以分享！

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/22.webp" height="auto" width="auto"  title="digit-x">


#### 五、导出正射影像

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/23.webp" height="auto" width="auto"  title="digit-x">

按默认设置导出即可。

### ③无人机的全景图制作

对无人机拍摄的全景图片数据进行处理，这次我们使用的平台为PTGui Pro。

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/24.webp" height="auto" width="auto"  title="digit-x">

首先将拍摄的图片数据导入进软件平台中。

然后点击对准图像，软件会自动对拍摄图像进行拼合。

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/25.webp" height="auto" width="auto"  title="digit-x">

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/26.webp" height="auto" width="auto"  title="digit-x">

在图像拼好了之后，可以对其进行裁剪编辑。

最后就可以对全景图进行导出啦，可以保存为.jpg/.tif/.psd等格式。

<img src="./imgs_/take_aDrone_toSee_theWorld_postDataProcessing/27.webp" height="auto" width="auto"  title="digit-x">

以上就是无人机教学的全部内容啦，请关注数字营造学社获取最新干货~


-----


图文来源：*文师鹏  黄昊霖*

图文排版： *肖景天*

校对：*王垚*
