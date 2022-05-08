> Created on Fri May  6 08:14:19 2022 @author: Richie Bao（西建大）

# 数字化设计——从参数化设计编程到城市空间数据分析方法

**随机调研：**

1. Grasshopper(参数化)、Dynamo(BIM)、Python（数据分析）和C（微控制器+传感器），自己掌握了几项编程技能？
2. 目前自己做设计使用哪些辅助工具？大概流程怎样的？
3. （建筑、景观和规划及生态）做研究（尤其数据分析）自己会使用哪些计算工具？研究方法是否有创新点？

**3小时后，我是否会尝试改变设计思维和研究方法？**

1. 是否意识到了设计思维的差异？尝试结合自己已有设计方法更新提升参数化设计方法，不断构建设计工具，增加未来设计工作竞争优势？
2. 研究生（硕士、博士，乃至到高校和科研院所），是否意识到了从定向到定量，用编程语言做数据分析，方法研究的重要性？尝试结合自己已有做研究的途径，用PYTHON语言提升创新的机会，把每一份研究构建出一个工具库，积累成工具集，成为工具的构建者？

## 1. 数字化|信息化（网络化）|智能化 时代与对自然的向往

### 1.1 数字化|信息化（网络化）|智能化

**你处于哪个时期（时代）？为什么？**

* 对于设计

1. 图板手绘，叠加草图纸。--->我是学生，我在练习基本功；我是设计师，习惯草图纸做设计，不过在概念阶段；
2. AutoCAD-2XXX+PS+3DMAX（不知是否还有效果图公司） 计算机平面制图辅助设计。---> 大家都还在用，要出施工图；
3. SketchUP（Rhino）+Lumion，三维方案推敲，与自行出效果图。---> 当前设计院主流工作模式，因为学起来简单；
4. 参数化设计-BIM(Grasshoper| Dynamo+Python | C#)。---> 终于进入到coding的世界，为了设计的自由；
5. 未来... ---> 待

* 对于研究

1. 能定量的，还在定向？---> 不知数据分析是什么？也未掌握相关工具；
2. 在使用QGIS | ArcGIS，...等弹窗集成工具做数据分析？还停留在Ian McHarg或者权重叠图时代？---> 到处找工具；
3. Python数据分析|大数据分析|机器学习|深度学习，...---> 为了不到处找工具，而自行构建，为了全世界的开源库，为了研究的自由，为了研究的创新性；
4. 未来... ---> 待

**VR+Urban Computing(Smart city)+Construction robots**

| VR_Arkio <br/> <video height='auto' width=100% controls><source src="./video/Arkio_v1_2.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>   | VR_Cities VR<br/><video height='auto' width=100% controls><source src="./video/Cities_ VR .mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>  |
|---|---|
|Computing_IAAC Computational Urban Planning & Design<br/><video height='auto' width=100% controls><source src="./video/AAC Computational Urban Planning & Design.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video> | This bricklaying robot could build 100 to 300 homes a year <br/><video height='auto' width=100% controls><source src="./video/This bricklaying robot could build 100 to 300 homes a year.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video> |

### 1.2 对自然的向往

城市的集聚、扩张，对自然土地的侵蚀，及城市与自然的割裂，使得城市环境恶化，生态问题突出，难以达到宜居的基本要求。空气、噪声污染，绿地碎片化，开敞空间不足，步行空间缺失，城市公共空间的生活品质趋于下降。拥挤、烦躁、疾病、压抑、孤独潜藏于城市繁荣的表面之下。现代城市的发展历经半个多世纪，在解决了人类生存和各类社会问题的同时，也以牺牲环境为代价，积累下各类潜在的城市问题。时至今日，城市环境恶化已经是不得不面对的重大问题，大量相关研究的跟进，都在试图为解决城市问题寻找方法。

|Causes and Effects of Climate Change _ National Geographic <br/> <video height='auto' width=100% controls><source src="./video/CLIMATE Change  May 05, 2022 news, naturel disasters, flooding, wildfire, tornado, earthquake-IzOVwpGfOoo.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video> | Air pollution in cities <br/><video height='auto' width=100% controls><source src="./video/Air pollution in cities-720p.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>|
|---|---|

[What Is Climate Change?](https://www.un.org/en/climatechange/what-is-climate-change) And emissions continue to rise. As a result, the Earth is now about 1.1°C warmer than it was in the late 1800s. The last decade (2011-2020) was the warmest on record.

Many people think climate change mainly means warmer temperatures. But temperature rise is only the beginning of the story. Because the Earth is a system, where everything is connected, changes in one area can influence changes in all others.

The consequences of climate change now include, among others, intense droughts, water scarcity, severe fires, rising sea levels, flooding, melting polar ice, catastrophic storms and declining biodiversity.

In a series of UN reports, thousands of scientists and government reviewers agreed that limiting global temperature rise to no more than 1.5°C would help us avoid the worst climate impacts and maintain a livable climate. Yet based on current national climate plans, global warming is projected to reach around 3.2°C by the end of the century.

* Simulated global temperature change (1850-2100)

<img src="./imgs_p/CCSM4_rcp85_global_temperature_change_spiral.gif" height="auto" width="auto"  title="digit-x">

### 1.3 科技-设计-环境

* 对于（规划）设计

数字化设计可以帮助设计师更好的分析理解场地（城市），及进行设计本身。合理的设计可以引导人们的（生活）习惯或行为，也可以调整局地小气候环境，对整个城市环境的改善尽微薄之力；

* 对于研究

尽量避免“务虚”神游般的论调，以设计想法似的概念，宣传似的口号去做研究。尽量用“务实”的数据进行分析，更新或提出新方法，解决实际，重要、尤其迫在眉睫的（环境、生活）问题。

<span style = "color:maroon;background-color:;font-size:30.0pt">从不懂设计到会做设计；从不会研究到知道怎么做研究。</span>

> 讲课人踩坑：硕士毕业后，在清华规划设计研究院工作，跟随朱育帆老师做设计，设计反复推敲修改（大大小小修改几十次也是正常），学习实践积累，日复一日，突然有一天，感觉我能做不错的设计了（推敲修改次数明显减少，参数化设计方法同时提升效率和创作能力。切记：第一份工作要跟对老板。）； 设计专业怎么做研究？还记得硕士时的文章不堪回首（对于选题，读上十几篇论文，就开始拍脑袋攒），即使博士阶段的文章也不堪入目（茫然），不知道怎么做研究！！！高校任教时，尝试多次自选题目，“做研究”，投文章，关键是等同行评审，学习意见。并梳理了'Landscape and Urban Planning'4年（15，16，17，18）的所有论文。直至开始写《城市空间数据分析方法》（还在进行中），用Python编程语言进行数据分析（定量），庞大的开源库（意味着无数人的研究成果可以学习，及使用），突然有一天（2021年的某一天），感觉我能做研究了（托Python数据分析的福，任何选题似乎都能找到创新点，“文思泉涌”！）。


## 2. 参数化设计编程

[Let’s talk about Grasshopper 2.0](https://discourse.mcneel.com/t/lets-talk-about-grasshopper-2-0/140402)；[Grasshopper 2 对外公测](https://mp.weixin.qq.com/s/ESDEkkY5tnprOt7Vl3BzgA)

参数化设计工具[Grasshopper](https://www.grasshopper3d.com/) 1.0(2007年发布，至今已15年)，发展的[扩展库](https://www.food4rhino.com/en)至少400组以上，涉及拓扑分析、最小曲面、包装展开、粒子系统、多代理、表皮细分、分形、形状语法（Shape Grammar Designing）、离散与聚合、体素化算法（Voxelization）、热点分析、动力学、图论、折纸、几何填充、编织等。Grasshopper 2.0（全面重新开发，更强大的全新参数化设计平台）已在公测阶段，2.0充满了创意，界面和交互方式的优化意味着友好的学习环境；彻底重写的代码，重新设计的数据类型、核心函数，多线程，意味着更好的计算速度和设计者使用体验，及更大数据处理需求；元数据标记对象信息，意味着基于参数化设计方法的BIM（LIM，CIM）将会迅速发展（重要的一个研究方向）。非常期待GH2的最终完成发布！

<a href=""><img src="./imgs_p/001.jpeg" height="auto" width=500 title="digit-x"></a>

### 2.1 空间设计思维VS数理逻辑思维

* e.g. 

| 示例名  | 1-纯粹空间推敲方法  | 2-参数化设计方法（构建数理逻辑）- 常规 | 3-参数化设计方法 - 生成  | 备注|
|---|---|---|---|---|
| 拉索 | 仿生形态不太好建模！绳索拉拽的位置不太好确定！  |  拖动点，控制点位置;随机Seed控制连接位置 <br/><img src="./imgs_p/011_s.png" height="auto" width="auto" title="digit-x"><br/><img src="./imgs_p/010_s.gif" height="auto" width="auto" title="digit-x"> | <span style = "color:maroon;background-color:gainsboro;font-size:10.5.0pt"> 协同-动力学（Kangaroo）</span> <br/><img src="./imgs_p/012_s.png" height="auto" width="auto" title="digit-x"><br/><img src="./imgs_p/011_s.gif" height="auto" width="auto" title="digit-x"> |   |
| 铺地 | 绘制或填充  |拖动点，控制点位置 <br/><img src="./imgs_p/002_c_s.png" height="auto" width="auto" title="digit-x"><br/><img src="./imgs_p/002_s.gif" height="auto" width="auto" title="digit-x"> |自动生成控制点（无条件）<br/><img src="./imgs_p/005.png" height="auto" width="auto" title="digit-x"> <br/> <img src="./imgs_p/004_s.gif" height="auto" width="auto" title="digit-x"> |  <img src="./imgs_p/003_s.jpg" height="auto" width="auto" title="digit-x">  |
| 地形 |  一根根绘制等高线？ | 控制边界和控制线+点<br/> <img src="./imgs_p/008_s.png" height="auto" width="auto" title="digit-x"><br/> <img src="./imgs_p/007_s.gif" height="auto" width="auto" title="digit-x">   |自动生成控制点--->控制线（无条件）<br/> <img src="./imgs_p/006_s.png" height="auto" width="auto" title="digit-x"><br/> <img src="./imgs_p/005_s.gif" height="auto" width="auto" title="digit-x">  | <img src="./imgs_p/009_s.jpg" height="auto" width="auto" title="digit-x">  |
| 桥 | 同拉索；合理的结构亦不好找！  |  控制点<br/> <img src="./imgs_p/017_s.png" height="auto" width="auto" title="digit-x"><br/> <img src="./imgs_p/016_duration_s.gif" height="auto" width="auto" title="digit-x"> |   <span style = "color:maroon;background-color:gainsboro;font-size:10.5.0pt"> 协同-结构（Karamba3D）</span> <br/><img src="./imgs_p/015_s.png" height="auto" width="auto" title="digit-x"><br/><img src="./imgs_p/013_duration_s.gif" height="auto" width="auto" title="digit-x"> <br/><img src="./imgs_p/014.png" height="auto" width="auto" title="digit-x"> |  |
|  |   |   |  <span style = "color:maroon;background-color:gainsboro;font-size:10.5.0pt"> 协同-生态（Ladybug Tools）</span> <br/>  |   |

### 2.2 设计方法数字化思辨

1. 思考设计时，一种直接思考某种形态，然后绘制或者三维模型构建；一种则思考某类形态通过何种逻辑构建出来，先有形态，后有逻辑；再者则思考何种逻辑构建出某类形态，先有逻辑，后有形态；
2. 参数化设计在寻找形态时，需要确定控制形态的参数，可以手动调参选择该类形态下的一种形态；也可以结构、动力学和生态等途径协同优化（进化算法）自行调参，确定最优形态；
3. 思维需要在空间设计思维和数理逻辑思维之间跳跃切换，需要适应这种设计思维方式，尤其数据类型、结构和数据流组织方式需要有深入认知；
4. 参数化设计过程是一个不断在创造设计方法和设计工具的过程，养成构建工具积累方法技术的习惯，这有助于设计效率的提升，及方法-算法总结和新方法的提出。

* 如果已经接触和应用了参数化设计方法，有什么地方需要改进？或能提出新的见解。

### 2.3 从0开始，进入到参数化设计编程领域推荐学习实践流程

Step_1. [Grasshopper参数化逻辑构建过程](https://www.bilibili.com/video/BV1iJ411C7FS?p=1)。虽然这个教程编写的较早，但对初学者很友好，被广泛传阅；

Step_2. [参数化设计编程——GRASSHOPPER](https://richiebao.github.io/parametric_design_coding_grasshopper/#/)。更新中，有大量代码段练习，可逐一或者挑选练习。培养建立设计工具模组的方法和习惯。这个阶段要养成参数化设计的习惯，不要再用常规方式；

Step_3. [参数化设计编程——GHPython](https://richiebao.github.io/parametric_design_coding_GHPython/#/)。这个更新最迟，也较慢，是用Python写参数化方法，可以更大程度的扩展设计方法探索途径（这涉及全世界无以计数的开源项目），让数据处理更自由，设计方法更具创新性。

> 注：由讲课人编写的6本老版教材，不再推荐阅读。包括《参数化逻辑构建过程》、《参数模型构建》、《编程景观》、《学习PYTHON—做个有编程能力的设计师》、《ArcGIS下的Python编程》和《折叠的程序》。

## 3. 城市空间数据分析方法

* Most Popular Programming Languages (Tiobe Index) | 2001-2021

<video height='auto' width=100% controls><source src="./video/Most Popular Programming Languages (Tiobe Index) _ 2001-2021_s.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video> 

<img src="./imgs_p/018.png" height="auto" width="auto" title="digit-x">

[python](https://www.python.org/)，目前[TIOBE](https://www.tiobe.com/tiobe-index/)排名为首位。

### 3.1 工具的桎梏

> Wikipedia claims there are approximately 700 programming languages, while others say that number is closer to 9000! The truth is, there’ve been countless programming languages created throughout history. 

**世界上有几百种编程语言，会几种？**   ): or (:

1. 那么多数据源（POI,OSM,AOT,Cityscape,...）涉及方方面面（业态、地图、城市环境传感器、分类数据、点云、遥感图像...），我却只能望而兴叹；
2. (python)庞大的库，涉及方方面面（...），又只能望而兴叹。

> Python的库高达137,000多个，涉及不可胜数的算法，领域，并无时无刻的不在持续增长，新方法在不断的涌现。在城市空间数据分析中，基础的数据分析涉及到Scipy、NumPy、Pandas、SymPy等库；地理信息数据分析涉及到GDAL、GeoPandas、Rasterio、Rasterstats、EarthPy、PySAL等库；数据可视化涉及到Matplotlib、Plotly、Seaborn、Bokeh、PyViz等库；机器学习涉及到Scikit-learn，Statsmodels等库；深度学习涉及到PyTorch、Tensorflow、MXNet等；网络分析涉及到NetworkX库等；数据库涉及到Sqlite3、MySAL库等；点云数据涉及到PDAL库等。包括了大数据分析、机器（深度）学习、图像处理等诸多规划设计领域智能化研究的方法、算法，这为城市空间数据分析方法研究提供了基础工具。

**工具就摆在那，非得用省事不费力的“老办法”（还是“笨方法”？），不想花功夫学新事物，也是没办法的事...**

### 3.2 研究与代码库

每一项研究，都会形成一个针对该项研究的Python库（工具）。把日积月累的成果汇集在一起，就构成工具集。不仅用于该项研究，亦可以分享，只需调用库即可选择执行其中的方法。

<span style = "color:maroon;background-color:;font-size:20.0pt">代码的过程，是数据分析的过程，是研究的过程，也是论文写作的过程。</span>

1. 涉及数据处理方法，数据库管理途径；
2. 涉及文件结构及配置方法；
3. 涉及依赖库环境；
4. 涉及数据处理分析方法，和新方法的提出；
5. 涉及数据可视化、地图构建和图表表达；
6. 涉及代码更新，发布于安装分享；
7. 涉及工具集成;
8. 涉及论文写作。

* e.g.

1. [城市街道视域景观指数与邻里尺度特征效应](https://richiebao.github.io/USDA_CH_final/#/./markdown/3_2_1_%E5%9F%8E%E5%B8%82%E8%A1%97%E9%81%93%E7%A9%BA%E9%97%B4%E6%8C%87%E6%95%B0%E8%AE%A1%E7%AE%97%E4%B8%8E%E5%AF%B9%E7%A9%BA%E9%97%B4%E7%89%B9%E5%BE%81%E5%88%86%E5%B8%83%E7%9A%84%E8%B4%A1%E7%8C%AE%E5%BA%A6);
2. [综合性公园公共交通可达性及规划配置均衡性](https://github.com/richieBao/codes_repository_for_papers_publication-richie_bao)。

### 2.4 从0开始，进入到城市空间数据分析领域推荐学习实践流程

对于规划、建筑、风景及生态等专业，首先Python编程语言。如果你还没有掌握，务必要从现在开始学习了。

1. Python的入门级教材、在线课程、交互练习异常多，可以根据自己的习惯，选择学习就好。当然，好的内容会让你少走弯路，并提高学习效率；
2. 跟随[城市空间数据分析方法](https://richiebao.github.io/USDA_CH_final/#/)学习，或者当作代码参考手册使用。更新完结出版中。

## 4. 成为工具的建构者——具有编程能力的设计师

不管是参数化设计编程还是使用Python编程语言的城市空间数据分析方法，都是设计师或者研究者自己在不断的创作工具来解决不同的设计内容或者研究问题。

不再被已有工具绑架思想，想自由的从事设计和研究，请......

<span style = "color:maroon;background-color:;font-size:30.0pt">成为工具的建构者！</span>

> 富余时间内容，[参数化设计编程——GRASSHOPPER](https://richiebao.github.io/parametric_design_coding_grasshopper/#/)
