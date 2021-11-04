> Created on Tue Sep 28 11\58\27 2021@author: Richie Bao-parameterize_archi_la_design_and_data_analysis 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# 参数化设计方法综述与启示 Overview and Enlightenment of Parametric Design Method

[GH库的扩展](http://cadesign.cn/digit_x/Parameterized_overview_chart.html)

`[GH库的扩展](http://cadesign.cn/digit_x/Parameterized_overview_chart.html ':include :type=iframe width=100% height=1200px')`

> 参数化设计正在拓展和改变着设计领域传统的设计思维方式，推动着设计朝向数字化和智能化方向发展。首先引入对参数化的基本解释，并就其发展史给与简要介绍。根据参数化的内在本质和特点，就当前拓展的领域，概括为9个主要方向，其中基础的部分为参数化设计工具界面体验和数据管理、处理及云平台；设计部分为设计空间形式的分析、构建，到优化生成和机器学习，结构分析、构建与设计空间形式协同优化，可持续性设计及设计空间形式协同优化，BIM与GIS的参数化途径和机器人建造与3D打印；以及交互硬件与信息数据流和图表报告及制图。并进一步指出参数化设计未来发展方向，以及设计师从工具的使用者成为工具建构者这一角色的转变。

风景园林数字化设计，主要包括参数化、大数据、智能化、信息化、虚拟仿真、机器人建造与3D打印、智能硬件等，已渗透到规划设计的各领域。其中参数化能够融入其它的所有方向，在风景园林数字化规划设计流程中，参数化设计占据着核心位置，是未来重要的发展方向之一。它既是一种技术，又是一个方法[1]。虽然数字化必然要依托计算机及运行的软件存在，但是参数化和算法思维不是关于任何一个计算机软件或者任何一个特定的语法，而是关于逻辑、几何、拓扑和交互[2-6]。

本文归纳总结当前参数化设计的主要拓展领域，以风景园林方向为主导，同时综合建筑和城市规划与风景园林叠合的部分，力图展现参数化设计解决风景园林问题的多种可能，促进风景园林的数字化，以及智能化发展。

## 1 参数化基本解释和简要发展史
### 1.1 参数化基本解释
除了实际搭建物理模型用于设计推敲之外，设计师经常使用计算机辅助工具建立二维或三维模型来推敲方案，但是这些模型绝大多数是以一种难以交互修改的方式所构建，尤其当设计空间变得复杂时，设计思维与模型推敲互相掣肘。为了解决这一问题，目前设计师已经开始广泛的使用参数化设计软件，指定模型各种参数之间的关系，当修改对应设计模型某一部分或多个部分的参数时，设计模型的其余部分做出相应或者适宜的反应进行模型的更新。这种基于设计者设计的关联规则，建立设计模型参数关系，描述设计方案的逻辑和意图，即从常规操控设计表现到应用系统性逻辑编码设计意图的设计思维模式转变过程允许设计师探索和优化多种设计的可能性[2-9]。

同时理解参数化设计的根源也并不隐藏在复杂的数学公式或者编码语法之下，相反它们存在于思维的组织和设计方法中。当设计者以这种方式理解代码算法，就有可能通过这种视角来构建设计问题，开启设计意图与计算迭代生成之间的对话.[3]。

### 1.2 参数化简要发展史
参数化实际上是计算机辅助设计中最早的想法之一，在1963年Ivan Sutherland的博士论文中，将参数变化置于Sketchpad系统（由Ivan Sutherland编写的计算机程序，并因此于1988年获得了图灵奖）的中心，一种能够适应不断变化语境的表现形式，创造与预见了未来计算机辅助设计（Computer-aided Design, CAD）系统的主要特征之一[4-7]。而后很多参数化系统不断的在实验室和公司企业中建立起来，最为成熟的参数化系统是非设计类的电子表格（Spreadsheet）。在设计领域参数化类软件最初出现于机械工程类学科，在建筑（景观）设计类领域，参数化的实质性影响仅在2000年左右才开始出现[4-11]。例如英国建筑联盟学院（Architectural Association School of Architecture，AA）在建筑类学科之下设置景观都市主义（Landscape Urbanism）研究生课程，因为参数化设计在AA建筑专业内部的广泛研究和大范围运用，其理论思想和技术手段也逐步深入到景观都市主义课程中，并在实际地块上进行论证和探索[5]。早起的CAD软件并不具备Sketchpad系统所承载的数据集成、参数化能力或者模拟潜力。一些设计公司的内部研究团队，如Foster + Partners事务所的专业模型团队，和Smartgeometry（SG）等新兴社区（专注于建筑、工程和建造<Architecture, Engineering and Construction (AEC)>中使用计算机作为智能设计辅助的非盈利组织。鼓励使用计算性和参数化软件工具实践的AEC专业人员、学者和学生之间的协作），促进创建参数化设计软件以及众多相关的技术和理论。在2003年，Generative Components（GC）一个CAD软件MicroStation的插件，成为第一个自由和广泛为设计师使用的参数化设计类软件[6-30]。参数化设计在建筑景观领域迅速崛起归功于2007年David Rutten在Robert McNeel and Associates（Rhino三维设计模型软件的创造者）构想了可视化编程工具Grasshopper（GH），基于图形用户界面（Graphical User Interface，GUI）的脚本引擎，使用可拖拽的数据块和函数（称为组件，Component）通过线连接起来，从而允许不懂文本式编程的用户使用计算逻辑操控Rhino工具并生成高度复杂的三维模型。在风景园林领域，GH不仅用于学术项目，也越来也多的用于专业项目。例如，Fletcher Studio工作室广泛应用GH模拟环境，并为旧金山South Park耗资280万美元的改造项目创建概念程序设计包和施工文档[7-208]。受GH影响，为简化用于建筑信息模型（Building Information Modeling, BIM）构建的Revit 软件中AEC工作流程，于2013年开发了可视化编程工具Dynamo，实现了BIM类软件中参数化的融入。

可视化编程的参数化工具通常并不是闭合的设计环境，融入了文本式的脚本编程语言，例如常用的Python语言，以及C#等，这样极大程度上拓展了设计师编写设计逻辑或算法的自由以及其拓展性。例如基于GH的扩展库高达约434组，涉猎到界面体验，数据管理分析，空间几何构建、分析、算法、生成，智能化设计，结构、可持续性设计，BIM和制造及机器人建造，3D打印，地理信息系统（Geographic Information System，GIS），及交互硬件等众多方向。

<a href="https://www.grasshopper3d.com/"><img src="./imgs_parae/001.png" height="auto" width="auto"  title="digit-x"></a>
<a href="https://dynamobim.org/"><img src="./imgs_parae/003.png" height="auto" width="auto"  title="digit-x"></a>

## 2 参数化相关拓展领域
当前参数化设计方法的研究内容已经拓展到从设计、分析到施工、建造，管理和维护的整个流程，从基本的设计几何构建下的形式逻辑探索，到融合结构与生态协同的设计形式优化，及BIM和GIS领域的结合，再到机器人建造和硬件信息交互，形成环环相扣的参数化流程，从而能够形成实时互动反馈的信息流，优化设计、结构、生态、建造的各个环节。截至2020年10月份，根据当前风景园林领域以中国园林和风景园林为代表的核心期刊中所发表的约19篇参数化设计相关论文，约为32篇博硕论文，约434组GH的扩展工具，以及数字化会议论文和具有代表性的国内外专著，可以将参数化设计相关拓展领域划分为如下几个方向，其关系如图1：

<a href="https://energyplus.net/"><img src="./imgs_parae/264.jpg" height="auto" width="auto"  title="digit-x"></a>

### 2.1 参数化设计工具界面体验
参数化设计工具除了尺寸驱动的方式之外（例如Gehry Technologies基于工业软件Catia开发的Digital Project，DP，设计领域已很少使用），通常都是基于编程语言或将其作为软件的代码脚本实现，包括文本式的编程语言（Python等），以及可视化编程（GH、Dynamo等）。由于时间的限制，许多设计人员无法掌握编写脚本所必需的高级语法知识，而可视化编程则允许设计人员更容易的将代码的运行结果与设计结果连接起来，而不需要学习原始文本式的代码编写[8]。但是如Python的缩进机制强制程序员养成良好的编程习惯使得代码清晰易读、易维护一样，可视化编程工具可以通过扩展工具对齐、组织规整组件，以及增加组件信息、操作提示、界面形式等途径增强可视化编程工具的易用性、易读性。

工具界面体验的改进虽然不是参数化的内容，而是工具本身GUI的属性，但是正如文本式编程语言一样，好的编程开发环境及好的编写习惯能够增强设计师代码的编写效率。

### 2.2 数据管理、处理及云平台
参数化设计代码就是理解问题，将问题分解为部分，为其设计数据结构和算法，进而组成一个完整程序设计[9]。因此代表设计空间的几何形式、属性，结构、生态的分析信息，以及融合BIM、GIS方法，和机器建造、硬件信息流等数据，数据管理和处理成为参数化设计的基础。包括数据的格式转换、输入输出、数据结构、数据运算（包含各类运算器和算法，例如循环、模糊逻辑、四元数求解器，自定义公式算法等）、运算性能、数据库、版本管理、代码管理、远程控制等。

同时，由于云计算的发展和网络速度的提升，位于参数化模型关系链下，基于本地计算的部分内容可以由云计算完成，例如风、日照和热环境的模拟，结构分析及优化计算等，从而释放本地计算的压力，尤其大型和复杂场景的模拟分析。亦可以基于网页实现三维模型的可视化，以及不同软件平台的模型转换。

### 2.3 设计空间形式的分析、构建，到优化生成和机器学习
设计的目的就是获得满足功能需求、适应环境变化、具有审美艺术性及创造性，最终建造的设计空间形体或环境空间。因为计算性设计（Computational Design）的发展，设计空间形式的分析和构建的新方法不断涌现，并逐渐的纳入到参数化的关系链下，成为其数据流的一部分。例如拓扑分析、最小曲面、包装展开、粒子系统、多代理、表皮细分、分形、形状语法（Shape Grammar Designing）、离散与聚合、体素化算法（Voxelization）、热点分析、动力学、图论、折纸、几何填充、编织等。每一类算法都为设计注入新的活力，而算法是一组规则或任务，可以反复执行，直到达到特定状态。在这连续的参数化设计过程下，算法定期更新以响应反馈循环，对特定场地条件做出响应[10]。

参数化实时互动反馈的数据流组织方式，实现了各类优化生成的设计方法，例如平面布局生成、台阶生成、地形生成、建筑单体生成等。Nicholas de Monchaux是加州大学伯克利分校的建筑和城市设计副教授，自2009年以来一直致力于地方规范的项目，旨在利用美国及国外多个城市的废弃城市用地，设计具有社会和生态活性的景观。许多这样的大都市地区有成百上千个这样的场地，因此试图找到一种方法，基于当地的背景系统地生成设计，而不是逐个地设计每个场地。为了促进这一过程，团队开发了一套基于GH参数化平台的组件，在地理空间数据和设计几何图形之间建立联系，一次为多个站点生成三维设计[7-210]。而引入的优化算法则为某些问题寻找最优答案提供了契机，该技术可能涉及数百个循环，并导致产生数千个不同的选项。但是，与其他方法相比，这种技术可以产生新的、性能更好的几何图形[11]。

机器学习的算法也在被逐渐的融入到参数化系统中，虽然目前应用程度有限，但是随着计算机计算性能的提升，参数化向区域尺度大数据方向的跨进，将实现大数据的参数化智能分析和规划，这对风景园林学科具有重要意义。

<a href="https://scikit-learn.org/stable/"><img src="./imgs_parae/265.jpg" height="auto" width="auto"  title="digit-x"></a>

<a href="https://pytorch.org/"><img src="./imgs_parae/266.jpg" height="auto" width="auto"  title="digit-x"></a>

<a href="https://www.food4rhino.com/en/app/lunchbox"><img src="./imgs_parae/267.jpg" height="auto" width="auto"  title="digit-x"></a>

### 2.4 结构分析、构建与设计空间形式协同优化
因为结构分析模拟的算法以扩展组件的形式嵌入到参数化的设计流程当中，例如典型代表Karamba，使得在设计阶段就可以实时的对建筑结构进行模拟及评估。并常常结合使用遗传算法（Genetic Algorithms, GA）、模拟退化算法（Simulated Annealing Algorithm）、进化算法（Evolutionary Algorithm，EA）、蚁群算法（Ant  Colony Optimization，ACO）等优化算法（或称启发式算法，Heuristics Algorithmic）通过不断的迭代产生新的几何形式演化优化结构并评估结构的性能[12-18]。例如Skidmore, Owings & Merrill (SOM)的结构工作室，利用搜索算法提出力-密度优化算法，对任意定义具有明确支撑结构和负载的材料，迭代的移除从力的加载点到支撑对象力传递的过程中作起最小作用的部分材料，从而最有效的使用现有材料[13]。

同时，随着结构的复杂程度增加，优化算法的过程相对繁琐，神经网络的应用成为一种优化的替代方法，将训练好的神经网络模型应用于共享同一领域不同问题的结构优化策略，将其从一个特定的问题转换为一个特定的领域[12-19]。

<a href="https://www.karamba3d.com/"><img src="./imgs_parae/268.jpg" height="auto" width="auto"  title="digit-x"></a>

### 2.5 可持续性设计及设计空间形式协同优化
可持续性设计通常包括气象数据分析、热环境、风环境、光环境、声环境分析，以及能源、建筑生命周期分析，和景观生态等。用于环境分析的数字化工具数量在迅速增长，部分为独立的模拟分析工具，而参数化设计将影响区域、城市和场地尺度上的环境，通过对太阳、阴影、风、视野和其它人类舒适度进行建模和分析，创造一个设计环境，方案可以被实时模拟和调整，从而创建由这些力量数字化塑造的景观[14-247]。例如Ladybug是由Mostapha Sadeghipour Roudsari开发的GH扩展组件，允许设计者导入和分析标准的气象数据并绘制图表，如太阳轨迹、风和辐射玫瑰图等。这些可以通过多种方式进行定制以运行辐射、阴影和视野分析。Honeybee是另一个GH扩展组件，可以连接到经过验证的模拟引擎，如EnergyPlus、Radiance、Daysim和OpenStudio，用于建筑能源、舒适性、采光和照明模拟[15]。

在更大的范围内，对正在使用的建筑物和城市及其相关环境影响和排放的研究通过实时数据提供信息，并进行模拟，以显示长期的影响。Thornton Tomasetti公司的研究团队开发的碳计算器，能够在设计阶段的早期计算出任何设计配置的总内涵能和碳（Embodied Energy and Carbon）。通过参考Inventory of Carbon and Energy (ICE)数据库，创建了一系列GH扩展组件，在设计过程中实时计算并可视化内涵碳。成都高新区“桂溪生态公园”项目通过参数化技术的引入，从土方精算、雨水径流分析、风环境模拟、日照分析等多种途径为生态设计策略提供技术支撑， 构建一个具备调节都市节奏功能的绿色循环体系[16]。南京牛首山景区北部片区的工程实践则研讨参数化拟自然水景模型的应用[17]。

如果可持续性设计的各类模拟分析，结合到优化算法，根据设计目的和分析标准，可以自动创建、评估和演化无以计数个选项以满足项目目标，自动寻找适合的空间形式。

<a href="https://www.ladybug.tools/index.html#header-slide-show"><img src="./imgs_parae/269.jpg" height="auto" width="auto"  title="digit-x"></a>

### 2.6 BIM与GIS的参数化途径
BIM是一个建立在协同操作和可靠的信息上的一体化过程，贯穿于项目的设计、建设和后期运营管理阶段。它有助于改善协作，提高精度，减少浪费，使得人们能在更早的阶段做出更明智的决定[18]。风景园林信息模型（Landscape Information Modeling，LIM）是BIM在风景园林领域中的拓展，是创建并利用数字化模型对风景园林工程项目的设计、建造和运营全过程进行管理和优化的过程、方法和技术[19]。BIM的参数化途径主要有三种类型，一种是基于BIM软件平台的可视化编程工具，例如基于Revit的Dynamo；二是参数化平台自身的拓展，例如GH的扩展组件VisualARQ、GTT等；三是基于尺寸驱动的参数化BIM平台，例如DP等。同时，GH的部分BIM扩展组件可以实现GH数据与Revit、ArchiCAD、Bently、Tekla 等BIM软件的数据交换。

BIM建筑设计软件主要针对建筑专业，虽然风景园林与建筑专业之间互相叠合，但是风景园林有其自身的专业特定，例如除了景观建筑外还包括地形、植被、水系、生物等自然要素，这些要素的信息繁杂多样与庞大，通常是使用GIS进行数据的管理与分析。但是GIS的建筑三维信息模型能力较弱，并不以工程项目为主，不包括多尺度适应的场地景观设计、改造、建设、管理运营等行业相关活动的数字信息[20]。但是GH的扩展组件拓展了GIS的地理信息数据转换与区域规划能力，GH强大的可拓展性，也为GIS和BIM的融合实现LIM的参数化提供了可能性。

<a href="https://www.food4rhino.com/en/browse?lang=en&sort_by=ds_changed&f%5B0%5D=im_field_unified_type%3A773&f%5B1%5D=im_field_platform_app%3A720&f%5B2%5D=im_field_term_reference_category%3A688"><img src="./imgs_parae/270.png" height="auto" width="auto"  title="digit-x"></a>

### 2.7 机器人建造与3D打印
参数化建立的模型，不仅会受到用户定义参数的变化影响，还会融入结构力、材料性能、热和光照变化以及环境条件的影响，从而能够对现实生活中对应的模型做出真实的反应。它们准确地表示了现有结构的内部构造逻辑，因此参数模型也可以展开或转换成可以数字化制造的几何体[2-9]。同时，当控制机械臂机器人，例如ABB、KUKA、HIWIN和Universal Robots等包含机器人指令的扩展组件嵌入参数化设计流程时，可以创建工具路径、模拟机器人运动、通过解算器和快速代码生成器生产执行代码等任务。而最近，7轴工业机器人的使用广泛，这些机械臂附着和控制着由这些机械臂附着和控制的末端执行器可以加工各种不同的材料。这些包括所有典型的切割系统(圆锯、刳刨、水射流、等离子和激光切割)，以及机械爪、弯管机、热线切割机等。机器人的灵活性允许打破工厂的界限，在现场操作[21]。因此机器人建造从一开始就嵌入材料信息辅助设计，模拟分析及优化建筑结构到机械臂建造模拟及执行代码生成，再到实际的加工建造的过程，形成了一个流畅、动态连续的工作流程。

今天，3D打印技术提供更快、更大、更强和更便宜的输出。从早期的概念到后续的项目开发、先进的制造工艺和全功能产品的集成，新颖的工作流程成为可能。在设计过程中，参数化建模与性能优化的结合重新定义了设计过程，因为材料被计算分配到最需要的地方。因此，形式是有效的，并在几何上更加复杂。而3D 打印是结构优化的理想技术，因为它可以制造出这些复杂的人工制品，实现复杂的几何形状和定制组件，而其它成熟的技术都无法做到这一点[22]。

<a href="https://www.robotsinarchitecture.org/kukaprc"><img src="./imgs_parae/271.jpg" height="auto" width="auto"  title="digit-x"></a>

### 2.8 交互硬件与信息数据流
Arduino和Raspberry Pi这样的低成本微控制器，拥有大量用户支持的开源编程环境，使设计师能够进入DIY电子产品的世界。并可以定制建立持续报告空间温度、湿度和压力，可见光和紫外线水平，风速、风向和含尘量，土壤水分和养分水平，甚至像光合作用这样复杂的由环境传感器收集的信息过程，亦可以连接到参数化平台中，实现实时连接，数据交换。

开源硬件的发展增加了景观设计师使用环境感知工具的机会，从这些硬件设备中收集到的信息直接连接到设计模型中，尤其嵌入到参数化模型的数据流中，强化了设计模型与环境感知的互动能力[23]。例如2013年Tulane University在现有的建筑学院大楼安装了一个由150个传感器组成的网络，以了解整个建筑立面的平均辐射温度。利用这些数据，他们对建筑进行改造达到以最小的能量达到最大舒适度的目的[24]。2017年，芝加哥更进一步，开始安装由500多个传感器节点组成的实时网络。每个节点都有一个传感器组合，用于测量从行人、车辆交通到空气质量等一系列数据[25]。

数据和物理世界的融合，嵌入为参数化系统数据流，不仅增强了设计过程，亦为建筑环境提供了更多的可能性，以响应用户和其中的活动，例如可以响应空间使用的水景、遮阳结构和灯光等[26]。

<a href="https://store-usa.arduino.cc/?gclid=EAIaIQobChMI6dXNydyh8wIVwxB9Ch2qgwneEAAYASAAEgJjH_D_BwE"><img src="./imgs_parae/272.jpg" height="auto" width="auto"  title="digit-x"></a>

<a href="https://www.ros.org/"><img src="./imgs_parae/273.jpg" height="auto" width="auto"  title="digit-x"></a>

### 2.9 图表报告及制图
为了能够更好的观察设计分析数据的变化情况，通常需要绘制图表以及打印报告，这一过程也被融入到了参数化的数据流当中，能够实现即时的数据图表反馈，以及动态的数据变化记录和捕捉。

## 3 从设计师到工具建构者
在过去的20年里，建筑的设计技术已经从软件的使用，转变为软件的开发和制定。这种从工具的使用者到工具制造者的转变意义深远。例如设计师和工程师在设计过程中加入了性能模拟、材料知识、构造装配逻辑和生产机械参数，从而创建定制的数字化工具，使项目在不同阶段的性能反馈成为可能，以创造新的设计机会，即结构、材料或环境性能可以成为建筑形式创造的一个基本参数[6-36]。而以编程为核心的参数化设计，本身就可以理解为针对要设计的某一项目或部分，又或解决某一特定问题，寻找解决的逻辑并以参数化的方式表达的过程，使得参数化景观成为一个自组织的开放系统，通过反馈回路，不断调整适应，满足复杂和不断变化的参数，这使得设计师不仅是方案的创造者，亦成为了算法的作者[27]。

GH的400多个扩展组件，很多均由设计师、工程师或研究团队自行开发，一方面是设计师自身为方便参数化编程，自行将常用的算法封装为组件方便日后调用；或者针对某一要解决的问题及调用某一算法，例如空间句法、寻找路径、视域分析、行人模拟、古建筑构造等，自行编写程序实现功能。因为可视化编程参数化工具的广泛应用，以及文本式编程Python语言的普及，设计师成为工具的建构者，增强了其解决问题的能力，这为设计提供了无限创造的可能性。

<a href="https://www.food4rhino.com/en"><img src="./imgs_parae/274.jpg" height="auto" width="auto"  title="digit-x"></a>

## 4 参数化设计未来发展启示
算法建模和可视化编程软件工具已经从学术和早期使用者的阴影中崛起，成为一个专业的文化意识 [28]。使用参数化和计算化的设计过程和技术不仅是前卫的，提高了我们理解和塑造我们环境的能力，而且，在另一个维度上，也是进化的，塑造了我们思考、交流和看待我们自己和我们世界的方式[14-243]。参数化设计未来的发展存在非常多创造性的可能性，参数化的系统化，大数据智能化分析的参数化及城市传感器有意识的城市网络，到机器人建造和3D打印的继续发展都是未来参数化设计领域继续得以发展的重要方向。

### 4.1 参数化的系统化
风景园林规划设计面对的是复杂的环境，需要调控多样、动态的设计要素与客观规律，在风景园林规划设计中运用参数化技术能够实现参数调控下的多目标实现与系统优化，是一种有效的规划设计调控方法[29]。Patrik Schumacher是Zaha Hadid建筑师事务所的合伙人，将参数化作为现代主义之后的新运动，写道：“我们必须一直追求参数化设计范式，渗透到学科的各个角落。系统的、适应性的变化和持续的差异化（而不仅仅是多样性），涉及从城市到构造细节层面的所有建筑设计任务。这意味着在所有尺度上的流动性[2-9]”。这一定程度上揭示了参数化系统化的内容，参数化通常可以贯穿于整个设计分析、施工建造到后期维护的整个纵向流程，而每一节点下又可以发展出无数的水平向分支解决特定的问题，而纵向之间、横向之间及纵横向之间是数据流交织，形成网络型的反馈回路，这样就可以根据设计的目的自由的组织出无数种可能的系统化结构。不同的项目通常有不同的系统化结构，而系统化是由各个能够完成特定任务的编程节点组成，这些编程节点可以被其它系统所调用。

类似于文本式编程语言的扩展性，GH可视化编程可以不断的开发新的扩展组件完成特定的任务，这不仅能由程序员来完成，更是由于可视化节点式编程的发展及Python语言的普及，更多的扩展组件是由设计师自身开发，从而跨越了程序员这道障碍，这为参数化的系统化的发展提供了可以实现的基础。系统内容不是固定的，而是不断拓展的过程，内容越丰富，可以构建的参数化系统越自由，越完善，且更具有创造力。

### 4.2 大数据智能化分析的参数化及城市传感器有意识的城市网络
目前参数化可视化节点式编程平台，在建筑、公园或者城市设计的尺度上能够达到非常高的精细化设计程度，但是面对城市尺度或者更大的区域尺度下，通常需要简化细节满足当前计算机性能的需求。而大数据分析，以及机器学习甚至深度学习一般是在Python语言环境下完成数据的分析计算。Python编程语言可以通过代码的编写实现参数化功能，但是很难实现代码书写和设计过程（尤其三维模型实现）的实时反馈，最大的可能是将大数据分析及机器学习的算法嵌入到参数化设计平台下，实现具有参数化意义的大数据智能化分析技术，这样将会大大拓展参数化系统的内容。虽然已有多种机器学习算法的扩展组件，但是应用程度有限，随着计算机计算能力的提高，这一方向将会得到进一步的发展。

城市传感器的布置和实时数据的产生，可以构建有意识的城市网络，将更多为城市环境增加的新感知能力纳入到参数化的设计当中，通过具有参数化意义的大数据智能化分析，作为参数之一影响规划设计形式。

### 4.3 机器人建造和3D打印的继续发展
所有数字化设计要素和制造工作流程以及工作流程自身的研究开发有待进一步完成。这涉及到多个领域，包括设计算法和技术，更快、更一致的新型3D打印材料和流程，失效分析，大规模建筑制造和装配的供应链优化以及机器人建造技术。3D数字制造发展的同时，使用多材料打印机和纳米技术制造出自然界中找不到的材料的4D数字制造，以及对材料进行编程以实现交互操作的N-D制造也在发展。而云机器人这一新兴概念将允许机器人设备连接到互联网上，将其对计算性能的需求迁移到网络上，并促进机器人从其它机器人的经验中学习[30]。当这些领域整合到无缝的参数化数字工作流，对大规模定制建筑和产品开发将开启前所未有的创新。

## 5 结论与讨论
从上述归纳内容可知，参数化已经发展的非常成熟，涵盖了设计到施工与建造及后期维护的整个流程，其中发展相对成熟的是数据管理及处理，设计空间形式的分析、构建和优化生成，结构分析、构建与结构协同，可持续性设计及设计空间形式协同这四个主要方向，其中数据管理和处理是所有方向的基础。在风景园林领域BIM（或LIM）和GIS的参数化途径刚刚起步，因为受到风景园林中自然要素信息化难度的制约，以及丰富的尺度变化、信息量的攀升对计算性能的要求和相对建筑专业更自由的设计方式，都在一定程度上增加了这一方向的推进难度。机器人建造和3D打印与一个国家的工业化，尤其智能化程度相关，当前风景园林基本处于智能化设计和手工建造共处的时间段，这种状况预计持续相对比较长的时间。交互硬件和信息数据流方向是相对发展最弱的一支，这与当前的需求和相对要投入高资本相关，当信息化技术进一步发展，数据的规范与开源，及成本进一步降低，将会推动可感知城市的实现。

参数化和脚本工具拓展了设计和研究的范畴，进一步融合跨学科领域的知识，增强了解决问题的能力，提升了设计分析研究的效率。当前从构筑物，场地设计和公园，到风景名胜区和区域生态规划，越来越多的风景园林设计师和学者依赖参数化设计工具解决设计规划问题。虽然可视化节点编程和文本式脚本，使得设计师进入到了计算机编程的世界，进一步增强了设计师思考和提出更深层次问题的本质和能力，但是对参数化设计技术的掌握尤其设计思维的转变需要长时间（数月甚至数年）的学习和不断实践，并不是每个设计师都愿意或者有机会进入到这个领域，尤其掌握参数化所必须的数学和编程能力。每一设计团队一般有几名具有参数化设计能力的设计师成为当前组合的一种方式。而数字化设计技术的普及，尤其高校相关课程的开设，例如苏黎世联邦理工学院风景园林高级研究硕士项目，由一系列学习班和项目组成，以递增的方式教授编程。从介绍Rhino和GH的研讨会开始，逐步引入更具挑战性的概念和代码编程[7-221]。课程涉及一系列技术和设计技法、理论和实践观点，通过对最新的数字化设计技术和模拟建造技术的高强度运用，学生将有能力完成复杂的设计任务，并且发展出新的设计技法，这对于未来挑战性的工作有很大的意义[31]。东南大学在风景园林规划设计教学中引入了参数化方法，自2012年起在本科四年级“景园规划设计III”课程中进行多年教学实践，逐步形成了一套参数化风景园林规划设计教学方法[32]。这些都在一定程度上，推进了参数化的普及。


> 参考文献(References)：

```
[1] 蔡凌豪.风景园林数字化规划设计概念谱系与流程图解[J].风景园林,2013(01):48-57.
[2]	Jabi W. Parametric design for architecture[M]. London:Laurence King Publishing, September, 2013. 
[3]	Cantrell B, Mekies A.Coding landscape[M]// Cantrell B, Mekies A. Codify: Parametric and computational design in landscape architecture. New York: Routledge, May 2018:5-36
[4]	Woodbury R. Elements of parametric Design[M]. New York:Routledge, July, 2010.
[5]	匡纬.风景园林“参数化”规划设计发展现状概述与思考[J].风景园林,2013(01):58-64.
[6]	Peters B.Parametric environmental design: simulation and generative processes[M]//Peters B, Peters T.Computing the environment: digital design tools for simulation and visualisation of sustainable architecture.Italy:Wiley, April, 2018:28-42.
[7]	Phillips A H. The new maker culture: computation and participation in design[M]//Cantrell B, Mekies A. Codify: parametric and computational design in Landscape Architecture. New York: Routledge, May 2018:205-224.
[8]	Cantrell B, Mekies A.Coding landscape[M]// Cantrell B, Mekies A. Codify: Parametric and computational design in Landscape Architecture. New York: Routledge, May 2018:5-36.
[9]	Woodbury R. Elements of parametric design[M]. New York:Routledge, July, 2010 : 49-64.
[10]	Reschke C. From documents to directives: experimental fast matter[M]//Cantrell B, Mekies A. Codify: parametric and computational design in Landscape Architecture. New York: Routledge, May 2018:268-278.
[11]	Peters B. Bespoke tools for a better world: the art of sustainable design at burohappold engineering [M]//Peters B, Peters T.Computing the environment: digital design tools for simulation and visualisation of sustainable architecture.Italy:Wiley, April, 2018:138-149.
[12]	Aksöz Z, Preisinger C.An Interactive structural optimization of space frame structures using machine learning[M]//Gengnagel C, Gengnagel C,Burry J editor.Impact: Design with all senses: proceedings of the design modelling symposium, Berlin 2019.Switzerland:Springer,August, 2019:18-31.
[13]	Besserud K.Engaging with Complexity: computational algorithms in architecture and urban design[M]//Andia A, Spiegelhalter T. Post-parametric automation in design and construction.Norwood :Artech House, November, 2014:39-45.
[14]	Culbertson K. Technology, evolution, and an ecology of cities[M]//Cantrell B, Mekies A. Codify: Parametric and computational design in Landscape Architecture. New York: Routledge, May 2018:243-253.
[15]	Peters T. New dialogues about energy: performance, carbon and climate [M]// Peters B, Peters T.Computing the environment: digital design tools for simulation and visualisation of sustainable architecture.Italy:Wiley, April, 2018:14-27.
[16]	刘司南,吕锐,王霞.参数化风景园林设计的方法实践——以成都市环城生态区桂溪生态公园景观为例[J].中国园林,2017,33(05):50-55.
[17]	成玉宁,袁旸洋.山地环境中拟自然水景参数化设计研究[J].中国园林,2015,31(07):10-14.
[18]	周梁俊.英国建筑信息模型(BIM)战略及其在风景园林行业落实情况的分析[J].风景园林,2016(04):122-128.
[19]	郭湧.论风景园林信息模型的概念内涵和技术应用体系[J].中国园林,2020,36(09):17-22.
[20]	赖文波,杜春兰,贾铠针,江虹.景观信息模型(LIM)框架构建研究——以重庆大学B校区三角地改造为例[J].中国园林,2015,31(07):26-30.
[21]	Beorkrem C. Material strategies in digital fabrication[M].Oxon :Routledg, July, 2017:10-15.
[22]	BAÑÓN C, RASPALL F. 3d printing architecture: workflows, applications, and trends [M]. Singapore:Springer, October, 2020:1-11.
[23]	Osborn B. Coding behavior: the agency of material in landscape architecture[M]// Cantrell B, Mekies A. Codify: Parametric and computational design in Landscape Architecture. New York: Routledge, May 2018:180-195.
[24]	Peters T. Designers need feedback: research and practice by kierantimberlake [M]//Peters B, Peters T.Computing the environment: digital design tools for simulation and visualisation of sustainable architecture.Italy:Wiley, April, 2018:118-127.
[25]	Phelps B. Beyond heuristic design[M]//Cantrell B, Mekies A. Codify: Parametric and computational design in Landscape Architecture. New York: Routledge, May 2018:196-204.
[26]	Fraguada L E, Melsom J. Code matters: consequent digital tool making in landscape architecture[M]//Cantrell B, Mekies A. Codify: Parametric and computational design in Landscape Architecture. New York: Routledge, May 2018:225-240.
[27]	Reschke C. From documents to directives: experimental fast matter[M]// Cantrell B, Mekies A. Codify: Parametric and computational design in Landscape Architecture. New York: Routledge, May 2018:268-278.
[28]	Robledo A F. The role of query and convergence in next-generation tool sets[M]//Cantrell B, Mekies A. Codify: parametric and computational design in Landscape Architecture. New York: Routledge, May 2018:171-179.
[29]	袁旸洋,成玉宁.过程、逻辑与模型——参数化风景园林规划设计解析[J].中国园林,2018,34(10):77-82.
[30]	Andia A, Spiegelhalter T. Toward automating design and construction[M]//Andia A, Spiegelhalter T. Post-parametric automation in design and construction.Norwood :Artech House, November, 2014:19-25.
[31]	冯潇.瑞士苏黎世联邦理工学院风景园林数字化设计探索——以苏黎世联邦理工学院风景园林高级研修课程为例[J].中国园林,2015,31(11):50-54.
[32]	袁旸洋,成玉宁,李哲.虚实相生——参数化风景园林规划设计教学研究[J].中国园林,2017,33(10):35-40.
```

##  GH扩展库

<a href=""><img src="./imgs_parae/284.jpg" height="auto" width="auto"  title="digit-x"></a>

## GH书籍

<a href=""><img src="./imgs_parae/285.jpg" height="auto" width="auto"  title="digit-x"></a>

> 第1次课-2021_11_3_Wed_3-4节 :2课时 课程完成时间标识线。稍微增加了下一节内容。
---  