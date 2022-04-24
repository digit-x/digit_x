> Created on Sun Jan 17 09\46\23 2021 @author: Richie Bao-Chicago.IIT

**Ruiqing BAO: Guide to the knowledge structure of the digital LA|UP|Archi design-Illinois Institute of Technology 2021-1-23**

# [Guide to the knowledge structure of the digital LA|UP|Archi design](https://github.com/richieBao/guide_to_digitalDesign_of_LAUPArhi_knowledgeStruc)

The rapid rise of parametric design in landscape architecture in 2007 was attributed to the visualization programming tool Grasshopper(GH) conceived by David Rutten in Robert  McNeel and Associates ( Rhino 3D design model software creators). It is based on a Graphical User Interface(GUI) scripting engine that uses drag-and-drop data blocks and functions (called components) wired together. And it allows users who do not understand text programming to manipulate the Rhino tool using computational logic and generate highly complex 3-dimensional models. In the landscape architecture field, GH is used not only for design projects but also for academic projects. For example, Fletcher Studio used GH simulation environments extensively and created concept programming packages and construction documentation for a 2.8 million dollars renovation project in San Francisco's South Park. Influenced by GH, to simplify the AEC(Architecture, Engineering, and Construction) workflow in Revit software used for Building Information Modeling(BIM) construction, visual programming tool Dynamo was developed in 2013 to realize the integration of parameterization in BIM software.

Parametric tools for visual programming are often not closed design environment, but rather textual scripting languages such as Python, commonly used, and C#. It dramatically expands the freedom and extensibility of designers to write design logic or algorithms. For example, based on the extension of the GH library of about 434 groups, in the interface, data management, spatial geometrical construction and analysis, algorithm, intelligent design, structure, and sustainable design, BIM and manufacturing and robot construction, 3d printing, GIS(Geographic Information System), and the interaction of hardware and so on.。

Simultaneously, another voice is also rising, big data, machine learning, deep learning, and intelligent design.

> About this lecture's format, I thought about some forms of presentation, PowerPoint presentation? MindManager mind map? Markdown text files? I remember that I used PPT long ago and used it to form planning and design text directly. Later, I was adapted to the form of a mind map, and I could freely think about the relationship between the issues. I did not have to worry about how the PPT should be typeset or even think about the base map, which restricted the thinking content's speed. While making fair use of the mind map, when I am a coder, I find that the coder's main thinking habit is 'laziness', which is focused on what should concentrate and leave the rest to the code to implement. Markdown then became the unwritten popular note of the code world. Then there was the Jupyter (a mashup of Markdown, code interactive interpreters, and plain text), which wrote code and took notes as it ran. So I thought about explaining digital design in the form of JupyterLab, which seems to fit the theme.

> At the same time, I also thought that the maximum effect of teaching should be to share as much as possible and practice, which is especially important in the world of code. So you can get the content of the handout through [guide_to_digitalDesign_of_LAUPArhi_knowledgeStruc](https://github.com/richieBao/guide_to_digitalDesign_of_LAUPArhi_knowledgeStruc),GitHub code hosting. 

## 1.Before and after 2010

> *It does not matter what tools you use, as long as it is a good design or planning.*

2010 may be a better demarcation point, the necessary components of Grasshopper almost finalize, related extension components also probably includes all directions. At the same time, big data and machine learning began to emerge. The appearance of the Sklearn library of Python marked the popularization of machine learning. The emergence of TensorFlow marked the beginning of the popularity of deep learning, and the emergence of PyTorch accelerated this trend.

### 1.1 In the era of handcraft
This stage will not go away because it is a necessary stage to learn design. Regardless of logical thinking, algorithms, machine/deep learning, it is essential to think about design itself. In the days of SketchUp, I did a lot of design work, and I knew the pain of SketchUp. We have to modify the model to make it clean, and we even can learn a lot about the model editing secrets. When the design is changed repeatedly,  the design concept step by step to achieve and to perfect, really can feel the joy of creation. (The lumbar disc protrusion is so come!)

This design took more than half of a year to elaborate. Because it also took a month to study the Song Dynasty architecture-related monographs, such as '营造法式 /building mode,' '梁思成全集6-7卷 /Liang Sicheng Complete Works 6-7 volumes', and a Japnese garden treatise from the parallel period of the Song Dynasty, '作庭记 /garden design.' We studied it all over and investigated the relevant legacy Tang and Song buildings and imitation. However, I left the planning institute to finish a Ph.D. and engage in digital research. Much of what happened later was lost. I only know that this project was rated as an excellent project in our institute. Of course, we received the design fee(Things designers have to relate to). 


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




<video width='auto' height='auto' controls><source src="./imgs/g_antiqueArchi.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>

```python
import utils
imgs_fn=utils.filePath_extraction(r'.\imgs\ancientArchi_section',["jpg"]) 
imgs_root=list(imgs_fn.keys())[0]
imgsFn_lst=imgs_fn[imgs_root]
columns=1
scale=1
utils.imgs_layoutShow(imgs_root,imgsFn_lst,columns,scale,figsize=(20,20))
```


    
<a href=""><img src="./imgs/w_001.png" height='auto' width='auto' title="caDesign"></a>  
    


### 1.2 In the era of parametric design

> *Designers have gone from being tool users to tool makers.*

The manual way is time-consuming and laborious, especially in constant adjustment and modification. Built half or the whole design of the model, at any time to modify, and the original model does not have the flexibility to adjust, so usually need to deliberate while rebuilding. Parametric design improves the above environment to a large extent. Although it takes a lot of effort to build formal logic initially, it can flexibly implement multiple variation structures of different sizes under the same design logic once it is completed.

The various DouGong parts of ancient buildings in the Song Dynasty and the DouGong constructed by parts are compiled here. We can observe the construction relationship between elements and some antique buildings structure, which is also the parametric model construction logic. Later, some scholars specialized in studying ancient buildings' parametric systems, forming more rich and detailed results.


```python
import glob
import utils
imgs_dougong_fps=glob.glob(r'.\code\12_DouGong(Qing)\*.jpg')
columns=3;scale=1
utils.imgs_layoutShow_FPList(imgs_dougong_fps,columns,scale,figsize=(20,20))
```


    
<a href=""><img src="./imgs/w_002.png" height='auto' width='auto' title="caDesign"></a>  
    



```python
import glob
import utils
imgs_dougong_fps=glob.glob(r'.\code\11_AncientArchi\*.jpg')
columns=3;scale=1
utils.imgs_layoutShow_FPList(imgs_dougong_fps,columns,scale,figsize=(20,20))
```


    
<a href=""><img src="./imgs/w_003.png" height='auto' width='auto' title="caDesign"></a> 
    


Due to parameterization technology, which expands the possibility of design forms, there are occasional special-form buildings. Cooperate with the structural engineer, adjust the parameter relationship, carry out the preliminary structural calculation, and meet the structure's requirements. The construction/building structure simulated by the designer in the design stage is different from the structural engineer's professional simulation. Still, it makes the design structure reasonable and avoids the unreasonable design structure in the structural analysis stage, resulting in extensive modifications. 


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


    
<a href=""><img src="./imgs/w_004.png" height='auto' width='auto' title="caDesign"></a> 
    


The parametric design contains rich content, which can be embedded in many directions. The following are several cases of solving different problems, which were studied around 2012, and can be quickly looked over. And there is a lot of information available on the internet.


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
Video(video_dic['02_Box flattened'])
```




<video width='auto' height='auto' controls><source src="./imgs/01_Terrain design.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>
<video width='auto' height='auto' controls><source src="./imgs/02_Box flattened.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>
<video width='auto' height='auto' controls><source src="./imgs/03_Terrain correlation and costing path.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>
<video width='auto' height='auto' controls><source src="./imgs/05_Hongqiao Evolutionary computation method.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>
<video width='auto' height='auto' controls><source src="./imgs/06_Plant community patch and species planning.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>
<video width='auto' height='auto' controls><source src="./imgs/08_Folding Architecture Concept.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>


Estimated Grasshopper extension Add-Ons in 2014 or so, there are more than 30 groups. Still, up to 434 groups, recent statistics are already a large number, which has covered each direction because more and more designers and scholars form all kinds of components(being libraries of GH) while doing design and research. Of course, many unannounced extensions are designed for use within the company to increase competitiveness; Or researchers whose academic work is not published.

Here is a piece of a code snippet in the form of a Sankey diagram of the more than 400 extension libraries. Let us look at the main research directions in the parametric design field at present, which also references the research in this field. It is also easy to find relevant functional components, quickly locate the plug-in name, and download the application.


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
title_text=r"Grasshopper App(add-on),) content correlation _Created on Sat Oct  3 21:18:36 2020; updated on Sun Jan 24 21:52:32 2021 @author:Richie Bao"
font_size=10
height=7000
html_savePath=r"./html/Parameterized overview chart.html"
Sankey(material_yaml_path,title_text=title_text,html_savePath=html_savePath,font_size=font_size,height=height)   #Sankey(material_yaml_path,html_savePath,font_size,height)  
```

<a href=""><img src="./imgs/w_005.png" height='auto' width='auto' title="caDesign"></a> 

In the driverless city project, the last stage mainly focused on the analysis of space mode. This phase is more in planning/landscape, exploring data analysis and information exchange approaches, such as database, OSM data exchange, 3D building data exchange, model data writing, and later related to various kinds of intelligent analysis. Work is still going on. The project is uploaded to the GitHub repository, [driverlessCity_LIM] (https://github.com/richieBao/driverlessCity_LIM), you can download the developed components, as well as database files. You can experiment with it yourself or use it directly in your design project.

```python
import matplotlib.pyplot as plt
from PIL import Image

img_LIM_fp=r'./imgs/misc/LIM.png'
img_LIM=Image.open(img_LIM_fp)
img_LIM
```

<a href=""><img src="./imgs/w_006.png" height='auto' width='auto' title="caDesign"></a> 

### 1.3 It is back to manual work.--VR 

Oculus Quest 2+Gravity sketch


```python
from IPython.display import Video
Video('./video/vr-01.mp4')
```

<video width='auto' height='auto' controls><source src="./imgs/vr-01.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>

## 2.Coding in the present and future
Focusing solely on parameterization does not maximize the benefits of the Code Age. Although GH(Dynamo) programming has dramatically improved the environment in which design tools are used, some design ideas are still difficult to implement with existing components, which is why it is necessary to drill down to the code level. There are 137,000+198,826（python libraries+python packages）in Python. What does it mean? It represents that we have countless tools to use. If you can not write code, then this precious wealth of knowledge may not be with you; what a pity!

The studies in this field are all in (https://richiebao.github.io/Urban-Spatial-Data-Analysis_python/#/) CH，和[Urban spatial data analysis method——python language implementation](https://richiebao.github.io/Urban-Spatial-Data-Analysis_python_EN/#/)EN, with an estimated 50 chapters. Since there is a lot of content, you can browse it by yourself. Here is just a method to obtain image object through object detection of deep learning and apply it to urban spatial feature analysis.


```python
from IPython.display import Video
Video('./video/17_13.mp4')
```

<video width='auto' height='auto' controls><source src="./imgs/17_13.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>


```python
Video('./video/segmentation_FCN_RESNET101_animation.mp4')
```

<video width='auto' height='auto' controls><source src="./imgs/segmentation_FCN_RESNET101_animation.mp4" height='auto' width='auto' title="caDesign" type='video/mp4'></video>


What knowledge do we need to achieve intelligent analysis and design? The following code categorizes the existing chapters' knowledge points and analyzes the relationships between them in a network structure. With this network structure, we can realize that you can know what knowledge you need by looking up the connection edges to determine how to perform an analysis. However, the roughly written code implementation of the network structure, its structure diagram expression is not very clear, can be further improved


```python
import pandas as pd
USDA_kp_fp=r'./data/urban spatial data analysis method knowledge points structure_CH_EN.xlsx'
USDA_kp=pd.read_excel(USDA_kp_fp,sheet_name='EN',header=[0,1],engine='openpyxl')
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
      <th>index</th>
      <th>chapter</th>
      <th>knowledge point</th>
      <th>algebra_regression</th>
      <th>statistics</th>
      <th>linear algebra</th>
      <th>calculus</th>
      <th>data processing</th>
      <th>data analysis</th>
      <th>data visulization</th>
      <th>computer vision</th>
      <th>machine learning</th>
      <th>deep learning</th>
      <th>framwork_platform</th>
      <th>data available</th>
      <th>darabase</th>
      <th>GIS</th>
      <th>RS</th>
      <th>examples</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Baidu Map POI data crawler and geospatial poin...</td>
      <td>Baidu map open platform</td>
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
      <td>Baidu Map POI data crawler and geospatial poin...</td>
      <td>Single category POI crawl</td>
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
      <td>Baidu Map POI data crawler and geospatial poin...</td>
      <td>Convert POI data in .csv format to Pandas' Dat...</td>
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
      <td>Baidu Map POI data crawler and geospatial poin...</td>
      <td>Convert the POI data with data format DataFram...</td>
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
      <td>Baidu Map POI data crawler and geospatial poin...</td>
      <td>Create a map using the Plotly library</td>
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
      <td>Sentinel-2, (clustering) land classification, ...</td>
      <td>VGG16 convolutional neural network</td>
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
      <td>Sentinel-2, (clustering) land classification, ...</td>
      <td>ImageNet dataset</td>
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
      <td>Sentinel-2, (clustering) land classification, ...</td>
      <td>SegNet Semantic segmentation/interpretation of...</td>
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
      <td>Sentinel-2, (clustering) land classification, ...</td>
      <td>ISPRS dataset dataset</td>
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
      <td>Sentinel-2, (clustering) land classification, ...</td>
      <td>superpixels-segmentation, the definition of hi...</td>
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

G_1=nx.from_pandas_edgelist(USDA_kp, source='chapter', target='knowledge point',create_using=nx.DiGraph()) #,create_using=nx.DiGraph();G=G.to_directed() 
#G_1=nx.from_pandas_edgelist(USDA_kp, '章节','知识点')     

classi=['algebra_regression','statistics', 'linear algebra', 'calculus', 'data processing','data analysis', 'data visulization',
        'computer vision', 'machine learning', 'deep learning', 'framwork_platform','data available', 'darabase', 'GIS', 'RS', 'examples']
USDA_kp['edges']=USDA_kp.apply(lambda row:[(row['knowledge point'],i) for i in classi if row[i]==1],axis=1)

import utils
edges=utils.flatten_lst(USDA_kp['edges'].tolist())
G_1.add_edges_from(edges)

G_1.add_nodes_from(pd.unique(USDA_kp['chapter']).tolist(),layer=0)
G_1.add_nodes_from(pd.unique(USDA_kp['knowledge point']).tolist(),layer=2)
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

<a href=""><img src="./imgs/w_007.png" height='auto' width='auto' title="caDesign"></a> 

```python
import hvplot.networkx as hvnx
hvnx.draw_spring(G_1, node_color=layer_colors,  font_size='10pt',with_labels=True,arrowstyle='->',arrowsize=0.1,width=2000, height=2000) #labels='club', font_size='10pt', node_color='club', cmap='Category10',
```


```python
import pandas as pd
USDA_kp_fp=r'./data/urban spatial data analysis method knowledge points structure_CH_EN.xlsx'
USDA_kp=pd.read_excel(USDA_kp_fp,sheet_name='EN',header=[0,1],engine='openpyxl')
USDA_kp.columns = USDA_kp.columns.get_level_values(1)
classi=['algebra_regression','statistics', 'linear algebra', 'calculus', 'data processing','data analysis', 'data visulization',
        'computer vision', 'machine learning', 'deep learning', 'framwork_platform','data available', 'darabase', 'GIS', 'RS', 'examples']
USDA_kp['edges']=USDA_kp.apply(lambda row:[(row['knowledge point'],i) for i in classi if row[i]==1],axis=1)
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

chapter=pd.unique(USDA_kp['chapter']).tolist()
Kp=pd.unique(USDA_kp['knowledge point']).tolist()
classi=list(USDA_kp.columns)[2:-1]

nodes=chapter+Kp+classi
nodes_idx_mapping={v:i for i,v in enumerate(nodes)}

opacity=0.4
cmap=plt.get_cmap('gist_ncar') #'gnuplot'
list_itemReplace=lambda lst,idx,v:[lst[i] if i!=idx else v for i in range(len(lst)) ]
nodes_colors=['rgba'+str(tuple(list_itemReplace([int(j*255) for j in cmap(i)],3,opacity))) for i in np.linspace(0, 1, len(nodes))]
random.shuffle(nodes_colors)

chapter2Kp=list(USDA_kp[['chapter','knowledge point']].itertuples(index=False,name=None))
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


title_text="Urban spatial data analysis method--knowledge points correlation"
font_size=13
height=2700
fig.update_layout(title_text=title_text,font_size=font_size,height=height)
fig.show()

html_savePath='./html/Urban spatial data analysis method--knowledge points correlation.html'
fig.write_html("%s"%html_savePath)
```

<a href=""><img src="./imgs/w_008.png" height='auto' width='auto' title="caDesign"></a> 


# It is a great opportunity to share with you some research in the field of digital design that may plant a seed in your mind that you may not notice until later. 

# **THX！**
