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

### 1.3.2 C VS Arduino C(Embedded C) 与IDE

> citing: wikipedia

* [C (programming language)](https://en.wikipedia.org/wiki/C_(programming_language))

C (/ˈsiː/, as in the letter c) is a general-purpose, procedural computer programming language supporting structured programming, lexical variable scope, and recursion, with a static type system. By design, C provides constructs that map efficiently to typical machine instructions. It has found lasting use in applications previously coded in assembly language. Such applications include operating systems and various application software for computer architectures that range from supercomputers to PLCs and embedded systems.

A successor to the programming language B, C was originally developed at Bell Labs by Dennis Ritchie between 1972 and 1973 to construct utilities running on Unix. It was applied to re-implementing the kernel of the Unix operating system.[6] During the 1980s, C gradually gained popularity. It has become one of the most widely used programming languages,[7][8] with C compilers from various vendors available for the majority of existing computer architectures and operating systems. C has been standardized by ANSI since 1989 (ANSI C) and by the International Organization for Standardization (ISO).

C is an imperative procedural language. It was designed to be compiled to provide low-level access to memory and language constructs that map efficiently to machine instructions, all with minimal runtime support. Despite its low-level capabilities, the language was designed to encourage cross-platform programming. A standards-compliant C program written with portability in mind can be compiled for a wide variety of computer platforms and operating systems with few changes to its source code.[9]

Since 2000, C has consistently ranked among the top two languages in the TIOBE index, a measure of the popularity of programming languages.[10] 

* [ANSI C](https://en.wikipedia.org/wiki/ANSI_C)

ANSI C, ISO C, and Standard C are successive standards for the C programming language published by the American National Standards Institute (ANSI) and the International Organization for Standardization (ISO). Historically, the names referred specifically to the original and best-supported version of the standard (known as C89 or C90). Software developers writing in C are encouraged to conform to the standards, as doing so helps portability between compilers. 

* [Embedded C](https://en.wikipedia.org/wiki/Embedded_C)

Embedded C is a set of language extensions for the C programming language by the C Standards Committee to address commonality issues that exist between C extensions for different embedded systems.

Embedded C programming typically requires nonstandard extensions to the C language in order to support enhanced microprocessor features such as fixed-point arithmetic, multiple distinct memory banks, and basic I/O operations. In 2008, the C Standards Committee extended the C language to address such capabilities by providing a common standard for all implementations to adhere to. It includes a number of features not available in normal C, such as fixed-point arithmetic, named address spaces and basic I/O hardware addressing. Embedded C uses most of the syntax and semantics of standard C, e.g., main() function, variable definition, datatype declaration, conditional statements (if, switch case), loops (while, for), functions, arrays and strings, structures and union, bit operations, macros, etc.[1]

A Technical Report was published in 2004[2] and a second revision in 2006.[3] 

__集成开发环境 IDE(integrated development environment)__

* [Arduino IDE](https://www.arduino.cc/en/software)

<img src="./imgs_C/009.jpg" height="auto" width="auto"  title="digit-x">

[The Arduino Integrated Development Environment (IDE)](https://en.wikipedia.org/wiki/Arduino_IDE) is a cross-platform application (for Windows, macOS, Linux) that is written in functions from C and C++.[3] It is used to write and upload programs to Arduino compatible boards, but also, with the help of third-party cores, other vendor development boards.[4]

The source code for the IDE is released under the GNU General Public License, version 2.[5] The Arduino IDE supports the languages C and C++ using special rules of code structuring.[6] The Arduino IDE supplies a software library from the Wiring project, which provides many common input and output procedures. User-written code only requires two basic functions, for starting the sketch and the main program loop, that are compiled and linked with a program stub main() into an executable cyclic executive program with the GNU toolchain, also included with the IDE distribution.[7] The Arduino IDE employs the program avrdude to convert the executable code into a text file in hexadecimal encoding that is loaded into the Arduino board by a loader program in the board's firmware.[8] By default, avrdude is used as the uploading tool to flash the user code onto official Arduino boards.[9] 

Arduino IDE is a derivative of the Processing IDE,[10] however as of version 2.0, the Processing IDE will be replaced with the Visual Studio Code-based Eclipse Theia IDE framework.[2] 

With the rising popularity of Arduino as a software platform, other vendors started to implement custom open source compilers and tools (cores) that can build and upload sketches to other microcontrollers that are not supported by Arduino's official line of microcontrollers.

In October 2019 the Arduino organization began providing early access to a new Arduino Pro IDE with debugging[12] and other advanced features.[13] 

* [MPLAB® X Integrated Development Environment (IDE)](https://www.microchip.com/en-us/tools-resources/develop/mplab-x-ide)

<img src="./imgs_C/010.jpg" height="auto" width="auto"  title="digit-x">

[MPLAB](https://en.wikipedia.org/wiki/MPLAB) is a proprietary freeware integrated development environment for the development of embedded applications on PIC and dsPIC microcontrollers, and is developed by Microchip Technology.[1][2][3][4][5][6][7][8]

MPLAB X is the latest edition of MPLAB, and is developed on the NetBeans platform.[9][10] MPLAB and MPLAB X support project management, code editing, debugging and programming of Microchip 8-bit PIC and AVR (including ATMEGA) microcontrollers, 16-bit PIC24 and dsPIC microcontrollers, as well as 32-bit SAM (ARM) and PIC32 (MIPS) microcontrollers.[11][12][13]

MPLAB is designed to work with MPLAB-certified devices such as the MPLAB ICD 3 and MPLAB REAL ICE, for programming and debugging PIC microcontrollers using a personal computer. PICKit programmers are also supported by MPLAB.

MPLAB X supports automatic code generation with the MPLAB Code Configurator and the MPLAB Harmony Configurator plugins. 

[First ATMEGA328P Project in MPLAB X](https://www.youtube.com/watch?v=mmT2bhHTdn0)

<video height='auto' width=100% controls><source src="./video/atmega328p_project_in_mplab_x.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>

* [Dev-C++(Open Source C/C++ IDE for Windows)](https://www.bloodshed.net/)

Dev-C++ is a full-featured C and C++ Integrated Development Environment (IDE) for Windows platforms. Millions of developers, students and researchers use Dev-C++ since the first version was released in 1998. It has been featured in dozens of C++ and scientific books and remains one of the favorite learning tool among universities & schools worldwide.

<img src="./imgs_C/011.jpg" height="auto" width="auto"  title="digit-x">

* [Code::Blocks](https://www.codeblocks.org/)

[Typing and running your first program in CodeBlocks](https://www.youtube.com/watch?v=dn7J5WuHqSg)

<video height='auto' width=100% controls><source src="./video/typing_and_running_your_first_program_in_codeblocks2.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>


## 1.4 微控制器+传感器+元器件

[DFRobot](https://www.dfrobot.com.cn/category-238-0-0-0-0-2-sort_order-DESC-1.html)

<img src="./imgs_C/013.jpg" height="auto" width="auto"  title="digit-x">


## 1.5 参考教材

Purdum, J. J. (2015). Beginning C for Arduino: Learn C programming for the Arduino. https://books.google.co.uk/books?id=ATYwCgAAQBAJ&pg=PA52&dq=C+data+types+and+size+bits&hl=en&sa=X&ved=0ahUKEwiw75CZrN_gAhWYBWMBHfvaA6EQ6AEITTAG#v=onepage&q=C data types and size bits&f=false

Miller, Dean;Perry, Greg M (2016). 	C programming: absolute beginner's guide (Third edition). Publisher: Que, Year: 2016

Jeff Szuhay (2020). Learn C Programming: A beginner's guide to learning C programming the easy and disciplined way. Publisher: Packt Publishing, Year: 2020

## 附：关于英语
