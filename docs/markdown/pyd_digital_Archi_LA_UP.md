> Created on Sun Jan 17 09\46\23 2021 @author: Richie Bao-Chicago.IIT

# [Guide to the knowledge structure of the digital LA|UP|Archi design](https://github.com/richieBao/guide_to_digitalDesign_of_LAUPArhi_knowledgeStruc)

参数化设计在建筑景观领域迅速崛起归功于2007年David Rutten在Robert McNeel and Associates（Rhino三维设计模型软件的创造者）构想了可视化编程工具Grasshopper（GH），基于图形用户界面（Graphical User Interface，GUI）的脚本引擎，使用可拖拽的数据块和函数（称为组件，Component）通过线连接起来，从而允许不懂文本式编程的用户使用计算逻辑操控Rhino工具并生成高度复杂的三维模型。在风景园林领域，GH不仅用于学术项目，也越来也多的用于专业项目。例如，Fletcher Studio工作室广泛应用GH模拟环境，并为旧金山South Park耗资280万美元的改造项目创建概念程序设计包和施工文档[7-208]。受GH影响，为简化用于建筑信息模型（Building Information Modeling, BIM）构建的Revit 软件中AEC工作流程，于2013年开发了可视化编程工具Dynamo，实现了BIM类软件中参数化的融入。

可视化编程的参数化工具通常并不是闭合的设计环境，融入了文本式的脚本编程语言，例如常用的Python语言，以及C#等，这样极大程度上拓展了设计师编写设计逻辑或算法的自由以及其拓展性。例如基于GH的扩展库高达约434组，涉猎到界面体验，数据管理分析，空间几何构建、分析、算法、生成，智能化设计，结构、可持续性设计，BIM和制造及机器人建造，3D打印，地理信息系统（Geographic Information System，GIS），及交互硬件等众多方向。

同时。另一种声音也在响起，大数据、机器学习、深度学习，智能化设计。

> 这次讲课的形式。思考了一些报告形式，PPT做报告？MindManager思维导图？markdown文本文件？记得很早很早以前，用过PPT，并还用其直接形成规划设计文本；后来更适应于思维导图的形式，可以自由的思考问题的关系，不用再管PPT该如何排版，甚至还要思考底图，这都桎梏了本该思维内容的速度；善用思维导图的同时，当在成为coder的时候，发现，coder的主要思维习惯是“惰性”，就是专注该专注的事情，其它的交给代码去实现，那么markdown成为了代码界不成文的流行记录笔记；再后来，便就是，编写代码，边运行，边记笔记的Jupyter(融合了markdown，代码交互式解释器，以及纯文本形式)。因此，我想了下，不妨用JupyterLab的形式给大家解释数字化设计，这似乎也契合了这一主题。

> 同时，也想到，教学效果的最大化，应该是能够尽可能分享所讲内容，并能够实践，这尤其在代码的世界里尤其重要。因此可以通过[guide_to_digitalDesign_of_LAUPArhi_knowledgeStruc](https://github.com/richieBao/guide_to_digitalDesign_of_LAUPArhi_knowledgeStruc),GitHub代码托管，获得该讲义内容。

## 1.Before and after 2010

> *It does not matter what tools you use, as long as it is a good design or planning.*

2010年也许是一个比较好的分界点，grasshopper的基本组件几近定型，相关的扩展组件也大概包括了各个方向；而同时，大数据和机器学习开始冒头，sklearn库的出现，标志机器学习开始普及；tesorflow出现，标志深度学习开始普及，而PyTorch的出现，则加剧了这一发展趋势。

### 1.1 In the era of handcraft
实际上这个阶段不会消失，因为这本身就是学习设计必经阶段，先不管逻辑思维、算法、机器/深度学习，只是简简单单的思考设计，并专注于设计自身，这很重要。在sketchup的年代，做个很多设计，亦知道SU的操作痛苦之处，为了获得干净的模型，不劳其烦的修改模型，甚至还掌握了很多不传之秘。当一遍遍修改设计（就是修改模型，因为设计是推敲出来的），设计的构想在一步步实现，并得以完善，真能体会到设计的喜悦之情。（腰间盘突出就是这么来的！）

这个设计，推敲了半年多的时间，因为还花了一个月时间把宋代的建筑相关的专著，例如<营造法式>，<梁思成6-7卷>，及比较<清式则例>，和宋代平行时代的一本日本造园专著<>研究了个遍，并考察了相关遗留唐宋建筑，和仿建。最终设计，觉得很满意，造出了宋代文人园林的气息。不过后来离开了规划院，专心攻读博士，从事数字化研究，很多后来的事情就不知了。只知这个项目在院里评委优秀项目，当然也拿到了设计费（设计师不得不关系的事情）。


```python
def imgs_slideShow(imgs_fp,suffix=r'jpg',scale=0.7):
    from PIL import Image
    import glob
    import numpy as np
    from tqdm.auto import tqdm
    
    import matplotlib.pyplot as plt
    import matplotlib.animation as animation    
    '''
    function - To dynamically show the images in a specified folder.
    '''   
    imgs_f=glob.glob(imgs_fp+'/*.'+suffix)
    #print(imgs_f)
    imgs=[Image.open(i) for i in imgs_f]
    img_resize=lambda img:img.resize([int(scale * s) for s in img.size] ) 
    imgs_resize=[img_resize(i) for i in imgs]
          
    fig=plt.figure(figsize=(20,10))
    ims=[[plt.imshow(f,animated=True,)] for f in imgs_resize]    
    anima=animation.ArtistAnimation(fig,ims,interval=1000, blit=True,repeat_delay=1000)
    return anima,ims
    
ancient_buildings_fp=r'.\imgs\ancientArhi_perspective'
anima,ims=imgs_slideShow(ancient_buildings_fp)

from IPython.display import HTML
HTML(anima.to_html5_video())
```


    
<video width='auto' height='auto' controls><source src="./imgs_pyd/g_antiqueArchi.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>
    



```python
import utils
imgs_fn=utils.filePath_extraction(r'.\imgs\ancientArchi_section',["jpg"]) 
imgs_root=list(imgs_fn.keys())[0]
imgsFn_lst=imgs_fn[imgs_root]
columns=1
scale=1
utils.imgs_layoutShow(imgs_root,imgsFn_lst,columns,scale,figsize=(20,20))
```


    
<a href=""><img src="./imgs_pyd/w_001.png" height='auto' width='auto' title="caDesign"></a> 
    


### 1.2 In the era of parametric design

> *Designers have gone from being tool users to tool makers.*

手工的方式的确是费时费力，尤其设计本身就是一个不断调整、修改的过程，建了一半或者整个设计的模型，说改就改的，而原模型不具有修改的弹性，因此通常需要边推敲边重建。那么参数化设计的方式则很大程度上改善了上述环境。虽然一开始的时候需要花费较多的精力构建形式逻辑，但是一旦完成，可以弹性实现不同尺寸，同一逻辑下的多个变化结构。

这里编写了宋代古建筑的各式斗拱构建，以及由其搭建的斗拱，可以清晰的观察到构件之间的搭接关系，和一些古代制式的房屋结构，这也正是参数化模型构建的逻辑。后来有学弟妹专门研究古建筑参数化结构，形成了更为丰富和细致的成果。


```python
import glob
import utils
imgs_dougong_fps=glob.glob(r'.\code\12_DouGong(Qing)\*.jpg')
columns=3;scale=1
utils.imgs_layoutShow_FPList(imgs_dougong_fps,columns,scale,figsize=(20,20))
```


    
<a href=""><img src="./imgs_pyd/w_002.png" height='auto' width='auto' title="caDesign"></a> 
    



```python
import glob
import utils
imgs_dougong_fps=glob.glob(r'.\code\11_AncientArchi\*.jpg')
columns=3;scale=1
utils.imgs_layoutShow_FPList(imgs_dougong_fps,columns,scale,figsize=(20,20))
```


    
<a href=""><img src="./imgs_pyd/w_003.png" height='auto' width='auto' title="caDesign"></a> 
    


因为参数化技术，拓展了设计形式的可能性，因此也会偶尔设计异形建筑，与结构师配合，调整参数关系，进行初步的结构计算，满足结构的要求。设计阶段由设计师模拟的结构，虽然与结构师专业模拟会有些出入，但是这确实使得设计结构趋于合理，避免结构分析阶段，因为设计结构的不合理，造成设计上较大的修改。这个设计的推敲，主要集中在几根结构线上，在满足功能基础上，使得360无死角的艺术空间形式呈现。


```python
def get_multipleFiletypes_fps_list(files_root,suffix=['jpg','png']):
    import os
    import glob
    '''
    function - Returns a list of file paths from a folder that specified multiple file types. 
    '''    
    exts=['*.{}'.format(i) for i in suffix]
    file_paths=[f for ext in exts for f in glob.glob(os.path.join(files_root, ext))]
    return file_paths

imgs_fish_root=r'.\code\44_architecture_fish'
imgs_fish_fps=get_multipleFiletypes_fps_list(files_root=imgs_fish_root,suffix=['jpg','png'])

columns=5;scale=1
utils.imgs_layoutShow_FPList(imgs_fish_fps,columns,scale,figsize=(20,20))
```


    
<a href=""><img src="./imgs_pyd/w_004.png" height='auto' width='auto' title="caDesign"></a> 
    


参数化设计领域包含的内容极其丰富，可以嵌入很多方向，下面给出了几个解决不同问题的案例，是在2012年左右的研究，可以快速的浏览。实际上，网络上搜索可以获得非常多的信息。


```python
import os
video_root=r'./video'
video_fps=get_multipleFiletypes_fps_list(files_root=video_root,suffix=['mp4'])
video_dic={os.path.splitext(os.path.basename(v))[-2]:v for v in video_fps}
print("files info:\n")
import pandas as pd
print(pd.DataFrame.from_dict(video_dic,orient='index'))
```

    files info:
    
                                                                                                   0
    01_Terrain design                                                  ./video\01_Terrain design.mp4
    02_Box flattened                                                    ./video\02_Box flattened.mp4
    03_Terrain correlation and costing path        ./video\03_Terrain correlation and costing pat...
    05_Hongqiao Evolutionary computation method    ./video\05_Hongqiao Evolutionary computation m...
    06_Plant community patch and species planning  ./video\06_Plant community patch and species p...
    08_Folding Architecture Concept                      ./video\08_Folding Architecture Concept.mp4
    17_13                                                                          ./video\17_13.mp4
    segmentation_FCN_RESNET101_animation            ./video\segmentation_FCN_RESNET101_animation.mp4
    


```python
from IPython.display import Video
Video(video_dic['01_Terrain design'])
```


<video width='auto' height='auto' controls><source src="./imgs_pyd/01_Terrain design.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>
<video width='auto' height='auto' controls><source src="./imgs_pyd/02_Box flattened.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>
<video width='auto' height='auto' controls><source src="./imgs_pyd/03_Terrain correlation and costing path.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>
<video width='auto' height='auto' controls><source src="./imgs_pyd/05_Hongqiao Evolutionary computation method.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>
<video width='auto' height='auto' controls><source src="./imgs_pyd/06_Plant community patch and species planning.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>
<video width='auto' height='auto' controls><source src="./imgs_pyd/08_Folding Architecture Concept.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>

估计grasshopper的扩展组件在2014年左右，才有30多组，但是最近再一统计，高达434组左右，这已经是一个不小的数字，包含的内容已经基本涵盖了各个方向，每个方向上也会有多组组件。因为越来越多的设计师和学者，在做设计和做研究的同时，就边设计，边研究，边形成组件以用于设计和研究，最后形成很多库，当然还有很多没有公布的扩展组件，用于设计公司内部使用，增加竞争力；或者研究者的学术成果，并未发表。

下面编写了一段代码，以Sankey图的形式把这400多个扩展库规律梳理，看下当前参数化设计领域主要的研究方向，为该领域的研究这也提供了选题的参考。以及，可以方便查找相关功能组件，快速定位插件名称，下载应用。


```python
def Sankey(material_yaml_path,title_text,html_savePath,font_size=10,height=7000,): #font_size=10,height=7000    
    import yaml,random,copy
    import numpy as np
    import pandas as pd
    import matplotlib.pyplot as plt
    from collections import OrderedDict
    cmaps=OrderedDict()
    
    '''
    function - 读取自定义的.yaml文件，建立桑基图（Sankey diagram）
    
    Paras:
    material_yaml_path - 数据文件路径
    title_text - 桑基图标题
    html_savePath - 桑基图保存为html文件路径
    font_size=10 - 图表字体大小
    height=7000 - 图表高度       
    '''
    
    #initialize parameters
    material_yaml_path=material_yaml_path
    title_text=title_text
    font_size=font_size
    height=height
    html_savePath=html_savePath

    with open(material_yaml_path,encoding='utf-8') as file:
        materials=yaml.safe_load(file)

    cmaps['Sequential']=['Greys', 'Purples', 'Blues', 'Greens', 'Oranges', 'Reds','YlOrBr', 'YlOrRd', 'OrRd', 'PuRd', 'RdPu', 'BuPu','GnBu', 'PuBu', 'YlGnBu', 'PuBuGn', 'BuGn', 'YlGn']
    classification=materials['mapping']['classification']
    classification_level_1=classification.keys()
    #classification_level_2=classification.values()
    cmaps_sequential=list(cmaps.values())[0]
    random.shuffle(cmaps_sequential)
    classification_level_1_colorSequential={k:v for k,v in zip(classification_level_1,cmaps_sequential[:len(classification_level_1)])}
    classification_level_1_color={key:list(plt.cm.get_cmap(v)(0))[:3]+[0.8] for key,v in classification_level_1_colorSequential.items()} #configure level_1 color
    classification_level_2_color={}
    for k,v in classification.items():
        classification_level_2_color.update({i:list(plt.cm.get_cmap(classification_level_1_colorSequential[k])(c_v)[:3])+[0.8] for i,c_v in zip(v,np.linspace(0.1,0.9,len(v)))}) #configure leve_2 color

    materials_data=pd.DataFrame([dic['data'] for dic in materials['materials'] if dic['data'][0]!=''],columns=['name','date','case','international','source'])    
    materials_data['color']=materials_data.source.apply(lambda row:{0:[31, 119, 180, 0.8],1:[255, 127, 14, 0.8],2:[44, 160, 44, 0.8],3:[214, 39, 40, 0.8],4:[148, 103, 189, 0.8],5:[140, 86, 75, 0.8]}[row])    

    flatten_lst=lambda lst: [m for n_lst in lst for m in flatten_lst(n_lst)] if type(lst) is list else [lst]
    label_idx={v:idx for idx,v in enumerate(flatten_lst(list(classification.values()))+list(classification.keys())+materials_data.name.to_list())}
    node_label=label_idx.keys()

    classification_level_1_color_255={k:[round(i*255) for i in v[:3]]+[v[-1]] for k,v in classification_level_1_color.items()}
    classification_level_2_color_255={k:[round(i*255) for i in v[:3]]+[v[-1]] for k,v in classification_level_2_color.items()}
    classification_color=copy.deepcopy(classification_level_1_color_255)
    classification_color.update(classification_level_2_color_255)
    classification_color.update({row['name']:row['color'] for idx,row in materials_data[['name','color']].iterrows()})
    node_color=[classification_color[label] for label in node_label]
    node_color_rgba=["rgba(%d,%d,%d,%.1f)"%(r,g,b,a) for r,g,b,a in node_color]

    link_data_=[]
    for dic in materials['materials']:
        if dic['data'][0]!='':
            link_data_.append([(dic['data'][0],link) for link in dic['link']])
    link_data=flatten_lst(link_data_)
    link_classification=flatten_lst([[(i,k) for i in v] for k,v in classification.items()])

    classification_sub=materials['mapping']['classification_sub']
    link_classification_sub=flatten_lst([[(i,k) for i in v] for k,v in classification_sub.items()])

    link_S_T=link_data+link_classification_sub #+link_classification

    link_source=[label_idx[s] for s,t in link_S_T]
    link_target=[label_idx[t] for s,t in link_S_T]

    link_num=len(link_S_T)
    link_value=[1]*link_num #further adjust
    opacity = 0.9
    link_color_rgba=[node_color_rgba[src].replace("0.8", str(opacity)) for src in link_target]
    link_label=[""]*link_num

    import plotly.graph_objects as go
    fig = go.Figure(data=[go.Sankey(
        valueformat = ".0f",
        valuesuffix = "TWh",
        # Define nodes
        node = dict(
          pad = 20,
          thickness = 15,
          line = dict(color = "black", width = 0.5),
          label=list(label_idx.keys()),
          color=node_color_rgba
        ),
        #domain=dict(column= ),
        # Add links
        link = dict(
          source=link_source,
          target=link_target,
          value=link_value,
          label=link_label,
          color=link_color_rgba
    ))])

    fig.update_layout(title_text=title_text,font_size=font_size,height=height) #fig.update_layout(title_text="参数化研究内容关联 <a href=''>link</a>",font_size=10)
    fig.show()
    fig.write_html("%s"%html_savePath)

material_yaml_path=r'./data/materials.yaml'    
title_text=r"Grasshopper App(add-on),) content correlation _Created on Sat Oct  3 21:18:36 2020 @author:Richie Bao"
font_size=10
height=7000
html_savePath=r"./html/Parameterized overview chart.html"
Sankey(material_yaml_path,title_text=title_text,html_savePath=html_savePath,font_size=font_size,height=height)   #Sankey(material_yaml_path,html_savePath,font_size,height)  
```

<a href=""><img src="./imgs_pyd/w_005.png" height='auto' width='auto' title="caDesign"></a> 


在无人驾驶项目中，上一阶段主要集中在空间模式的分析上。这一阶段则更偏向于规划/景观领域，探索数据分析、信息交换的途径，例如数据库的确定，OSM数据交换，3D数据交换，模型数据写入，以及后面还会涉及到各类智能化分析，工作仍在进行。其项目位于GiHub托管，[driverlessCity_LIM](https://github.com/richieBao/driverlessCity_LIM)，可以下载已开发的组件，以及数据库等文件，自行实验，或直接用于自己的设计当中。


```python
import matplotlib.pyplot as plt
from PIL import Image

img_LIM_fp=r'./imgs/misc/LIM.png'
img_LIM=Image.open(img_LIM_fp)
img_LIM
```


<a href=""><img src="./imgs_pyd/w_006.png" height='auto' width='auto' title="caDesign"></a> 
    



### 1.3 又回到手工作业时代--VR 


```python
from IPython.display import Video
Video('./video/vr-01.mp4')
```

<video width='auto' height='auto' controls><source src="./imgs_pyd/vr-01.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>



## 2.Coding in the present and future
如果仅仅是专注于参数化，并不能最大限度的发挥代码时代的优势，虽然节点式（GH,Dynamo）编程极大程度上改善了设计工具应用环境，但是某些设计想法仍然很难用已有的组件实现，这也就是为什么需要进一步深入到代码层面。再者，python的库据统计有137,000+198,826（python libraries+python packages），这个数量是及其惊人了，这代表了什么？代表了我们有无数的工具可以使用，如果不会写代码，那么这些宝贵的知识财富，可能就与你无缘了，多么可惜！

这方面的研究都在[城市空间数据分析方法——PYTHON语言实现](https://richiebao.github.io/Urban-Spatial-Data-Analysis_python/#/)中文版，和[Urban spatial data analysis method——python language implementation](https://richiebao.github.io/Urban-Spatial-Data-Analysis_python_EN/#/)英文版，章节仍然再更新，预计50大章。因为内容非常多，可以自行浏览，这里仅举一个通过深度学习下对象检测方法获取图像内容，用于城市空间特征分析的方法。


```python
from IPython.display import Video
Video('./video/17_13.mp4')
```


<video width='auto' height='auto' controls><source src="./imgs_pyd/17_13.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>




```python
Video('./video/segmentation_FCN_RESNET101_animation.mp4')
```


<video width='auto' height='auto' controls><source src="./imgs_pyd/segmentation_FCN_RESNET101_animation.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>



我们究竟需要哪些知识，才能够实现智能化分析和设计？这里把已有章节的知识点归类，并以网络结构的形式分析知识点之间的关系。通过这个网络结构，可以实现，如果想确定实现某项分析，那么通过连接边的查找，可以知道需要哪些知识。不过，粗略编写的这个网络结构代码实现，结构图表达不是非常的清晰，可以进一步改进。


```python
import pandas as pd
USDA_kp_fp=r'./data/urban spatial data analysis method knowledge points structure_CH_EN.xlsx'
USDA_kp=pd.read_excel(USDA_kp_fp,sheet_name='CH',header=[0,1],engine='openpyxl')
USDA_kp.columns = USDA_kp.columns.get_level_values(1)
USDA_kp
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
      <th>索引</th>
      <th>章节</th>
      <th>知识点</th>
      <th>代数_回归</th>
      <th>统计学</th>
      <th>线性代数</th>
      <th>微积分</th>
      <th>数据处理</th>
      <th>数据分析</th>
      <th>数据可视化</th>
      <th>计算机视觉</th>
      <th>机器学习</th>
      <th>深度学习</th>
      <th>框架_平台</th>
      <th>可获取数据</th>
      <th>数据库</th>
      <th>地理信息系统</th>
      <th>遥感影像</th>
      <th>实例</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>单个分类POI数据爬取与地理空间点地图</td>
      <td>百度地图开放平台</td>
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
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>单个分类POI数据爬取与地理空间点地图</td>
      <td>单个分类POI爬取</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
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
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>单个分类POI数据爬取与地理空间点地图</td>
      <td>将.csv格式的POI数据转换为pandas的DataFrame</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
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
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>单个分类POI数据爬取与地理空间点地图</td>
      <td>将数据格式为DataFramed的POI数据转换为GeoPandas地理空间数据GeoDat...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>单个分类POI数据爬取与地理空间点地图</td>
      <td>使用plotly库建立地图</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>170</th>
      <td>25</td>
      <td>Sentinel-2，(聚类)土地分类，VGG16，SegNet遥感影像语义分割/解译，超像...</td>
      <td>VGG16卷积神经网络</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>171</th>
      <td>25</td>
      <td>Sentinel-2，(聚类)土地分类，VGG16，SegNet遥感影像语义分割/解译，超像...</td>
      <td>ImageNet数据集</td>
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
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>172</th>
      <td>25</td>
      <td>Sentinel-2，(聚类)土地分类，VGG16，SegNet遥感影像语义分割/解译，超像...</td>
      <td>SegNet遥感影像语义分割/解译</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>173</th>
      <td>25</td>
      <td>Sentinel-2，(聚类)土地分类，VGG16，SegNet遥感影像语义分割/解译，超像...</td>
      <td>ISPRS dataset数据集</td>
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
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>174</th>
      <td>25</td>
      <td>Sentinel-2，(聚类)土地分类，VGG16，SegNet遥感影像语义分割/解译，超像...</td>
      <td>超像素级分割(superpixels-segmentation)，高空分辨率特征尺度界定，及...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>175 rows × 19 columns</p>
</div>




```python
def G_draw(G,layout='spring_layout',node_color=None,node_size=None,figsize=(30, 30),font_size=12):    
    import matplotlib
    import matplotlib.pyplot as plt
    import networkx as nx
    '''
    function - To show a networkx graph
    '''
    #解决中文显示问题
    plt.rcParams['font.sans-serif'] = ['DengXian'] # 指定默认字体 'KaiTi','SimHei'
    plt.rcParams['axes.unicode_minus'] = False # 解决保存图像是负号'-'显示为方块的问题
    fig, ax = plt.subplots(figsize=figsize)
    #nx.draw_shell(G, with_labels=True)
    layout_dic={
        'spring_layout':nx.spring_layout,   
        'random_layout':nx.random_layout,
        'circular_layout':nx.circular_layout,
        'kamada_kawai_layout':nx.kamada_kawai_layout,
        'shell_layout':nx.shell_layout,
        'spiral_layout':nx.spiral_layout,
    }

    pos=layout_dic[layout](G) 
    nx.draw(G_1,pos,with_labels=True,node_color=node_color,node_size=node_size,font_size=font_size)  #nx.draw(G, pos, font_size=16, with_labels=False)
    
import plotly.graph_objects as go
import networkx as nx

G_1=nx.from_pandas_edgelist(USDA_kp, source='章节', target='知识点',create_using=nx.DiGraph()) #,create_using=nx.DiGraph();G=G.to_directed() 
#G_1=nx.from_pandas_edgelist(USDA_kp, '章节','知识点')     

classi=['代数_回归', '统计学', '线性代数', '微积分', '数据处理', '数据分析','数据可视化', '计算机视觉', '机器学习', '深度学习', '框架_平台', '可获取数据', '数据库', '地理信息系统','遥感影像', '实例']
USDA_kp['edges']=USDA_kp.apply(lambda row:[(row['知识点'],i) for i in classi if row[i]==1],axis=1)

import utils
edges=utils.flatten_lst(USDA_kp['edges'].tolist())
G_1.add_edges_from(edges)

G_1.add_nodes_from(pd.unique(USDA_kp['章节']).tolist(),layer=0)
G_1.add_nodes_from(pd.unique(USDA_kp['知识点']).tolist(),layer=2)
G_1.add_nodes_from(classi,layer=3)
colors=[
    "gold",
    "violet",
    "limegreen",
    "darkorange",
]
layer_colors=[colors[data["layer"]] for v, data in G_1.nodes(data=True)]
connections_num=[(len(list(nx.edges(G_1,[n])))+2)*50  for n in list(G_1.nodes())]
print("_"*50)
```

    __________________________________________________
    


```python
G_draw(G_1,font_size=11,node_color=layer_colors,node_size=connections_num,layout='spring_layout')
nx.write_gpickle(G_1,'./model/G_1.pkl') #nx.read_gpickle('./model/G_1.pkl')
```


    
<a href=""><img src="./imgs_pyd/w_007.png" height='auto' width='auto' title="caDesign"></a> 
    

```python
import hvplot.networkx as hvnx

hvnx.draw_spring(G_1, node_color=layer_colors,  font_size='10pt',with_labels=True,arrowstyle='->',arrowsize=0.1,width=2000, height=2000) #labels='club', font_size='10pt', node_color='club', cmap='Category10',

```

```python
import pandas as pd
USDA_kp_fp=r'./data/urban spatial data analysis method knowledge points structure_CH_EN.xlsx'
USDA_kp=pd.read_excel(USDA_kp_fp,sheet_name='CH',header=[0,1],engine='openpyxl')
USDA_kp.columns = USDA_kp.columns.get_level_values(1)
classi=['代数_回归', '统计学', '线性代数', '微积分', '数据处理', '数据分析','数据可视化', '计算机视觉', '机器学习', '深度学习', '框架_平台', '可获取数据', '数据库', '地理信息系统','遥感影像', '实例']
USDA_kp['edges']=USDA_kp.apply(lambda row:[(row['知识点'],i) for i in classi if row[i]==1],axis=1)
```

```python
import plotly.graph_objects as go
import matplotlib.pyplot as plt
from matplotlib import colors
import pandas as pd
import numpy as np
import itertools
import random

#colors = [colors.to_rgba(c) for c in plt.rcParams['axes.prop_cycle'].by_key()['color']]

chapter=pd.unique(USDA_kp['章节']).tolist()
Kp=pd.unique(USDA_kp['知识点']).tolist()
classi=list(USDA_kp.columns)[2:-1]

nodes=chapter+Kp+classi
nodes_idx_mapping={v:i for i,v in enumerate(nodes)}

opacity=0.4
cmap=plt.get_cmap('gist_ncar') #'gnuplot'
list_itemReplace=lambda lst,idx,v:[lst[i] if i!=idx else v for i in range(len(lst)) ]
nodes_colors=['rgba'+str(tuple(list_itemReplace([int(j*255) for j in cmap(i)],3,opacity))) for i in np.linspace(0, 1, len(nodes))]
random.shuffle(nodes_colors)

chapter2Kp=list(USDA_kp[['章节','知识点']].itertuples(index=False,name=None))
Kp_classi=list(itertools.chain(*USDA_kp['edges'].to_list()))
edges_mapping=chapter2Kp+Kp_classi
edge_source=[nodes_idx_mapping[i[0]] for i in edges_mapping]
edge_target=[nodes_idx_mapping[i[1]] for i in edges_mapping]

link_num=len(edge_source)
link_value=[1]*link_num #further adjust

opacity_edge=0.2
link_color_rgba=[nodes_colors[src].replace(str(opacity),str(opacity_edge)) for src in edge_target]
link_label=[""]*link_num
```

```python
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


title_text="城市空间数据分析方法——知识点关联_完成部分"
font_size=13
height=2700
fig.update_layout(title_text=title_text,font_size=font_size,height=height)
fig.show()

html_savePath='./html/城市空间数据分析方法——知识点关联.html'
fig.write_html("%s"%html_savePath)

```

<a href=""><img src="./imgs_pyd/046.png" height='auto' width='auto' title="caDesign"></a> 



```python
import pandas as pd
USDA_kp_fp=r'./data/urban spatial data analysis method knowledge points structure_CH_EN.xlsx'
USDA_kp=pd.read_excel(USDA_kp_fp,sheet_name='tools',header=[0,1],engine='openpyxl')
USDA_kp.columns = USDA_kp.columns.get_level_values(1)
classi=['代数_回归', '统计学', '线性代数', '微积分', '数据处理', '数据分析','数据可视化', '计算机视觉', '机器学习', '深度学习', '框架_平台', '可获取数据', '数据库', '地理信息系统','遥感影像', '实例']
USDA_kp['edges']=USDA_kp.apply(lambda row:[(row['代码工具'],i) for i in classi if row[i]==1],axis=1)
```


```python
import plotly.graph_objects as go
import matplotlib.pyplot as plt
from matplotlib import colors
import pandas as pd
import numpy as np
import itertools
import random

#colors = [colors.to_rgba(c) for c in plt.rcParams['axes.prop_cycle'].by_key()['color']]

chapter=pd.unique(USDA_kp['章节']).tolist()
Kp=pd.unique(USDA_kp['代码工具']).tolist()
classi=list(USDA_kp.columns)[2:-1]

nodes=chapter+Kp+classi
nodes_idx_mapping={v:i for i,v in enumerate(nodes)}

opacity=0.4
cmap=plt.get_cmap('gist_stern') #'gnuplot' ; 'gist_ncar'; 'nipy_spectral'
list_itemReplace=lambda lst,idx,v:[lst[i] if i!=idx else v for i in range(len(lst)) ]
nodes_colors=['rgba'+str(tuple(list_itemReplace([int(j*255) for j in cmap(i)],3,opacity))) for i in np.linspace(0, 1, len(nodes))]
random.shuffle(nodes_colors)

chapter2Kp=list(USDA_kp[['章节','代码工具']].itertuples(index=False,name=None))
Kp_classi=list(itertools.chain(*USDA_kp['edges'].to_list()))
edges_mapping=chapter2Kp+Kp_classi
edge_source=[nodes_idx_mapping[i[0]] for i in edges_mapping]
edge_target=[nodes_idx_mapping[i[1]] for i in edges_mapping]

link_num=len(edge_source)
link_value=[1]*link_num #further adjust

opacity_edge=0.2
link_color_rgba=[nodes_colors[src].replace(str(opacity),str(opacity_edge)) for src in edge_target]
link_label=[""]*link_num
```


```python
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


title_text="城市空间数据分析方法——代码工具关联_完成部分"
font_size=13
height=3000
fig.update_layout(title_text=title_text,font_size=font_size,height=height)
fig.show()

html_savePath='./html/城市空间数据分析方法——代码工具关联.html'
fig.write_html("%s"%html_savePath)
```

<a href=""><img src="./imgs_pyd/047.png" height='auto' width='auto' title="caDesign"></a> 

<a href=""><img src="./icon/ant_05.png" height='auto' width='350' title="caDesign"></a> 