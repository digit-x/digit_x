> Created on Fri Dec 23 07:23:20 2022 @author: richie bao

# 参数化设计——小白->实践->探索

> 成为工具的建构者——具有编程能力的设计师！

```mermaid
gantt

dateFormat  YYYY-MM
title 报告人的参数化设计探索历程
excludes weekdays 2014-01-10

section A 学习历程(0-9段)
小白0段（花了1周，全日制，学习跟练官方教材，照学照做***思考数据结构） 2009.07              :done,    des2, 2009-07,2009-08
小白1段（设计院项目实践，一个逻辑思考甚至花费2天时间，有些则出不来。就是连线，连来连去的；设计思维和逻辑思维开始协调）2009.07 - 2010.07        :done,    des2, 2009-07,2010-07
小白2->3段（系统学习梳理所有基础组件功用，拓展扩展组件，形成可随时查阅的组件字典——还是传统编辑模式，非程序员文本编辑工具 ）2010.07 - 2012.07 :done,    des2, 2010-07,2012-07
小白？段（花了1周，全日制，学习了VB script，后发现无用，弃之。权当练习了代码思维 ） :done,    des2, 2010-10,2010-11
小白🠉段（花了10天左右，全日制，学习了Python script。真正进入到程序员领域，理解了参数化与代码千丝万缕的联系）2011.01 :done,    des2, 2011-01,2011-02
小白4——>5段（快跟学Python大部头书+PythonScript设计实践+参数化设计实践+反复应用练习，螺旋循环积累）2011.01 - 2015.01 :done,    des2, 2011-01,2015-01
小白6段（竟然花了1.5-2年的时间自修电子工程->机器人建造）2015.01 - 2017.02 :done,    des2, 2015-01,2017-02
小白？段（掣che肘，思考进入Python大数据+机器/深度学习->数据分析<-->参数化）2016.02 - 2017.08 :done,    des2, 2017-02,2017-08
小白7段（城市空间数据分析方法<-->参数化_成为工具的建构者）2017.08 -  :active,    des2, 2017-08,2027-08

section B 教材专著
《参数化逻辑构建过程》《参数模型构建》《折叠的程序》《编程景观》2010.07 - 2015.06            :done,    des3, 2010-07,2015-06
《ArcGIS下的Python编程》《学习PYTHON—做个有编程能力的设计师》2010.07 - 2015.06            :done,    des3, 2010-07,2015-06
《城市空间数据分析方法》2017.08 - 2023.12            :active,    des4, 2017-08,2023-12
《参数化设计编程——GRASSHOPPER | 构建模组库——成为工具的建构者》》 2022.01 - 2023.12         :active,    des5, 2022-01,2023-12
《参数化设计编程——GHPython | 用算法做决策——面向设计师的参数化认知逻辑》 2022.01 -  2024.12          :active,    des6, 2022-01,2024-12
《微控制器城市空间实验与C语言》 2024.01 -             :active    des8, 2024-01,2026-01

section C 项目实践
学习-实践-学习-实践-学习的循环 2009.07 -             :active,    des2, 2009-07,2027-08
```

## :o: 踩过的坑（人生苦短）

> 你我以为的参数化设计领域是否相同？

:interrobang: Microsoft 365究竟是为谁服务的？为格式（样式）困扰值得吗？和做笔记的重要性

* 做笔记的目的：

1. 快速记录，不用过度关心样式格式；
2. 代码格式高亮显示，具有易读性；
3. 方便文件管理，可有序无限扩展；
4. 易于搜索，快速定位搜索主题；
5. 可以发布在线，在线更新**知识积累**，随时随地查看及分享；
6. 方便不同平台、格式转换，具有广泛的弹性，易于迁移；
7. **自主性**。

| 轻量级标记语言  | 解释器  |集成式|在线版|
|---|---|---|---|
| <img src="./imgs_p/param_practicce2exploration/01.png" height="auto" width=50  title="digit-x"> <br/> [markdown](https://www.markdownguide.org/)| <img src="./imgs_p/param_practicce2exploration/02.png" height="auto" width=50  title="digit-x"> <br/> [Visual Studio Code（VSC）](https://code.visualstudio.com/)  |<img src="./imgs_p/param_practicce2exploration/03.jpg" height="auto" width=50  title="digit-x"> <br/> [jupyterlab](https://jupyter.org/)|<img src="./imgs_p/param_practicce2exploration/04.png" height="auto" width=50  title="digit-x"> [Colab](https://colab.research.google.com/notebooks/welcome.ipynb?authuser=1#scrollTo=5fCEDCU_qrC0)<br/><img src="./imgs_p/param_practicce2exploration/05.png" height="auto" width=50  title="digit-x"> [kaggle](https://www.kaggle.com/code)|


:interrobang: 做了那么多设计，写了那么多逻辑代码，为什么新的设计任务还要重新思考写逻辑？ 

* 解决的方法⟶建立模组库|包+案例的必要性

1. 对于GH，建立模组；

<img src="./imgs_p/param_practicce2exploration/06.png" height="auto" width="auto"  title="digit-x"> 


<img src="./imgs_p/param_practicce2exploration/07_s.jpg" height="auto" width="auto"  title="digit-x"> 

2. 对于Python，建立包。

例如：[城市空间数据分析方法-USDA 库手册](https://richiebao.github.io/USDA_PyPI/#/)

:interrobang: 你在做别人已完成的工作，为什么要从0开始及必须从0开始？

<span style = "color:Goldenrod;background-color:;font-size:15.0pt">避免重复造轮子！</span>

已有扩展或库不稳定时，需要自行写代码。

:interrobang: 什么都需要藏着掖着吗？版权与分享精神，共建社区




:interrobang: 参数化设计工具是工具的按钮吗？，参数化设计是工具也不是工具



:interrobang: 参数化设计只是参数化吗？横行霸道的领域拓展


:interrobang: 太多东西，麻爪了？抓住核心——代码（编程）



:interrobang: 学写代码背完就了之了？练习实践反复的不断重复




##  :shipit: 学习途径推荐




## 1个地球，1个世界！