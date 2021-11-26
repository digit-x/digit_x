> Created on Sat Nov 20 19\26\38 2021 @author: Richie Bao-python_code_archi_la_design_method_study 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

## 常用python库

### 1.1 最常用的数据格式

* [Numpy](https://numpy.org/)  #array(数组)，最常用的数据形式之一，提高计算的效率，大部分扩展库支持Numpy，例如机器学习库Sklearn和深度学习库Pytorch等.

* [pandas](https://pandas.pydata.org/) #DataFrame(数据框或表)和Series，最常用的数据形式之一，方便数据处理，尤其与GIS中的Table(表)结构相同。

### 1.2 数据可视化

* [Matplotlib](https://matplotlib.org/) #最为基础和常用的数据图形库，除了静态图表，也支持动画和交互方式。

* [Plotly-python](https://plotly.com/python/) #可交互，支持Web,具有丰富的图表形式，根据领域细分为基础类、统计类、科学类、金融类、地图类、人工智能类和生物信息类等。

* [Bokeh](https://docs.bokeh.org/en/latest/index.html) #支持Web可交互的数据可视化。

* [seaborn](https://seaborn.pydata.org/) #统计数据可视化

* [Folium](https://python-visualization.github.io/folium/) #支持地图，GIS数据。

* [Altair](https://altair-viz.github.io/) #Declarative Visualization in Python.

### 1.3 科学计算库

* [Scipy](https://scipy.org/) #必用科学计算库。Fundamental algorithms for scientific computing in Python

* [SymPy](https://www.sympy.org/en/index.html) #表达公式，学习数学知识，强烈推荐库。

### 1.4 统计与机器学习

* [statsmodels](https://www.statsmodels.org/stable/index.html) #statsmodels is a Python module that provides classes and functions for the estimation of many different statistical models, as well as for conducting statistical tests, and statistical data exploration.

* [scikit-learn](https://scikit-learn.org/stable/) #必用机器学习库。

* [PyTorch](https://pytorch.org/) #必用深度学习库。

* [TensorFlow](https://www.tensorflow.org/) #深度学习库.

### 1.5 [GIS]地理信息系统支持

* [GeoPandas](https://geopandas.org/en/stable/) #首推GIS处理库，偏向于Shape等矢量数据。基于pandas数据格式的地理信息扩展。

* [rasterstats](https://pythonhosted.org/rasterstats/) #地理栅格数据处理，例如遥感影像等。

* [Rasterio](https://rasterio.readthedocs.io/en/latest/) #地理栅格数据处理.

* [EarthPy](https://earthpy.readthedocs.io/en/latest/) #EarthPy is a python package that makes it easier to plot and work with spatial raster and vector data using open source tools. Earthpy depends upon geopandas which has a focus on vector data and rasterio with facilitates input and output of raster data files. It also requires matplotlib for plotting operations.

* [GDAL](https://gdal.org/) #几乎是所有GIS库的核心，但是很少直接使用。

* [Shapely](https://shapely.readthedocs.io/en/latest/manual.html) #必须掌握处理矢量几何图形的工具，通常配合GIS的Shape数据格式。

* [PySAL](https://pysal.org/) #空间数据统计分析，必须掌握的库。The python spatial analysis library for open source, cross platform geospatial data sicence.

### 1.6 数据库

* [SQLite](https://www.sqlite.org/index.html) #规划设计类，必用轻便型数据库。

* [PostgreSQL](https://www.postgresql.org/) #结合QGIS，规划类必用数据库。

### 1.7 Web
 
* [Flask](https://flask.palletsprojects.com/en/1.1.x/) #轻便型网站开发，可以布局训练好的机器学习模型。

### 1.8 GUI-graphical user interface

* [tkinter](https://docs.python.org/3/library/tkinter.html) #建立可交互的GUI。

* [pygame](https://www.pygame.org/news) #虽然为编写电子游戏的库，但是在规划设计领域通常用于类似GUI的数据交互。

### 1.9 网络结构（network）

* [NetworkX](https://networkx.org/) #网络结构分析。

* [pyvis](https://pyvis.readthedocs.io/en/latest/index.html) #Interactive network visualizations

### 1.10 点云数据处理

* [Open3D](http://www.open3d.org/docs/release/introduction.html) #很好用的点云数据处理库。

* [PDAL](https://pdal.io/) #PDAL is a C++ library for translating and manipulating point cloud data.

* [PCL](https://pointclouds.org/) #The Point Cloud Library (PCL) is a standalone, large scale, open project for 2D/3D image and point cloud processing.

### 1.11 计算机视觉

* [OpenCV-Open Source Computer Vision Library](https://opencv.org/) # is an open-source library that includes several hundreds of computer vision algorithms.

* [PIL-pillow](https://python-pillow.org/) #PIL is the Python Imaging Library.

* [scikit-image](https://scikit-image.org/) #scikit-image is a collection of algorithms for image processing. 

* [VTK](https://vtk.org/about/#overview) #The Visualization Toolkit (VTK) is an open-source, freely available software system for 3D computer graphics, modeling, image processing, volume rendering, scientific visualization, and 2D plotting. 

### 1.12 机器人

* [ROS](https://www.ros.org/) #可以使用python编程机器人。The Robot Operating System (ROS) is a set of software libraries and tools that help you build robot applications. From drivers to state-of-the-art algorithms, and with powerful developer tools, ROS has what you need for your next robotics project. And it's all open source.


> 第12次课-2021_11_29_Mon_7-8节 :2课时 课程完成时间标识线。结课......

---   

