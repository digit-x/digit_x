# 1. 城市空间实验->微控制器|传感器->C语言

## 1.1 城市空间实验+Robot

### 1.1.1 [Array of things](https://arrayofthings.github.io/)

> citing: https://arrayofthings.github.io/

__1）数据获取阶段——微控制器+传感器（估计为C/C++嵌入式编程）获取城市空间环境数据流。数据下载地址：[Waggle Datasets](https://www.mcs.anl.gov/research/projects/waggle/downloads/datasets/index.php)__

<video height='auto' width=100% controls><source src="./video/Array of Things Introductory Video.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>

What if a light pole told you to watch out for an icy patch of sidewalk ahead? What if an app told you the most populated route for a late-night walk to the El station by yourself? What if you could get weather and air quality information block-by-block, instead of city-by-city?

The Array of Things (AoT) is an experimental urban measurement system comprising programmable, modular "nodes" with sensors and computing capability so that they can analyze data internally, for instance counting the number of vehicles at an intersection (and then deleting the image data rather than sending it to a data center). AoT nodes are installed in Chicago and a growing number of partner cities to collect real-time data on the city’s environment, infrastructure, and activity for research and public use. The concept of AoT is analogous to a “fitness tracker” for the city, measuring factors that impact livability in the urban environment, such as climate, air quality, and noise.

<img src="./imgs_C/001.jpg" height="auto" width="auto"  title="digit-x">

<img src="./imgs_C/002.jpg" height="auto" width=300  title="digit-x">

__2) 数据分析阶段，强烈推荐使用python语言。__

> citing: [城市空间数据分析方法](https://richiebao.github.io/Urban-Spatial-Data-Analysis_python/#/)

传感器分布：

<img src="./imgs_C/003.png" height="auto" width="auto"  title="digit-x">

data文件部分数据：

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
      <th>timestamp</th>
      <th>node_id</th>
      <th>subsystem</th>
      <th>sensor</th>
      <th>parameter</th>
      <th>value_raw</th>
      <th>value_hrf</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018/01/01 00:00:06</td>
      <td>001e0610e532</td>
      <td>chemsense</td>
      <td>at0</td>
      <td>temperature</td>
      <td>-1106</td>
      <td>-11.06</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018/01/01 00:00:06</td>
      <td>001e0610e532</td>
      <td>chemsense</td>
      <td>at1</td>
      <td>temperature</td>
      <td>-1077</td>
      <td>-10.77</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018/01/01 00:00:06</td>
      <td>001e0610e532</td>
      <td>chemsense</td>
      <td>at2</td>
      <td>temperature</td>
      <td>-1009</td>
      <td>-10.09</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018/01/01 00:00:06</td>
      <td>001e0610e532</td>
      <td>chemsense</td>
      <td>at3</td>
      <td>temperature</td>
      <td>-972</td>
      <td>-9.72</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018/01/01 00:00:06</td>
      <td>001e0610e532</td>
      <td>chemsense</td>
      <td>chemsense</td>
      <td>id</td>
      <td>NaN</td>
      <td>541eec3ebfa6</td>
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
    </tr>
    <tr>
      <th>769628</th>
      <td>2018/01/01 23:59:59</td>
      <td>001e0610e540</td>
      <td>metsense</td>
      <td>pr103j2</td>
      <td>temperature</td>
      <td>372</td>
      <td>-17.15</td>
    </tr>
    <tr>
      <th>769629</th>
      <td>2018/01/01 23:59:59</td>
      <td>001e0610e540</td>
      <td>metsense</td>
      <td>spv1840lr5h_b</td>
      <td>intensity</td>
      <td>811</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>769630</th>
      <td>2018/01/01 23:59:59</td>
      <td>001e0610e540</td>
      <td>metsense</td>
      <td>tmp112</td>
      <td>temperature</td>
      <td>NaN</td>
      <td>-17.81</td>
    </tr>
    <tr>
      <th>769631</th>
      <td>2018/01/01 23:59:59</td>
      <td>001e0610e540</td>
      <td>metsense</td>
      <td>tsl250rd</td>
      <td>intensity</td>
      <td>2</td>
      <td>0.101</td>
    </tr>
    <tr>
      <th>769632</th>
      <td>2018/01/01 23:59:59</td>
      <td>001e0610e540</td>
      <td>metsense</td>
      <td>tsys01</td>
      <td>temperature</td>
      <td>NaN</td>
      <td>-18.47</td>
    </tr>
  </tbody>
</table>
<p>769633 rows × 7 columns</p>
</div>

某时刻的温度数据（QGIS建立地图）：

<img src="./imgs_C/004.jpg" height="auto" width="auto"  title="digit-x">


### 1.1.2 [无人驾驶城市](https://www.lirio.work/research/project-one-nxws4-3w8ll-pd9jk)

__1）数据获取阶段——激光雷达+[ROS机器人系统](https://www.ros.org/)__

<img src="./imgs_C/005.jpg" height="auto" width="auto"  title="digit-x">

__2) 数据分析阶段，python语言+[grasshopper参数化设计](https://www.grasshopper3d.com/)。__

<img src="./imgs_C/006.gif" height="auto" width="auto"  title="digit-x">

### 1.1.3 互动装置 interactive installation

* [Forest fire detector concept Interactive Installation Art](https://www.youtube.com/watch?v=WkP04dU_ezo)

<video height='auto' width=100% controls><source src="./video/forest_fire_detector_concept_interactive_installation_art.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>

* [INTERACTIVE INSTALLATION:SYMBIOSIS](https://www.youtube.com/watch?v=yKsFcQPzzwI)

<video height='auto' width=100% controls><source src="./video/interactive_installation_symbiosis.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>

### 1.1.4 Robot（拓展）

* [Human-like robot "wakes up" as UK company unveils android Ameca](https://www.youtube.com/watch?v=RCi3dib4u4c)

<video height='auto' width=100% controls><source src="./video/human_like_robot_wakes_up_as_uk_company_unveils_android_ameca.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>

* [Can A Thousand Tiny Swarming Robots Outsmart Nature? | Deep Look](https://www.youtube.com/watch?v=dDsmbwOrHJs&list=PLQKMAMEJb0id3Eb3EdrXx_IibAvVlZ3-U)

<video height='auto' width=100% controls><source src="./video/can_a_thousand_tiny_swarming_robots_outsmart_nature_deep_look.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>

* [Spot on Site: Construction Solution](https://www.youtube.com/watch?v=0NYJ_9FIHZA)

<video height='auto' width=100% controls><source src="./video/spot_on_site_construction_solution.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>

## 1.2 Arduino(+ROS)

### 1.2.1 [Arduino](https://www.arduino.cc/)

<img src="./imgs_C/007.webp" height="auto" width="auto"  title="digit-x">

* [You can learn Arduino in 15 minutes](https://www.youtube.com/watch?v=nL34zDTPkcs)

<video height='auto' width=100% controls><source src="./video/you_can_learn_arduino_in_15_minutes.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>

### 1.2.1 [+ROS](https://www.ros.org/)

<video height='auto' width=100% controls><source src="./video/ROS_Introduction_captioned-vimeo.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>

## 1.3 C 编程语言

### 1.3.1 [TIOBE](https://www.tiobe.com/tiobe-index/)

TIOBE Index for December 2021

<img src="./imgs_C/008.jpg" height="auto" width="auto"  title="digit-x">