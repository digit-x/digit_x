> Created on Wed Sep  8 18\27\31 2021 @author: Richie Bao-python_code_archi_la_design_method_study 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# 数据库_SQlite及与grasshopper的数据交换

预备知识：[SQLite 数据库-基础](https://richiebao.github.io/Urban-Spatial-Data-Analysis_python/#/./notebook_code/sqlite)+[OpenStreetMap（OSM）数据处理](https://richiebao.github.io/Urban-Spatial-Data-Analysis_python/#/./notebook_code/OSM_dataProcessing)

## 1. [OSM（OpenStreetMap）](https://www.openstreetmap.org/#map=13/41.8679/-87.6569)数据处理

> 参考 [OpenStreetMap（OSM）数据处理](https://richiebao.github.io/Urban-Spatial-Data-Analysis_python/#/./notebook_code/OSM_dataProcessing)

[pyosmium](https://github.com/osmcode/pyosmium) `conda update pip`->`pip install osmium` 

## 1.1 读取OSM数据


```python
import osmium as osm
import pandas as pd
import datetime
import shapely.wkb as wkblib
wkbfab=osm.geom.WKBFactory()

class osmHandler(osm.SimpleHandler):    
    '''
    class-通过继承osmium类 class osmium.SimpleHandler读取.osm数据. 
    '''
    
    def __init__(self):
        osm.SimpleHandler.__init__(self)
        self.osm_node=[]
        self.osm_way=[]
        self.osm_area=[]
        
    def node(self,n):
        wkb=wkbfab.create_point(n)
        point=wkblib.loads(wkb,hex=True)
        self.osm_node.append([
            'node',
            point,
            n.id,
            n.version,
            n.visible,
            pd.Timestamp(n.timestamp),
            n.uid,
            n.user,
            n.changeset,
            len(n.tags),
            {tag.k:tag.v for tag in n.tags},
            ])

    def way(self,w):     
        try:
            wkb=wkbfab.create_linestring(w)
            linestring=wkblib.loads(wkb, hex=True)
            self.osm_way.append([
                'way',
                linestring,
                w.id,
                w.version,
                w.visible,
                pd.Timestamp(w.timestamp),
                w.uid,
                w.user,
                w.changeset,
                len(w.tags),
                {tag.k:tag.v for tag in w.tags}, 
                ])
        except:
            pass
        
    def area(self,a):     
        try:
            wkb=wkbfab.create_multipolygon(a)
            multipolygon=wkblib.loads(wkb, hex=True)
            self.osm_area.append([
                'area',
                multipolygon,
                a.id,
                a.version,
                a.visible,
                pd.Timestamp(a.timestamp),
                a.uid,
                a.user,
                a.changeset,
                len(a.tags),
                {tag.k:tag.v for tag in a.tags}, 
                ])
        except:
            pass      
```


```python
osm_Chicago_fp=r"./data/IIT.osm"

a_T=datetime.datetime.now()
print("start time:",a_T)

osm_handler=osmHandler() #实例化类osmHandler()
osm_handler.apply_file(osm_Chicago_fp,locations=True) #调用 class osmium.SimpleHandler的apply_file方法
b_T=datetime.datetime.now()
print("end time:",b_T)
duration=(b_T-a_T).seconds/60
print("Total time spend:%.2f minutes"%duration)
```

    start time: 2021-09-23 21:26:17.596773
    end time: 2021-09-23 21:26:18.769780
    Total time spend:0.02 minutes
    

## 1.2 保存读取的OSM为geopandas数据格式


```python
def save_osm(osm_handler,osm_type,save_path=r"./data/",fileType="GPKG"):
    import geopandas as gpd
    import os
    import datetime
    
    a_T=datetime.datetime.now()
    print("start time:",a_T)    
    
    '''
    function-根据条件逐个保存读取的osm数据（node, way and area）
    
    Paras:
    osm_handler - osm返回的node,way和area数据
    osm_type - 要保存的osm元素类型
    save_path - 保存路径
    fileType - 保存的数据类型，shp, GeoJSON, GPKG
    '''
    def duration(a_T):
        b_T=datetime.datetime.now()
        print("end time:",b_T)
        duration=(b_T-a_T).seconds/60
        print("Total time spend:%.2f minutes"%duration)
        
    def save_gdf(osm_node_gdf,fileType,osm_type):
        if fileType=="GeoJSON":
            osm_node_gdf.to_file(os.path.join(save_path,"osm_%s.geojson"%osm_type),driver='GeoJSON')
        elif fileType=="GPKG":
            osm_node_gdf.to_file(os.path.join(save_path,"osm_%s.gpkg"%osm_type),driver='GPKG')
        elif fileType=="shp":
            osm_node_gdf.to_file(os.path.join(save_path,"osm_%s.shp"%osm_type))

    crs={'init': 'epsg:4326'} #配置坐标系统，参考：https://spatialreference.org/        
    osm_columns=['type','geometry','id','version','visible','ts','uid','user','changeet','tagLen','tags']
    if osm_type=="node":
        osm_node_gdf=gpd.GeoDataFrame(osm_handler.osm_node,columns=osm_columns,crs=crs)
        save_gdf(osm_node_gdf,fileType,osm_type)
        duration(a_T)
        return osm_node_gdf

    elif osm_type=="way":
        osm_way_gdf=gpd.GeoDataFrame(osm_handler.osm_way,columns=osm_columns,crs=crs)
        save_gdf(osm_way_gdf,fileType,osm_type)
        duration(a_T)
        return osm_way_gdf
        
    elif osm_type=="area":
        osm_area_gdf=gpd.GeoDataFrame(osm_handler.osm_area,columns=osm_columns,crs=crs)
        save_gdf(osm_area_gdf,fileType,osm_type)
        duration(a_T)
        return osm_area_gdf
```


```python
node_gdf=save_osm(osm_handler,osm_type="node",save_path=r"./data/",fileType="GPKG")
```

    start time: 2021-09-23 21:29:42.828257
    

    C:\Users\richi\anaconda3\envs\py_la_archi\lib\site-packages\pyproj\crs\crs.py:53: FutureWarning: '+init=<authority>:<code>' syntax is deprecated. '<authority>:<code>' is the preferred initialization method. When making the change, be mindful of axis order changes: https://pyproj4.github.io/pyproj/stable/gotchas.html#axis-order-changes-in-proj-6
      return _prepare_from_string(" ".join(pjargs))
    

    end time: 2021-09-23 21:29:45.519268
    Total time spend:0.03 minutes
    


```python
way_gdf=save_osm(osm_handler,osm_type="way",save_path=r"./data/",fileType="GPKG")
```

    start time: 2021-09-23 21:30:51.686464
    end time: 2021-09-23 21:30:52.223121
    Total time spend:0.00 minutes
    


```python
area_gdf=save_osm(osm_handler,osm_type="area",save_path=r"./data/",fileType="GPKG")
```

    start time: 2021-09-23 21:30:58.507470
    end time: 2021-09-23 21:30:58.925352
    Total time spend:0.00 minutes
    

## 1.3 读取与查看地图数据


```python
def start_time():
    import datetime
    '''
    function-计算当前时间
    '''
    start_time=datetime.datetime.now()
    print("start time:",start_time)
    return start_time

def duration(start_time):
    import datetime
    '''
    function-计算持续时间
    
    Paras:
    start_time - 开始时间
    '''
    end_time=datetime.datetime.now()
    print("end time:",end_time)
    duration=(end_time-start_time).seconds/60
    print("Total time spend:%.2f minutes"%duration)
```


```python
import geopandas as gpd
import util_pyd

start_time=util_pyd.start_time()
read_way_gdf=gpd.read_file("./data/osm_way.gpkg")
util_pyd.duration(start_time)
```

    start time: 2021-09-23 21:43:16.840113
    end time: 2021-09-23 21:43:16.986753
    Total time spend:0.00 minutes
    


```python
read_way_gdf
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
      <th>type</th>
      <th>id</th>
      <th>version</th>
      <th>visible</th>
      <th>ts</th>
      <th>uid</th>
      <th>user</th>
      <th>changeet</th>
      <th>tagLen</th>
      <th>tags</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>way</td>
      <td>4341221</td>
      <td>13</td>
      <td>True</td>
      <td>2019-10-18T03:14:35</td>
      <td>298815</td>
      <td>miha12</td>
      <td>75868386</td>
      <td>12</td>
      <td>{"bicycle": "no", "bridge": "yes", "foot": "no...</td>
      <td>LINESTRING (-87.62021 41.84793, -87.61730 41.8...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>way</td>
      <td>4680600</td>
      <td>51</td>
      <td>True</td>
      <td>2018-10-03T17:13:44</td>
      <td>755543</td>
      <td>Rallysta74</td>
      <td>63167767</td>
      <td>11</td>
      <td>{"bicycle": "no", "foot": "no", "hgv": "design...</td>
      <td>LINESTRING (-87.63151 41.84236, -87.63150 41.8...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>way</td>
      <td>4680601</td>
      <td>54</td>
      <td>True</td>
      <td>2018-10-03T17:13:44</td>
      <td>755543</td>
      <td>Rallysta74</td>
      <td>63167767</td>
      <td>11</td>
      <td>{"bicycle": "no", "foot": "no", "hgv": "design...</td>
      <td>LINESTRING (-87.63117 41.80929, -87.63111 41.8...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>way</td>
      <td>4694845</td>
      <td>10</td>
      <td>True</td>
      <td>2018-07-31T15:54:32</td>
      <td>7409020</td>
      <td>krinkov76239</td>
      <td>61234561</td>
      <td>14</td>
      <td>{"bicycle": "no", "bridge": "yes", "destinatio...</td>
      <td>LINESTRING (-87.63437 41.84526, -87.63372 41.8...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>way</td>
      <td>5010719</td>
      <td>19</td>
      <td>True</td>
      <td>2017-11-08T09:30:33</td>
      <td>5677293</td>
      <td>bhavana naga</td>
      <td>53605003</td>
      <td>5</td>
      <td>{"destination": "Lake Shore Drive;22nd Street"...</td>
      <td>LINESTRING (-87.63038 41.83753, -87.63042 41.8...</td>
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
    </tr>
    <tr>
      <th>2043</th>
      <td>way</td>
      <td>861153219</td>
      <td>1</td>
      <td>True</td>
      <td>2020-10-20T18:12:51</td>
      <td>1237170</td>
      <td>Zol87</td>
      <td>92784762</td>
      <td>1</td>
      <td>{"highway": "footway"}</td>
      <td>LINESTRING (-87.63076 41.83448, -87.63073 41.8...</td>
    </tr>
    <tr>
      <th>2044</th>
      <td>way</td>
      <td>861153220</td>
      <td>1</td>
      <td>True</td>
      <td>2020-10-20T18:12:51</td>
      <td>1237170</td>
      <td>Zol87</td>
      <td>92784762</td>
      <td>1</td>
      <td>{"highway": "steps"}</td>
      <td>LINESTRING (-87.63073 41.83342, -87.63071 41.8...</td>
    </tr>
    <tr>
      <th>2045</th>
      <td>way</td>
      <td>871062530</td>
      <td>1</td>
      <td>True</td>
      <td>2020-11-13T06:18:17</td>
      <td>10086304</td>
      <td>dhakanth</td>
      <td>94037437</td>
      <td>2</td>
      <td>{"access": "private", "highway": "service"}</td>
      <td>LINESTRING (-87.62191 41.83797, -87.62181 41.8...</td>
    </tr>
    <tr>
      <th>2046</th>
      <td>way</td>
      <td>874627890</td>
      <td>1</td>
      <td>True</td>
      <td>2020-11-19T10:37:09</td>
      <td>10480284</td>
      <td>zmankits</td>
      <td>94429553</td>
      <td>1</td>
      <td>{"highway": "service"}</td>
      <td>LINESTRING (-87.62047 41.84481, -87.62057 41.8...</td>
    </tr>
    <tr>
      <th>2047</th>
      <td>way</td>
      <td>875929298</td>
      <td>1</td>
      <td>True</td>
      <td>2020-11-23T00:33:44</td>
      <td>12169822</td>
      <td>Charles Fang</td>
      <td>94602824</td>
      <td>6</td>
      <td>{"access": "private", "amenity": "parking", "c...</td>
      <td>LINESTRING (-87.63231 41.84707, -87.63250 41.8...</td>
    </tr>
  </tbody>
</table>
<p>2048 rows × 11 columns</p>
</div>




```python
read_way_gdf.plot(cmap="Blues",figsize=(30,30))
```
    
<a href=""><img src="./imgs_pyd/pyd_01.png" height="auto" width="auto" title="digit-x"></a>
    



```python
start_time=util_pyd.start_time()
read_area_gdf=gpd.read_file("./data/osm_area.gpkg")
util_pyd.duration(start_time)
```

    start time: 2021-09-23 21:43:52.968520
    end time: 2021-09-23 21:43:53.110177
    Total time spend:0.00 minutes
    


```python
read_area_gdf
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
      <th>type</th>
      <th>id</th>
      <th>version</th>
      <th>visible</th>
      <th>ts</th>
      <th>uid</th>
      <th>user</th>
      <th>changeet</th>
      <th>tagLen</th>
      <th>tags</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>area</td>
      <td>47919496</td>
      <td>2</td>
      <td>True</td>
      <td>2008-04-25T05:20:22</td>
      <td>35667</td>
      <td>encleadus</td>
      <td>256513</td>
      <td>3</td>
      <td>{"access": "permissive", "amenity": "parking",...</td>
      <td>MULTIPOLYGON (((-87.63503 41.83261, -87.63500 ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>area</td>
      <td>56585498</td>
      <td>4</td>
      <td>True</td>
      <td>2020-11-08T04:53:57</td>
      <td>1237170</td>
      <td>Zol87</td>
      <td>93727773</td>
      <td>11</td>
      <td>{"amenity": "school", "denomination": "catholi...</td>
      <td>MULTIPOLYGON (((-87.62515 41.83284, -87.62508 ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>area</td>
      <td>65712820</td>
      <td>3</td>
      <td>True</td>
      <td>2019-02-25T18:29:52</td>
      <td>9403823</td>
      <td>ferdous1</td>
      <td>67558097</td>
      <td>1</td>
      <td>{"landuse": "retail"}</td>
      <td>MULTIPOLYGON (((-87.63395 41.85267, -87.63395 ...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>area</td>
      <td>77080582</td>
      <td>5</td>
      <td>True</td>
      <td>2020-11-08T04:53:57</td>
      <td>1237170</td>
      <td>Zol87</td>
      <td>93727773</td>
      <td>11</td>
      <td>{"addr:city": "Chicago", "addr:housenumber": "...</td>
      <td>MULTIPOLYGON (((-87.62477 41.83384, -87.62450 ...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>area</td>
      <td>268284574</td>
      <td>1</td>
      <td>True</td>
      <td>2011-10-21T18:35:17</td>
      <td>365944</td>
      <td>homeslice60148</td>
      <td>9618544</td>
      <td>3</td>
      <td>{"leisure": "pitch", "name": "Ed Glancy Field"...</td>
      <td>MULTIPOLYGON (((-87.62486 41.83888, -87.62479 ...</td>
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
    </tr>
    <tr>
      <th>1333</th>
      <td>area</td>
      <td>1686440066</td>
      <td>1</td>
      <td>True</td>
      <td>2020-09-01T17:10:33</td>
      <td>604536</td>
      <td>jimjoe45</td>
      <td>90257292</td>
      <td>1</td>
      <td>{"building": "yes"}</td>
      <td>MULTIPOLYGON (((-87.61998 41.83832, -87.61997 ...</td>
    </tr>
    <tr>
      <th>1334</th>
      <td>area</td>
      <td>1686440068</td>
      <td>1</td>
      <td>True</td>
      <td>2020-09-01T17:10:33</td>
      <td>604536</td>
      <td>jimjoe45</td>
      <td>90257292</td>
      <td>1</td>
      <td>{"building": "yes"}</td>
      <td>MULTIPOLYGON (((-87.61996 41.83805, -87.61996 ...</td>
    </tr>
    <tr>
      <th>1335</th>
      <td>area</td>
      <td>1700790582</td>
      <td>1</td>
      <td>True</td>
      <td>2020-09-23T07:48:03</td>
      <td>8476127</td>
      <td>nightrider209</td>
      <td>91346203</td>
      <td>3</td>
      <td>{"footway": "sidewalk", "highway": "footway", ...</td>
      <td>MULTIPOLYGON (((-87.63466 41.84996, -87.63465 ...</td>
    </tr>
    <tr>
      <th>1336</th>
      <td>area</td>
      <td>1700790584</td>
      <td>1</td>
      <td>True</td>
      <td>2020-09-23T07:48:03</td>
      <td>8476127</td>
      <td>nightrider209</td>
      <td>91346203</td>
      <td>3</td>
      <td>{"footway": "sidewalk", "highway": "footway", ...</td>
      <td>MULTIPOLYGON (((-87.63464 41.84906, -87.63463 ...</td>
    </tr>
    <tr>
      <th>1337</th>
      <td>area</td>
      <td>1751858596</td>
      <td>1</td>
      <td>True</td>
      <td>2020-11-23T00:33:44</td>
      <td>12169822</td>
      <td>Charles Fang</td>
      <td>94602824</td>
      <td>6</td>
      <td>{"access": "private", "amenity": "parking", "c...</td>
      <td>MULTIPOLYGON (((-87.63332 41.84688, -87.63298 ...</td>
    </tr>
  </tbody>
</table>
<p>1338 rows × 11 columns</p>
</div>




```python
read_area_gdf.plot(cmap="Blues",figsize=(20,20))
```
 

<a href=""><img src="./imgs_pyd/pyd_02.png" height="auto" width="auto" title="digit-x"></a>



```python
start_time=util_pyd.start_time()
read_node_gdf=gpd.read_file("./data/osm_node.gpkg")
util_pyd.duration(start_time)
```

    start time: 2021-09-23 21:48:17.393131
    end time: 2021-09-23 21:48:18.178141
    Total time spend:0.00 minutes
    


```python
read_node_gdf
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
      <th>type</th>
      <th>id</th>
      <th>version</th>
      <th>visible</th>
      <th>ts</th>
      <th>uid</th>
      <th>user</th>
      <th>changeet</th>
      <th>tagLen</th>
      <th>tags</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>node</td>
      <td>26098007</td>
      <td>20</td>
      <td>True</td>
      <td>2010-04-30T10:42:30</td>
      <td>207745</td>
      <td>NE2</td>
      <td>4565099</td>
      <td>0</td>
      <td>{}</td>
      <td>POINT (-87.62069 41.84792)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>node</td>
      <td>26098008</td>
      <td>21</td>
      <td>True</td>
      <td>2010-04-30T10:42:30</td>
      <td>207745</td>
      <td>NE2</td>
      <td>4565099</td>
      <td>0</td>
      <td>{}</td>
      <td>POINT (-87.62266 41.84789)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>node</td>
      <td>26098009</td>
      <td>21</td>
      <td>True</td>
      <td>2016-09-01T10:04:05</td>
      <td>3153457</td>
      <td>GidonW</td>
      <td>41845107</td>
      <td>1</td>
      <td>{"curve_geometry": "yes"}</td>
      <td>POINT (-87.62384 41.84783)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>node</td>
      <td>26098011</td>
      <td>20</td>
      <td>True</td>
      <td>2010-04-30T10:42:30</td>
      <td>207745</td>
      <td>NE2</td>
      <td>4565099</td>
      <td>0</td>
      <td>{}</td>
      <td>POINT (-87.62450 41.84774)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>node</td>
      <td>26098012</td>
      <td>20</td>
      <td>True</td>
      <td>2010-04-30T10:42:30</td>
      <td>207745</td>
      <td>NE2</td>
      <td>4565099</td>
      <td>0</td>
      <td>{}</td>
      <td>POINT (-87.62545 41.84752)</td>
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
    </tr>
    <tr>
      <th>12466</th>
      <td>node</td>
      <td>8152559988</td>
      <td>1</td>
      <td>True</td>
      <td>2020-11-23T00:33:44</td>
      <td>12169822</td>
      <td>Charles Fang</td>
      <td>94602824</td>
      <td>0</td>
      <td>{}</td>
      <td>POINT (-87.63327 41.84694)</td>
    </tr>
    <tr>
      <th>12467</th>
      <td>node</td>
      <td>8152559989</td>
      <td>1</td>
      <td>True</td>
      <td>2020-11-23T00:33:44</td>
      <td>12169822</td>
      <td>Charles Fang</td>
      <td>94602824</td>
      <td>0</td>
      <td>{}</td>
      <td>POINT (-87.63331 41.84690)</td>
    </tr>
    <tr>
      <th>12468</th>
      <td>node</td>
      <td>8152559990</td>
      <td>1</td>
      <td>True</td>
      <td>2020-11-23T00:33:44</td>
      <td>12169822</td>
      <td>Charles Fang</td>
      <td>94602824</td>
      <td>0</td>
      <td>{}</td>
      <td>POINT (-87.63332 41.84688)</td>
    </tr>
    <tr>
      <th>12469</th>
      <td>node</td>
      <td>8152559991</td>
      <td>1</td>
      <td>True</td>
      <td>2020-11-23T00:33:44</td>
      <td>12169822</td>
      <td>Charles Fang</td>
      <td>94602824</td>
      <td>0</td>
      <td>{}</td>
      <td>POINT (-87.63298 41.84683)</td>
    </tr>
    <tr>
      <th>12470</th>
      <td>node</td>
      <td>8152559992</td>
      <td>1</td>
      <td>True</td>
      <td>2020-11-23T00:33:44</td>
      <td>12169822</td>
      <td>Charles Fang</td>
      <td>94602824</td>
      <td>0</td>
      <td>{}</td>
      <td>POINT (-87.63227 41.84674)</td>
    </tr>
  </tbody>
</table>
<p>12471 rows × 11 columns</p>
</div>




```python
read_node_gdf.plot(cmap="Blues",figsize=(30,30))
```
    
<a href=""><img src="./imgs_pyd/pyd_03.png" height="auto" width="auto" title="digit-x"></a>
    


## 2. [SQLite](https://www.sqlite.org/index.html)<->GHPython
### 2.1 OSM数据写入SQLite


```python
def OSM2SQLite_database(osm_handler,db_fp,epsg=None): 
    from sqlalchemy import create_engine    
    from sqlalchemy.orm import sessionmaker
    from sqlalchemy.ext.declarative import declarative_base
    from sqlalchemy import Column, Integer, String,Integer,Text,Float,Boolean,Date,DateTime,Time
    
    import os,shapely
    import pandas as pd
    import numpy as np
    import geopandas as gpd
    
    osm_columns=['type','geometry','id','version','visible','ts','uid','user','changeset','tagLen','tags']
    # print([len(i) for i in osm_handler.osm_node])
    # print(osm_handler.osm_node[10:13])
    # print(list(osm_handler.osm_node[10][1].coords)[0])
    
    if epsg is not None:
        crs_target={'init': 'epsg:%d'%epsg}
        
    crs={'init': 'epsg:4326'}    
    if epsg is not None:
        osm_node_db=pd.DataFrame(gpd.GeoDataFrame(osm_handler.osm_node,columns=osm_columns,crs=crs).to_crs(epsg=epsg))
        # print("+"*50)
        # print(osm_node_db)
    else:
        osm_node_db=pd.DataFrame(osm_handler.osm_node,columns=osm_columns)
    
    osm_node_db.geometry=osm_node_db.geometry.apply(lambda row:str(row.coords[0]))
    osm_node_db.tags=osm_node_db.tags.apply(lambda row:str(row))

    # osm_node_db=osm_node_db.astype({'type':'str',
    #                                 'geometry':'',
    #                                 'id',
    #                                 'version',
    #                                 'visible',
    #                                 'ts',
    #                                 'uid',
    #                                 'user',
    #                                 'changeset',
    #                                 'tagLen',
    #                                 'tags'})
    # osm_node_db['ts']=osm_node_db.ts.astype('object')    
    # print(osm_node_db)
    # print(osm_node_db.dtypes)

    if epsg is not None:
        osm_way_db=pd.DataFrame(gpd.GeoDataFrame(osm_handler.osm_way,columns=osm_columns,crs=crs).to_crs(epsg=epsg)) 
    else:
        osm_way_db=pd.DataFrame(osm_handler.osm_way,columns=osm_columns)    
    osm_way_db.geometry=osm_way_db.geometry.apply(lambda row:str(list(row.coords)))    
    osm_way_db.tags=osm_way_db.tags.apply(lambda row:str(row))
    # print(osm_way_db.geometry)
    
    if epsg is not None:
        osm_area_db=pd.DataFrame(gpd.GeoDataFrame(osm_handler.osm_area,columns=osm_columns,crs=crs).to_crs(epsg=epsg)) 
    else:         
        osm_area_db=pd.DataFrame(osm_handler.osm_area,columns=osm_columns) 
    osm_area_db.geometry=osm_area_db.geometry.apply(lambda row:str([list(r.exterior.coords) for r in row] ))  
    osm_area_db.tags=osm_area_db.tags.apply(lambda row:str(row))
    #print(osm_area_db)      
    
    engine=create_engine('sqlite:///'+'\\\\'.join(db_fp.split('\\')),echo=True)  
    print(engine)
    '''
    Base = declarative_base()
    class node(Base):
        __tablename__='node'
        __table_args__ = {'extend_existing': True}
        type_=Column(String(20))
        geometry=Column(Text)
        id_=Column(Integer,primary_key=True)
        version=Column(Integer)
        visible=Column(Boolean)
        ts=Column(DateTime)
        uid=Column(Integer)
        user=Column(String(100))
        changeset=Column(Integer)
        taglen=Column(Integer)
        tags=Column(Text)
        
        def __repr__(self):
            return '<node %s>'%self.id_
    print(node.__table__)

    Base.metadata.create_all(engine, checkfirst=True)
    Session = sessionmaker(bind=engine)
    session = Session()
    session.add_all(osm_handler.osm_node)
    session.commit()
    '''
    try:
        osm_node_db.to_sql('node',con=engine,if_exists='replace') #if_exists='append'        
    except:
        print("_"*50,'\n','the node table has been existed...')       
    try:    
        osm_way_db.to_sql('way',con=engine,)
    except:
        print("_"*50,'\n','the way table has been existed...')    
    try:    
        osm_area_db.to_sql('area',con=engine,)
    except:
        print("_"*50,'\n','the way table has been existed...')    
```


```python
osm_Chicago_fp=r"./data/IIT.osm"
import util_pyd
osm_handler=util_pyd.osmHandler() #实例化类osmHandler()
osm_handler.apply_file(osm_Chicago_fp,locations=True) #调用 class osmium.SimpleHandler的apply_file方法

OSM_db_fp=r'../database/OSM_sqlit.db'
epsg=32616
OSM2SQLite_database(osm_handler,OSM_db_fp,epsg=32616)
```

    Engine(sqlite:///../database/OSM_sqlit.db)
    2021-09-23 23:09:40,636 INFO sqlalchemy.engine.base.Engine SELECT CAST('test plain returns' AS VARCHAR(60)) AS anon_1
    2021-09-23 23:09:40,637 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,638 INFO sqlalchemy.engine.base.Engine SELECT CAST('test unicode returns' AS VARCHAR(60)) AS anon_1
    2021-09-23 23:09:40,639 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,640 INFO sqlalchemy.engine.base.Engine PRAGMA main.table_info("node")
    2021-09-23 23:09:40,640 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,642 INFO sqlalchemy.engine.base.Engine PRAGMA temp.table_info("node")
    2021-09-23 23:09:40,643 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,646 INFO sqlalchemy.engine.base.Engine 
    CREATE TABLE node (
    	"index" BIGINT, 
    	type TEXT, 
    	geometry TEXT, 
    	id BIGINT, 
    	version BIGINT, 
    	visible BOOLEAN, 
    	ts TIMESTAMP, 
    	uid BIGINT, 
    	user TEXT, 
    	changeset BIGINT, 
    	"tagLen" BIGINT, 
    	tags TEXT, 
    	CHECK (visible IN (0, 1))
    )
    
    
    2021-09-23 23:09:40,647 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,657 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-23 23:09:40,658 INFO sqlalchemy.engine.base.Engine CREATE INDEX ix_node_index ON node ("index")
    2021-09-23 23:09:40,659 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,667 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-23 23:09:40,680 INFO sqlalchemy.engine.base.Engine BEGIN (implicit)
    2021-09-23 23:09:40,805 INFO sqlalchemy.engine.base.Engine INSERT INTO node ("index", type, geometry, id, version, visible, ts, uid, user, changeset, "tagLen", tags) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
    2021-09-23 23:09:40,806 INFO sqlalchemy.engine.base.Engine ((0, 'node', '(448473.2155021054, 4633077.753913923)', 26098007, 20, 1, '2010-04-30 10:42:30.000000', 207745, 'NE2', 4565099, 0, '{}'), (1, 'node', '(448309.74485118233, 4633074.718070096)', 26098008, 21, 1, '2010-04-30 10:42:30.000000', 207745, 'NE2', 4565099, 0, '{}'), (2, 'node', '(448211.9478524396, 4633068.999047837)', 26098009, 21, 1, '2016-09-01 10:04:05.000000', 3153457, 'GidonW', 41845107, 1, "{'curve_geometry': 'yes'}"), (3, 'node', '(448157.32590432547, 4633059.40312257)', 26098011, 20, 1, '2010-04-30 10:42:30.000000', 207745, 'NE2', 4565099, 0, '{}'), (4, 'node', '(448078.1010730234, 4633035.585836472)', 26098012, 20, 1, '2010-04-30 10:42:30.000000', 207745, 'NE2', 4565099, 0, '{}'), (5, 'node', '(447996.60058130085, 4633004.890901338)', 26098013, 20, 1, '2010-04-30 10:42:30.000000', 207745, 'NE2', 4565099, 0, '{}'), (6, 'node', '(447934.7609603471, 4632983.602077102)', 26098015, 21, 1, '2016-09-01 10:04:05.000000', 3153457, 'GidonW', 41845107, 1, "{'curve_geometry': 'yes'}"), (7, 'node', '(447899.96376946324, 4632975.2068495825)', 26098016, 23, 1, '2010-04-30 12:29:38.000000', 207745, 'NE2', 4565849, 0, '{}')  ... displaying 10 of 12471 total bound parameter sets ...  (12469, 'node', '(447452.4742006921, 4632964.067943237)', 8152559991, 1, 1, '2020-11-23 00:33:44.000000', 12169822, 'Charles Fang', 94602824, 0, '{}'), (12470, 'node', '(447511.2868949126, 4632952.87563649)', 8152559992, 1, 1, '2020-11-23 00:33:44.000000', 12169822, 'Charles Fang', 94602824, 0, '{}'))
    2021-09-23 23:09:40,849 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-23 23:09:40,863 INFO sqlalchemy.engine.base.Engine PRAGMA main.table_info("way")
    2021-09-23 23:09:40,864 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,865 INFO sqlalchemy.engine.base.Engine PRAGMA temp.table_info("way")
    2021-09-23 23:09:40,865 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,867 INFO sqlalchemy.engine.base.Engine 
    CREATE TABLE way (
    	"index" BIGINT, 
    	type TEXT, 
    	geometry TEXT, 
    	id BIGINT, 
    	version BIGINT, 
    	visible BOOLEAN, 
    	ts TIMESTAMP, 
    	uid BIGINT, 
    	user TEXT, 
    	changeset BIGINT, 
    	"tagLen" BIGINT, 
    	tags TEXT, 
    	CHECK (visible IN (0, 1))
    )
    
    
    2021-09-23 23:09:40,868 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,874 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-23 23:09:40,875 INFO sqlalchemy.engine.base.Engine CREATE INDEX ix_way_index ON way ("index")
    2021-09-23 23:09:40,876 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,882 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-23 23:09:40,888 INFO sqlalchemy.engine.base.Engine BEGIN (implicit)
    2021-09-23 23:09:40,909 INFO sqlalchemy.engine.base.Engine INSERT INTO way ("index", type, geometry, id, version, visible, ts, uid, user, changeset, "tagLen", tags) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
    2021-09-23 23:09:40,910 INFO sqlalchemy.engine.base.Engine ((0, 'way', '[(448513.7707659328, 4633077.7940055225), (448755.1153237307, 4633081.118174979)]', 4341221, 13, 1, '2019-10-18 03:14:35.000000', 298815, 'miha12', 75868386, 12, "{'bicycle': 'no', 'bridge': 'yes', 'foot': 'no', 'hgv': 'designated', 'highway': 'motorway', 'horse': 'no', 'lanes': '4', 'layer': '1', 'name': 'Stevenson Expressway', 'oneway': 'yes', 'ref': 'I 55', 'sidewalk': 'none'}"), (1, 'way', '[(447570.9468051607, 4632466.38035604), (447571.77960881713, 4632435.107466386), (447571.3757623687, 4632303.392537945), (447582.85475258005, 4632082 ... (752 characters truncated) ... 70255, 4628892.7301831255), (447514.79807274253, 4628844.033236901), (447506.0628183288, 4628815.151442737), (447501.07672099344, 4628793.425852216)]', 4680600, 51, 1, '2018-10-03 17:13:44.000000', 755543, 'Rallysta74', 63167767, 11, "{'bicycle': 'no', 'foot': 'no', 'hgv': 'designated', 'highway': 'motorway', 'horse': 'no', 'lanes': '3', 'maxspeed': '45 mph', 'name': 'Dan Ryan Expressway', 'oneway': 'yes', 'ref': 'I 90;I 94', 'sidewalk': 'none'}"), (2, 'way', '[(447572.1928686945, 4628794.601999841), (447577.11208439805, 4628810.643293995), (447588.43190955324, 4628839.717154565), (447600.00489338615, 46288 ... (790 characters truncated) ... 2.0748888763, 4631583.62467594), (447661.1754830077, 4631640.9794999), (447659.4578182925, 4631778.050495889), (447661.1199019373, 4631929.85292933)]', 4680601, 54, 1, '2018-10-03 17:13:44.000000', 755543, 'Rallysta74', 63167767, 11, "{'bicycle': 'no', 'foot': 'no', 'hgv': 'designated', 'highway': 'motorway', 'horse': 'no', 'lanes': '3', 'maxspeed': '45 mph', 'name': 'Dan Ryan Expressway', 'oneway': 'yes', 'ref': 'I 90;I 94', 'sidewalk': 'none'}"), (3, 'way', '[(447335.3900290237, 4632789.911061618), (447389.3642562064, 4632758.534468282), (447427.49429292436, 4632732.282675224), (447451.3328393642, 4632711 ... (306 characters truncated) ... 436469254, 4632531.445021981), (447566.3767297526, 4632508.795088059), (447569.5315160393, 4632488.441828154), (447570.9468051607, 4632466.38035604)]', 4694845, 10, 1, '2018-07-31 15:54:32.000000', 7409020, 'krinkov76239', 61234561, 14, "{'bicycle': 'no', 'bridge': 'yes', 'destination:ref': 'I 90;I 94', 'foot': 'no', 'hgv': 'designated', 'highway': 'motorway', 'horse': 'no', 'lanes': '3', 'layer': '1', 'name': 'Dan Ryan Expressway', 'oneway': 'yes', 'ref': 'I 90;I 94', 'sidewalk': 'none', 'surface': 'concrete'}"), (4, 'way', '[(447661.1199019373, 4631929.85292933), (447657.2924109428, 4631966.0998019725), (447654.2665205943, 4632188.464519435), (447657.00783448515, 4632307 ... (66 characters truncated) ... 586533, 4632395.591193829), (447650.9840830293, 4632440.299235713), (447647.6048981325, 4632469.703252888), (447635.64587228897, 4632514.4927683445)]', 5010719, 19, 1, '2017-11-08 09:30:33.000000', 5677293, 'bhavana naga', 53605003, 5, "{'destination': 'Lake Shore Drive;22nd Street', 'destination:ref': 'I 55 North', 'destination:street': 'Stevenson Expressway', 'highway': 'motorway_link', 'oneway': 'yes'}"), (5, 'way', '[(448352.56290212314, 4633232.384748061), (448352.15015665186, 4633285.416857806), (448351.2029473984, 4633336.865079657)]', 5016263, 13, 1, '2020-02-03 19:43:59.000000', 18480, 'nickvet419', 80496663, 3, "{'bicycle': 'designated', 'highway': 'residential', 'name': 'South Indiana Avenue'}"), (6, 'way', '[(448226.1866158322, 4633030.25632078), (448221.16919608373, 4633016.125014998), (448221.75144110306, 4632994.558278072), (448223.09103168827, 4632959.528905664), (448225.72372848063, 4632877.345761003)]', 5043580, 52, 1, '2018-08-20 22:03:31.000000', 755543, 'Rallysta74', 61837493, 5, "{'highway': 'primary', 'lanes': '2', 'name': 'South Michigan Avenue', 'old_ref': 'US 54', 'oneway': 'yes'}"), (7, 'way', '[(448084.855663239, 4633106.963871885), (448084.2457152748, 4633117.827295774), (448083.4599485533, 4633161.54652967), (448082.2467892942, 4633228.652335662)]', 5046195, 20, 1, '2020-07-09 07:42:48.000000', 10127767, 'kshtdh', 87749024, 2, "{'highway': 'tertiary', 'name': 'South Wabash Avenue'}")  ... displaying 10 of 2048 total bound parameter sets ...  (2046, 'way', '[(448489.1367087716, 4632731.617494706), (448480.7691805826, 4632727.19224456), (448477.6943392986, 4632724.483067246), (448476.59992608894, 4632722. ... (150 characters truncated) ... 6347, 4632708.2709084265), (448480.83705308073, 4632705.562649171), (448484.37375798746, 4632703.249827929), (448489.46870413574, 4632701.725183719)]', 874627890, 1, 1, '2020-11-19 10:37:09.000000', 10480284, 'zmankits', 94429553, 1, "{'highway': 'service'}"), (2047, 'way', '[(447508.17636690446, 4632990.4608864), (447492.59868775477, 4632990.053746026), (447477.02018830366, 4632989.535613028), (447464.2021984778, 4632988 ... (224 characters truncated) ... 1390974517, 4632969.552559297), (447452.4742006921, 4632964.067943237), (447511.2868949126, 4632952.87563649), (447508.17636690446, 4632990.4608864)]', 875929298, 1, 1, '2020-11-23 00:33:44.000000', 12169822, 'Charles Fang', 94602824, 6, "{'access': 'private', 'amenity': 'parking', 'capacity': '66', 'capacity:disabled': '3', 'parking': 'surface', 'surface': 'asphalt'}"))
    2021-09-23 23:09:40,920 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-23 23:09:40,933 INFO sqlalchemy.engine.base.Engine PRAGMA main.table_info("area")
    2021-09-23 23:09:40,934 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,935 INFO sqlalchemy.engine.base.Engine PRAGMA temp.table_info("area")
    2021-09-23 23:09:40,936 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,938 INFO sqlalchemy.engine.base.Engine 
    CREATE TABLE area (
    	"index" BIGINT, 
    	type TEXT, 
    	geometry TEXT, 
    	id BIGINT, 
    	version BIGINT, 
    	visible BOOLEAN, 
    	ts TIMESTAMP, 
    	uid BIGINT, 
    	user TEXT, 
    	changeset BIGINT, 
    	"tagLen" BIGINT, 
    	tags TEXT, 
    	CHECK (visible IN (0, 1))
    )
    
    
    2021-09-23 23:09:40,938 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,946 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-23 23:09:40,947 INFO sqlalchemy.engine.base.Engine CREATE INDEX ix_area_index ON area ("index")
    2021-09-23 23:09:40,947 INFO sqlalchemy.engine.base.Engine ()
    2021-09-23 23:09:40,953 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-23 23:09:40,958 INFO sqlalchemy.engine.base.Engine BEGIN (implicit)
    2021-09-23 23:09:40,972 INFO sqlalchemy.engine.base.Engine INSERT INTO area ("index", type, geometry, id, version, visible, ts, uid, user, changeset, "tagLen", tags) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
    2021-09-23 23:09:40,973 INFO sqlalchemy.engine.base.Engine ((0, 'area', '[[(447270.41677931603, 4631386.317897884), (447272.2759430798, 4631251.388651448), (447305.0482373512, 4631249.725263117), (447323.65535837604, 46312 ... (189 characters truncated) ... 01972, 4631252.984343427), (447466.14854727266, 4631252.090025023), (447465.7296178561, 4631389.140374752), (447270.41677931603, 4631386.317897884)]]', 47919496, 2, 1, '2008-04-25 05:20:22.000000', 35667, 'encleadus', 256513, 3, "{'access': 'permissive', 'amenity': 'parking', 'created_by': 'Potlatch 0.8b'}"), (1, 'area', '[[(448091.4969382361, 4631406.276904446), (448095.9222827784, 4631219.033281709), (448112.48564639065, 4631215.148788833), (448233.183075586, 4631215.892925133), (448230.9406646214, 4631406.928963752), (448091.4969382361, 4631406.276904446)]]', 56585498, 4, 1, '2020-11-08 04:53:57.000000', 1237170, 'Zol87', 93727773, 11, "{'amenity': 'school', 'denomination': 'catholic', 'ele': '181', 'gnis:county_id': '031', 'gnis:created': '01/15/1980', 'gnis:feature_id': '407014', 'gnis:state_id': '17', 'name': 'De La Salle Institute', 'religion': 'christian', 'wikidata': 'Q5244528', 'wikipedia': 'en:De La Salle Institute'}"), (2, 'area', '[[(447376.78245639656, 4633612.75770272), (447376.95107984403, 4633597.367311116), (447499.94897418364, 4633599.158415663), (447503.6441491695, 46331 ... (187 characters truncated) ... 9387058, 4633220.630221428), (447589.4347749149, 4633614.577419045), (447441.6366611401, 4633613.311801552), (447376.78245639656, 4633612.75770272)]]', 65712820, 3, 1, '2019-02-25 18:29:52.000000', 9403823, 'ferdous1', 67558097, 1, "{'landuse': 'retail'}"), (3, 'area', '[[(448123.3401816751, 4631516.722171732), (448146.14942251577, 4631516.556312433), (448146.2474925716, 4631530.046016658), (448123.4382948734, 4631530.2118760375), (448123.3401816751, 4631516.722171732)]]', 77080582, 5, 1, '2020-11-08 04:53:57.000000', 1237170, 'Zol87', 93727773, 11, "{'addr:city': 'Chicago', 'addr:housenumber': '3333', 'addr:postcode': '60616', 'addr:state': 'IL', 'addr:street': 'South Wabash Avenue', 'addr:street ... (12 characters truncated) ... ash', 'addr:street:prefix': 'South', 'addr:street:type': 'Avenue', 'building': 'university', 'chicago:building_id': '413609', 'name': 'Pi Kappa Phi'}"), (4, 'area', '[[(448120.3766811521, 4632076.1799451), (448125.62169878493, 4632063.273141703), (448131.8086493493, 4632056.5773026915), (448144.21794638335, 463204 ... (103 characters truncated) ... 2314078657, 4632149.568290073), (448169.2374235309, 4632164.86148963), (448130.5119293234, 4632168.251966819), (448120.3766811521, 4632076.1799451)]]', 268284574, 1, 1, '2011-10-21 18:35:17.000000', 365944, 'homeslice60148', 9618544, 3, "{'leisure': 'pitch', 'name': 'Ed Glancy Field', 'sport': 'baseball'}"), (5, 'area', '[[(447791.12273616565, 4632246.019755288), (447796.2266520122, 4632237.754884075), (447808.386860645, 4632245.204976656), (447815.6160364872, 4632233 ... (226 characters truncated) ... 991335, 4632273.896790994), (447793.37972499547, 4632268.509510805), (447802.8325710957, 4632253.19555462), (447791.12273616565, 4632246.019755288)]]', 268284580, 4, 1, '2020-11-22 04:12:26.000000', 1237170, 'Zol87', 94568168, 10, "{'addr:city': 'Chicago', 'addr:housenumber': '2951', 'addr:postcode': '60616', 'addr:state': 'IL', 'addr:street': 'South Federal Street', 'addr:street:name': 'Federal', 'addr:street:prefix': 'South', 'addr:street:type': 'Street', 'building': 'apartments', 'chicago:building_id': '402691'}"), (6, 'area', '[[(447895.7391513325, 4632640.230371949), (447908.65927500726, 4632627.933496084), (447899.1802954988, 4632618.043139319), (447907.3925161764, 463261 ... (227 characters truncated) ... 186404, 4632648.443910265), (447914.93759168126, 4632639.523835942), (447904.5404523348, 4632649.415067489), (447895.7391513325, 4632640.230371949)]]', 268284582, 4, 1, '2020-11-22 04:12:26.000000', 1237170, 'Zol87', 94568168, 10, "{'addr:city': 'Chicago', 'addr:housenumber': '2710', 'addr:postcode': '60616', 'addr:state': 'IL', 'addr:street': 'South State Street', 'addr:street:name': 'State', 'addr:street:prefix': 'South', 'addr:street:type': 'Street', 'building': 'apartments', 'chicago:building_id': '397507'}"), (7, 'area', '[[(447893.96983109193, 4632328.797211836), (447911.2994904586, 4632323.341044181), (447907.4163516417, 4632310.056620988), (447917.6388962657, 463230 ... (227 characters truncated) ... 868442, 4632347.728419986), (447910.2744814622, 4632335.328932264), (447897.2510519652, 4632339.243595737), (447893.96983109193, 4632328.797211836)]]', 268284588, 4, 1, '2020-11-22 04:12:26.000000', 1237170, 'Zol87', 94568168, 10, "{'addr:city': 'Chicago', 'addr:housenumber': '2920', 'addr:postcode': '60616', 'addr:state': 'IL', 'addr:street': 'South State Street', 'addr:street:name': 'State', 'addr:street:prefix': 'South', 'addr:street:type': 'Street', 'building': 'apartments', 'chicago:building_id': '401409'}")  ... displaying 10 of 1338 total bound parameter sets ...  (1336, 'area', '[[(447316.7332435035, 4633212.128653131), (447317.27700232307, 4633168.877379826), (447317.7982343582, 4633127.069701676), (447533.54941587936, 4633129.475769381), (447533.12076813163, 4633171.626930953), (447532.69303029997, 4633213.9002247285), (447316.7332435035, 4633212.128653131)]]', 1700790584, 1, 1, '2020-09-23 07:48:03.000000', 8476127, 'nightrider209', 91346203, 3, "{'footway': 'sidewalk', 'highway': 'footway', 'surface': 'concrete'}"), (1337, 'area', '[[(447423.91390974517, 4632969.552559297), (447452.4742006921, 4632964.067943237), (447511.2868949126, 4632952.87563649), (447508.17636690446, 463299 ... (228 characters truncated) ... 64592, 4632981.191676769), (447428.83290377876, 4632975.74521859), (447425.20199204324, 4632971.974675984), (447423.91390974517, 4632969.552559297)]]', 1751858596, 1, 1, '2020-11-23 00:33:44.000000', 12169822, 'Charles Fang', 94602824, 6, "{'access': 'private', 'amenity': 'parking', 'capacity': '66', 'capacity:disabled': '3', 'parking': 'surface', 'surface': 'asphalt'}"))
    2021-09-23 23:09:40,980 INFO sqlalchemy.engine.base.Engine COMMIT
    

* 使用[DB Browser for SQLite](https://www.google.com/search?q=db+browsser&oq=db+browsser&aqs=chrome..69i57.10841j0j15&sourceid=chrome&ie=UTF-8)读取SQLite文件查看数据

<a href=""><img src="./imgs_pyd/017.jpg" height="auto" width="auto" title="caDesign"></a>

<a href=""><img src="./imgs_pyd/018.jpg" height="auto" width="auto" title="caDesign"></a>

### 2.2 GHPython读取SQLite中的数据

<a href=""><img src="./imgs_pyd/019.jpg" height="auto" width="auto" title="caDesign"></a>

<a href=""><img src="./imgs_pyd/020.jpg" height="auto" width="auto" title="caDesign"></a>

**组件-SQL read**

```python
"""Read SQL table.
    Inputs:
        database_sql: SQL database file path
        table: Specify a table
    Output:
        output: table content"""

__author__ = "Richie Bao-Chicago.IIT(driverless city project)"
__version__ = "2020.11.30"

ghenv.Component.Name = 'SQL read'
ghenv.Component.NickName = 'SQL read'
ghenv.Component.Message = '0.0.1'
ghenv.Component.Category = 'driverless-city'
ghenv.Component.SubCategory = '0 :: SQL'
ghenv.Component.AdditionalHelpFromDocStrings = '5'

import rhinoscriptsyntax as rs
import sqlite3
import ghpythonlib.treehelpers as th
import Rhino

conn=sqlite3.connect(database_sql)
with conn:
    cur=conn.cursor()
    cur.execute("SELECT * FROM %s"%table)
    rows=cur.fetchall()

layerTree=th.list_to_tree(rows, source=[0,])
output=layerTree
```

**组件-coordi2pts**

```python
"""Read SQL table.
    Inputs:
        sql_data: SQL data readed
        field: data field ready to handle
    Output:
        out_by_field: export data by the field specified"""

__author__ = "Richie Bao-Chicago.IIT(driverless city project)"
__version__ = "2020.12.09"

ghenv.Component.Name = 'coordinates string to points'
ghenv.Component.NickName = 'coordi2pts'
ghenv.Component.Message = '0.0.1'
ghenv.Component.Category = 'driverless-city'
ghenv.Component.SubCategory = '0 :: SQL'
ghenv.Component.AdditionalHelpFromDocStrings = '5'

import rhinoscriptsyntax as rs
import sqlite3
import ghpythonlib.treehelpers as th
import Rhino

data=th.tree_to_list(sql_data)

if field=='geometry':    
    x=[eval(d[2])[0] for d in data]
    y=[eval(d[2])[1] for d in data]
    
    out_by_field=th.list_to_tree([x,y],source=[0,0])
    
if field=='tags':
    tags=[d[11] for d in data]
    out_by_field=tags
```

**组件-way2pts**

```python
"""Read SQL table.
    Inputs:
        sql_data: SQL data readed
        field: data field ready to handle
    Output:
        out_by_field: export data by the field specified"""

__author__ = "Richie Bao-Chicago.IIT(driverless city project)"
__version__ = "2020.12.09"

ghenv.Component.Name = 'way to points'
ghenv.Component.NickName = 'way2pts'
ghenv.Component.Message = '0.0.1'
ghenv.Component.Category = 'driverless-city'
ghenv.Component.SubCategory = '0 :: SQL'
ghenv.Component.AdditionalHelpFromDocStrings = '5'

import rhinoscriptsyntax as rs
import sqlite3
import ghpythonlib.treehelpers as th
import Rhino

data=th.tree_to_list(sql_data)

if field=='geometry':   
    x=[[v[0] for v in eval(d[2])] for d in data]
    y=[[v[1] for v in eval(d[2])] for d in data]
    
    X=th.list_to_tree(x,source=[0,0])
    Y=th.list_to_tree(y,source=[0,0])    
    
if field=='tags':
    tags=[d[11] for d in data]
    out_by_field=tags
```

**组件-tags2group**

```python
"""Processing the tags of OSM.
    Inputs:
        tags: tags string
        field: data field ready to handle
    Output:
        labels: all of the tags name
        tags_bool: 
            """

__author__ = "Richie Bao-Chicago.IIT(driverless city project)"
__version__ = "2020.12.09"

ghenv.Component.Name = 'tags group'
ghenv.Component.NickName = 'tags2group'
ghenv.Component.Message = '0.0.1'
ghenv.Component.Category = 'driverless-city'
ghenv.Component.SubCategory = '0 :: SQL'
ghenv.Component.AdditionalHelpFromDocStrings = '5'

import rhinoscriptsyntax as rs
import sqlite3
import ghpythonlib.treehelpers as th
import Rhino

tag_b=[]
if tag_given is not None:
    for row in tags:
        #print(eval(row).keys())
        if tag_given in eval(row).keys():
            tag_b.append(1)
        else:
            tag_b.append(0)

tags_bool=tag_b
```

**组件-closest obj Ikx**

```python
"""find the nearest obj index.
    Inputs:
        objs: objects list
        point: a point used to find the nearest object
    Output:
        closest_obj: the closest object
        obj_idx: the closest object index
        closest_point:the nereast point projecting to the closest object
            """

__author__ = "Richie Bao-Chicago.IIT(driverless city project)"
__version__ = "2020.12.09"

ghenv.Component.Name = 'closest objec indx'
ghenv.Component.NickName = 'closest obj Ikx'
ghenv.Component.Message = '0.0.1'
ghenv.Component.Category = 'driverless-city'
ghenv.Component.SubCategory = '0 :: SQL'
ghenv.Component.AdditionalHelpFromDocStrings = '5'

import rhinoscriptsyntax as rs
import sqlite3
import ghpythonlib.treehelpers as th
import Rhino

results=rs.PointClosestObject(point,objs)
if results:
    obj=results[0]
    closest_point=results[1]

    idx=0
    for o in objs:
        if o==obj:
            obj_idx=idx
        idx+=1

closest_obj=obj
```

<a href=""><img src="./imgs_pyd/021.jpg" height="auto" width="auto" title="caDesign"></a>

<a href=""><img src="./imgs_pyd/022.jpg" height="auto" width="auto" title="caDesign"></a>

## 3. [GeoJson](https://geojson.org/)格式的建筑高度数据

> [.json](https://en.wikipedia.org/wiki/JSON)

### 3.1 将GeoJson格式的建筑高度数据写入数据库

```json
{"type":"FeatureCollection","features":[
{"id":"osm-r2824157","type":"Feature","properties":{"name":"Fuller Park Fieldhouse","height":9.2,"origId":327623,"roofShape":"gabled"},"geometry":{"type":"Polygon","coordinates":[[[-87.634777,41.81162],[-87.634383,41.811628],[-87.634388,41.811802],[-87.634418,41.811802],[-87.63442,41.811872],[-87.634332,41.811939],[-87.634126,41.811942],[-87.634138,41.812346],[-87.63507,41.81233],[-87.635058,41.81193],[-87.634847,41.811932],[-87.634752,41.811856],[-87.634751,41.811796],[-87.634782,41.811796],[-87.634777,41.81162]],[[-87.63469,41.811904],[-87.634749,41.811946],[-87.634803,41.811984],[-87.634809,41.812191],[-87.634773,41.812192],[-87.634598,41.812195],[-87.634419,41.812198],[-87.634389,41.812199],[-87.634382,41.811982],[-87.634426,41.811948],[-87.634477,41.811908],[-87.63459,41.811906],[-87.63469,41.811904]]]}},
```


```python
def json_3DBuilding2gp(json_3DBuilding_fp,epsg=None,boundary=None):
    import pandas as pd
    import geopandas as gpd
    from shapely.geometry import Polygon,MultiPolygon
    from shapely.geometry import shape
    import numpy as np
    '''
    funtion - 读取json格式3d 建筑，转换为GeoPandas格式
    '''
    
    building_3D=pd.read_json(json_3DBuilding_fp)
    # print(building_3D)
    feature_columns=list(building_3D.iloc[0,1].keys())
    properties_columns=list(building_3D.iloc[0,1]['properties'].keys())    
    # print(feature_columns,'\n',properties_columns)
    
    for column in feature_columns:
        building_3D[column]=building_3D.features.apply(lambda row:row[column])
    
    #print(building_3D.geometry)
    building_3D['geometry']=building_3D.geometry.apply(shape)  
    
    mask=Polygon(boundary)
    building_3D['mask']=building_3D.geometry.apply(lambda row:row.within(mask)) 
    # print(building_3D.geometry)
    # print(building_3D.shape)
    # building_3D.drop(building_3D[building_3D['mask'] == False].index, inplace=True)
    building_3D.query('mask', inplace=True)
    # print(building_3D.shape)  
    
    building_3D['height']=building_3D.properties.apply(lambda row:row['height'] if 'height' in row.keys() else 0 )
    
    if epsg is not None:
        crs_target={'init': 'epsg:%d'%epsg}
        
    crs={'init': 'epsg:4326'}    
    if epsg is not None:    
        building_3D_db=pd.DataFrame(gpd.GeoDataFrame(building_3D,crs=crs).to_crs(epsg=epsg))
        # print("+"*50)
        # print(osm_node_db)
    else:
        building_3D_db=pd.DataFrame(building_3D)        
    
    building_3D_db.geometry=building_3D_db.geometry.apply(lambda row:str(list(zip(*row.exterior.coords.xy))) )  
    
    for column in building_3D_db.columns:
        building_3D_db[column]=building_3D_db[column].apply(lambda row:str(row))
    
    building_3D_db_drop=building_3D_db.drop(['features'],axis=1)
    return building_3D_db_drop

def building_3D2SQLite_database(building_3D_db,db_fp):
    from sqlalchemy import create_engine 
    '''
    function - 将3D building 写入数据库
    '''
    engine = create_engine('sqlite:///'+'\\\\'.join(db_fp.split('\\')),echo=True) 
    try:
        building_3D_db.to_sql('building_3d',con=engine,if_exists='replace') #if_exists='append'
        print("data has been written into database...")        
    except:
        print("_"*50,'\n','the building_3d table has been existed...')
```


```python
json_3DBuilding_fp=r'./data/Chicago_3dbuildings.json'
epsg=32616
boundary=[(-87.630609, 41.830851),(-87.603174, 41.831122),(-87.603020, 41.847229),(-87.641234, 41.847334)]
building_3D_db=json_3DBuilding2gp(json_3DBuilding_fp,epsg,boundary)

db_fp=r'../database/OSM_sqlit.db'
building_3D2SQLite_database(building_3D_db,db_fp)
```

    C:\Users\richi\anaconda3\envs\py_la_archi\lib\site-packages\pyproj\crs\crs.py:53: FutureWarning: '+init=<authority>:<code>' syntax is deprecated. '<authority>:<code>' is the preferred initialization method. When making the change, be mindful of axis order changes: https://pyproj4.github.io/pyproj/stable/gotchas.html#axis-order-changes-in-proj-6
      return _prepare_from_string(" ".join(pjargs))
    

    2021-09-24 16:09:43,581 INFO sqlalchemy.engine.base.Engine SELECT CAST('test plain returns' AS VARCHAR(60)) AS anon_1
    2021-09-24 16:09:43,581 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:09:43,582 INFO sqlalchemy.engine.base.Engine SELECT CAST('test unicode returns' AS VARCHAR(60)) AS anon_1
    2021-09-24 16:09:43,583 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:09:43,585 INFO sqlalchemy.engine.base.Engine PRAGMA main.table_info("building_3d")
    2021-09-24 16:09:43,586 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:09:43,588 INFO sqlalchemy.engine.base.Engine PRAGMA temp.table_info("building_3d")
    2021-09-24 16:09:43,589 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:09:43,593 INFO sqlalchemy.engine.base.Engine 
    CREATE TABLE building_3d (
    	"index" BIGINT, 
    	type TEXT, 
    	id TEXT, 
    	properties TEXT, 
    	geometry TEXT, 
    	mask TEXT, 
    	height TEXT
    )
    
    
    2021-09-24 16:09:43,594 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:09:43,601 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-24 16:09:43,602 INFO sqlalchemy.engine.base.Engine CREATE INDEX ix_building_3d_index ON building_3d ("index")
    2021-09-24 16:09:43,602 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:09:43,608 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-24 16:09:43,612 INFO sqlalchemy.engine.base.Engine BEGIN (implicit)
    2021-09-24 16:09:43,628 INFO sqlalchemy.engine.base.Engine INSERT INTO building_3d ("index", type, id, properties, geometry, mask, height) VALUES (?, ?, ?, ?, ?, ?, ?)
    2021-09-24 16:09:43,629 INFO sqlalchemy.engine.base.Engine ((56, 'Feature', 'osm-r2393146', "{'type': 'education:school', 'height': 11.4, 'origId': 367652, 'roofShape': 'flat'}", '[(448650.5212520368, 4632386.264357656), (448651.24956760684, 4632337.515950401), (448663.2073731921, 4632337.762948572), (448663.328658763, 4632331. ... (943 characters truncated) ... 1221128, 4632331.467585399), (448708.16227265325, 4632331.332670397), (448706.57062712236, 4632387.08230708), (448650.5212520368, 4632386.264357656)]', 'True', '11.4'), (241, 'Feature', 'osm-w38540291', "{'name': 'Pi Kappa Phi', 'type': 'education:university', 'height': 8.3, 'origId': 354271, 'roofShape': 'flat'}", '[(448146.1160477731, 4631516.534348619), (448146.2137143102, 4631529.968539656), (448123.3796064278, 4631530.134580316), (448123.2818968789, 4631516.700389199), (448146.1160477731, 4631516.534348619)]', 'True', '8.3'), (337, 'Feature', 'US-USGS-0487101', "{'height': 3.5, 'origId': 373362}", '[(446916.3401659091, 4632756.599988102), (446916.6267775627, 4632750.49105167), (446927.7554612488, 4632750.963368281), (446927.3858211356, 4632757.072922156), (446916.3401659091, 4632756.599988102)]', 'True', '3.5'), (346, 'Feature', 'US-USGS-0499658', "{'height': 7.7, 'origId': 372831, 'roofShape': 'gabled'}", '[(447228.7628102929, 4632712.421425763), (447228.55894850753, 4632718.529732337), (447211.867121163, 4632717.987099211), (447212.0709686851, 4632711.878792032), (447228.7628102929, 4632712.421425763)]', 'True', '7.7'), (347, 'Feature', 'US-USGS-0499704', "{'height': 8, 'origId': 373034}", '[(447229.96203657245, 4632728.623322101), (447229.7581745599, 4632734.731628791), (447213.06638524914, 4632734.188993193), (447213.27023299737, 4632728.0806859), (447229.96203657245, 4632728.623322101)]', 'True', '8.0'), (349, 'Feature', 'US-USGS-0489967', "{'height': 3.5, 'origId': 375097, 'roofShape': 'gabled'}", '[(447252.04613085603, 4632863.253596403), (447247.39549713174, 4632863.065940122), (447247.56568660756, 4632852.405541106), (447252.21550584765, 4632852.482170931), (447252.04613085603, 4632863.253596403)]', 'True', '3.5'), (350, 'Feature', 'US-USGS-0489039', "{'height': 7.3, 'origId': 372939, 'roofShape': 'flat'}", '[(447280.20018280693, 4632729.806130878), (447257.3674271769, 4632729.64189979), (447257.41184490983, 4632724.423035242), (447280.24461724295, 4632724.587266626), (447280.20018280693, 4632729.806130878)]', 'True', '7.3'), (351, 'Feature', 'US-USGS-0489684', "{'height': 3.4, 'origId': 375317, 'roofShape': 'gabled'}", '[(447263.43344476406, 4632876.271215753), (447263.55102602014, 4632869.7194164535), (447269.7789894401, 4632869.895417077), (447269.6614024691, 4632876.447216222), (447263.43344476406, 4632876.271215753)]', 'True', '3.4')  ... displaying 10 of 2464 total bound parameter sets ...  (95433, 'Feature', 'US-47-4467159', '{}', '[(448518.15930913284, 4631268.236246056), (448510.686057698, 4631268.290186402), (448510.6363695259, 4631261.406554708), (448518.1096281733, 4631261.352614376), (448518.15930913284, 4631268.236246056)]', 'True', '0.0'), (95434, 'Feature', 'US-47-4700761', '{}', '[(448628.3222299626, 4631586.6593138985), (448622.5099613635, 4631586.701179396), (448622.47317228385, 4631581.593966385), (448628.2854450452, 4631581.552100896), (448628.3222299626, 4631586.6593138985)]', 'True', '0.0'))
    2021-09-24 16:09:43,643 INFO sqlalchemy.engine.base.Engine COMMIT
    data has been written into database...
    

<a href=""><img src="./imgs_pyd/023.jpg" height="auto" width="auto" title="caDesign"></a>

### 3.2 GHPython中读取建筑高度数据

<a href=""><img src="./imgs_pyd/024.jpg" height="auto" width="auto" title="caDesign"></a>

**组件-3D building**

```python
"""Read SQL table.
    Inputs:
        sql_data: SQL data readed 
        
    Output:
        X: building points Coordinate-x
        Y: building points Coordinate-y
        height:building height     
        properties: building properties
        """

__author__ = "Richie Bao-Chicago.IIT(driverless city project)"
__version__ = "2021.01.13"

ghenv.Component.Name = '3D building'
ghenv.Component.NickName = '3D building'
ghenv.Component.Message = '0.0.1'
ghenv.Component.Category = 'driverless-city'
ghenv.Component.SubCategory = '0 :: SQL'
ghenv.Component.AdditionalHelpFromDocStrings = '5'

import rhinoscriptsyntax as rs
import sqlite3
import ghpythonlib.treehelpers as th
import Rhino

data=th.tree_to_list(sql_data)


x=[[v[0] for v in eval(d[4])] for d in data]
y=[[v[1] for v in eval(d[4])] for d in data]
    
X=th.list_to_tree(x,source=[0,0])
Y=th.list_to_tree(y,source=[0,0])
height=[d[6] for d in data]
properties=[d[3] for d in data]
```

<a href=""><img src="./imgs_pyd/026.jpg" height="auto" width="auto" title="caDesign"></a>

<a href=""><img src="./imgs_pyd/025.jpg" height="auto" width="auto" title="caDesign"></a>

## 4. .csv格式的交通数据

>[comma-separated values (CSV)](https://en.wikipedia.org/wiki/Comma-separated_values)

[Chicago data portal](https://data.cityofchicago.org/)

### 4.1 将交通数据写入数据库


```python
def traffic_congestion2gp_current(csv_trafficCongestionSegs_fp,epsg=None,boundary=None):
    '''  
    function - convert traffic congestion segments dataset to geompandas format
    Parameters
    ----------
    csv_trafficCongestionSegs_fp : TYPE
        DESCRIPTION.
    epsg : TYPE, optional
        DESCRIPTION. The default is None.
    boudnary : TYPE, optional
        DESCRIPTION. The default is None.

    Returns
    -------
    traffic_congestion_db : TYPE
        DESCRIPTION.
    '''
 
    import pandas as pd
    import geopandas as gpd
    from shapely.geometry import LineString
    from shapely.geometry import Polygon,MultiPolygon
    
    traffic_congestion=pd.read_csv(csv_trafficCongestionSegs_fp,sep=',')
    #print(traffic_congestion)
    #print(traffic_congestion.columns)
    traffic_congestion['geometry']=traffic_congestion.apply(lambda row:LineString([(row['START_LONGITUDE'],row[' START_LATITUDE']),(row['END_LONGITUDE'],row[' END_LATITUDE'])]),axis=1)
    #print(traffic_congestion)
    if boundary:
        mask=Polygon(boundary)
        traffic_congestion['mask']=traffic_congestion.geometry.apply(lambda row:row.within(mask))
        traffic_congestion.query('mask',inplace=True)
    
    crs={'init': 'epsg:4326'}
    if epsg is not None: 
        traffic_congestion_db=pd.DataFrame(gpd.GeoDataFrame(traffic_congestion,crs=crs).to_crs(epsg=epsg))
    else:
        traffic_congestion_db=pd.DataFrame(traffic_congestion)

    #print(traffic_congestion_db.geometry)
    traffic_congestion_db.geometry=traffic_congestion_db.geometry.apply(lambda row:str(list(row.coords)))
    # print(traffic_congestion_db.geometry)
    #print(traffic_congestion_db.columns)
    
    return traffic_congestion_db

def traffic_congestion2gp_hitorical(csv_trafficCongestionSegs_historical_fp,segs_id=None,merge_method='mean'):
    import pandas as pd
    trafficCongestionSegs_historical=pd.read_csv(csv_trafficCongestionSegs_historical_fp,sep=',')
    # print(trafficCongestionSegs_historical.shape)
    # print(segs_id.shape)
    # print(trafficCongestionSegs_historical['SEGMENTID'].isin(segs_id))
    trafficCongestionSegs_historical_extraction=trafficCongestionSegs_historical[trafficCongestionSegs_historical['SEGMENTID'].isin(segs_id)]
    # print(trafficCongestionSegs_historical_extraction)
    if merge_method=='max':
        trCon_merge=trafficCongestionSegs_historical_extraction.groupby(['SEGMENTID']).max()
    else:
        trCon_merge=trafficCongestionSegs_historical_extraction.groupby(['SEGMENTID']).mean()
    
    # print(trCon_merge_mean)
    return trCon_merge

def df2SQLite(df,db_fp,table_name):
    '''        
    function - write dataframe type date into SQLite,given talbe name
    Parameters
    ----------
    df : TYPE
        DESCRIPTION.
    db_fp : TYPE
        DESCRIPTION.
    table_name : TYPE
        DESCRIPTION.

    Returns
    -------
    None.

    '''
    from sqlalchemy import create_engine
    engine=create_engine('sqlite:///'+'\\\\'.join(db_fp.split('\\')),echo=True) 
    
    try:
        df.to_sql(table_name,con=engine,if_exists='replace') #if_exists='append'
        print("data has been written into database...")
        
    except:
        print("_"*50,'\n','the table has been existed...')
```


```python
csv_trafficCongestionSegs_fp=r'./data/Chicago_Traffic_Tracker_-_Congestion_Estimates_by_Segments.csv'
epsg=32616
# boundary=[(-87.630609, 41.830851),(-87.603174, 41.831122),(-87.603020, 41.847229),(-87.641234, 41.847334)] #IIT region
bottom_left=(-87.652605,  41.828429)
top_right=(-87.589944,  41.923381)
boundary=[(bottom_left[0],bottom_left[1]),(top_right[0],bottom_left[1]),(top_right[0], top_right[1]),(bottom_left[0], top_right[1])]

traffic_congestion_current_db=traffic_congestion2gp_current(csv_trafficCongestionSegs_fp,epsg=epsg,boundary=None)

db_fp=r'../database/OSM_sqlit.db'  
df2SQLite(traffic_congestion_current_db,db_fp,table_name='trafficCongestionSegs_current')
```

    2021-09-24 16:40:33,739 INFO sqlalchemy.engine.base.Engine SELECT CAST('test plain returns' AS VARCHAR(60)) AS anon_1
    2021-09-24 16:40:33,740 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:40:33,740 INFO sqlalchemy.engine.base.Engine SELECT CAST('test unicode returns' AS VARCHAR(60)) AS anon_1
    2021-09-24 16:40:33,741 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:40:33,741 INFO sqlalchemy.engine.base.Engine PRAGMA main.table_info("trafficCongestionSegs_current")
    2021-09-24 16:40:33,742 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:40:33,744 INFO sqlalchemy.engine.base.Engine PRAGMA temp.table_info("trafficCongestionSegs_current")
    2021-09-24 16:40:33,744 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:40:33,747 INFO sqlalchemy.engine.base.Engine 
    CREATE TABLE "trafficCongestionSegs_current" (
    	"index" BIGINT, 
    	"SEGMENTID" BIGINT, 
    	"STREET" TEXT, 
    	"DIRECTION" TEXT, 
    	"FROM_STREET" TEXT, 
    	"TO_STREET" TEXT, 
    	"LENGTH" FLOAT, 
    	" STREET_HEADING" TEXT, 
    	" COMMENTS" TEXT, 
    	"START_LONGITUDE" FLOAT, 
    	" START_LATITUDE" FLOAT, 
    	"END_LONGITUDE" FLOAT, 
    	" END_LATITUDE" FLOAT, 
    	" CURRENT_SPEED" BIGINT, 
    	" LAST_UPDATED" TEXT, 
    	geometry TEXT
    )
    
    
    2021-09-24 16:40:33,747 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:40:33,756 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-24 16:40:33,757 INFO sqlalchemy.engine.base.Engine CREATE INDEX "ix_trafficCongestionSegs_current_index" ON "trafficCongestionSegs_current" ("index")
    2021-09-24 16:40:33,758 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:40:33,766 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-24 16:40:33,771 INFO sqlalchemy.engine.base.Engine BEGIN (implicit)
    2021-09-24 16:40:33,788 INFO sqlalchemy.engine.base.Engine INSERT INTO "trafficCongestionSegs_current" ("index", "SEGMENTID", "STREET", "DIRECTION", "FROM_STREET", "TO_STREET", "LENGTH", " STREET_HEADING", " COMMENTS", "START_LONGITUDE", " START_LATITUDE", "END_LONGITUDE", " END_LATITUDE", " CURRENT_SPEED", " LAST_UPDATED", geometry) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
    2021-09-24 16:40:33,789 INFO sqlalchemy.engine.base.Engine ((0, 1284, 'Chicago', 'WB', 'Lake Shore Dr', 'Michigan', 0.37, 'E', None, -87.617048, 41.896936, -87.624241, 41.896834999999996, -1, '2011-08-10 00:00:00.0', '[(448815.06530773675, 4638517.30991686), (448218.31242001, 4638510.412457186)]'), (1, 951, 'Washington', 'WB', 'Kedzie', 'Schraeder', 0.28, 'W', None, -87.7061691246, 41.882931525100005, -87.7117472865, 41.8828184746, -1, '2010-07-21 14:51:00.0', '[(441409.516977097, 4637019.435307947), (440946.59307760914, 4637010.707041764)]'), (2, 750, 'Elston', 'SE', 'Milwaukee', 'Austin', 0.33, 'N', None, -87.7832243493, 41.992664603499996, -87.77807307350001, 41.9899051234, -1, '2010-07-21 14:51:10.0', '[(435127.5575748982, 4649258.443495537), (435551.4434640395, 4648948.169465974)]'), (3, 1164, 'Harlem', 'SB', 'Ogden', 'Pershing', 0.173022844032, 'S', 'Outside City Limits', -87.80292190600001, 41.8237508093, -87.8030252034, 41.8212448346, -1, '2010-07-21 14:51:15.0', '[(433320.49313555943, 4630519.347005599), (433309.3141645007, 4630241.197231921)]'), (4, 1122, '127th', 'EB', 'Western', 'I-57 Expy', 0.9078922218850001, 'W', 'Outside City Limits', -87.6800769707, 41.6625116506, -87.66253983680001, 41.6628497334, -1, '2010-07-21 14:51:06.0', '[(443380.8640801675, 4612529.758868322), (444841.1971633171, 4612555.921856456)]'), (5, 682, 'Dr Martin L King Jr', 'NB', 'Bishop Ford Expy', '95th', 0.58, 'S', None, -87.61377145530001, 41.7134413253, -87.61397181049999, 41.7218553843, -1, '2010-07-21 14:50:44.0', '[(448941.37676864164, 4618142.717913102), (448931.3712178549, 4619077.000228344)]'), (6, 794, 'Hollywood', 'WB', 'Lake Shore Dr', 'Ridge', 0.45, 'W', 'part of Peterson', -87.65351719709999, 41.9855339538, -87.6624036279, 41.985180094, -1, '2010-07-21 14:50:56.0', '[(445864.8780041314, 4648376.6080441335), (445128.4492221212, 4648342.974271909)]'), (7, 964, 'Ramp To Kennedy', 'WB', 'Orleans', 'Kennedy Expy', 0.57, 'W', 'Oneway', -87.6370995163, 41.893150334299996, -87.6481798833, 41.8925171987, -1, '2010-07-21 14:51:01.0', '[(447148.63744654204, 4638109.15366914), (446228.91684667446, 4638045.742758888)]')  ... displaying 10 of 1257 total bound parameter sets ...  (1255, 400, 'Western', 'SB', 'Diversey', 'Kennedy Expy', 0.3, 'N', None, -87.6880664353, 41.932180159699996, -87.6879335859, 41.927982840300004, -1, '2021-02-17 10:30:52.0', '[(442955.35755180067, 4642475.208799379), (442962.6324383224, 4642009.098886211)]'), (1256, 1297, 'State', 'SB', 'Chicago', 'Wacker', 0.68, 'N', None, -87.628298, 41.896677000000004, -87.62807600000001, 41.886813000000004, -1, '2021-02-17 10:30:53.0', '[(447881.6484955768, 4638495.326660483), (447892.0472734206, 4637400.016669467)]'))
    2021-09-24 16:40:33,801 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-24 16:40:33,810 INFO sqlalchemy.engine.base.Engine SELECT name FROM sqlite_master WHERE type='table' ORDER BY name
    2021-09-24 16:40:33,810 INFO sqlalchemy.engine.base.Engine ()
    data has been written into database...
    


```python
csv_trafficCongestionSegs_historical_fp=r'./data/Chicago_Traffic_Tracker_-_Historical_Congestion_Estimates_by_Segment_-_2011-2018.csv'
traffic_congestion2gp_hitorical_db=traffic_congestion2gp_hitorical(csv_trafficCongestionSegs_historical_fp,segs_id=traffic_congestion_current_db.SEGMENTID)
```


```python
df2SQLite(traffic_congestion2gp_hitorical_db,db_fp,table_name='trafficCongestionSegs_historical')
```

    2021-09-24 16:53:08,022 INFO sqlalchemy.engine.base.Engine SELECT CAST('test plain returns' AS VARCHAR(60)) AS anon_1
    2021-09-24 16:53:08,023 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:53:08,024 INFO sqlalchemy.engine.base.Engine SELECT CAST('test unicode returns' AS VARCHAR(60)) AS anon_1
    2021-09-24 16:53:08,025 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:53:08,026 INFO sqlalchemy.engine.base.Engine PRAGMA main.table_info("trafficCongestionSegs_historical")
    2021-09-24 16:53:08,027 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:53:08,029 INFO sqlalchemy.engine.base.Engine PRAGMA temp.table_info("trafficCongestionSegs_historical")
    2021-09-24 16:53:08,030 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:53:08,033 INFO sqlalchemy.engine.base.Engine 
    CREATE TABLE "trafficCongestionSegs_historical" (
    	"SEGMENTID" BIGINT, 
    	"BUS COUNT                " FLOAT, 
    	"MESSAGE COUNT" FLOAT, 
    	"SPEED" FLOAT
    )
    
    
    2021-09-24 16:53:08,033 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:53:08,038 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-24 16:53:08,039 INFO sqlalchemy.engine.base.Engine CREATE INDEX "ix_trafficCongestionSegs_historical_SEGMENTID" ON "trafficCongestionSegs_historical" ("SEGMENTID")
    2021-09-24 16:53:08,040 INFO sqlalchemy.engine.base.Engine ()
    2021-09-24 16:53:08,045 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-24 16:53:08,047 INFO sqlalchemy.engine.base.Engine BEGIN (implicit)
    2021-09-24 16:53:08,053 INFO sqlalchemy.engine.base.Engine INSERT INTO "trafficCongestionSegs_historical" ("SEGMENTID", "BUS COUNT                ", "MESSAGE COUNT", "SPEED") VALUES (?, ?, ?, ?)
    2021-09-24 16:53:08,054 INFO sqlalchemy.engine.base.Engine ((1, 0.7203484953765568, 3.73788016462665, 13.91212785290502), (2, 1.0730129884013042, 7.173820086589342, 15.778021273184029), (3, 1.0188679245283019, 5.548452616387835, 17.079961515847987), (4, 1.0245336469079054, 6.363354535250414, 17.13084611684216), (5, 0.9739697471805013, 5.493291998503394, 19.6789245817521), (6, 0.944465230637661, 5.8835319899513605, 15.222833930194025), (7, 0.8658399700678817, 5.718798439253835, 16.22016141963761), (8, 0.8914426211983537, 5.820140039553157, 14.601902827516168)  ... displaying 10 of 1071 total bound parameter sets ...  (1308, 0.547062910898498, 1.2999091346410818, 6.519001550056123), (1309, 0.9144796621946657, 3.4301672991608316, 10.72761772409001))
    2021-09-24 16:53:08,060 INFO sqlalchemy.engine.base.Engine COMMIT
    2021-09-24 16:53:08,067 INFO sqlalchemy.engine.base.Engine SELECT name FROM sqlite_master WHERE type='table' ORDER BY name
    2021-09-24 16:53:08,067 INFO sqlalchemy.engine.base.Engine ()
    data has been written into database...
    

<a href=""><img src="./imgs_pyd/028.jpg" height="auto" width="auto" title="caDesign"></a>

**组件-traffic congestion_current**

```python
"""Read SQL table.
    Inputs:
        sql_data: SQL data readed 
        
    Output:
        X: building points Coordinate-x
        Y: building points Coordinate-y
        height:building height     
        properties: building properties
        """

__author__ = "Richie Bao-Chicago.IIT(driverless city project)"
__version__ = "2021.02.17"

ghenv.Component.Name = 'traffic congestion_current'
ghenv.Component.NickName = 'traffic congestion_current'
ghenv.Component.Message = '0.0.1'
ghenv.Component.Category = 'driverless-city'
ghenv.Component.SubCategory = '0 :: SQL'
ghenv.Component.AdditionalHelpFromDocStrings = '5'

import rhinoscriptsyntax as rs
import sqlite3
import ghpythonlib.treehelpers as th
import Rhino

data=th.tree_to_list(sql_data)

segs_start_=[list(eval(d[15])[0])+[0] for d in data]
#print(segs_start_)
segs_start=th.list_to_tree(segs_start_)

segs_end_=[list(eval(d[15])[1])+[0] for d in data]
segs_end=th.list_to_tree(segs_end_)

street=[d[2] for d in data]
direction=[d[3] for d in data]
heading=[d[7] for d in data]

current_speed=[d[13] for d in data]
segs_id=[d[1] for d in data]


'''
Chicago Traffic Tracker - Congestion Estimates by Segments

This dataset contains the current estimated speed for about 1250 segments covering 300 miles of arterial roads. For a more detailed description, go to: http://bit.ly/Q9AZAD.

The Chicago Traffic Tracker estimates traffic congestion on Chicago’s arterial streets (nonfreeway
streets) in real-time by continuously monitoring and analyzing GPS traces received from Chicago Transit Authority (CTA) buses. Two types of congestion estimates are produced every ten minutes: 1) by Traffic Segments and 2) by Traffic Regions or Zones. Congestion estimate by traffic segments gives the observed speed typically for one-half mile of a street in one direction of traffic.

Traffic Segment level congestion is available for about 300 miles of principal arterials. Congestion by Traffic Region gives the average traffic condition for all arterial street segments within a region. A traffic region is comprised of two or three community areas with comparable traffic patterns. 29 regions are created to cover the entire city (except O’Hare airport area).
This dataset contains the current estimated speed for about 1250 segments covering 300 miles of arterial roads.
There is much volatility in traffic segment speed. However, the congestion estimates for the traffic regions remain consistent for relatively longer period. Most volatility in arterial speed comes from the very nature of the arterials themselves. Due to a myriad of factors, including but not limited to frequent
intersections, traffic signals, transit movements, availability of alternative routes, crashes, short length of the segments, etc. speed on individual arterial segments can fluctuate from heavily congested to no congestion and back in a few minutes. The segment speed and traffic region congestion estimates
together may give a better understanding of the actual traffic conditions.
'''
```

**组件-traffic congestion_historical**

```python
"""Read SQL table.
    Inputs:
        sql_data: SQL data readed 
        
    Output:
        X: building points Coordinate-x
        Y: building points Coordinate-y
        height:building height     
        properties: building properties
        """

__author__ = "Richie Bao-Chicago.IIT(driverless city project)"
__version__ = "2021.02.17"

ghenv.Component.Name = 'traffic congestion_historical'
ghenv.Component.NickName = 'traffic congestion_historical'
ghenv.Component.Message = '0.0.1'
ghenv.Component.Category = 'driverless-city'
ghenv.Component.SubCategory = '0 :: SQL'
ghenv.Component.AdditionalHelpFromDocStrings = '5'

import rhinoscriptsyntax as rs
import sqlite3
import ghpythonlib.treehelpers as th
import Rhino

data=th.tree_to_list(sql_data)

speed=[d[3] for d in data]
message_count=[d[2] for d in data]
bus_count=[d[1] for d in data]
segs_id=[d[0] for d in data]
vals_dict={id:[s,m,b]for id,s,m,b in zip(segs_id,speed,message_count,bus_count)}

if segs_id_:
    speed_=[]
    message_count_=[]
    bus_count_=[]
    for idx in segs_id_:
        if idx in segs_id:
            speed_.append(vals_dict[idx][0])
            message_count_.append(vals_dict[idx][1])
            bus_count_.append(vals_dict[idx][2])
        else:
            speed_.append(-1)
            message_count_.append(-1)
            bus_count_.append(-1)
    speed=speed_
    message_count=message_count_
    bus_count=bus_count_
    segs_id=segs_id_
    
    
'''
Chicago Traffic Tracker - Historical Congestion Estimates by Segment - 2011-2018

This dataset contains the historical estimated congestion for 1270 traffic segments, in selected time periods from August 2011 to May 2018. Newer records are in https://data.cityofchicago.org/d/sxs8-h27x. The most recent estimates for each segment are in https://data.cityofchicago.org/d/n4j6-wkkf.

The Chicago Traffic Tracker estimates traffic congestion on Chicago’s arterial streets (non-freeway streets) in real-time by continuously monitoring and analyzing GPS traces received from Chicago Transit Authority (CTA) buses. Two types of congestion estimates are produced every 10 minutes: 1) by Traffic Segments and 2) by Traffic Regions or Zones. Congestion estimates by traffic segments gives observed speed typically for one-half mile of a street in one direction of traffic. Traffic Segment level congestion is available for about 300 miles of principal arterials. Congestion by Traffic Region gives the average traffic condition for all arterial street segments within a region. A traffic region is comprised of two or three community areas with comparable traffic patterns. 29 regions are created to cover the entire city (except O’Hare airport area). There is much volatility in traffic segment speed. However, the congestion estimates for the traffic regions remain consistent for a relatively longer period. Most volatility in arterial speed comes from the very nature of the arterials themselves. Due to a myriad of factors, including but not limited to frequent intersections, traffic signals, transit movements, availability of alternative routes, crashes, short length of the segments, etc. Speed on individual arterial segments can fluctuate from heavily congested to no congestion and back in a few minutes. The segment speed and traffic region congestion estimates together may give a better understanding of the actual traffic conditions. Current estimates of traffic congestion by region are available at http://bit.ly/Vz3rIh.
'''
```

<a href=""><img src="./imgs_pyd/029.jpg" height="auto" width="auto" title="caDesign"></a>

## [pandas](https://pandas.pydata.org/) + [GeoPandas](https://geopandas.org/)

`sklearn.utils.Bunch(**kwargs)` [Bunch](https://scikit-learn.org/stable/modules/generated/sklearn.utils.Bunch.html)


```python
'''提取分析所需数据，并转换为sklearn的bunch存储方式，统一格式，方便读取。'''
def json2bunch(fName):   #传入数据，面向不同的数据存储方式，需要调整函数内读取的代码
    import json
    import numpy as np
    from sklearn.datasets import base
    from sklearn.utils import Bunch 
    '''
    function - 转换百度POI单类数据为sklearn的bunch格式
    
    Paras:
    fName - POI的.json数据格式文件
    
    Return:
    dataBunch - 返回POI的bunch数据格式
    '''
    
    infoDic=[]
    f=open(fName)
    jsonDecodes=json.load(f)
    j=0
    for info in jsonDecodes:
        condiKeys=info['detail_info'].keys()
        if 'price' in condiKeys and'overall_rating' in condiKeys and 'service_rating' in condiKeys and 'facility_rating' in condiKeys and 'hygiene_rating' in condiKeys and 'image_num' in condiKeys and 'comment_num' in condiKeys and 'favorite_num' in condiKeys: #提取的键都有数据时，才提取，否则忽略掉此数据
            if 50<float(info['detail_info']['price'])<1000: #设置价格区间，提取数据
                j+=1
                infoDic.append([info['location']['lat'],info['location']['lng'],info['detail_info']['price'],info['detail_info']['overall_rating'],info['detail_info']['service_rating'],info['detail_info']['facility_rating'],info['detail_info']['hygiene_rating'],info['detail_info']['image_num'],info['detail_info']['comment_num'],info['detail_info']['favorite_num'],info['detail_info']['checkin_num'],info['name']])
            else:pass
        else:pass
    print('.....................................',j)

    data=np.array([(v[0],v[1],v[2],v[3],v[4],v[5],v[6],v[7],v[8],v[9],v[10]) for v in infoDic],dtype='float')  #解释变量(特征)数据部分
    targetInfo=np.array([v[11] for v in infoDic])  #目标变量(类标)部分
    dataBunch=Bunch(DESCR=r'info of poi',data=data,feature_names=['lat','lng','price','overall_rating','service_rating','facility_rating','hygiene_rating','image_num','comment_num','favorite_num','checkin_num'],target=targetInfo,target_names=['price','name'])  #建立sklearn的数据存储格式bunch
    return dataBunch #返回bunch格式的数据
```


```python
hotel_poi_fp=r'./data/poi_1_hotel.json'
hotel_poiBunch=json2bunch(hotel_poi_fp)
```

    ..................................... 818
    


```python
hotel_poiBunch
```




    {'DESCR': 'info of poi',
     'data': array([[ 34.266776, 108.951985, 166.      , ..., 110.      ,   0.      ,
               0.      ],
            [ 34.255235, 108.951857, 406.      , ...,  30.      ,   0.      ,
               0.      ],
            [ 34.265239, 108.944081, 268.      , ...,  47.      ,   0.      ,
               0.      ],
            ...,
            [ 34.273892, 108.973558,  51.5     , ...,   0.      ,   0.      ,
               0.      ],
            [ 34.277038, 108.975278, 380.      , ...,   0.      ,   0.      ,
               0.      ],
            [ 34.276597, 108.972583,  63.      , ...,   2.      ,   0.      ,
               0.      ]]),
     'feature_names': ['lat',
      'lng',
      'price',
      'overall_rating',
      'service_rating',
      'facility_rating',
      'hygiene_rating',
      'image_num',
      'comment_num',
      'favorite_num',
      'checkin_num'],
     'target': array(['锦江之星酒店(钟楼店)', '西安君乐城堡酒店', '荣民国际酒店', '美丽豪酒店', '西京国际饭店', '西安东舍精品酒店',
            '汉庭酒店(西安钟鼓楼广场店)', '建苑大厦', '全季酒店(西安钟鼓楼店)', '西安美莎酒店', '美伦酒店',
            '桔子酒店精选西安南门店', '西安金花豪生国际大酒店', '和颐酒店(南大街店)', '南方酒店西木头市店',
            '如家精选酒店(西安钟楼南大街粉巷店)', 'IU酒店(西安钟鼓楼广场店)', '丽晶酒店', '布丁酒店(钟鼓楼回民街店)',
            '天阅酒店', '如家快捷酒店(西安钟楼店)', '格林豪泰酒店(西安西门店)', '钟楼饭店', '速8酒店(西安钟鼓楼广场店)',
            '汉庭酒店(西安钟鼓楼回民街店)', '西安馨乐庭城中服务公寓', '速8酒店(钟楼北大街店)', '西安钟楼回民街老孙家精品客栈',
            '格林豪泰酒店(西安含光门店)', '西安兴阳酒店', '如家快捷酒店(西安如家钟鼓楼广场店)', '西安馨宜商务酒店小南门店',
            '汉庭酒店(西安含光门明城墙店)', '西安天成商务酒店', '西安联邦中航商务酒店', '西安诚轩酒店',
            '如家快捷酒店(西安钟鼓楼回民街店)', '汉庭酒店(南大街店)', '梦幻西西里宾馆', '静湖泉商务酒店',
            '锦江都城西安钟楼西门酒店', '布丁酒店(钟鼓楼回民街百盛店)', '西安印时尚主题酒店',
            '如家快捷酒店(西安西门外第一中学店)', '西安古城驿家客栈', '如家精选酒店西安钟楼店', '布丁酒店(钟鼓楼西大街店)',
            '西安钟楼王子公寓酒店', '西安永宁驿栈（原永宁宫大酒店）', '7天连锁酒店(西安西大街店)', '秦道商务酒店',
            '海友酒店(西大街店)', '西安钟楼美家公寓', '成功喜豪酒店(甜水井店)', '天都宾馆(西大街店)',
            '西安福缘阁酒店钟楼回民街地铁站店', '海友酒店(南门店)', '7天酒店(西安西北大学丰庆路店)', '西安钟楼木果酒店',
            '文苑大酒店', '西安城市酒店', '布丁酒店(西安明城墙南门店)', '凡间客栈(钟鼓楼店)', '钟楼清怡公寓酒店',
            '金朗庭商务酒店', '7天连锁酒店(西安西门店)', '金易时尚酒店', '通城商务酒店', '中楼抱抱公寓', '都市春天酒店',
            '西安微少爷智慧酒店西门店', '城角快捷酒店', '钟楼皇城客栈', '西安加利利连锁西水快捷酒店', '万景酒店',
            '微少爷酒店（西关正街店）', '西安优悦公寓酒店', '速8酒店(西安西大街桥梓口店)', '湘子少庭酒店',
            '西安多多酒店公寓', '西安钟楼新世界酒店公寓', '西安钟楼公寓酒店', '西安爱雅酒店', '钟航商务宾馆',
            '西安钟楼阳光巴黎公寓酒店', '西安钟楼汉庭公寓酒店', '西安嘉隆酒店钟楼店', '西安博利雅酒店', '西安钟楼锦江公寓酒店',
            '陕北人家酒店公寓', '永宁宫大酒店', '孙家大院客栈', '西安宏府壹号公寓酒店', '西安钟楼星座公寓酒店',
            '西安鑫和泰客栈', '明皇公寓酒店', '富润酒店(丰庆店)', '好心情宾馆', '西安美吖精品公寓酒店',
            '西安豪嘉公寓式酒店', '西安钟楼全意酒店', '西安华晨酒店公寓(钟鼓楼回民街店)', '西安永泰酒店', '西安钟楼福来客栈',
            '博泰商务酒店(南大街店)', '天樾明泰酒店', '陕西文物大厦-商务酒店', '钓鱼岛·酒店', '国铭花园酒店',
            '西安钟楼万象酒店回民街地铁站店', '西安钟楼万家公寓酒店', '西航宾馆', '西安万盛宾馆', '西安雅居连锁酒店（钟楼店）',
            '西安诺尔主题酒店', '西安钟楼驿站公寓酒店', '西安回民街家庭客栈', '真不同公寓酒店', '西安钟楼嘉境公寓酒店回民街店',
            '西安千熙精品公寓酒店(钟鼓楼永宁门地铁站店)', '西安钟楼新时代酒店公寓', '西安晨小溪客栈钟楼回民街店',
            '西安麦麸万顺元客栈', '西安钟楼回坊酒店', '宣源酒店', '西安钟楼伊家酒店', '华天快捷宾馆', '百合商务宾馆',
            '西安钟楼天意公寓客栈钟楼回民街地铁站店', '西安丝绸之路商务酒店公寓', '西安迈亨酒店公寓（钟鼓楼店）', '福荣商务宾馆',
            '西安途乐公寓酒店钟楼地铁站店', '西安薇蜜艺术主题酒店', '西安假日空间酒店公寓', '天乐宾馆',
            '西安钟鼓楼壹号公寓酒店', '相居庭公寓酒店（西安北大街宏府嘉汇店）', '西安英佳酒店', '西安西京客栈',
            '西安花儿客栈钟鼓楼地铁站回民美食街店', '西安钟楼亨泰公寓酒店', '西安平兴钟楼公寓酒店', '西安大唐公寓酒店',
            '朱雀宾馆', '西安莲湖钟楼居家商务酒店', '西秦旅馆(环城西路南段)', '多客宾馆(庙后街店)',
            '西安钟楼华都之星酒店钟楼回民街店', '西安邻家公寓式酒店', '西安钟楼米花公寓酒店', '西安都乐酒店公寓',
            '西安宏府嘉会广场(日租公寓)', '西安钟鼓楼优品酒店', '西安康泰公寓酒店', '西安文苑臻品主题酒店', '鸿福宾馆',
            '东府宾馆', '西安唐韵公寓酒店', '去呼呼酒店(西安钟楼华美公寓酒店)', '西安钟楼四季公寓酒店',
            '西安钟楼芒果公寓酒店', '西安逗号酒店公寓', '西安雅之家酒店公寓北大街钟楼店', '利朗公寓酒店', '望京家居酒店',
            '西安美溪公寓酒店钟鼓楼回民街店', '长安驿站酒店公寓(北大街店)', '金昌盛宾馆', '西安黄河谣公寓酒店',
            '西安钟楼四季公寓酒店', '西安鸿鑫快捷酒店', '西府商务旅馆', '邦客·得福旅馆', '金城宾馆(西安琉璃街店)',
            '西安东阁商务宾馆', '西安馨如家酒店公寓', '西安青果公寓酒店', '钟楼君悦主题酒店', '西安好家短租公寓',
            '星程蕾德曼酒店(朱雀店)', '西安爱巢宾馆', '西安众悦酒店', '汇友宾馆', '荣鑫商务旅馆', '优客公寓酒店',
            '乌托邦公寓酒店', '西安乐途酒店公寓', '西安昊林商务公寓', '德桂园大酒店', '西安爱乐公寓酒店', '天瑞宾馆',
            '鑫宇公寓酒店（西安钟楼店）', '西安雅居宾馆', '西安钟楼锦航酒店', '家馨宾馆', '西安路客公寓酒店',
            '西安怡都宾馆', '丽庭酒店', '西安和谐招待所', '西安天信酒店钟楼回民街店', '西安钟楼阳光巴黎公寓酒店（回民街店）',
            '宜家宾馆', '天铭旅馆', '西安钟楼途悦公寓酒店', '红方宾馆西门店', '丽豪酒店',
            '西安七天假日酒店钟楼回民街地铁站店', '西安钟鼓楼公寓酒店', '西安宾至公寓酒店（钟鼓楼回民街店）', '启阳旅馆',
            '鼓楼酒店', '西安东光商务宾馆', '佳园宾馆(北马道巷店)', '亚丁宾馆', '聚缘宾馆(西羊市店)',
            '五颜六色的青年客栈(钟楼店)', '西安太阳雨公寓酒店', '西安钟楼睿途公寓酒店', '西安嘉隆皇城酒店',
            '西安钟楼印象公寓酒店', '西安九九公寓酒店钟楼店', '西安坐标酒店(钟楼店)', '西安人在旅途公寓酒店', '金赞酒店',
            '西安正之道青年旅舍', '长虹宾馆', '西安凯丽公寓式酒店', '西安钟楼漫时光酒店公寓钟鼓楼回民街店', '志远客栈',
            '古城公寓酒店', '忆家酒店公寓（西安钟鼓楼回民街店）（原忆家公寓钟鼓楼店（西安莲湖店））', '钟楼雅度酒店公寓',
            '西安钟楼旅途驿站公寓酒店（回民街店）', '卫安招待所', '西安7尚8夏假日酒店', '西安鑫德公寓酒店钟楼回民街店',
            '西安晴天公寓酒店', '钟楼公寓-1号', '西安美景酒店', '西安竹青堂会所', '宏达宾馆', '星月小宾馆',
            '西安子腾短租公寓', '明悦宾馆', '神府弘商务酒店', '钟楼金花酒店', '西安城墙里公寓酒店钟鼓楼回民街店',
            '花园宾馆(西安莲湖店)', '如意宾馆', '西安温馨如家宾馆', '西安文理学院-宾馆', '和信宾馆',
            '龙腾宾馆(西安桥梓口店)', '陕南旅馆', '古都旅馆', '西安钟楼一路发商务酒店', '雅豪酒店', '西安老长安公寓',
            '合顺宾馆(西门店)', '安定宾馆（西安西门店）', '车巷旅馆', '西安西雅公寓酒店', '西安他她公寓酒店日租公寓',
            '西安清悦公寓连锁酒店', '西安顺风旅馆', '西安爱酷酒店公寓', '古都文化大酒店',
            '陕西省止园饭店（陕西省人民政府招待所）', '华美达酒店(华美达兆瑞酒店)', '飞鹿商务酒店', '天朗时代大酒店',
            '君诚国际酒店', '西安中心戴斯酒店', '警苑饭店', '华山国际酒店', '西安世纪山水酒店', '西安紫金山大酒店',
            '恒佳好世界大酒店', '西安昊德商务酒店', '西安亚朵酒店钟楼回民街店', '西安途悦酒店', '锦苑富润大饭店',
            '7天连锁酒店(西安钟鼓楼北大街地铁站店)', '景玉商旅酒店', '坤逸酒店', '速8酒店(西安洒金桥地铁站店)',
            '含光戴斯酒店', '汉庭酒店(西安北大街十字酒店)', '天悦精选酒店', '假日国际公寓式酒店(钟楼北大街回民街地铁站店)',
            '澳都酒店', '西安金桥酒店', '如家快捷酒店(西安北关自强西路店)', '凯迪斯曼酒店', '芒果酒店(玉祥门店)',
            '7天优品酒店(西安北门安远门地铁站店)', '7天连锁酒店(西安北大街地铁站回民街店)', '西安秦逸轩酒店',
            '巴蜀商务酒店(西安店)', '金海大酒店', '西安唐圣阁快捷酒店', '如家快捷酒店(西安钟楼北大街地铁站店)',
            '美豪酒店(西安莲湖店)', '莫泰酒店(西安青年路店)', '速8酒店(西安北门店)', '富润酒店(北门店)',
            '古都印象左右客酒店', '布丁酒店(西安莲湖店)', '布丁酒店(回民街美食城店)', '西安故乡朗庭商务酒店',
            '如家快捷酒店(西安莲湖路回坊美食街店)', '嘉隆大酒店', '伊家家居酒店', '西安马家小院客栈', '滨江商务酒店',
            '西安商务宾馆(莲湖佳苑东南)', '丽居商务酒店', '都市商务酒店北郊店', '西安钟楼英仕曼公寓酒店', '枫林快捷酒店',
            '布丁酒店(西安北关安远门地铁口店)', '镐安温泉酒店', '海友酒店(西安西门外店)', '西安速客快捷酒店（99自强西路店）',
            '速8酒店(西安玉祥门店)', '宁西宾馆', '玉祥门西站街如家联盟吉彩酒店', '西安悠客主题酒店', '军安王朝大酒店',
            '西安琉夏印象驿馆', '西安逸园精品酒店钟楼回民街店', '巴蜀商务酒店工农路分店', '速8酒店(西安环城北路店店)',
            '西安众悦酒店', '骏怡连锁酒店', '陕西工运宾馆', '斯利商务酒店(北关店)', '西安宜家公寓酒店', '美程连锁酒店',
            '世家快捷酒店', '星座宾馆', '家园宾馆', '君悦宾馆', '西安叮当酒店公寓', '西安紫藤公寓酒店',
            '西安柠檬空间酒店公寓', '蓝箭宾馆', '西安木舍House钟楼北门店', '舟济宾馆', '西安好捷酒店（莲湖路店）)',
            '沁园宾馆(西安北大街店)', '西安康城宾馆', '西安爱人码头短租公寓家庭式酒店', '元宏宾馆', '金帝商务宾馆',
            '天赐商务宾馆', '西安市一家人旅馆', '西安蜗居公寓酒店', '西安汇宾招待所', '西安钟楼短租公寓酒店',
            '西安缘梦宾馆', '顺家快捷酒店', '西安军瑞假日公寓酒店', '汉庭怡莱酒店(北门明城墙店)', '四海宾馆(西安习武园)',
            '凯宾酒店', '西安北关正街家庭旅馆', '西安鑫悦家庭宾馆', '祥云宾馆', '华泰宾馆(工农路店)',
            '鹏程宾馆(丰禾路店)', '好家园招待所(许士庙街)', '金泰宾馆(西安莲湖店)', '格陵兰商务宾馆',
            '金豪宾馆(西安青年路店)', '吉利宾馆', '天坛客房部', '西安莲湖区好客宾馆', '西安天宝安远公寓',
            '多客宾馆(一中北路)', '君悦宾馆', '公园宾馆', '一城一家青年旅舍', '舒可兰旅馆', '梧桐假日公寓酒店',
            '西安纸坊客栈', '西安古城快捷酒店', '西安头羊客栈钟楼店', '西安莲湖区龙凤宾馆', '西安君苑宾馆', '如家宾馆',
            '多客宾馆', '凯一顺宾馆', '营地商务酒店', '西安美联公寓酒店', '西安新城区都市宾馆', '天星宾馆', '恒泰宾馆',
            '五一旅馆', '阳光招待所', '西安ZT公寓酒店', 'QQ宾馆', '西安瑞庭宾馆(纸坊村)', '西安钟楼秦韵公寓酒店',
            '西安幸福宾馆', '西安秦州宾馆', '西安99短租公寓', '高速神州酒店', '皇城豪门酒店', '维也纳酒店(西安钟楼店)',
            '秦唐一号酒店', '西安皇城海航商务酒店', '政协大酒店', '富海明都酒店', '艾斯汀酒店', '富森城市酒店',
            '西安楠林国际酒店', '西安阿房宫维景国际大酒店', '西安华榕国际酒店', '奥罗国际大酒店',
            '锦江之星酒店(西安建国门店)', '新西北酒店', '荣江国际酒店', '亚朵酒店西安南门店', '华辰酒店',
            '兴正元国际酒店', '7天连锁酒店(西安钟楼店)', '西安加利利连锁酒店碑林博物馆店', '西安成功国际酒店',
            '布丁酒店(钟楼北店)', '如家快捷酒店(西安钟楼东大街万达广场店)', '陕西南方酒店（西安案板街店）',
            '左艺术时尚精品酒店(西安钟楼店)', '西安富润酒店(南门店)', 'Zhotels智尚酒店西安钟楼骡马市店', '柠檬漫莎酒店',
            '百时快捷酒店(解放路店)', '华威国际酒店', '美豪酒店(东大街店)', '甲字商务酒店', '汉庭酒店(西安钟楼东大街店)',
            '西安卡森酒店（建国路店）', '西安美伦酒店欣美店', '7天连锁酒店(西安东大街建国路口店)', '西安文德会员酒店',
            '爆米花影院酒店', '汉庭酒店(西安钟楼骡马市店)', '人人居酒店菊花园店', '西安美桐年华酒店', '和平门酒店',
            '如家快捷酒店(西安钟楼骡马市步行街店)', '如家快捷酒店(西安南门碑林博物馆店)', '锦江之星酒店(西安骡马市店)',
            '申鹏酒店', '如家快捷酒店(西安东门店)', '7天连锁酒店(西安大差市店)', '梦飞酒店',
            '7天连锁酒店(西安东大街店)', '唐苑宾馆', '嘉禾商务酒店', '申鹏国际商务酒店', '海友酒店(西安建国门店)',
            '西安乐巢酒店', '城舍尚品公寓酒店', '阳光商务快捷酒店', '尚客优快捷酒店(菊花园店)',
            '润佳连锁酒店(西安钟楼新城广场店)', '巢悦精品酒店', '西安加利利连锁酒店钟楼东大街万达广场店',
            '汉庭酒店(西安东门外店)', '凯撒宫酒店', '关中客栈', '如家快捷酒店(西安钟楼菊花园店)', '悦朗酒店',
            '汉庭酒店(西安东门店)', '顺延酒店', '如家快捷酒店(西安东大街百盛购物广场店)', '西安加利利连锁酒店东门外古城墙店',
            '佰连商务酒店', '德邻酒店', '新疆饭店', '易佰酒店西安明城墙东门外店', '速8酒店(万达新天地店)',
            '玺韵假日大酒店', '西安大华饭店钟楼东大街骡马市店', '西安青年会楷都酒店', '中安之家西安梅园宾馆(和平门店)',
            '亿通快捷酒店', '唯梦概念酒店', '西安蓝萨国际假日酒店', '国宾酒店', '西安嘉斯顿酒店(速8酒店东羊市店)',
            '西安聚龙商务宾馆', '芍药居旅馆', '文商宾馆', '颐悦煤田酒店', '西安恋巢酒店', '雅客酒店',
            '西安新天地优选精品酒店', '芙蓉商务宾馆(西华门店)', '小憩驿站酒店大差市店',
            '西安米诺尚品公寓酒店钟楼南门地铁站精选店', '品居公寓酒店', '西安一夕iiihouse客栈钟楼店', '超越酒店',
            '花美时民宿酒店(西安南门广场店)', '艾雷科斯酒店（西安东大街店）', '西安添禧度假酒店公寓',
            '西安如意快捷酒店(柏树林分店)', '西安艾斯汀国际公寓酒店', '仁和快捷宾馆', '久鼎商务酒店', '仁和居酒店',
            '西安博雅居酒店', '润佳连锁酒店(新合作钟楼回民街新城广场店)', '西安城池精品公寓酒店', '星期天快捷酒店',
            '西安云町假日主题酒店', '煜life酒店', '西安天泽商务宾馆', '德正酒店', '西安多客宾馆', '鑫源快捷宾馆',
            '蓝月亮时尚宾馆', '玉华宫宾馆', '西安亚米公寓酒店', '西安马里奥精品酒店公寓', '西安新昌和旅馆',
            '西安南门蒙娜丽莎精品酒店公寓', '西安诚泰主题酒店', '中天商务宾馆', '西安唐韵公寓酒店南门店',
            '唐都温泉宾馆(太乙路店)', '蔷薇宾馆(东一路)', '金鑫宾馆', '大唐小舍精品公寓(钟鼓楼南门店)',
            '艾思丽精品睡眠酒店（西安东门店）', '西安南门口的家住宿', '西安林晖宾馆南新街店', '舜江国际公寓', '西安六福客栈',
            '西安V美精品公寓（钟鼓楼店）', '腾达宾馆', '万达宾馆', '邮政酒店', '香山宾馆', '汉庭酒店(西安钟楼东大街店)',
            '七色客栈', '西安雅轩快捷酒店', '白云宾馆(建国路店)', '海丰商务宾馆', '西安碑林区雅客宾馆',
            '西安金帝宾馆(东木头市店)', '青藤快捷旅馆', '好客商务宾馆', '西安金帝宾馆东大街店',
            '西安五洲环球精品度假公寓钟楼南门古城墙店', '宜家旅馆', '西安亚高酒店公寓', '西安华林宾馆',
            '西安关中书院-专家公寓', '西安熙园酒店', '西安榜样公寓碑林区店', '西安东大街正信宾馆', '西安嘉华宾馆（尚勤路店）',
            '万融宾馆', '汇源宾馆', '西安喜进商务旅馆', '一家人旅馆（西安安居巷店）', '望园宾馆尚勤店',
            '西安友谊宾馆开通巷店', '西安怡万宾馆', '西安Xbed互联网酒店国际花园店', '西安蓝月亮快捷旅馆',
            '西安阳光家庭旅馆', '西安开通宾馆', '好心情快捷宾馆', '伯朗酒店', '西安春天里公寓式酒店', '万佳快捷酒店',
            '金地酒店', '西安泰元精品酒店', '富莱酒店', '佳润快捷酒店', '西安一夜长安公寓酒店', '瑞祥宾馆', '天鸿宾馆',
            '西安理想公寓酒店', '西安觅你主题酒店钟鼓楼东大街万达广场店', '西安芒果公寓', '滨江花园酒店', '西安鑫园旅馆',
            '西安第七日酒店公寓钟楼店', '西安大差舒特公寓酒店', '嘉丽酒店', '祥和宾馆', '西安凯米kimi公寓酒店',
            '西安南门零压力精品酒店式公寓', '西安西北农林科技大学西安办事处招待所', '西安舜都宾馆', '龙泉宾馆(西一路)',
            '祥和旅馆', '西安亚航公寓', '西安N家度假公寓酒店', '西安学院商务宾馆（关中书院-专家公寓）', '西安九都旅馆',
            '鸿运客栈', '西安顺馨商务宾馆', '西安商务旅馆', '西安七色光商务酒店', '西丽酒店(红十字会巷)',
            '西安彩虹半岛酒店式家庭公寓', '西安劲邦假日酒店', '西安青檬酒店', '斑斓酒店(西安南门店)',
            '西安艺术主题酒店东新街店', '西安快乐驿站酒店公寓大差市店', '糖果逸栈快捷酒店', '润丰商务旅馆', '仁和客房部',
            '安西商务旅馆', '开心公寓', '西安马克时尚公寓酒店', '西安西秦印象精品公寓酒店（钟楼南门店）', '新帅宾馆',
            '兴庆客栈', '东升商务宾馆', '西安永保金悦宾馆', '千君旅馆', '西安伊诺依酒店', '西安麗圆宾馆南郭门店',
            '西安绿叶公寓酒店', '西安人民广场索菲特大酒店', '西安万达希尔顿酒店', '西安阳光国际大酒店',
            '王子星月酒店(西五路店)', '西安途家斯维登度假公寓(万景少公馆)', '惠源锦江国际酒店', '全季酒店(解放路万达店)',
            '汉庭酒店(火车站店)', '维也纳酒店(西安火车站店)', '汉庭酒店', '锦江之星酒店(西安钟楼北大街店)',
            '西安尚德大厦', '7天连锁酒店(西安火车站东广场店)', '速8酒店(西安火车站店)', '汉庭酒店(西安钟楼北大街店)',
            '解放宾馆', '锦江之星酒店', '美原国际酒店', '陕西广电宾馆', '7天连锁酒店(西安火车站店)',
            '7天连锁酒店(火车站机场大巴站店)', '西安民幸精品酒店', '西安加利利连锁酒店解放路店',
            '格林豪泰酒店(西安火车站尚勤门店)', '7天连锁酒店(西安民乐园万达广场店)', '西安星程酒店火车站店',
            '都市118连锁酒店(西安火车站店)', '汉庭酒店(西安解放路万达广场二店)', '布丁酒店(西安火车站东广场店)',
            '全季酒店(西安北大街西五路店)', '布丁酒店(西安火车站西广场店)', '金融宾馆', '锦江之星酒店(西安北门地铁站店)',
            '西安豪华美居(人民大厦)', '如家快捷酒店(西安钟楼北店)', '汉庭酒店(西安火车站南店)',
            '如家快捷酒店(西安火车站店)', '如家快捷酒店(西安北门店)', '汉庭酒店(西安钟楼店)',
            '如家快捷酒店(西安康复路商城环城东路店)', '如家快捷酒店(西安北门火车站店)', '铁路饭店',
            '速8酒店(西安钟楼新民街店)', '金雨酒店', '莫泰酒店(西安火车站店)', '速8酒店(西安火车站东广场尚勤门店)',
            '西安皇城168快捷酒店（西华门店）', '派酒店西安明城墙火车站店', '感受主题酒店', '阳光宾馆',
            '布丁酒店(西安钟鼓楼北新街店)', '西安爱巢精品主题公寓（火车站店）', '西安天顺大酒店（火车站店）',
            '富驿时尚酒店(西安火车站店)', '满天星酒店', '海源商务酒店', '派酒店(火车站五路口地铁站店)', '西源宾馆',
            '西安秦好快捷酒店', '西安99优选酒店火车站店', '盾辉宾馆', '相居庭公寓酒店', '润佳快捷酒店(北大街店)',
            '质行假日酒店', '东欣商务酒店', '福厚商务快捷酒店', '秦骊宾馆', '陕西省供销合作社招待所商务酒店',
            '车之友快捷酒店', '澜庭快捷酒店', '鑫源大厦', '西安美苏公寓酒店', '8号客栈(8号主题酒店)', '东平宾馆',
            '西安时光小筑客栈（火车站万达店）', '西安好家商务酒店火车站店', '绿岛宾馆', '海友酒店(西安火车站店)',
            '西安万达舒特公寓酒店', '邮政快捷酒店', '西安优家公寓', '蜀寓连锁商务酒店(西安西五路店)',
            '西安柒天优品酒店火车站广场店', '金顺宾馆', '西安众森连锁酒店火车站店', '西安华庭宾馆火车站店',
            '西安福康宾馆火车站店', '西安和谐宾馆尚勤路店', '西安衮雪酒店', '速8酒店(西安民乐园万达店)', '尚德宾馆',
            '格林豪泰酒店西安新城区火车站五路口地铁站店', '北门旅馆', '伊家公寓式家居酒店（西安民乐园店）', '雅悦宾馆',
            '城市之星快捷酒店', '西安亚美公寓酒店（民乐园万达广场店）', '民政宾馆', '万朵商务酒店', '文森快捷酒店',
            '西安乐巢快捷酒店', '红都宾馆,', '西安顺康宾馆', '西安南方酒店（东八路店）', '西安永联酒店', '西安小西安客栈',
            '西安白水会馆', '西安中至信商务酒店火车站店', '西安君怡旅馆', '西安星城宾馆', '恒源商务酒店', '喜客来宾馆',
            '西安斯利旅馆', '城墙快捷酒店', '海云快捷酒店', '客好居快捷宾馆', '99宾馆西五路店', '丽都招待所',
            '鸿吉宾馆', '顺家快捷酒店(火车站店)', '西安品尚酒店', '行摄之家青年宾馆', '西安惠家酒店公寓火车站万达广场店',
            '悦客来(东波店)', '西安启航商务宾馆', '渭河宾馆', '环桥宾馆', '天祥旅馆', '西安凯拓宾馆', '西安星晨旅馆',
            '新城饭店', '西安云雅优客快捷酒店', '悦客来泽苑酒店', '凯新宾馆', '新青年客栈',
            '西安印象公寓酒店火车站万达广场店', '西安聚星庄宾馆', '秦居宾馆', '西安蓝翔宾馆', '如约酒店西安', '快捷宾馆',
            '家祥宾馆', '西安星期八酒店公寓', 'Xbed互联网酒店(西安皇城壹号公寓店)', '万千假日主题酒店', '富友宾馆',
            '如家宾馆', '99旅馆(西安大明宫遗址公园店)', '迎宾招待所(环城北路)', '金鼎商务酒店', '大元宾馆',
            '天虹宾馆', 'LOVE酒店', '北门宾馆', '天润招待所', '星云商务宾馆', '新航宾馆', '宝康商务宾馆',
            'e家旅馆', '西安泽苑商务酒店', '友好宾馆', '西安市新城区大地招待所', '索菲特西安人民大厦东楼',
            '西安市万达广场日租宾馆', '新城饭店-客房中心', '西安火车站商务宾馆', '幽雅旅馆', '家和宾馆',
            '西安龙泉度假公寓火车站店', '房产宾馆', '西安星光商务宾馆', '西安北海酒店', '西安黄金海岸公寓酒店',
            '恒佳快捷酒店', '西安皇城诺雅酒店', '裕丰旅馆（西安火车站店）', '陕西友谊宾馆', '商务宾馆', '紫悦公寓酒店',
            '古城宾馆(西七路)', '美天假日酒店', '卡尔斯酒店', '晋鑫宾馆(火车站店)', '织带厂招待所', '四海宾馆'],
           dtype='<U35'),
     'target_names': ['price', 'name']}



### Series
#### 建立Series

* 由列表创建，索引默认为整数型


```python
import pandas as pd
from pandas import Series,DataFrame

cols=['lat','lng','price','overall_rating','service_rating','facility_rating','hygiene_rating','image_num','comment_num','favorite_num','checkin_num']
obj=Series(cols)
print(obj)
```

    0                 lat
    1                 lng
    2               price
    3      overall_rating
    4      service_rating
    5     facility_rating
    6      hygiene_rating
    7           image_num
    8         comment_num
    9        favorite_num
    10        checkin_num
    dtype: object
    

* 由字典建立，key为索引，value为值


```python
dic=dict(zip(cols,hotel_poiBunch.data[40].tolist()))
print(dic)
obj4=Series(dic)
print("_"*50)
print(obj4)
```

    {'lat': 34.264806, 'lng': 108.926551, 'price': 100.0, 'overall_rating': 4.6, 'service_rating': 4.6, 'facility_rating': 4.5, 'hygiene_rating': 4.6, 'image_num': 30.0, 'comment_num': 21.0, 'favorite_num': 0.0, 'checkin_num': 0.0}
    __________________________________________________
    lat                 34.264806
    lng                108.926551
    price              100.000000
    overall_rating       4.600000
    service_rating       4.600000
    facility_rating      4.500000
    hygiene_rating       4.600000
    image_num           30.000000
    comment_num         21.000000
    favorite_num         0.000000
    checkin_num          0.000000
    dtype: float64
    

* 由字典建立，并给定index参数，按给定参数列表提取，如果没有该索引则值为NaN(not a number)


```python
obj5=Series(dic,index=['lat','lng','price','service'])
obj5
```




    lat         34.264806
    lng        108.926551
    price      100.000000
    service           NaN
    dtype: float64



#### 由1组数据<numpy数据类型>，及与之相关的数据标签<索引>组成。索引默认为整数型索引


```python
print("obj.values->{}".format(obj.values))
print("obj.index->{}".format(obj.index))
```

    obj.values->['lat' 'lng' 'price' 'overall_rating' 'service_rating' 'facility_rating'
     'hygiene_rating' 'image_num' 'comment_num' 'favorite_num' 'checkin_num']
    obj.index->RangeIndex(start=0, stop=11, step=1)
    


```python
# 给定标签
obj2=Series(obj4.values,index=obj)
print(obj2)
```

    lat                 34.264806
    lng                108.926551
    price              100.000000
    overall_rating       4.600000
    service_rating       4.600000
    facility_rating      4.500000
    hygiene_rating       4.600000
    image_num           30.000000
    comment_num         21.000000
    favorite_num         0.000000
    checkin_num          0.000000
    dtype: float64
    


```python
# 就地修改索引值
cols=['latitude','longitude','price','overall_rating','service_rating','facility_rating','hygiene_rating','image_num','comment_num','favorite_num','checkin_num']
obj2.index=cols
print("obj2.index->{}".format(obj2.index))
```

    obj2.index->Index(['latitude', 'longitude', 'price', 'overall_rating', 'service_rating',
           'facility_rating', 'hygiene_rating', 'image_num', 'comment_num',
           'favorite_num', 'checkin_num'],
          dtype='object')
    

#### 由索引提取Series的值


```python
print("obj2[['latitude','longitude']]->{}".format(obj2[['latitude','longitude']]))
```

    obj2[['latitude','longitude']]->latitude      34.264806
    longitude    108.926551
    dtype: float64
    

#### 运算


```python
obj3=Series(hotel_poiBunch.data[...,2],hotel_poiBunch.target)
print(obj3)
```

    锦江之星酒店(钟楼店)    166.0
    西安君乐城堡酒店       406.0
    荣民国际酒店         268.0
    美丽豪酒店          200.0
    西京国际饭店         329.0
                   ...  
    美天假日酒店          93.0
    卡尔斯酒店          168.0
    晋鑫宾馆(火车站店)      51.5
    织带厂招待所         380.0
    四海宾馆            63.0
    Length: 818, dtype: float64
    


```python
print("obj3[obj3>500]->{}".format(obj3[obj3>500]))
print("_"*50)
print("obj3*0.7->{}".format(obj3*0.7))
print("_"*50)
import numpy as np
print("np.ceil(obj3*0.7)->{}".format(np.ceil(obj3*0.7)))
print("_"*50)
print("'lat' in obj4->{}".format('lat' in obj4))
print("_"*50)
print("pd.isnull(obj5)->{}".format(pd.isnull(obj5)))
print("_"*50)
print("pd.notnull(obj5)->{}".format(pd.notnull(obj5)))
print("_"*50)
print("obj5.isnull()->{}".format(obj5.isnull()))
```

    obj3[obj3>500]->西安东舍精品酒店        526.0
    西安泰元精品酒店        630.0
    西安人民广场索菲特大酒店    628.0
    西安万达希尔顿酒店       666.0
    dtype: float64
    __________________________________________________
    obj3*0.7->锦江之星酒店(钟楼店)    116.20
    西安君乐城堡酒店       284.20
    荣民国际酒店         187.60
    美丽豪酒店          140.00
    西京国际饭店         230.30
                    ...  
    美天假日酒店          65.10
    卡尔斯酒店          117.60
    晋鑫宾馆(火车站店)      36.05
    织带厂招待所         266.00
    四海宾馆            44.10
    Length: 818, dtype: float64
    __________________________________________________
    np.ceil(obj3*0.7)->锦江之星酒店(钟楼店)    117.0
    西安君乐城堡酒店       285.0
    荣民国际酒店         188.0
    美丽豪酒店          140.0
    西京国际饭店         231.0
                   ...  
    美天假日酒店          66.0
    卡尔斯酒店          118.0
    晋鑫宾馆(火车站店)      37.0
    织带厂招待所         266.0
    四海宾馆            45.0
    Length: 818, dtype: float64
    __________________________________________________
    'lat' in obj4->True
    __________________________________________________
    pd.isnull(obj5)->lat        False
    lng        False
    price      False
    service     True
    dtype: bool
    __________________________________________________
    pd.notnull(obj5)->lat         True
    lng         True
    price       True
    service    False
    dtype: bool
    __________________________________________________
    obj5.isnull()->lat        False
    lng        False
    price      False
    service     True
    dtype: bool
    

#### 运算中，自动对其不同索引的数据


```python
print("obj5->{}".format(obj5))
print("_"*50)
print("obj4->{}".format(obj4))
print("_"*50)
print("obj4+obj5->{}".format(obj4+obj5))
```

    obj5->lat         34.264806
    lng        108.926551
    price      100.000000
    service           NaN
    dtype: float64
    __________________________________________________
    obj4->lat                 34.264806
    lng                108.926551
    price              100.000000
    overall_rating       4.600000
    service_rating       4.600000
    facility_rating      4.500000
    hygiene_rating       4.600000
    image_num           30.000000
    comment_num         21.000000
    favorite_num         0.000000
    checkin_num          0.000000
    dtype: float64
    __________________________________________________
    obj4+obj5->checkin_num               NaN
    comment_num               NaN
    facility_rating           NaN
    favorite_num              NaN
    hygiene_rating            NaN
    image_num                 NaN
    lat                 68.529612
    lng                217.853102
    overall_rating            NaN
    price              200.000000
    service                   NaN
    service_rating            NaN
    dtype: float64
    

#### Series对象本身及其索引都有一个name属性


```python
obj5.name='poi数据'
obj5.index.name='poi数据索引'
print(obj5)
```

    poi数据索引
    lat         34.264806
    lng        108.926551
    price      100.000000
    service           NaN
    Name: poi数据, dtype: float64
    

### DataFrame
#### DF的建立
表格型数据结构，每列可以是不同的值类型，既有行索引，又有列索引


```python
df1=DataFrame(hotel_poiBunch.data[0:10,:3],columns=cols[:3])
print(df1)
```

        latitude   longitude  price
    0  34.266776  108.951985  166.0
    1  34.255235  108.951857  406.0
    2  34.265239  108.944081  268.0
    3  34.266445  108.947120  200.0
    4  34.265829  108.943414  329.0
    5  34.261837  108.948272  526.0
    6  34.263523  108.949807  187.0
    7  34.261119  108.944958  100.0
    8  34.264448  108.950193  280.0
    9  34.255650  108.950651  192.0
    

* 由字典建立，key为列索引


```python
df2=DataFrame(dict(zip(cols[:3],hotel_poiBunch.data[0:10,:3].T.tolist())))
print(df2)
```

        latitude   longitude  price
    0  34.266776  108.951985  166.0
    1  34.255235  108.951857  406.0
    2  34.265239  108.944081  268.0
    3  34.266445  108.947120  200.0
    4  34.265829  108.943414  329.0
    5  34.261837  108.948272  526.0
    6  34.263523  108.949807  187.0
    7  34.261119  108.944958  100.0
    8  34.264448  108.950193  280.0
    9  34.255650  108.950651  192.0
    

\- 由字典建立，并给定index参数，按给定参数列表提取，如果没有该索引则值为NaN(not a number)


```python
df3=DataFrame(dict(zip(cols[:3],hotel_poiBunch.data[0:10,:3].T.tolist())),columns=['latitude','longitude','price'])
print(df3)
```

        latitude   longitude  price
    0  34.266776  108.951985  166.0
    1  34.255235  108.951857  406.0
    2  34.265239  108.944081  268.0
    3  34.266445  108.947120  200.0
    4  34.265829  108.943414  329.0
    5  34.261837  108.948272  526.0
    6  34.263523  108.949807  187.0
    7  34.261119  108.944958  100.0
    8  34.264448  108.950193  280.0
    9  34.255650  108.950651  192.0
    

\_ 同时给定columns和index参数


```python
df3=DataFrame(dict(zip(cols[:3],hotel_poiBunch.data[0:10,:3].T.tolist())),columns=['latitude','longitude','price'],index=hotel_poiBunch.target[:10])
print(df3)
```

                     latitude   longitude  price
    锦江之星酒店(钟楼店)     34.266776  108.951985  166.0
    西安君乐城堡酒店        34.255235  108.951857  406.0
    荣民国际酒店          34.265239  108.944081  268.0
    美丽豪酒店           34.266445  108.947120  200.0
    西京国际饭店          34.265829  108.943414  329.0
    西安东舍精品酒店        34.261837  108.948272  526.0
    汉庭酒店(西安钟鼓楼广场店)  34.263523  108.949807  187.0
    建苑大厦            34.261119  108.944958  100.0
    全季酒店(西安钟鼓楼店)    34.264448  108.950193  280.0
    西安美莎酒店          34.255650  108.950651  192.0
    

\_ 使用嵌套字典建立df


```python
df5=DataFrame({'荣民国际酒店':{'lat':34.266776,'lng':108.951985,'price':166.0},'西安君乐城堡酒店':{'lat':34.255235,'lng':108.951857,'price':406.0}})
print(df5)
```

               荣民国际酒店    西安君乐城堡酒店
    lat     34.266776   34.255235
    lng    108.951985  108.951857
    price  166.000000  406.000000
    

#### 基本操作
由索引提取DF值

* 列索引


```python
print("df3['latitude']->{}".format(df3['latitude']))
print("_"*50)
print("df3.latitude->{}".format(df3.latitude))
```

    df3['latitude']->锦江之星酒店(钟楼店)       34.266776
    西安君乐城堡酒店          34.255235
    荣民国际酒店            34.265239
    美丽豪酒店             34.266445
    西京国际饭店            34.265829
    西安东舍精品酒店          34.261837
    汉庭酒店(西安钟鼓楼广场店)    34.263523
    建苑大厦              34.261119
    全季酒店(西安钟鼓楼店)      34.264448
    西安美莎酒店            34.255650
    Name: latitude, dtype: float64
    __________________________________________________
    df3.latitude->锦江之星酒店(钟楼店)       34.266776
    西安君乐城堡酒店          34.255235
    荣民国际酒店            34.265239
    美丽豪酒店             34.266445
    西京国际饭店            34.265829
    西安东舍精品酒店          34.261837
    汉庭酒店(西安钟鼓楼广场店)    34.263523
    建苑大厦              34.261119
    全季酒店(西安钟鼓楼店)      34.264448
    西安美莎酒店            34.255650
    Name: latitude, dtype: float64
    

* 行索引


```python
print("df3.loc['全季酒店(西安钟鼓楼店)']->{}".format(df3.loc['全季酒店(西安钟鼓楼店)']))
```

    df3.loc['全季酒店(西安钟鼓楼店)']->latitude     34.2644
    longitude     108.95
     price           NaN
    Name: 全季酒店(西安钟鼓楼店), dtype: object
    

#### 赋值


```python
df3['price']=hotel_poiBunch.data[2,:10]
print(df3)
```

        latitude   longitude       price
    0  34.266776  108.951985   34.265239
    1  34.255235  108.951857  108.944081
    2  34.265239  108.944081  268.000000
    3  34.266445  108.947120    4.400000
    4  34.265829  108.943414    4.600000
    5  34.261837  108.948272    4.400000
    6  34.263523  108.949807    4.600000
    7  34.261119  108.944958   30.000000
    8  34.264448  108.950193   47.000000
    9  34.255650  108.950651    0.000000
    

* 指定行索引给定值


```python
val=Series(hotel_poiBunch.data[:5,2],hotel_poiBunch.target[:5])
print(val)
df3['price']=val
print(df3)
```

    锦江之星酒店(钟楼店)    166.0
    西安君乐城堡酒店       406.0
    荣民国际酒店         268.0
    美丽豪酒店          200.0
    西京国际饭店         329.0
    dtype: float64
                     latitude   longitude  price
    锦江之星酒店(钟楼店)     34.266776  108.951985  166.0
    西安君乐城堡酒店        34.255235  108.951857  406.0
    荣民国际酒店          34.265239  108.944081  268.0
    美丽豪酒店           34.266445  108.947120  200.0
    西京国际饭店          34.265829  108.943414  329.0
    西安东舍精品酒店        34.261837  108.948272    NaN
    汉庭酒店(西安钟鼓楼广场店)  34.263523  108.949807    NaN
    建苑大厦            34.261119  108.944958    NaN
    全季酒店(西安钟鼓楼店)    34.264448  108.950193    NaN
    西安美莎酒店          34.255650  108.950651    NaN
    

* 赋值时，如果不存在索引，则创建该索引


```python
df3['newCol']=df3.price==406.0
print(df3)
```

                     latitude   longitude  price  newCol
    锦江之星酒店(钟楼店)     34.266776  108.951985  166.0   False
    西安君乐城堡酒店        34.255235  108.951857  406.0    True
    荣民国际酒店          34.265239  108.944081  268.0   False
    美丽豪酒店           34.266445  108.947120  200.0   False
    西京国际饭店          34.265829  108.943414  329.0   False
    西安东舍精品酒店        34.261837  108.948272    NaN   False
    汉庭酒店(西安钟鼓楼广场店)  34.263523  108.949807    NaN   False
    建苑大厦            34.261119  108.944958    NaN   False
    全季酒店(西安钟鼓楼店)    34.264448  108.950193    NaN   False
    西安美莎酒店          34.255650  108.950651    NaN   False
    

#### 转置


```python
df3.T
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
      <th>锦江之星酒店(钟楼店)</th>
      <th>西安君乐城堡酒店</th>
      <th>荣民国际酒店</th>
      <th>美丽豪酒店</th>
      <th>西京国际饭店</th>
      <th>西安东舍精品酒店</th>
      <th>汉庭酒店(西安钟鼓楼广场店)</th>
      <th>建苑大厦</th>
      <th>全季酒店(西安钟鼓楼店)</th>
      <th>西安美莎酒店</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>latitude</th>
      <td>34.2668</td>
      <td>34.2552</td>
      <td>34.2652</td>
      <td>34.2664</td>
      <td>34.2658</td>
      <td>34.2618</td>
      <td>34.2635</td>
      <td>34.2611</td>
      <td>34.2644</td>
      <td>34.2557</td>
    </tr>
    <tr>
      <th>longitude</th>
      <td>108.952</td>
      <td>108.952</td>
      <td>108.944</td>
      <td>108.947</td>
      <td>108.943</td>
      <td>108.948</td>
      <td>108.95</td>
      <td>108.945</td>
      <td>108.95</td>
      <td>108.951</td>
    </tr>
    <tr>
      <th>price</th>
      <td>166</td>
      <td>406</td>
      <td>268</td>
      <td>200</td>
      <td>329</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>newCol</th>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



#### df的index和columns的name属性


```python
df5.index.name='info';df5.columns.name='name'
print(df5)
```

    name       荣民国际酒店    西安君乐城堡酒店
    info                         
    lat     34.266776   34.255235
    lng    108.951985  108.951857
    price  166.000000  406.000000
    


```python
print("df5.index->{}".format(df5.index))
print("_"*50)
print("df5.columns->{}".format(df5.columns))
```

    df5.index->Index(['lat', 'lng', 'price'], dtype='object', name='info')
    __________________________________________________
    df5.columns->Index(['荣民国际酒店', '西安君乐城堡酒店'], dtype='object', name='name')
    

#### 值的返回


```python
df5.values
```




    array([[ 34.266776,  34.255235],
           [108.951985, 108.951857],
           [166.      , 406.      ]])



### 基本功能
#### 重新索引

* Series

\_ 根据新索引重排，如果新索引中有当前不存在，则引入缺失值NaN


```python
print("cols->{}".format(cols))
obj1=Series(dict(zip(cols,hotel_poiBunch.data[40].tolist())))
print("obj1->{}".format(obj1))
colsNew=['facility_rating','hygiene_rating','service_rating','price','area']
obj1_reidx=obj1.reindex(colsNew)
print("_"*50)
print("obj1_reidx->{}".format(obj1_reidx))
```

    cols->['latitude', 'longitude', 'price', 'overall_rating', 'service_rating', 'facility_rating', 'hygiene_rating', 'image_num', 'comment_num', 'favorite_num', 'checkin_num']
    obj1->latitude            34.264806
    longitude          108.926551
    price              100.000000
    overall_rating       4.600000
    service_rating       4.600000
    facility_rating      4.500000
    hygiene_rating       4.600000
    image_num           30.000000
    comment_num         21.000000
    favorite_num         0.000000
    checkin_num          0.000000
    dtype: float64
    __________________________________________________
    obj1_reidx->facility_rating      4.5
    hygiene_rating       4.6
    service_rating       4.6
    price              100.0
    area                 NaN
    dtype: float64
    

\_ 指定缺失值


```python
obj1.reindex(colsNew,fill_value=500)
```




    facility_rating      4.5
    hygiene_rating       4.6
    service_rating       4.6
    price              100.0
    area               500.0
    dtype: float64



\_ method参数，可以对索引可排序对象做插值处理

ffill/pad向前填充

bfill/backfill向后填充


```python
obj2=Series(hotel_poiBunch.data[:10,2],hotel_poiBunch.target[:10])
print(obj2)
```

    锦江之星酒店(钟楼店)       166.0
    西安君乐城堡酒店          406.0
    荣民国际酒店            268.0
    美丽豪酒店             200.0
    西京国际饭店            329.0
    西安东舍精品酒店          526.0
    汉庭酒店(西安钟鼓楼广场店)    187.0
    建苑大厦              100.0
    全季酒店(西安钟鼓楼店)      280.0
    西安美莎酒店            192.0
    dtype: float64
    


```python
obj2.index=range(10)
print(obj2)
```

    0    166.0
    1    406.0
    2    268.0
    3    200.0
    4    329.0
    5    526.0
    6    187.0
    7    100.0
    8    280.0
    9    192.0
    dtype: float64
    


```python
obj2.reindex(range(12),method='ffill')
```




    0     166.0
    1     406.0
    2     268.0
    3     200.0
    4     329.0
    5     526.0
    6     187.0
    7     100.0
    8     280.0
    9     192.0
    10    192.0
    11    192.0
    dtype: float64



* DataFrame

\_ 行重新索引


```python
df1=DataFrame(dict(zip(cols[:3],hotel_poiBunch.data[:10,:3].T.tolist())),columns=['latitude','longitude','price'],index=hotel_poiBunch.target[:10])
df1
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
      <th>latitude</th>
      <th>longitude</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>锦江之星酒店(钟楼店)</th>
      <td>34.266776</td>
      <td>108.951985</td>
      <td>166.0</td>
    </tr>
    <tr>
      <th>西安君乐城堡酒店</th>
      <td>34.255235</td>
      <td>108.951857</td>
      <td>406.0</td>
    </tr>
    <tr>
      <th>荣民国际酒店</th>
      <td>34.265239</td>
      <td>108.944081</td>
      <td>268.0</td>
    </tr>
    <tr>
      <th>美丽豪酒店</th>
      <td>34.266445</td>
      <td>108.947120</td>
      <td>200.0</td>
    </tr>
    <tr>
      <th>西京国际饭店</th>
      <td>34.265829</td>
      <td>108.943414</td>
      <td>329.0</td>
    </tr>
    <tr>
      <th>西安东舍精品酒店</th>
      <td>34.261837</td>
      <td>108.948272</td>
      <td>526.0</td>
    </tr>
    <tr>
      <th>汉庭酒店(西安钟鼓楼广场店)</th>
      <td>34.263523</td>
      <td>108.949807</td>
      <td>187.0</td>
    </tr>
    <tr>
      <th>建苑大厦</th>
      <td>34.261119</td>
      <td>108.944958</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>全季酒店(西安钟鼓楼店)</th>
      <td>34.264448</td>
      <td>108.950193</td>
      <td>280.0</td>
    </tr>
    <tr>
      <th>西安美莎酒店</th>
      <td>34.255650</td>
      <td>108.950651</td>
      <td>192.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1_reidx=df1.reindex(hotel_poiBunch.target[:10][-1::-1])
df1_reidx
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
      <th>latitude</th>
      <th>longitude</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>西安美莎酒店</th>
      <td>34.255650</td>
      <td>108.950651</td>
      <td>192.0</td>
    </tr>
    <tr>
      <th>全季酒店(西安钟鼓楼店)</th>
      <td>34.264448</td>
      <td>108.950193</td>
      <td>280.0</td>
    </tr>
    <tr>
      <th>建苑大厦</th>
      <td>34.261119</td>
      <td>108.944958</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>汉庭酒店(西安钟鼓楼广场店)</th>
      <td>34.263523</td>
      <td>108.949807</td>
      <td>187.0</td>
    </tr>
    <tr>
      <th>西安东舍精品酒店</th>
      <td>34.261837</td>
      <td>108.948272</td>
      <td>526.0</td>
    </tr>
    <tr>
      <th>西京国际饭店</th>
      <td>34.265829</td>
      <td>108.943414</td>
      <td>329.0</td>
    </tr>
    <tr>
      <th>美丽豪酒店</th>
      <td>34.266445</td>
      <td>108.947120</td>
      <td>200.0</td>
    </tr>
    <tr>
      <th>荣民国际酒店</th>
      <td>34.265239</td>
      <td>108.944081</td>
      <td>268.0</td>
    </tr>
    <tr>
      <th>西安君乐城堡酒店</th>
      <td>34.255235</td>
      <td>108.951857</td>
      <td>406.0</td>
    </tr>
    <tr>
      <th>锦江之星酒店(钟楼店)</th>
      <td>34.266776</td>
      <td>108.951985</td>
      <td>166.0</td>
    </tr>
  </tbody>
</table>
</div>



\_ 列重新索引


```python
df1.reindex(columns=['price','lat','lng','area'])
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
      <th>price</th>
      <th>lat</th>
      <th>lng</th>
      <th>area</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>锦江之星酒店(钟楼店)</th>
      <td>166.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>西安君乐城堡酒店</th>
      <td>406.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>荣民国际酒店</th>
      <td>268.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>美丽豪酒店</th>
      <td>200.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>西京国际饭店</th>
      <td>329.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>西安东舍精品酒店</th>
      <td>526.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>汉庭酒店(西安钟鼓楼广场店)</th>
      <td>187.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>建苑大厦</th>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>全季酒店(西安钟鼓楼店)</th>
      <td>280.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>西安美莎酒店</th>
      <td>192.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



\_ 行列同时索引。插值只能按行索引


```python
df1.reindex(index=hotel_poiBunch.target[:10][-1::-1],columns=['price','lat','lng','newPrice'],fill_value=600)
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
      <th>price</th>
      <th>lat</th>
      <th>lng</th>
      <th>newPrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>西安美莎酒店</th>
      <td>192.0</td>
      <td>600.0</td>
      <td>600.0</td>
      <td>600.0</td>
    </tr>
    <tr>
      <th>全季酒店(西安钟鼓楼店)</th>
      <td>280.0</td>
      <td>600.0</td>
      <td>600.0</td>
      <td>600.0</td>
    </tr>
    <tr>
      <th>建苑大厦</th>
      <td>100.0</td>
      <td>600.0</td>
      <td>600.0</td>
      <td>600.0</td>
    </tr>
    <tr>
      <th>汉庭酒店(西安钟鼓楼广场店)</th>
      <td>187.0</td>
      <td>600.0</td>
      <td>600.0</td>
      <td>600.0</td>
    </tr>
    <tr>
      <th>西安东舍精品酒店</th>
      <td>526.0</td>
      <td>600.0</td>
      <td>600.0</td>
      <td>600.0</td>
    </tr>
    <tr>
      <th>西京国际饭店</th>
      <td>329.0</td>
      <td>600.0</td>
      <td>600.0</td>
      <td>600.0</td>
    </tr>
    <tr>
      <th>美丽豪酒店</th>
      <td>200.0</td>
      <td>600.0</td>
      <td>600.0</td>
      <td>600.0</td>
    </tr>
    <tr>
      <th>荣民国际酒店</th>
      <td>268.0</td>
      <td>600.0</td>
      <td>600.0</td>
      <td>600.0</td>
    </tr>
    <tr>
      <th>西安君乐城堡酒店</th>
      <td>406.0</td>
      <td>600.0</td>
      <td>600.0</td>
      <td>600.0</td>
    </tr>
    <tr>
      <th>锦江之星酒店(钟楼店)</th>
      <td>166.0</td>
      <td>600.0</td>
      <td>600.0</td>
      <td>600.0</td>
    </tr>
  </tbody>
</table>
</div>



#### 丢弃指定轴上的项

* Series

给定索引丢弃该值，返回丢弃值后的新对象


```python
print("obj1->{}".format(obj1))
print("_"*50)
newObj=obj1.drop('checkin_num')
print("newObj->{}".format(newObj))
newObj_=obj1.drop(['checkin_num','facility_rating'])
print("_"*50)
print("newObj_->{}".format(newObj_))
```

    obj1->latitude            34.264806
    longitude          108.926551
    price              100.000000
    overall_rating       4.600000
    service_rating       4.600000
    facility_rating      4.500000
    hygiene_rating       4.600000
    image_num           30.000000
    comment_num         21.000000
    favorite_num         0.000000
    checkin_num          0.000000
    dtype: float64
    __________________________________________________
    newObj->latitude            34.264806
    longitude          108.926551
    price              100.000000
    overall_rating       4.600000
    service_rating       4.600000
    facility_rating      4.500000
    hygiene_rating       4.600000
    image_num           30.000000
    comment_num         21.000000
    favorite_num         0.000000
    dtype: float64
    __________________________________________________
    newObj_->latitude           34.264806
    longitude         108.926551
    price             100.000000
    overall_rating      4.600000
    service_rating      4.600000
    hygiene_rating      4.600000
    image_num          30.000000
    comment_num        21.000000
    favorite_num        0.000000
    dtype: float64
    

* DataFrame


```python
print("df1->{}".format(df1))
print("_"*50)
df1.drop(['西京国际饭店','建苑大厦'])
```

    df1->                 latitude   longitude  price
    锦江之星酒店(钟楼店)     34.266776  108.951985  166.0
    西安君乐城堡酒店        34.255235  108.951857  406.0
    荣民国际酒店          34.265239  108.944081  268.0
    美丽豪酒店           34.266445  108.947120  200.0
    西京国际饭店          34.265829  108.943414  329.0
    西安东舍精品酒店        34.261837  108.948272  526.0
    汉庭酒店(西安钟鼓楼广场店)  34.263523  108.949807  187.0
    建苑大厦            34.261119  108.944958  100.0
    全季酒店(西安钟鼓楼店)    34.264448  108.950193  280.0
    西安美莎酒店          34.255650  108.950651  192.0
    __________________________________________________
    




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
      <th>latitude</th>
      <th>longitude</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>锦江之星酒店(钟楼店)</th>
      <td>34.266776</td>
      <td>108.951985</td>
      <td>166.0</td>
    </tr>
    <tr>
      <th>西安君乐城堡酒店</th>
      <td>34.255235</td>
      <td>108.951857</td>
      <td>406.0</td>
    </tr>
    <tr>
      <th>荣民国际酒店</th>
      <td>34.265239</td>
      <td>108.944081</td>
      <td>268.0</td>
    </tr>
    <tr>
      <th>美丽豪酒店</th>
      <td>34.266445</td>
      <td>108.947120</td>
      <td>200.0</td>
    </tr>
    <tr>
      <th>西安东舍精品酒店</th>
      <td>34.261837</td>
      <td>108.948272</td>
      <td>526.0</td>
    </tr>
    <tr>
      <th>汉庭酒店(西安钟鼓楼广场店)</th>
      <td>34.263523</td>
      <td>108.949807</td>
      <td>187.0</td>
    </tr>
    <tr>
      <th>全季酒店(西安钟鼓楼店)</th>
      <td>34.264448</td>
      <td>108.950193</td>
      <td>280.0</td>
    </tr>
    <tr>
      <th>西安美莎酒店</th>
      <td>34.255650</td>
      <td>108.950651</td>
      <td>192.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1.drop(['latitude','longitude'],axis=1)
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
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>锦江之星酒店(钟楼店)</th>
      <td>166.0</td>
    </tr>
    <tr>
      <th>西安君乐城堡酒店</th>
      <td>406.0</td>
    </tr>
    <tr>
      <th>荣民国际酒店</th>
      <td>268.0</td>
    </tr>
    <tr>
      <th>美丽豪酒店</th>
      <td>200.0</td>
    </tr>
    <tr>
      <th>西京国际饭店</th>
      <td>329.0</td>
    </tr>
    <tr>
      <th>西安东舍精品酒店</th>
      <td>526.0</td>
    </tr>
    <tr>
      <th>汉庭酒店(西安钟鼓楼广场店)</th>
      <td>187.0</td>
    </tr>
    <tr>
      <th>建苑大厦</th>
      <td>100.0</td>
    </tr>
    <tr>
      <th>全季酒店(西安钟鼓楼店)</th>
      <td>280.0</td>
    </tr>
    <tr>
      <th>西安美莎酒店</th>
      <td>192.0</td>
    </tr>
  </tbody>
</table>
</div>



#### 索引/选取/过滤

* Series


```python
print("obj1->{}".format(obj1))
print("_"*50)
print("obj1['price']->{}".format(obj1['price']))
print("_"*50)
print("obj1[['latitude','longitude']]->{}".format(obj1[['latitude','longitude']]))
print("_"*50)
print("obj1[-5:-3]->{}".format(obj1[-5:-3]))
print("_"*50)
print("obj1['latitude':'price']->{}".format(obj1['latitude':'price'])) #标签切片，包含末端(封闭区间)
print("_"*50)
print("obj2->{}".format(obj2))
print("_"*50)
print("obj2[obj2>300]->{}".format(obj2[obj2>300]))
print("_"*50)
obj2[2:4]=900
print("obj2[2:4]=900->{}".format(obj2))
```

    obj1->latitude            34.264806
    longitude          108.926551
    price              100.000000
    overall_rating       4.600000
    service_rating       4.600000
    facility_rating      4.500000
    hygiene_rating       4.600000
    image_num           30.000000
    comment_num         21.000000
    favorite_num         0.000000
    checkin_num          0.000000
    dtype: float64
    __________________________________________________
    obj1['price']->100.0
    __________________________________________________
    obj1[['latitude','longitude']]->latitude      34.264806
    longitude    108.926551
    dtype: float64
    __________________________________________________
    obj1[-5:-3]->hygiene_rating     4.6
    image_num         30.0
    dtype: float64
    __________________________________________________
    obj1['latitude':'price']->latitude      34.264806
    longitude    108.926551
    price        100.000000
    dtype: float64
    __________________________________________________
    obj2->0    166.0
    1    406.0
    2    900.0
    3    900.0
    4    329.0
    5    526.0
    6    187.0
    7    100.0
    8    280.0
    9    192.0
    dtype: float64
    __________________________________________________
    obj2[obj2>300]->1    406.0
    2    900.0
    3    900.0
    4    329.0
    5    526.0
    dtype: float64
    __________________________________________________
    obj2[2:4]=900->0    166.0
    1    406.0
    2    900.0
    3    900.0
    4    329.0
    5    526.0
    6    187.0
    7    100.0
    8    280.0
    9    192.0
    dtype: float64
    

* DataFrame


```python
print("df1->{}".format(df1))
print("_"*50)
print("df1['price']->{}".format(df1['price']))
print("_"*50)
print("df1[[['latitude','longitude']]->{}".format(df1[['latitude','longitude']]))
print("_"*50)
print("df1[:2]>{}".format(df1[:2]))
print("_"*50)
print("df1[df1['price']>300]>{}".format(df1[df1['price']>300]))
df1['price'][df1['price']>300]=900
print("_"*50)
print("df1->{}".format(df1))
print("_"*50)
print("df1['price']>300->{}".format(df1['price']>300))
print("_"*50)
print("df1.loc['全季酒店(西安钟鼓楼店)',['latitude','longitude']]->{}".format(df1.loc['全季酒店(西安钟鼓楼店)',['latitude','longitude']]))
print("_"*50)
print("df1.iloc[-2]->{}".format(df1.iloc[-2]))
print("_"*50)
print("df1.loc[:'西京国际饭店','price']->{}".format(df1.loc[:'西京国际饭店','price']))
print("_"*50)
print("df1.loc[df1.price>300]->{}".format(df1.loc[df1.price>300]))
```

    df1->                 latitude   longitude  price
    锦江之星酒店(钟楼店)     34.266776  108.951985  166.0
    西安君乐城堡酒店        34.255235  108.951857  900.0
    荣民国际酒店          34.265239  108.944081  268.0
    美丽豪酒店           34.266445  108.947120  200.0
    西京国际饭店          34.265829  108.943414  900.0
    西安东舍精品酒店        34.261837  108.948272  900.0
    汉庭酒店(西安钟鼓楼广场店)  34.263523  108.949807  187.0
    建苑大厦            34.261119  108.944958  100.0
    全季酒店(西安钟鼓楼店)    34.264448  108.950193  280.0
    西安美莎酒店          34.255650  108.950651  192.0
    __________________________________________________
    df1['price']->锦江之星酒店(钟楼店)       166.0
    西安君乐城堡酒店          900.0
    荣民国际酒店            268.0
    美丽豪酒店             200.0
    西京国际饭店            900.0
    西安东舍精品酒店          900.0
    汉庭酒店(西安钟鼓楼广场店)    187.0
    建苑大厦              100.0
    全季酒店(西安钟鼓楼店)      280.0
    西安美莎酒店            192.0
    Name: price, dtype: float64
    __________________________________________________
    df1[[['latitude','longitude']]->                 latitude   longitude
    锦江之星酒店(钟楼店)     34.266776  108.951985
    西安君乐城堡酒店        34.255235  108.951857
    荣民国际酒店          34.265239  108.944081
    美丽豪酒店           34.266445  108.947120
    西京国际饭店          34.265829  108.943414
    西安东舍精品酒店        34.261837  108.948272
    汉庭酒店(西安钟鼓楼广场店)  34.263523  108.949807
    建苑大厦            34.261119  108.944958
    全季酒店(西安钟鼓楼店)    34.264448  108.950193
    西安美莎酒店          34.255650  108.950651
    __________________________________________________
    df1[:2]>              latitude   longitude  price
    锦江之星酒店(钟楼店)  34.266776  108.951985  166.0
    西安君乐城堡酒店     34.255235  108.951857  900.0
    __________________________________________________
    df1[df1['price']>300]>           latitude   longitude  price
    西安君乐城堡酒店  34.255235  108.951857  900.0
    西京国际饭店    34.265829  108.943414  900.0
    西安东舍精品酒店  34.261837  108.948272  900.0
    __________________________________________________
    df1->                 latitude   longitude  price
    锦江之星酒店(钟楼店)     34.266776  108.951985  166.0
    西安君乐城堡酒店        34.255235  108.951857  900.0
    荣民国际酒店          34.265239  108.944081  268.0
    美丽豪酒店           34.266445  108.947120  200.0
    西京国际饭店          34.265829  108.943414  900.0
    西安东舍精品酒店        34.261837  108.948272  900.0
    汉庭酒店(西安钟鼓楼广场店)  34.263523  108.949807  187.0
    建苑大厦            34.261119  108.944958  100.0
    全季酒店(西安钟鼓楼店)    34.264448  108.950193  280.0
    西安美莎酒店          34.255650  108.950651  192.0
    __________________________________________________
    df1['price']>300->锦江之星酒店(钟楼店)       False
    西安君乐城堡酒店           True
    荣民国际酒店            False
    美丽豪酒店             False
    西京国际饭店             True
    西安东舍精品酒店           True
    汉庭酒店(西安钟鼓楼广场店)    False
    建苑大厦              False
    全季酒店(西安钟鼓楼店)      False
    西安美莎酒店            False
    Name: price, dtype: bool
    __________________________________________________
    df1.loc['全季酒店(西安钟鼓楼店)',['latitude','longitude']]->latitude      34.264448
    longitude    108.950193
    Name: 全季酒店(西安钟鼓楼店), dtype: float64
    __________________________________________________
    df1.iloc[-2]->latitude      34.264448
    longitude    108.950193
    price        280.000000
    Name: 全季酒店(西安钟鼓楼店), dtype: float64
    __________________________________________________
    df1.loc[:'西京国际饭店','price']->锦江之星酒店(钟楼店)    166.0
    西安君乐城堡酒店       900.0
    荣民国际酒店         268.0
    美丽豪酒店          200.0
    西京国际饭店         900.0
    Name: price, dtype: float64
    __________________________________________________
    df1.loc[df1.price>300]->           latitude   longitude  price
    西安君乐城堡酒店  34.255235  108.951857  900.0
    西京国际饭店    34.265829  108.943414  900.0
    西安东舍精品酒店  34.261837  108.948272  900.0
    

#### 算数运算和数据对齐

* Series


```python
obj1=Series(hotel_poiBunch.data[:10,5],hotel_poiBunch.target[:10])
print("obj1->{}".format(obj1))
print("_"*50)
obj2=Series(hotel_poiBunch.data[:13,6],hotel_poiBunch.target[:13])
print("obj2->{}".format(obj2))
print("_"*50)
print("obj1+obj2->{}".format(obj1+obj2))
```

    obj1->锦江之星酒店(钟楼店)       4.4
    西安君乐城堡酒店          4.2
    荣民国际酒店            4.4
    美丽豪酒店             4.6
    西京国际饭店            4.2
    西安东舍精品酒店          4.9
    汉庭酒店(西安钟鼓楼广场店)    4.9
    建苑大厦              4.0
    全季酒店(西安钟鼓楼店)      4.8
    西安美莎酒店            4.3
    dtype: float64
    __________________________________________________
    obj2->锦江之星酒店(钟楼店)       4.5
    西安君乐城堡酒店          4.5
    荣民国际酒店            4.6
    美丽豪酒店             4.7
    西京国际饭店            4.5
    西安东舍精品酒店          4.9
    汉庭酒店(西安钟鼓楼广场店)    4.9
    建苑大厦              4.4
    全季酒店(西安钟鼓楼店)      4.9
    西安美莎酒店            4.4
    美伦酒店              4.6
    桔子酒店精选西安南门店       4.9
    西安金花豪生国际大酒店       4.4
    dtype: float64
    __________________________________________________
    obj1+obj2->全季酒店(西安钟鼓楼店)      9.7
    建苑大厦              8.4
    桔子酒店精选西安南门店       NaN
    汉庭酒店(西安钟鼓楼广场店)    9.8
    美丽豪酒店             9.3
    美伦酒店              NaN
    荣民国际酒店            9.0
    西京国际饭店            8.7
    西安东舍精品酒店          9.8
    西安君乐城堡酒店          8.7
    西安美莎酒店            8.7
    西安金花豪生国际大酒店       NaN
    锦江之星酒店(钟楼店)       8.9
    dtype: float64
    

* DataFrame


```python
df1=DataFrame(dict(zip(('facility_rating','hygiene_rating','price'),hotel_poiBunch.data[:10,[5,6,2]].T.tolist())),index=hotel_poiBunch.target[:10])
print("df1->{}".format(df1))
print("_"*50)
df2=DataFrame(dict(zip(('facility_rating','hygiene_rating','price'),hotel_poiBunch.data[:13,[5,6,2]].T.tolist())),index=hotel_poiBunch.target[:13])
print("df2->{}".format(df2))
print("_"*50)
print("df1+df2->{}".format(df1+df2))
```

    df1->                facility_rating  hygiene_rating  price
    锦江之星酒店(钟楼店)                 4.4             4.5  166.0
    西安君乐城堡酒店                    4.2             4.5  406.0
    荣民国际酒店                      4.4             4.6  900.0
    美丽豪酒店                       4.6             4.7  900.0
    西京国际饭店                      4.2             4.5  329.0
    西安东舍精品酒店                    4.9             4.9  526.0
    汉庭酒店(西安钟鼓楼广场店)              4.9             4.9  187.0
    建苑大厦                        4.0             4.4  100.0
    全季酒店(西安钟鼓楼店)                4.8             4.9  280.0
    西安美莎酒店                      4.3             4.4  192.0
    __________________________________________________
    df2->                facility_rating  hygiene_rating  price
    锦江之星酒店(钟楼店)                 4.4             4.5  166.0
    西安君乐城堡酒店                    4.2             4.5  406.0
    荣民国际酒店                      4.4             4.6  900.0
    美丽豪酒店                       4.6             4.7  900.0
    西京国际饭店                      4.2             4.5  329.0
    西安东舍精品酒店                    4.9             4.9  526.0
    汉庭酒店(西安钟鼓楼广场店)              4.9             4.9  187.0
    建苑大厦                        4.0             4.4  100.0
    全季酒店(西安钟鼓楼店)                4.8             4.9  280.0
    西安美莎酒店                      4.3             4.4  192.0
    美伦酒店                        4.4             4.6  211.0
    桔子酒店精选西安南门店                 4.8             4.9  269.0
    西安金花豪生国际大酒店                 4.1             4.4  338.0
    __________________________________________________
    df1+df2->                facility_rating  hygiene_rating   price
    全季酒店(西安钟鼓楼店)                9.6             9.8   560.0
    建苑大厦                        8.0             8.8   200.0
    桔子酒店精选西安南门店                 NaN             NaN     NaN
    汉庭酒店(西安钟鼓楼广场店)              9.8             9.8   374.0
    美丽豪酒店                       9.2             9.4  1800.0
    美伦酒店                        NaN             NaN     NaN
    荣民国际酒店                      8.8             9.2  1800.0
    西京国际饭店                      8.4             9.0   658.0
    西安东舍精品酒店                    9.8             9.8  1052.0
    西安君乐城堡酒店                    8.4             9.0   812.0
    西安美莎酒店                      8.6             8.8   384.0
    西安金花豪生国际大酒店                 NaN             NaN     NaN
    锦江之星酒店(钟楼店)                 8.8             9.0   332.0
    

* 其它

\_ 设置填充值

add();  sub();  div();  mul()


```python
df1.add(df2,fill_value=0)
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
      <th>facility_rating</th>
      <th>hygiene_rating</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>全季酒店(西安钟鼓楼店)</th>
      <td>9.6</td>
      <td>9.8</td>
      <td>560.0</td>
    </tr>
    <tr>
      <th>建苑大厦</th>
      <td>8.0</td>
      <td>8.8</td>
      <td>200.0</td>
    </tr>
    <tr>
      <th>桔子酒店精选西安南门店</th>
      <td>4.8</td>
      <td>4.9</td>
      <td>269.0</td>
    </tr>
    <tr>
      <th>汉庭酒店(西安钟鼓楼广场店)</th>
      <td>9.8</td>
      <td>9.8</td>
      <td>374.0</td>
    </tr>
    <tr>
      <th>美丽豪酒店</th>
      <td>9.2</td>
      <td>9.4</td>
      <td>1800.0</td>
    </tr>
    <tr>
      <th>美伦酒店</th>
      <td>4.4</td>
      <td>4.6</td>
      <td>211.0</td>
    </tr>
    <tr>
      <th>荣民国际酒店</th>
      <td>8.8</td>
      <td>9.2</td>
      <td>1800.0</td>
    </tr>
    <tr>
      <th>西京国际饭店</th>
      <td>8.4</td>
      <td>9.0</td>
      <td>658.0</td>
    </tr>
    <tr>
      <th>西安东舍精品酒店</th>
      <td>9.8</td>
      <td>9.8</td>
      <td>1052.0</td>
    </tr>
    <tr>
      <th>西安君乐城堡酒店</th>
      <td>8.4</td>
      <td>9.0</td>
      <td>812.0</td>
    </tr>
    <tr>
      <th>西安美莎酒店</th>
      <td>8.6</td>
      <td>8.8</td>
      <td>384.0</td>
    </tr>
    <tr>
      <th>西安金花豪生国际大酒店</th>
      <td>4.1</td>
      <td>4.4</td>
      <td>338.0</td>
    </tr>
    <tr>
      <th>锦江之星酒店(钟楼店)</th>
      <td>8.8</td>
      <td>9.0</td>
      <td>332.0</td>
    </tr>
  </tbody>
</table>
</div>



\_ DataFrame与Series之间运算_广播(broadcasting)


```python
print("df1->{}".format(df1))
series=df1.iloc[0]
print("_"*50)
print("series->{}".format(series))
print("_"*50)
print("df1-series->{}".format(df1-series))
series=df1['facility_rating']
print("_"*50)
print("series->{}".format(series))
print("_"*50)
print("df1.sub(series,axis=0)->{}".format(df1.sub(series,axis=0)))
```

    df1->                facility_rating  hygiene_rating  price
    锦江之星酒店(钟楼店)                 4.4             4.5  166.0
    西安君乐城堡酒店                    4.2             4.5  406.0
    荣民国际酒店                      4.4             4.6  900.0
    美丽豪酒店                       4.6             4.7  900.0
    西京国际饭店                      4.2             4.5  329.0
    西安东舍精品酒店                    4.9             4.9  526.0
    汉庭酒店(西安钟鼓楼广场店)              4.9             4.9  187.0
    建苑大厦                        4.0             4.4  100.0
    全季酒店(西安钟鼓楼店)                4.8             4.9  280.0
    西安美莎酒店                      4.3             4.4  192.0
    __________________________________________________
    series->facility_rating      4.4
    hygiene_rating       4.5
    price              166.0
    Name: 锦江之星酒店(钟楼店), dtype: float64
    __________________________________________________
    df1-series->                facility_rating  hygiene_rating  price
    锦江之星酒店(钟楼店)                 0.0             0.0    0.0
    西安君乐城堡酒店                   -0.2             0.0  240.0
    荣民国际酒店                      0.0             0.1  734.0
    美丽豪酒店                       0.2             0.2  734.0
    西京国际饭店                     -0.2             0.0  163.0
    西安东舍精品酒店                    0.5             0.4  360.0
    汉庭酒店(西安钟鼓楼广场店)              0.5             0.4   21.0
    建苑大厦                       -0.4            -0.1  -66.0
    全季酒店(西安钟鼓楼店)                0.4             0.4  114.0
    西安美莎酒店                     -0.1            -0.1   26.0
    __________________________________________________
    series->锦江之星酒店(钟楼店)       4.4
    西安君乐城堡酒店          4.2
    荣民国际酒店            4.4
    美丽豪酒店             4.6
    西京国际饭店            4.2
    西安东舍精品酒店          4.9
    汉庭酒店(西安钟鼓楼广场店)    4.9
    建苑大厦              4.0
    全季酒店(西安钟鼓楼店)      4.8
    西安美莎酒店            4.3
    Name: facility_rating, dtype: float64
    __________________________________________________
    df1.sub(series,axis=0)->                facility_rating  hygiene_rating  price
    锦江之星酒店(钟楼店)                 0.0             0.1  161.6
    西安君乐城堡酒店                    0.0             0.3  401.8
    荣民国际酒店                      0.0             0.2  895.6
    美丽豪酒店                       0.0             0.1  895.4
    西京国际饭店                      0.0             0.3  324.8
    西安东舍精品酒店                    0.0             0.0  521.1
    汉庭酒店(西安钟鼓楼广场店)              0.0             0.0  182.1
    建苑大厦                        0.0             0.4   96.0
    全季酒店(西安钟鼓楼店)                0.0             0.1  275.2
    西安美莎酒店                      0.0             0.1  187.7
    

#### 函数应用和映射

* numpy的元素级数组方法可以用于操作pandas对象


```python
df3=DataFrame(dict(zip(('facility_rating','hygiene_rating'),hotel_poiBunch.data[:10,[5,6]].T.tolist())),index=hotel_poiBunch.target[:10])
print("df3->{}".format(df3))
print("_"*50)
print("np.amax(df3,axis=0)->{}".format(np.amax(df3,axis=0)))
```

    df3->                facility_rating  hygiene_rating
    锦江之星酒店(钟楼店)                 4.4             4.5
    西安君乐城堡酒店                    4.2             4.5
    荣民国际酒店                      4.4             4.6
    美丽豪酒店                       4.6             4.7
    西京国际饭店                      4.2             4.5
    西安东舍精品酒店                    4.9             4.9
    汉庭酒店(西安钟鼓楼广场店)              4.9             4.9
    建苑大厦                        4.0             4.4
    全季酒店(西安钟鼓楼店)                4.8             4.9
    西安美莎酒店                      4.3             4.4
    __________________________________________________
    np.amax(df3,axis=0)->facility_rating    4.9
    hygiene_rating     4.9
    dtype: float64
    

* df.apply()方法，将函数应用到行列数组上


```python
f=lambda x:x.max()-x.min()
print("df3.apply(f)->{}".format(df3.apply(f)))

def f(x):
    return Series([x.min(),x.max()],index=['min','max'])

print("_"*50)
print("df3.apply(f)->{}".format(df3.apply(f)))
```

    df3.apply(f)->facility_rating    0.9
    hygiene_rating     0.5
    dtype: float64
    __________________________________________________
    df3.apply(f)->     facility_rating  hygiene_rating
    min              4.0             4.4
    max              4.9             4.9
    

* df.applymap()方法，将函数应用到元素级


```python
format_fun=lambda x:'%.2f'%x
df3.applymap(format_fun)
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
      <th>facility_rating</th>
      <th>hygiene_rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>锦江之星酒店(钟楼店)</th>
      <td>4.40</td>
      <td>4.50</td>
    </tr>
    <tr>
      <th>西安君乐城堡酒店</th>
      <td>4.20</td>
      <td>4.50</td>
    </tr>
    <tr>
      <th>荣民国际酒店</th>
      <td>4.40</td>
      <td>4.60</td>
    </tr>
    <tr>
      <th>美丽豪酒店</th>
      <td>4.60</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>西京国际饭店</th>
      <td>4.20</td>
      <td>4.50</td>
    </tr>
    <tr>
      <th>西安东舍精品酒店</th>
      <td>4.90</td>
      <td>4.90</td>
    </tr>
    <tr>
      <th>汉庭酒店(西安钟鼓楼广场店)</th>
      <td>4.90</td>
      <td>4.90</td>
    </tr>
    <tr>
      <th>建苑大厦</th>
      <td>4.00</td>
      <td>4.40</td>
    </tr>
    <tr>
      <th>全季酒店(西安钟鼓楼店)</th>
      <td>4.80</td>
      <td>4.90</td>
    </tr>
    <tr>
      <th>西安美莎酒店</th>
      <td>4.30</td>
      <td>4.40</td>
    </tr>
  </tbody>
</table>
</div>



#### 排序和排名

* 排序

\_ 按索引排序

Series


```python
print("obj1->{}".format(obj1))
print("_"*50)
print("obj1.sort_index()->{}".format(obj1.sort_index())) #按拼音字母顺序
```

    obj1->锦江之星酒店(钟楼店)       4.4
    西安君乐城堡酒店          4.2
    荣民国际酒店            4.4
    美丽豪酒店             4.6
    西京国际饭店            4.2
    西安东舍精品酒店          4.9
    汉庭酒店(西安钟鼓楼广场店)    4.9
    建苑大厦              4.0
    全季酒店(西安钟鼓楼店)      4.8
    西安美莎酒店            4.3
    dtype: float64
    __________________________________________________
    obj1.sort_index()->全季酒店(西安钟鼓楼店)      4.8
    建苑大厦              4.0
    汉庭酒店(西安钟鼓楼广场店)    4.9
    美丽豪酒店             4.6
    荣民国际酒店            4.4
    西京国际饭店            4.2
    西安东舍精品酒店          4.9
    西安君乐城堡酒店          4.2
    西安美莎酒店            4.3
    锦江之星酒店(钟楼店)       4.4
    dtype: float64
    

DataFrame


```python
df4=DataFrame(dict(zip(['lat','lng','price','overall_rating','service_rating','facility_rating','hygiene_rating','image_num','comment_num','favorite_num','checkin_num'],hotel_poiBunch.data[:10,:].T.tolist())),index=hotel_poiBunch.target[:10])
print("df4->{}".format(df4))
print("_"*50)
print("df4.sort_index()->{}".format(df4.sort_index()))
print("_"*50)
print("df4.sort_index(axis=1)->{}".format(df4.sort_index(axis=1)))
```

    df4->                      lat         lng  price  overall_rating  service_rating  \
    锦江之星酒店(钟楼店)     34.266776  108.951985  166.0             4.5             4.6   
    西安君乐城堡酒店        34.255235  108.951857  406.0             4.3             4.5   
    荣民国际酒店          34.265239  108.944081  900.0             4.4             4.6   
    美丽豪酒店           34.266445  108.947120  900.0             4.7             4.9   
    西京国际饭店          34.265829  108.943414  329.0             4.3             4.4   
    西安东舍精品酒店        34.261837  108.948272  526.0             4.8             4.9   
    汉庭酒店(西安钟鼓楼广场店)  34.263523  108.949807  187.0             4.8             4.9   
    建苑大厦            34.261119  108.944958  100.0             4.1             4.2   
    全季酒店(西安钟鼓楼店)    34.264448  108.950193  280.0             4.8             4.8   
    西安美莎酒店          34.255650  108.950651  192.0             4.4             4.3   
    
                    facility_rating  hygiene_rating  image_num  comment_num  \
    锦江之星酒店(钟楼店)                 4.4             4.5       30.0        110.0   
    西安君乐城堡酒店                    4.2             4.5       30.0         30.0   
    荣民国际酒店                      4.4             4.6       30.0         47.0   
    美丽豪酒店                       4.6             4.7       30.0         44.0   
    西京国际饭店                      4.2             4.5       40.0         28.0   
    西安东舍精品酒店                    4.9             4.9       30.0         21.0   
    汉庭酒店(西安钟鼓楼广场店)              4.9             4.9       23.0         23.0   
    建苑大厦                        4.0             4.4       30.0         19.0   
    全季酒店(西安钟鼓楼店)                4.8             4.9       25.0         17.0   
    西安美莎酒店                      4.3             4.4       30.0         29.0   
    
                    favorite_num  checkin_num  
    锦江之星酒店(钟楼店)              0.0          0.0  
    西安君乐城堡酒店                 0.0          0.0  
    荣民国际酒店                   0.0          0.0  
    美丽豪酒店                    0.0          0.0  
    西京国际饭店                   0.0          0.0  
    西安东舍精品酒店                 0.0          0.0  
    汉庭酒店(西安钟鼓楼广场店)           0.0          0.0  
    建苑大厦                     0.0          0.0  
    全季酒店(西安钟鼓楼店)             0.0          0.0  
    西安美莎酒店                   0.0          0.0  
    __________________________________________________
    df4.sort_index()->                      lat         lng  price  overall_rating  service_rating  \
    全季酒店(西安钟鼓楼店)    34.264448  108.950193  280.0             4.8             4.8   
    建苑大厦            34.261119  108.944958  100.0             4.1             4.2   
    汉庭酒店(西安钟鼓楼广场店)  34.263523  108.949807  187.0             4.8             4.9   
    美丽豪酒店           34.266445  108.947120  900.0             4.7             4.9   
    荣民国际酒店          34.265239  108.944081  900.0             4.4             4.6   
    西京国际饭店          34.265829  108.943414  329.0             4.3             4.4   
    西安东舍精品酒店        34.261837  108.948272  526.0             4.8             4.9   
    西安君乐城堡酒店        34.255235  108.951857  406.0             4.3             4.5   
    西安美莎酒店          34.255650  108.950651  192.0             4.4             4.3   
    锦江之星酒店(钟楼店)     34.266776  108.951985  166.0             4.5             4.6   
    
                    facility_rating  hygiene_rating  image_num  comment_num  \
    全季酒店(西安钟鼓楼店)                4.8             4.9       25.0         17.0   
    建苑大厦                        4.0             4.4       30.0         19.0   
    汉庭酒店(西安钟鼓楼广场店)              4.9             4.9       23.0         23.0   
    美丽豪酒店                       4.6             4.7       30.0         44.0   
    荣民国际酒店                      4.4             4.6       30.0         47.0   
    西京国际饭店                      4.2             4.5       40.0         28.0   
    西安东舍精品酒店                    4.9             4.9       30.0         21.0   
    西安君乐城堡酒店                    4.2             4.5       30.0         30.0   
    西安美莎酒店                      4.3             4.4       30.0         29.0   
    锦江之星酒店(钟楼店)                 4.4             4.5       30.0        110.0   
    
                    favorite_num  checkin_num  
    全季酒店(西安钟鼓楼店)             0.0          0.0  
    建苑大厦                     0.0          0.0  
    汉庭酒店(西安钟鼓楼广场店)           0.0          0.0  
    美丽豪酒店                    0.0          0.0  
    荣民国际酒店                   0.0          0.0  
    西京国际饭店                   0.0          0.0  
    西安东舍精品酒店                 0.0          0.0  
    西安君乐城堡酒店                 0.0          0.0  
    西安美莎酒店                   0.0          0.0  
    锦江之星酒店(钟楼店)              0.0          0.0  
    __________________________________________________
    df4.sort_index(axis=1)->                checkin_num  comment_num  facility_rating  favorite_num  \
    锦江之星酒店(钟楼店)             0.0        110.0              4.4           0.0   
    西安君乐城堡酒店                0.0         30.0              4.2           0.0   
    荣民国际酒店                  0.0         47.0              4.4           0.0   
    美丽豪酒店                   0.0         44.0              4.6           0.0   
    西京国际饭店                  0.0         28.0              4.2           0.0   
    西安东舍精品酒店                0.0         21.0              4.9           0.0   
    汉庭酒店(西安钟鼓楼广场店)          0.0         23.0              4.9           0.0   
    建苑大厦                    0.0         19.0              4.0           0.0   
    全季酒店(西安钟鼓楼店)            0.0         17.0              4.8           0.0   
    西安美莎酒店                  0.0         29.0              4.3           0.0   
    
                    hygiene_rating  image_num        lat         lng  \
    锦江之星酒店(钟楼店)                4.5       30.0  34.266776  108.951985   
    西安君乐城堡酒店                   4.5       30.0  34.255235  108.951857   
    荣民国际酒店                     4.6       30.0  34.265239  108.944081   
    美丽豪酒店                      4.7       30.0  34.266445  108.947120   
    西京国际饭店                     4.5       40.0  34.265829  108.943414   
    西安东舍精品酒店                   4.9       30.0  34.261837  108.948272   
    汉庭酒店(西安钟鼓楼广场店)             4.9       23.0  34.263523  108.949807   
    建苑大厦                       4.4       30.0  34.261119  108.944958   
    全季酒店(西安钟鼓楼店)               4.9       25.0  34.264448  108.950193   
    西安美莎酒店                     4.4       30.0  34.255650  108.950651   
    
                    overall_rating  price  service_rating  
    锦江之星酒店(钟楼店)                4.5  166.0             4.6  
    西安君乐城堡酒店                   4.3  406.0             4.5  
    荣民国际酒店                     4.4  900.0             4.6  
    美丽豪酒店                      4.7  900.0             4.9  
    西京国际饭店                     4.3  329.0             4.4  
    西安东舍精品酒店                   4.8  526.0             4.9  
    汉庭酒店(西安钟鼓楼广场店)             4.8  187.0             4.9  
    建苑大厦                       4.1  100.0             4.2  
    全季酒店(西安钟鼓楼店)               4.8  280.0             4.8  
    西安美莎酒店                     4.4  192.0             4.3  
    

> 如果降序排列则传入参数ascending=False

\_ 按值排序

Series


```python
print("obj1->{}".format(obj1))
print("_"*50)
print("obj1.sort_values()->{}".format(obj1.sort_values()))
```

    obj1->锦江之星酒店(钟楼店)       4.4
    西安君乐城堡酒店          4.2
    荣民国际酒店            4.4
    美丽豪酒店             4.6
    西京国际饭店            4.2
    西安东舍精品酒店          4.9
    汉庭酒店(西安钟鼓楼广场店)    4.9
    建苑大厦              4.0
    全季酒店(西安钟鼓楼店)      4.8
    西安美莎酒店            4.3
    dtype: float64
    __________________________________________________
    obj1.sort_values()->建苑大厦              4.0
    西安君乐城堡酒店          4.2
    西京国际饭店            4.2
    西安美莎酒店            4.3
    锦江之星酒店(钟楼店)       4.4
    荣民国际酒店            4.4
    美丽豪酒店             4.6
    全季酒店(西安钟鼓楼店)      4.8
    西安东舍精品酒店          4.9
    汉庭酒店(西安钟鼓楼广场店)    4.9
    dtype: float64
    

DataFrame


```python
print("df2->{}".format(df2))
print("_"*50)
print("df2.sort_values(by='facility_rating')->{}".format(df2.sort_values(by='facility_rating')))
print("_"*50)
print("df2.sort_values(by=['facility_rating','price'])->{}".format(df2.sort_values(by=['facility_rating','price'])))
```

    df2->                facility_rating  hygiene_rating  price
    锦江之星酒店(钟楼店)                 4.4             4.5  166.0
    西安君乐城堡酒店                    4.2             4.5  406.0
    荣民国际酒店                      4.4             4.6  900.0
    美丽豪酒店                       4.6             4.7  900.0
    西京国际饭店                      4.2             4.5  329.0
    西安东舍精品酒店                    4.9             4.9  526.0
    汉庭酒店(西安钟鼓楼广场店)              4.9             4.9  187.0
    建苑大厦                        4.0             4.4  100.0
    全季酒店(西安钟鼓楼店)                4.8             4.9  280.0
    西安美莎酒店                      4.3             4.4  192.0
    美伦酒店                        4.4             4.6  211.0
    桔子酒店精选西安南门店                 4.8             4.9  269.0
    西安金花豪生国际大酒店                 4.1             4.4  338.0
    __________________________________________________
    df2.sort_values(by='facility_rating')->                facility_rating  hygiene_rating  price
    建苑大厦                        4.0             4.4  100.0
    西安金花豪生国际大酒店                 4.1             4.4  338.0
    西安君乐城堡酒店                    4.2             4.5  406.0
    西京国际饭店                      4.2             4.5  329.0
    西安美莎酒店                      4.3             4.4  192.0
    锦江之星酒店(钟楼店)                 4.4             4.5  166.0
    荣民国际酒店                      4.4             4.6  900.0
    美伦酒店                        4.4             4.6  211.0
    美丽豪酒店                       4.6             4.7  900.0
    全季酒店(西安钟鼓楼店)                4.8             4.9  280.0
    桔子酒店精选西安南门店                 4.8             4.9  269.0
    西安东舍精品酒店                    4.9             4.9  526.0
    汉庭酒店(西安钟鼓楼广场店)              4.9             4.9  187.0
    __________________________________________________
    df2.sort_values(by=['facility_rating','price'])->                facility_rating  hygiene_rating  price
    建苑大厦                        4.0             4.4  100.0
    西安金花豪生国际大酒店                 4.1             4.4  338.0
    西京国际饭店                      4.2             4.5  329.0
    西安君乐城堡酒店                    4.2             4.5  406.0
    西安美莎酒店                      4.3             4.4  192.0
    锦江之星酒店(钟楼店)                 4.4             4.5  166.0
    美伦酒店                        4.4             4.6  211.0
    荣民国际酒店                      4.4             4.6  900.0
    美丽豪酒店                       4.6             4.7  900.0
    桔子酒店精选西安南门店                 4.8             4.9  269.0
    全季酒店(西安钟鼓楼店)                4.8             4.9  280.0
    汉庭酒店(西安钟鼓楼广场店)              4.9             4.9  187.0
    西安东舍精品酒店                    4.9             4.9  526.0
    

* 排名

average:平均排名

min:最小排名

max:最大排名

first:在原始数组中出现的顺序排名

\_ Series


```python
print("obj1->{}".format(obj1))
print("_"*50)
print("obj1.rank()->{}".format(obj1.rank()))
print("_"*50)
print("obj1.rank(method='first')->{}".format(obj1.rank(method='first')))
print("_"*50)
print("obj1.rank(ascending=False,method='max')->{}".format(obj1.rank(ascending=False,method='max')))
```

    obj1->锦江之星酒店(钟楼店)       4.4
    西安君乐城堡酒店          4.2
    荣民国际酒店            4.4
    美丽豪酒店             4.6
    西京国际饭店            4.2
    西安东舍精品酒店          4.9
    汉庭酒店(西安钟鼓楼广场店)    4.9
    建苑大厦              4.0
    全季酒店(西安钟鼓楼店)      4.8
    西安美莎酒店            4.3
    dtype: float64
    __________________________________________________
    obj1.rank()->锦江之星酒店(钟楼店)       5.5
    西安君乐城堡酒店          2.5
    荣民国际酒店            5.5
    美丽豪酒店             7.0
    西京国际饭店            2.5
    西安东舍精品酒店          9.5
    汉庭酒店(西安钟鼓楼广场店)    9.5
    建苑大厦              1.0
    全季酒店(西安钟鼓楼店)      8.0
    西安美莎酒店            4.0
    dtype: float64
    __________________________________________________
    obj1.rank(method='first')->锦江之星酒店(钟楼店)        5.0
    西安君乐城堡酒店           2.0
    荣民国际酒店             6.0
    美丽豪酒店              7.0
    西京国际饭店             3.0
    西安东舍精品酒店           9.0
    汉庭酒店(西安钟鼓楼广场店)    10.0
    建苑大厦               1.0
    全季酒店(西安钟鼓楼店)       8.0
    西安美莎酒店             4.0
    dtype: float64
    __________________________________________________
    obj1.rank(ascending=False,method='max')->锦江之星酒店(钟楼店)        6.0
    西安君乐城堡酒店           9.0
    荣民国际酒店             6.0
    美丽豪酒店              4.0
    西京国际饭店             9.0
    西安东舍精品酒店           2.0
    汉庭酒店(西安钟鼓楼广场店)     2.0
    建苑大厦              10.0
    全季酒店(西安钟鼓楼店)       3.0
    西安美莎酒店             7.0
    dtype: float64
    

\_ DataFrame


```python
print("df1->{}".format(df1))
print("_"*50)
print("df1.rank()->{}".format(df1.rank()))
print("_"*50)
print("df1.rank(axis=1)->{}".format(df1.rank(axis=1)))
```

    df1->                facility_rating  hygiene_rating  price
    锦江之星酒店(钟楼店)                 4.4             4.5  166.0
    西安君乐城堡酒店                    4.2             4.5  406.0
    荣民国际酒店                      4.4             4.6  900.0
    美丽豪酒店                       4.6             4.7  900.0
    西京国际饭店                      4.2             4.5  329.0
    西安东舍精品酒店                    4.9             4.9  526.0
    汉庭酒店(西安钟鼓楼广场店)              4.9             4.9  187.0
    建苑大厦                        4.0             4.4  100.0
    全季酒店(西安钟鼓楼店)                4.8             4.9  280.0
    西安美莎酒店                      4.3             4.4  192.0
    __________________________________________________
    df1.rank()->                facility_rating  hygiene_rating  price
    锦江之星酒店(钟楼店)                 5.5             4.0    2.0
    西安君乐城堡酒店                    2.5             4.0    7.0
    荣民国际酒店                      5.5             6.0    9.5
    美丽豪酒店                       7.0             7.0    9.5
    西京国际饭店                      2.5             4.0    6.0
    西安东舍精品酒店                    9.5             9.0    8.0
    汉庭酒店(西安钟鼓楼广场店)              9.5             9.0    3.0
    建苑大厦                        1.0             1.5    1.0
    全季酒店(西安钟鼓楼店)                8.0             9.0    5.0
    西安美莎酒店                      4.0             1.5    4.0
    __________________________________________________
    df1.rank(axis=1)->                facility_rating  hygiene_rating  price
    锦江之星酒店(钟楼店)                 1.0             2.0    3.0
    西安君乐城堡酒店                    1.0             2.0    3.0
    荣民国际酒店                      1.0             2.0    3.0
    美丽豪酒店                       1.0             2.0    3.0
    西京国际饭店                      1.0             2.0    3.0
    西安东舍精品酒店                    1.5             1.5    3.0
    汉庭酒店(西安钟鼓楼广场店)              1.5             1.5    3.0
    建苑大厦                        1.0             2.0    3.0
    全季酒店(西安钟鼓楼店)                1.0             2.0    3.0
    西安美莎酒店                      1.0             2.0    3.0
    

\_ min/max


```python
pp=Series([7,-5,7,4,2,0,4])
print("_"*50)
print("pp.rank(method='min')->{}".format(pp.rank(method='min')))
print("_"*50)
print("pp.rank(method='max')->{}".format(pp.rank(method='max')))
```

    __________________________________________________
    pp.rank(method='min')->0    6.0
    1    1.0
    2    6.0
    3    4.0
    4    3.0
    5    2.0
    6    4.0
    dtype: float64
    __________________________________________________
    pp.rank(method='max')->0    7.0
    1    1.0
    2    7.0
    3    5.0
    4    3.0
    5    2.0
    6    5.0
    dtype: float64
    

#### 带有重复值的轴索引

* Series


```python
print("obj->{}".format(obj))
print("_"*50)
print("obj.index.is_unique>{}".format(obj.index.is_unique))
```

    obj->0                 lat
    1                 lng
    2               price
    3      overall_rating
    4      service_rating
    5     facility_rating
    6      hygiene_rating
    7           image_num
    8         comment_num
    9        favorite_num
    10        checkin_num
    dtype: object
    __________________________________________________
    obj.index.is_unique>True
    

* DataFrame


```python
print("df1->{}".format(df1))
print("_"*50)
print("df1.index.is_unique->{}".format(df1.index.is_unique))
```

    df1->                facility_rating  hygiene_rating  price
    锦江之星酒店(钟楼店)                 4.4             4.5  166.0
    西安君乐城堡酒店                    4.2             4.5  406.0
    荣民国际酒店                      4.4             4.6  900.0
    美丽豪酒店                       4.6             4.7  900.0
    西京国际饭店                      4.2             4.5  329.0
    西安东舍精品酒店                    4.9             4.9  526.0
    汉庭酒店(西安钟鼓楼广场店)              4.9             4.9  187.0
    建苑大厦                        4.0             4.4  100.0
    全季酒店(西安钟鼓楼店)                4.8             4.9  280.0
    西安美莎酒店                      4.3             4.4  192.0
    __________________________________________________
    df1.index.is_unique->True
    

<a href=""><img src="./icon/ant_02.jpg" height="auto" width="auto" title="caDesign"></a>  
