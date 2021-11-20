> Created on Sat Oct  9 09\47\48 2021 @author: Richie Bao-python_code_archi_la_design_method_study 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# 机器学习——聚类，CA +clustering (植被群落斑块)/调研路径与图像
## 1. 聚类 K-Means算法例举
监督学习(Supervised learning)，是机器学习的一种方法，可以学习训练数据集建立学习模型用于预测新的实例。回归属于监督学习，所用到的数据集被标识分类，通常包括特征值（自变量）和目标值（标签，因变量）。非监督学习(Unsupervised learning)，则没有给定事先标记的数据集，自动对数据集进行分类或分群。聚类则属于非监督学习。

聚类是把相似的对象通过静态分类的方法分成不同的组别或者更多的子集(subset)，这样让在同一个子集中的成员对象都有相似的一些属性。聚类涉及的算法很多，K-Means是常见算法的一种，通过尝试将样本分离到$n$个方差相等的组中对数据进行聚类，最小化指标（criterion），例如(inertia)或聚类内平方和指标(within-cluster sum-of-squares criterion)，其公式可表达为：$\sum_{i=0}^n  min(  \|  x_{i} -  \mu _{j}   \|^{2}  ):\mu _{j} \in C$。*Python Machine Learning* 对K-Means算法解释的非常清晰，引用其中的案例加以说明。

<a href=""><img src="./imgs_pyd/15_1.jpg" height="auto" width="auto" title="digit-x"></a>

图中假设了两个簇$C_{0} $和$C_{1} $，即$k=2$。首先随机放置两个质心（centroid）,并标识为old_centroid，通过逐个计算每个点分别到两个质心的距离，比较大小，将点归属于距离最近的质心（代表簇或组分类），例如对于点$a$，其到质心$C_{0}$ 距离$d_{0} $小于到质心$C_{1}$的距离，因此点$a$归属于质心$d_{0} $所代表的簇。将所有的点根据距离远近归属于不同的质心所代表的类之后，由所归属簇中的点的均值作为新的质心，图中用new_centroid假设标识。分别计算旧质心和新质心的距离，如果所有簇质心的新旧距离值为0，则意味质心没有发生变化，即完成聚类，否则，用新的质心重复上一轮的计算，直至质心新旧距离值为0为止。只要有足够的时间，K-Means总是收敛的，但它可能是一个局部最小值。这高度依赖于质心的初始化。因此，计算通常要进行多次，并对质心进行不同的初始化。
    
下述代码根据上述计算原理自定义聚类函数，同时使用Sklearn库的KMeans方法直接实现，比较二者之间的差异。自定义的K-Means所计算的结果有多种可能，通过多次运行会获得与Sklearn库一致的结果。Sklearn库的KMeans方法通过`init='k-means++'`使得质心彼此之间保持距离，解决了局部最小值的问题，证明比随机初始化得到更好的结果。
    
    
>  参考文献
> 1. Wei-Meng Lee.Python Machine Learning[M].US:Wiley.April, 2019.
2. Giuseppe Bonaccorso.Mastering Machine Learning Algorithms: Expert techniques for implementing popular machine learning algorithms, fine-tuning your models, and understanding how they work[M].Birmingham:Packt Publishing.January, 2020.




```python
import numpy as np
class K_Means:
    '''
    class - 定义K-Means算法
    
    Paras:
    X - 待分簇的数据（数组）
    k - 分簇数量
    figsize - Matplotlib图表大小
    '''    
    def __init__(self,X,k,figsize):              
        self.X=X
        self.k=k
        self.figsize=figsize              

    def euclidean_distance(self,a,b,ax=1):
        import numpy as np 
        '''
        function - 计算两点距离。To calculate the distance between two points
        
        Paras:
        a - 2维度数组，例如[[3,4]
                           [5,6]
                           [1,4]]
        b - 2维度数组
        ax - 计算轴
        '''
        return np.linalg.norm(a-b, axis=ax)
    
    def update(self,ax): 
        from copy import deepcopy
        import numpy as np
        '''
        function - K-Means算法
        '''
        #生产随机质心 generate k random points (centroids)
        Cx=np.random.randint(np.min(X[:,0]), np.max(X[:,0]), size=self.k)
        Cy=np.random.randint(np.min(X[:,1]), np.max(X[:,1]), size=self.k)
        ax.scatter(Cx, Cy,label="original random centroids",marker='*',c='gainsboro',s=200)  

        C=np.array(list(zip(Cx, Cy)), dtype=np.float64) #质心数组 -represent the k centroids as a matrix
        C_prev=np.zeros(C.shape) #建立同质心数组形状，值为0的数组-create a matrix of 0 with same dimension as C (centroids)
        clusters=np.zeros(len(X))#存储每个点所属子群-to store the cluster each point belongs to    
        distance_differences=self.euclidean_distance(C, C_prev)#计算质心与C_prev之间的距离-measure the distance between the centroids and C_prev        
        
        #循环计算，缩小前一步和后一步质心距离的差异 -loop as long as there is still a difference in distance between the previous and current centroids
        count=0
        while distance_differences.any() != 0:
            print("epoch:%d"%count)
            #将每个值分配到最近的簇-assign each value to its closest cluster
            for i in range(len(self.X)):
                distances=self.euclidean_distance(self.X[i], C)
                cluster=np.argmin(distances) #延着一个轴，返回最小值索引-returns the indices of the minimum values along an axis
                clusters[i]=cluster
                
            C_prev=deepcopy(C) #存储前一质心-store the prev centroids

            #通过取均值寻找新的质心-find the new centroids by taking the average value
            for i in range(k):
                points=[X[j] for j in range(len(X)) if clusters[j]==i] #取簇i中的所有点-take all the points in cluster i
                if len(points)!=0:
                    C[i]=np.mean(points,axis=0)

            distance_differences=self.euclidean_distance(C, C_prev) #计算前一与后一质心的距离-find the distances between the old centroids and the new centroids
            print("distance_differences:",distance_differences)
            count+=1

        #打印散点图-plot the scatter plot
        colors=['b','r','y','g','c','m']
        for i in range(k):
            points=np.array([X[j] for j in range(len(X)) if clusters[j] == i])
            if len(points) > 0:
                ax.scatter(points[:, 0], points[:, 1], s=10, c=colors[i])
            else:
                print("Plesae regenerate your centroids again.")#这意味着其中一个簇没有点 this means that one of the clusters has no points
            #ax.scatter(points[:, 0], points[:, 1], s=10, c=colors[i])
            ax.scatter(C[:, 0], C[:, 1], marker='*', s=200, c='red')     
            
    def sklearn_KMeans(self,ax):
        from sklearn.cluster import KMeans
        '''
        function - 使用Sklearn库的KMeans算法聚类
        '''
        kmeans=KMeans(n_clusters=self.k)
        kmeans=kmeans.fit(self.X)
        labels=kmeans.predict(self.X)
        centroids = kmeans.cluster_centers_
        
        c = ['b','r','y','g','c','m']
        colors = [c[i] for i in labels]        
        ax.scatter(centroids[:, 0], centroids[:, 1], marker='*', s=200, c='red')
        
        print("预测(7,5)的簇为：%d"%kmeans.predict([[7,5]])[0])
    
    def execution(self):
        %matplotlib inline
        import matplotlib.pyplot as plt
        '''
        function - 执行
        '''
        fig, axs=plt.subplots(1,2,figsize=self.figsize)  
        axs[0].scatter(self.X[:,0], self.X[:,1],label="points")
        axs[0].set_title(r'K-Means definition', fontsize=15)
        self.update(axs[0])
    
        axs[1].scatter(self.X[:,0], self.X[:,1],label="points")
        axs[1].set_title(r'sklearn_KMeans', fontsize=15)
        self.sklearn_KMeans(axs[1])
    
        axs[0].set_xlabel('x')
        axs[0].set_ylabel('y')
        axs[0].legend(loc='upper left', frameon=False)
        plt.show()
            
kmeans_dataset=[(1,1),(2,2),(2,3),(1,4),(3,3),(6,7),(7,8),(6,8),(7,6),(6,9),(2,5),(7,8),(8,9),(6,7),(7,8),(3,1),(8,4),(8,6),(8,9)]     
X=np.array(kmeans_dataset) 
k=3 #配置分组的数量（亦随机生成中心的数量）
figsize=(18,8)
K=K_Means(X,k,figsize)
K.execution()
```

    epoch:0
    distance_differences: [2.6925824  2.99175823 1.50923086]
    epoch:1
    distance_differences: [1.3        0.4056785  0.23570226]
    epoch:2
    distance_differences: [0.38873013 0.32363651 1.79505494]
    epoch:3
    distance_differences: [0.38873013 0.         0.70710678]
    epoch:4
    distance_differences: [0. 0. 0.]
    预测(7,5)的簇为：2
    


    
<a href=""><img src="./imgs_pyd/052.png" height="auto" width="auto" title="digit-x"></a>
    


* 聚类算法比较

[Sklearn官网聚类部分](https://scikit-learn.org/stable/modules/clustering.html#clustering)提供了一组代码，比较多个不同聚类算法，其归结如下：

| 方法名称 Method name                  |参数 Parameters                                                       |扩展性 Scalability                                                 |用例 Usecase                                                                   |几何 Geometry (metric used)                       |
|------------------------------|------------------------------------------------------------------|-------------------------------------------------------------|---------------------------------------------------------------------------|----------------------------------------------|
| K-Means                      | number of clusters                                               | Very large n_samples, medium n_clusters with MiniBatch code | General-purpose, even cluster size, flat geometry, not too many clusters  | Distances between points                     |
| Affinity propagation         | damping, sample preference                                       | Not scalable with n_samples                                 | Many clusters, uneven cluster size, non-flat geometry                     | Graph distance (e.g. nearest-neighbor graph) |
| Mean-shift                   | bandwidth                                                        | Not scalable with n_samples                                 | Many clusters, uneven cluster size, non-flat geometry                     | Distances between points                     |
| Spectral clustering          | number of clusters                                               | Medium n_samples, small n_clusters                          | Few clusters, even cluster size, non-flat geometry                        | Graph distance (e.g. nearest-neighbor graph) |
| Ward hierarchical clustering | number of clusters or distance threshold                         | Large n_samples and n_clusters                              | Many clusters, possibly connectivity constraints                          | Distances between points                     |
| Agglomerative clustering     | number of clusters or distance threshold, linkage type, distance | Large n_samples and n_clusters                              | Many clusters, possibly connectivity constraints, non Euclidean distances | Any pairwise distance                        |
| DBSCAN                       | neighborhood size                                                | Very large n_samples, medium n_clusters                     | Non-flat geometry, uneven cluster sizes                                   | Distances between nearest points             |
| OPTICS                       | minimum cluster membership                                       | Very large n_samples, large n_clusters                      | Non-flat geometry, uneven cluster sizes, variable cluster density         | Distances between points                     |
| Gaussian mixtures            | many                                                             | Not scalable                                                | Flat geometry, good for density estimation                                | Mahalanobis distances to centers             |
| Birch                        | branching factor, threshold, optional global clusterer.          | Large n_clusters and n_samples                              | Large dataset, outlier removal, data reduction.                           | Euclidean distance between points            |

其官网代码迁移如下。


```python
# import warnings filter
from warnings import simplefilter
# ignore all future warnings
simplefilter(action='ignore', category=FutureWarning)

print(__doc__)
import time
import warnings

import numpy as np
import matplotlib.pyplot as plt

from sklearn import cluster, datasets, mixture
from sklearn.neighbors import kneighbors_graph
from sklearn.preprocessing import StandardScaler
from itertools import cycle, islice

np.random.seed(0)

# ============
# Generate datasets. We choose the size big enough to see the scalability
# of the algorithms, but not too big to avoid too long running times
# ============
n_samples = 1500
noisy_circles = datasets.make_circles(n_samples=n_samples, factor=.5,noise=.05)
noisy_moons = datasets.make_moons(n_samples=n_samples, noise=.05)
blobs = datasets.make_blobs(n_samples=n_samples, random_state=8)
no_structure = np.random.rand(n_samples, 2), None

# Anisotropicly distributed data
random_state = 170
X, y = datasets.make_blobs(n_samples=n_samples, random_state=random_state)
transformation = [[0.6, -0.6], [-0.4, 0.8]]
X_aniso = np.dot(X, transformation)
aniso = (X_aniso, y)

# blobs with varied variances
varied = datasets.make_blobs(n_samples=n_samples,
                             cluster_std=[1.0, 2.5, 0.5],
                             random_state=random_state)

# ============
# Set up cluster parameters
# ============
plt.figure(figsize=(9 * 2 + 3, 12.5))
plt.subplots_adjust(left=.02, right=.98, bottom=.001, top=.96, wspace=.05,hspace=.01)

plot_num = 1

default_base = {'quantile': .3,
                'eps': .3,
                'damping': .9,
                'preference': -200,
                'n_neighbors': 10,
                'n_clusters': 3,
                'min_samples': 20,
                'xi': 0.05,
                'min_cluster_size': 0.1}

datasets = [
    (noisy_circles, {'damping': .77, 'preference': -240,'quantile': .2, 'n_clusters': 2,'min_samples': 20, 'xi': 0.25}),
    (noisy_moons, {'damping': .75, 'preference': -220, 'n_clusters': 2}),
    (varied, {'eps': .18, 'n_neighbors': 2,'min_samples': 5, 'xi': 0.035, 'min_cluster_size': .2}),
    (aniso, {'eps': .15, 'n_neighbors': 2,'min_samples': 20, 'xi': 0.1, 'min_cluster_size': .2}),
    (blobs, {}),
    (no_structure, {})]

for i_dataset, (dataset, algo_params) in enumerate(datasets):
    # update parameters with dataset-specific values
    params = default_base.copy()
    params.update(algo_params)

    X, y = dataset

    # normalize dataset for easier parameter selection
    X = StandardScaler().fit_transform(X)

    # estimate bandwidth for mean shift
    bandwidth = cluster.estimate_bandwidth(X, quantile=params['quantile'])

    # connectivity matrix for structured Ward
    connectivity = kneighbors_graph(X, n_neighbors=params['n_neighbors'], include_self=False)
    # make connectivity symmetric
    connectivity = 0.5 * (connectivity + connectivity.T)

    # ============
    # Create cluster objects
    # ============
    ms = cluster.MeanShift(bandwidth=bandwidth, bin_seeding=True)
    two_means = cluster.MiniBatchKMeans(n_clusters=params['n_clusters'])
    ward = cluster.AgglomerativeClustering(n_clusters=params['n_clusters'], linkage='ward',connectivity=connectivity)
    spectral = cluster.SpectralClustering(n_clusters=params['n_clusters'], eigen_solver='arpack',affinity="nearest_neighbors")
    dbscan = cluster.DBSCAN(eps=params['eps'])
    optics = cluster.OPTICS(min_samples=params['min_samples'],xi=params['xi'],min_cluster_size=params['min_cluster_size'])
    affinity_propagation = cluster.AffinityPropagation(damping=params['damping'], preference=params['preference'])
    average_linkage = cluster.AgglomerativeClustering(linkage="average", affinity="cityblock",n_clusters=params['n_clusters'], connectivity=connectivity)
    birch = cluster.Birch(n_clusters=params['n_clusters'])
    gmm = mixture.GaussianMixture(n_components=params['n_clusters'], covariance_type='full')

    clustering_algorithms = (
        ('MiniBatchKMeans', two_means),
        ('AffinityPropagation', affinity_propagation),
        ('MeanShift', ms),
        ('SpectralClustering', spectral),
        ('Ward', ward),
        ('AgglomerativeClustering', average_linkage),
        ('DBSCAN', dbscan),
        ('OPTICS', optics),
        ('Birch', birch),
        ('GaussianMixture', gmm)
    )

    for name, algorithm in clustering_algorithms:
        t0 = time.time()

        # catch warnings related to kneighbors_graph
        with warnings.catch_warnings():
            warnings.filterwarnings(
                "ignore",
                message="the number of connected components of the " +
                "connectivity matrix is [0-9]{1,2}" +
                " > 1. Completing it to avoid stopping the tree early.",
                category=UserWarning)
            warnings.filterwarnings(
                "ignore",
                message="Graph is not fully connected, spectral embedding" +
                " may not work as expected.",
                category=UserWarning)
            algorithm.fit(X)

        t1 = time.time()
        if hasattr(algorithm, 'labels_'):
            y_pred = algorithm.labels_.astype(int)
        else:
            y_pred = algorithm.predict(X)

        plt.subplot(len(datasets), len(clustering_algorithms), plot_num)
        if i_dataset == 0:
            plt.title(name, size=18)

        colors = np.array(list(islice(cycle(['#377eb8', '#ff7f00', '#4daf4a',
                                             '#f781bf', '#a65628', '#984ea3',
                                             '#999999', '#e41a1c', '#dede00']),
                                      int(max(y_pred) + 1))))
        # add black color for outliers (if any)
        colors = np.append(colors, ["#000000"])
        plt.scatter(X[:, 0], X[:, 1], s=10, color=colors[y_pred])

        plt.xlim(-2.5, 2.5)
        plt.ylim(-2.5, 2.5)
        plt.xticks(())
        plt.yticks(())
        plt.text(.99, .01, ('%.2fs' % (t1 - t0)).lstrip('0'),
                 transform=plt.gca().transAxes, size=15,
                 horizontalalignment='right')
        plot_num += 1

plt.show()
```

    Automatically created module for IPython interactive environment
    


    
<a href=""><img src="./imgs_pyd/053.png" height="auto" width="auto" title="digit-x"></a>
    


## 2. CA(mesa)+DBSCAN聚类

> [CA 迁移mesa案例](https://github.com/projectmesa/mesa/tree/main/examples/conways_game_of_life)


```python
# cell
from mesa import Agent

class Cell(Agent):
    """Represents a single ALIVE or DEAD cell in the simulation."""

    DEAD = 0
    ALIVE = 1

    def __init__(self, pos, model, init_state=DEAD):
        """
        Create a cell, in the given state, at the given x, y position.
        """
        super().__init__(pos, model)
        self.x, self.y = pos
        self.state = init_state
        self._nextState = None

    @property
    def isAlive(self):
        return self.state == self.ALIVE

    @property
    def neighbors(self):
        return self.model.grid.neighbor_iter((self.x, self.y), True)

    def step(self):
        """
        Compute if the cell will be dead or alive at the next tick.  This is
        based on the number of alive or dead neighbors.  The state is not
        changed here, but is just computed and stored in self._nextState,
        because our current state may still be necessary for our neighbors
        to calculate their next state.
        """

        # Get the neighbors and apply the rules on whether to be alive or dead
        # at the next tick.
        live_neighbors = sum(neighbor.isAlive for neighbor in self.neighbors)

        # Assume nextState is unchanged, unless changed below.
        self._nextState = self.state
        if self.isAlive:
            if live_neighbors < 2 or live_neighbors > 3:
                self._nextState = self.DEAD
        else:
            if live_neighbors == 3:
                self._nextState = self.ALIVE

    def advance(self):
        """
        Set the state to the new computed state -- computed in step().
        """
        self.state = self._nextState
```


```python
# model
from mesa import Model
from mesa.time import SimultaneousActivation
from mesa.space import Grid
import numpy as np

#from cell import Cell
from mesa.datacollection import DataCollector

def clustering(model):
    from sklearn.cluster import DBSCAN
    
    cell_state=[agent.state for agent in model.schedule.agents]
    width,height=model.grid.width,model.grid.width
    cell_state_array=np.array(cell_state).reshape(width,height)
    w_coords,h_coords=np.meshgrid(range(width),range(height),indexing='ij')
    cell_coords=np.array([w_coords,h_coords]).T
    cell_coords_flatten=cell_coords.reshape(-1,2)
    cell_coords_flatten_1=cell_coords_flatten[list(map(bool,cell_state))]
    #print(cell_coords_flatten_1)
    #print(cell_coords_flatten_1.shape)       
    
    db=DBSCAN(eps=5, min_samples=10).fit(cell_coords_flatten_1)
    labels=db.labels_    
    #print(np.unique(labels))
    
    #labels_dic=dict(zip(cell_coords_flatten_1.tolist(),labels))
    labels_array=np.full((width,height),-2)
    count=0
    for i,j in cell_coords_flatten_1:
        #print(i,j)
        labels_array[i,j]=labels[count]
        count+=1
        
    #print(np.unique(labels_array))
    #print("+"*50)          
    return labels_array

class ConwaysGameOfLife(Model):
    """
    Represents the 2-dimensional array of cells in Conway's
    Game of Life.
    """

    def __init__(self, height=50*2, width=50*2):
        """
        Create a new playing area of (height, width) cells.
        """

        # Set up the grid and schedule.

        # Use SimultaneousActivation which simulates all the cells
        # computing their next state simultaneously.  This needs to
        # be done because each cell's next state depends on the current
        # state of all its neighbors -- before they've changed.
        self.schedule = SimultaneousActivation(self)

        # Use a simple grid, where edges wrap around.
        self.grid = Grid(height, width, torus=True)
        
        #datacollector
        self.datacollector=DataCollector(
            model_reporters={"clustering": clustering},
            agent_reporters={"state": "state"}
            )

        # Place a cell at each location, with some initialized to
        # ALIVE and some to DEAD.
        for (contents, x, y) in self.grid.coord_iter():
            cell = Cell((x, y), self)
            if self.random.random() < 0.4:
                cell.state = cell.ALIVE
            self.grid.place_agent(cell, (x, y))
            self.schedule.add(cell)

        self.running = True

    def step(self):
        """
        Have the scheduler advance each cell by one step
        """
        # collect data
        self.datacollector.collect(self)     
        
        self.schedule.step()
```


```python
clustering_=model.datacollector.get_model_vars_dataframe()
#print(clustering_["clustering"])
#print(clustering_["clustering"][2])
clustering_current=clustering_["clustering"][len(clustering_["clustering"])-1]
print(clustering_current.shape)

width,height=model.grid.width,model.grid.width
w_coords,h_coords=np.meshgrid(range(width),range(height),indexing='ij')
x=np.arange(width + 1)
y=np.arange(height + 1)

fig, ax = plt.subplots(figsize=(10,10))
ax.pcolormesh(x, y, clustering_current, shading='flat',cmap='tab20b', vmin=clustering_current.min(), vmax=clustering_current.max())   

#print("_"*50)
#state=model.datacollector.get_agent_vars_dataframe()
#print(state["state"][0])
```
    
<a href=""><img src="./imgs_pyd/054.png" height="auto" width="auto" title="digit-x"></a>


```python
import matplotlib.pyplot as plt
from IPython.display import HTML
import matplotlib.animation

model=ConwaysGameOfLife(100,100)  

def model_step(steps=10):
    for i in range(steps):    
        model.step()
        clustering_=model.datacollector.get_model_vars_dataframe()
        clustering_current=clustering_["clustering"][len(clustering_["clustering"])-1] 
        #return clustering_current
        yield clustering_current
    

width,height=model.grid.width,model.grid.width
x=np.arange(width + 1)
y=np.arange(height + 1)

fig, ax=plt.subplots(figsize=(10,10))   
clustering_current=np.full((width,height),-2)
cluster_mesh=ax.pcolormesh(x, y, clustering_current, shading='flat',cmap='tab20b', vmin=-2, vmax=10)

def update(clustering_current):
    cluster_mesh.set_array(clustering_current.ravel())
    return cluster_mesh
    
anima=matplotlib.animation.FuncAnimation(fig, update, frames=model_step(steps=100), blit=False, interval=100)   
HTML(anima.to_html5_video())        
```

    
<a href=""><img src="./imgs_pyd/CA_mesa.gif" height="auto" width="auto" title="digit-x"></a>
    



```python
#保存动画为.gif文件
from matplotlib.animation import FuncAnimation, PillowWriter 
writer=PillowWriter(fps=25) 
anima.save(r"./imgs/CA_mesa.gif", writer=writer)
```

## 3. 调研路径与图像
### 3.1 Exif(Exchangeable image file format) 可交换图像格式
Exif是专门为数码相机相片设定的档案格式，可以记录照片的属性和拍摄信息。当前用手机拍摄的照片根据设置通常都包含Exif信息，其中也可能包括GPS位置信息。Exif包括哪些信息内容，可以通过from PIL.ExifTags import TAGS调入TAGS，打印查看，其中用于数据分析相对比较关键的一些信息包括拍摄的时间，图像大小，GPS位置数据和记录时间等。

```
conda update --all
conda install -c anaconda pillow 
```

GPS信息的记录格式有可能不同，例如下述获取的'GPSInfo'中，2: (41.0, 52.0, 55.38)，但是也有可能为((19, 1), (31, 1), (5139, 100))格式。注意，自定义的Exif数据提取的函数，仅实现了第一种情况。


```python
def img_exif_info(img_fp,printing=True):
    from PIL import Image
    from PIL.ExifTags import TAGS
    from datetime import datetime
    import re,time
    '''
    function - 提取数码照片的属性信息和拍摄数据，即可交换图像文件格式（Exchangeable image file format，Exif）
    
    Paras:
    img_fn - 一个图像的文件路径
    '''
    
    img=Image.open(img_fp,)
    try:
        img_exif=img.getexif()
        exif_={TAGS[k]: v for k, v in img_exif.items() if k in TAGS}  
        #print(exif_)
        #由2017:07:20 09:16:58格式时间，转换为时间戳，用于比较时间先后。
        time_lst=[int(i) for i in re.split(' |:',exif_['DateTimeOriginal'])]
        time_tuple=datetime.timetuple(datetime(time_lst[0], time_lst[1], time_lst[2], time_lst[3], time_lst[4], time_lst[5],))
        time_stamp=time.mktime(time_tuple)
        exif_["timestamp"]=time_stamp
        
    except ValueError:
        print("exif not found!")
    for tag_id in img_exif: #提取Exif信息 iterating over all EXIF data fields
        tag=TAGS.get(tag_id,tag_id) #获取标签名 get the tag name, instead of human unreadable tag id
        data=img_exif.get(tag_id)
        if isinstance(data,bytes): #解码 decode bytes 
            try:
                data=data.decode()
            except ValueError:
                data="tag:%s data not found."%tag
        if printing:   
            print(f"{tag:30}:{data}")

    #将度转换为浮点数，Decimal Degrees = Degrees + minutes/60 + seconds/3600
    if 'GPSInfo' in exif_:   
        GPSInfo=exif_['GPSInfo']
        geo_coodinate={
            "GPSLatitude":float(GPSInfo[2][0]+GPSInfo[2][1]/60+GPSInfo[2][2]/3600),
            "GPSLongitude":float(GPSInfo[4][0]+GPSInfo[4][1]/60+GPSInfo[4][2]/3600),
            "GPSAltitude":GPSInfo[6],
            "GPSTimeStamp_str":"%d:%f:%f"%(GPSInfo[7][0],GPSInfo[7][1]/10,GPSInfo[7][2]/100),#字符形式
            "GPSTimeStamp":float(GPSInfo[7][0]+GPSInfo[7][1]/10+GPSInfo[7][2]/100),#浮点形式
            "GPSImgDirection":GPSInfo[17],
            "GPSDestBearing":GPSInfo[24],
            "GPSDateStamp":GPSInfo[29],
            "GPSHPositioningError":GPSInfo[31],            
        }    
        if printing: 
            print("_"*50)
            print(geo_coodinate)
        return exif_,geo_coodinate
    else:
        return exif_

import os, util_pyd
img_ChicagoDowntown_root=r'./data/imgs_ChicagoDowntown'
img_example_2=os.path.join(img_ChicagoDowntown_root,r'2019-10-11_120110.jpg')
img_exif_2,geo_coodinate=img_exif_info(img_example_2,)  
```

    ExifVersion                   :0221
    ComponentsConfiguration       :
    ShutterSpeedValue             :6.909027361693629
    DateTimeOriginal              :2019:10:11 12:01:11
    DateTimeDigitized             :2019:10:11 12:01:11
    ApertureValue                 :1.6959938128383605
    BrightnessValue               :5.888149338229669
    ExposureBiasValue             :0.0
    MeteringMode                  :5
    Flash                         :24
    FocalLength                   :4.0
    ColorSpace                    :65535
    ExifImageWidth                :4032
    FocalLengthIn35mmFilm         :28
    SceneCaptureType              :0
    Make                          :Apple
    ExifImageHeight               :3024
    SubsecTimeOriginal            :201
    SubsecTimeDigitized           :201
    Model                         :iPhone X
    SubjectLocation               :(2015, 1511, 2217, 1330)
    Orientation                   :1
    YCbCrPositioning              :1
    SensingMethod                 :2
    ExposureTime                  :0.008333333333333333
    XResolution                   :72.0
    YResolution                   :72.0
    FNumber                       :1.8
    SceneType                     :
    ExposureProgram               :2
    GPSInfo                       :{1: 'N', 2: (41.0, 52.0, 55.38), 3: 'W', 4: (87.0, 37.0, 26.43), 5: b'\x00', 6: 182.35323716873532, 7: (17.0, 1.0, 9.99), 12: 'K', 13: 0.0, 16: 'T', 17: 177.5288773523686, 23: 'T', 24: 177.5288773523686, 29: '2019:10:11', 31: 10.0}
    CustomRendered                :2
    ISOSpeedRatings               :25
    ResolutionUnit                :2
    ExposureMode                  :0
    FlashPixVersion               :0100
    WhiteBalance                  :0
    Software                      :https://heic.online
    LensSpecification             :(4.0, 6.0, 1.8, 2.4)
    LensMake                      :Apple
    LensModel                     :iPhone X back dual camera 4mm f/1.8
    DateTime                      :2019:10:11 12:01:11
    ExifOffset                    :218
    MakerNote                     :tag:MakerNote data not found.
    __________________________________________________
    {'GPSLatitude': 41.88205, 'GPSLongitude': 87.62400833333334, 'GPSAltitude': 182.35323716873532, 'GPSTimeStamp_str': '17:0.100000:0.099900', 'GPSTimeStamp': 17.1999, 'GPSImgDirection': 177.5288773523686, 'GPSDestBearing': 177.5288773523686, 'GPSDateStamp': '2019:10:11', 'GPSHPositioningError': 10.0}
    
    * 排布显示芝加哥市中心调研图像


```python
def imgs_layoutShow(imgs_root,imgsFn_lst,columns,scale,figsize=(15,10)):
    import math,os
    import matplotlib.pyplot as plt
    from PIL import Image
    '''
    function - 显示一个文件夹下所有图片，便于查看。
    
    Paras:
    imgs_root - 图像所在根目录
    imgsFn_lst - 图像名列表
    columns - 列数
    '''
    rows=math.ceil(len(imgsFn_lst)/columns)
    fig,axes=plt.subplots(rows,columns,sharex=True,sharey=True,figsize=figsize)   #布局多个子图，每个子图显示一幅图像
    ax=axes.flatten()  #降至1维，便于循环操作子图
    for i in range(len(imgsFn_lst)):
        img_path=os.path.join(imgs_root,imgsFn_lst[i]) #获取图像的路径
        img_array=Image.open(img_path) #读取图像为数组，值为RGB格式0-255        
        img_resize=img_array.resize([int(scale * s) for s in img_array.size] ) #传入图像的数组，调整图片大小
        ax[i].imshow(img_resize)  #显示图像
        ax[i].set_title(i+1)
    fig.tight_layout() #自动调整子图参数，使之填充整个图像区域  
    fig.suptitle("images show",fontsize=14,fontweight='bold',y=1.02)
    plt.show()
```


```python
imgs_ChicagoDowntown_fn=util_pyd.filePath_extraction(r'./data/imgs_ChicagoDowntown',["jpg"]) 
imgs_ChicagoDowntown_root=list(imgs_ChicagoDowntown_fn.keys())[0]
imgsFn_ChicagoDowntown_lst=imgs_ChicagoDowntown_fn[imgs_ChicagoDowntown_root]
columns=6
scale=0.2
imgs_layoutShow(imgs_ChicagoDowntown_root,imgsFn_ChicagoDowntown_lst,columns,scale,figsize=(15,15))
```

<a href=""><img src="./imgs_pyd/055.png" height="auto" width="auto" title="digit-x"></a>
    


在Exif信息提取函数中，根据'DateTimeOriginal201707:20 091658'时间信息，计算时间戳用于图像按照拍摄时间排序，则可以绘制图片拍摄的行走路径。此次绘制使用Plotly库提供的go方法实现。该方法在调用地图时，不需提供mapbox地图数据的访问许可（access token）。


```python
imgs_ChicagoDowntown_coordi=[]
for fn in imgsFn_ChicagoDowntown_lst:
    img_exif_2,geo_coodinate=img_exif_info(os.path.join(imgs_ChicagoDowntown_root,fn),printing=False) 
    imgs_ChicagoDowntown_coordi.append((geo_coodinate['GPSLatitude'],-geo_coodinate['GPSLongitude'],geo_coodinate['GPSAltitude'],img_exif_2['timestamp'])) #注意经度负号

import pandas as pd
pd.set_option('display.float_format', lambda x: '%.5f' % x)
import plotly.graph_objects as go
imgs_ChicagoDowntown_coordi_df=pd.DataFrame(data=imgs_ChicagoDowntown_coordi,columns=["lat","lon","elevation",'timestamp']).sort_values(by=['timestamp']) #按图片拍摄时间戳排序

fig=go.Figure(go.Scattermapbox(mode = "markers+lines",lat=imgs_ChicagoDowntown_coordi_df.lat, lon=imgs_ChicagoDowntown_coordi_df.lon,marker = {'size': 10})) #亦可以选择列，通过size=""配置增加显示信息
fig.update_layout(
    margin ={'l':0,'t':0,'b':0,'r':0},
    mapbox = {
        'center': {'lon': 10, 'lat': 10},
        'style': "stamen-terrain",
        'center': {'lon': -87.62401, 'lat': 41.88205},
        'zoom': 14})
#fig.add_trace()

fig.show()
```

<a href=""><img src="./imgs_pyd/048.jpg" height="auto" width="auto" title="digit-x"></a>


### 3.2 RGB色彩的三维图示
包含色彩信息（RGB）的数据投射到三维空间中，可以通过判断区域色彩在三维空间域中的分布情况来把握Red、Green 和Blue 色彩分量的变化情况。


```python
def imgs_colorSpace(imgs_root,imgsFn_lst,columns,scale,figsize=(15,10)):
    import math,os
    import matplotlib.pyplot as plt
    from PIL import Image
    import numpy as np
    from tqdm import tqdm
    '''
    function - 将像素RGB颜色投射到色彩空间中，直观感受图像颜色的分布。
    
    Paras:
    imgs_root - 图像所在根目录
    imgsFn_lst - 图像名列表
    columns - 列数
    '''
    rows=math.ceil(len(imgsFn_lst)/columns)
    fig=plt.figure()    
    for i in tqdm(range(len(imgsFn_lst))):
        ax=fig.add_subplot(rows,columns,i+1, projection='3d')  #不断增加子图，并设置投影为3d模式，可以显示三维坐标空间 
        img=os.path.join(imgs_root,imgsFn_lst[i])
        img_path=os.path.join(imgs_root,imgsFn_lst[i]) #获取图像的路径
        img_array=Image.open(img_path) #读取图像为数组，值为RGB格式0-255        
        img_resize=img_array.resize([int(scale * s) for s in img_array.size] ) #传入图像的数组，调整图片大小
        img_array=np.asarray(img_resize)
        ax.scatter(img_array[:,:,0],img_array[:,:,1],img_array[:,:,2], c=(img_array/255).reshape(-1,3), marker='+',s=0.1) #用RGB的三个分量值作为颜色的空间坐标，并显示其颜色。设置颜色时，需要将0-255缩放至0-1区间
        ax.set_xlabel('r',labelpad=1)
        ax.set_ylabel('g')
        ax.set_zlabel('b',labelpad=2)
        ax.set_title(i+1)
        fig.set_figheight(figsize[0])
        fig.set_figwidth(figsize[1])
    print("Ready to show...")
    fig.tight_layout()
    plt.show() 
    
imgs_fn=util_pyd.filePath_extraction(r'./data/default_20170720081441',["jpg"]) 
imgs_root=list(imgs_fn.keys())[0]
imgsFn_lst=imgs_fn[imgs_root]
columns=6
scale=0.05   
imgs_colorSpace(imgs_root,imgsFn_lst,columns,scale,figsize=(20,20))
```

<a href=""><img src="./imgs_pyd/049.png" height="auto" width="auto" title="digit-x"></a>


### 3.3 聚类图像主题色
城市色彩，也称“城市环境色彩”，泛指城市各个构成要素公共空间部分所呈现出 的色彩面貌总和。城市色彩包含大量复杂多变的元素，因此必须科学地调查与分析，才能实现有效引导和规划其发展。在城市色彩相关的研究上，主要有：分析城市色彩特征，调查与定量分析，更新与保护机制研究；景观环境色彩构成，以及利用 MATLAB 计算插值与应用回归算法，实现城市色彩主色调意向图的自动填充，得到城市色彩主色调理想色彩地图的研究。部分传统的研究由于受到数据分析技术的限制，对于批量的城市影像提取主题色时偏重手工提取，不仅增加时间成本，也影响分析的精度，同时在数据分析方法和数据信息表达上亦受到限制。因此有必要借助机器学习中的聚类等方法，自主聚类主题色，并通过Python的Matplotlib标准库实现数据增强表达。

首先在Python程序语言中批量读取图像，因为所拍摄的图像分辨率较高，而色彩分析不需要这样的高精度，因此通过压缩图像降 低图像大小来节约分析的时间。然后设置色彩主题色聚类的数量为7，即获取每幅图像的7个主题色。采用KMeans聚类算法分类色彩提取主题色，提取所有图像的主题色之后汇总于一个数组中。在数据增强可视化方面设计了散点形式打印主题色，直观反映城市色彩印象。通过城市主题色的提取、色彩印象感官的呈现来研究城市色彩，可以针对不同的城市空间、不同的调研时间，分析色彩的变化。

聚类部分，参考了上述Sklearn提供的'Comparing different clustering algorithms on toy datasets'代码，从下面代码中也可以观察到代码迁移的痕迹。


```python
import util_pyd

def img_rescale(img_path,scale):
    from PIL import Image
    import numpy as np
    '''
    function - 读取与压缩图像，返回2维度数组
    
    Paras:
    imgsPath_lst - 待处理图像列表
    '''
    img=Image.open(img_path) #读取图像为数组，值为RGB格式0-255  
    img_resize=img.resize([int(scale * s) for s in img.size] ) #传入图像的数组，调整图片大小
    img_3d=np.array(img_resize)
    h, w, d=img_3d.shape
    img_2d=np.reshape(img_3d, (h*w, d))  #调整数组形状为2维

    return img_3d,img_2d
    

def img_theme_color(imgs_root,imgsFn_lst,columns,scale,):   
    import os,time,warnings
    import numpy as np
    import matplotlib.pyplot as plt
    from tqdm import tqdm
    from sklearn.preprocessing import StandardScaler
    from sklearn import cluster, datasets, mixture
    from itertools import cycle, islice
    
    '''
    function - 聚类的方法提取图像主题色，并打印图像、聚类预测类的二维显示和主题色带
    
    Paras:
    imgs_root - 图像所在根目录
    imgsFn_lst - 图像名列表
    columns - 列数    
    '''
    #设置聚类参数，本实验中仅使用了KMeans算法，其它算法可以自行尝试
    kmeans_paras={'quantile': .3,
                  'eps': .3,
                  'damping': .9,
                  'preference': -200,
                  'n_neighbors': 10,
                  'n_clusters': 7}     
        
    imgsPath_lst=[os.path.join(imgs_root,p) for p in imgsFn_lst]
    imgs_rescale=[(img_rescale(img,scale)) for img in imgsPath_lst]  
    datasets=[((i[1],None),{}) for i in imgs_rescale] #基于img_2d的图像数据，用于聚类计算
    img_lst=[i[0] for i in imgs_rescale]  #基于img_3d的图像数据，用于图像显示
    
    themes=np.zeros((kmeans_paras['n_clusters'], 3))  #建立0占位的数组，用于后面主题数据的追加。'n_clusters'为提取主题色的聚类数量，此处为7，轴2为3，是色彩的RGB数值
    (img_3d,img_2d)=imgs_rescale[0]  #可以1次性提取元组索引值相同的值，img就是img_3d，而pix是img_2d
    img2d_V,img2d_H=img_2d.shape  #获取img_2d数据的形状，用于pred预测初始数组的建立
    pred=np.zeros((img2d_V))  #建立0占位的pred预测数组，用于后面预测结果数据的追加，即图像中每一个像素点属于设置的7个聚类中的哪一组，预测给定类标
    
    plt.figure(figsize=(6*3+3, len(imgsPath_lst)*2))  #图表大小的设置，根据图像的数量来设置高度，宽度为3组9个子图，每组包括图像、预测值散点图和主题色
    plt.subplots_adjust(left=.02, right=.98, bottom=.001, top=.96, wspace=.3,hspace=.3)  #调整图，避免横纵向坐标重叠    
    subplot_num=1  #子图的计数  
    
    for i_dataset, (dataset, algo_params) in tqdm(enumerate(datasets)):  #循环pixData数据，即待预测的每个图像数据。enumerate()函数将可迭代对象组成一个索引序列，可以同时获取索引和值，其中i_dataset为索引，从整数0开始
        X, y=dataset  #用于机器学习的数据一般包括特征值和类标，此次实验为无监督分类的聚类实验，没有类标，并将其在前文中设置为None对象
        Xstd=StandardScaler().fit_transform(X)  #标准化数据仅用于二维图表的散点，可视化预测值，而不用于聚类，聚类数据保持色彩的0-255值范围
        #此次实验使用KMeans算法，参数为'n_clusters'一项。不同算法计算效率不同，例如MiniBatchKMeans和KMeans算法计算较快
        km=cluster.KMeans(n_clusters=kmeans_paras['n_clusters'])
        clustering_algorithms=(('KMeans',km),)
        for name, algorithm in clustering_algorithms: 
            t0=time.time()  
            #警告错误，使用warning库
            with warnings.catch_warnings():
                warnings.filterwarnings(
                    "ignore",
                    message="the number of connected components of the " +"connectivity matrix is [0-9]{1,2}" +" > 1. Completing it to avoid stopping the tree early.",
                    category=UserWarning)
                warnings.filterwarnings(
                    "ignore",
                    message="Graph is not fully connected, spectral embedding" +" may not work as expected.",
                    category=UserWarning)
                algorithm.fit(X)  #通过fit函数执行聚类算法            
        
            quantize=np.array(algorithm.cluster_centers_, dtype=np.uint8) #返回聚类的中心，为主题色
            themes=np.vstack((themes,quantize))  #将计算获取的每一图像主题色追加到themes数组中
            t1=time.time()  #计算聚类算法所需时间
            '''获取预测值/分类类标'''   
            if hasattr(algorithm, 'labels_'):
                y_pred=algorithm.labels_.astype(np.int32)
            else:
                y_pred=algorithm.predict(X)  
            pred=np.hstack((pred,y_pred))  #将计算获取的每一图像聚类预测结果追加到pred数组中
            fig_width=(len(clustering_algorithms)+2)*3  #水平向子图数
            plt.subplot(len(datasets), fig_width,subplot_num)
            plt.imshow(img_lst[i_dataset])  #图像显示子图
            
            plt.subplot(len(datasets),fig_width, subplot_num+1)
            if i_dataset == 0:
                plt.title(name, size=18)
            colors = np.array(list(islice(cycle(['#377eb8', '#ff7f00', '#4daf4a','#f781bf', '#a65628', '#984ea3','#999999', '#e41a1c', '#dede00']),int(max(y_pred) + 1))))  #设置预测类标分类颜色
            plt.scatter(Xstd[:, 0], Xstd[:, 1], s=10, color=colors[y_pred]) #预测类标子图
            plt.xlim(-2.5, 2.5)
            plt.ylim(-2.5, 2.5)
            plt.xticks(())
            plt.yticks(())
            plt.text(.99, .01, ('%.2fs' % (t1 - t0)).lstrip('0'),transform=plt.gca().transAxes, size=15,horizontalalignment='right')  #子图中显示聚类计算时间长度，     
            #图像主题色子图参数配置
            plt.subplot(len(datasets), fig_width,subplot_num+2)
            t=1
            pale=np.zeros(img_lst[i_dataset].shape, dtype=np.uint8)
            h, w,_=pale.shape
            ph=h/len(quantize)
            for y in range(h):
                pale[y,::] = np.array(quantize[int(y/ph)], dtype=np.uint8)
            plt.imshow(pale)    
            t+=1  
            subplot_num+=3    
    plt.show()            
    return themes,pred
    
imgs_fn=util_pyd.filePath_extraction(r'./data/default_20170720081441',["jpg"]) 
imgs_root=list(imgs_fn.keys())[0]
imgsFn_lst=imgs_fn[imgs_root]
columns=6
scale=0.2
themes,pred=img_theme_color(imgs_root,imgsFn_lst[:6],columns,scale,)  
```

    6it [01:04, 10.73s/it]
    

<a href=""><img src="./imgs_pyd/057.png" height="auto" width="auto" title="digit-x"></a>


```python
def themeColor_impression(theme_color):
    from sklearn import datasets
    from numpy.random import rand
    import matplotlib.pyplot as plt
    '''
    function - 显示所有图像主题色，获取总体印象
    
    Paras:
    theme_color - 主题色数组
    '''
    n_samples=theme_color.shape[0]
    random_state=170  #可为默认，不设置该参数，获得随机图形
    #利用scikit的datasets数据集构建有差异变化的斑点
    varied=datasets.make_blobs(n_samples=n_samples,cluster_std=[1.0, 2.5, 0.5],random_state=random_state)
    (x,y)=varied    
    fig, ax=plt.subplots(figsize=(10,10))
    scale=1000.0*rand(n_samples)  #设置斑点随机大小
    ax.scatter(x[...,0], x[...,1], c=themes/255,s=scale,alpha=0.7, edgecolors='none')  #将主题色赋予斑点
    ax.grid(True)       
    plt.show()   
themeColor_impression(themes)    
```

<a href=""><img src="./imgs_pyd/056.jpg" height="auto" width="auto" title="digit-x"></a>


```python
def save_as_json(array,save_root,fn):
    import time,os,json
    '''
    function - 保存文件,将文件存储为json数据格式
    
    Paras:
    array - 待保存的数组
    save_root - 文件保存的根目录 
    fn - 保存的文件名
    '''
    json_file=open(os.path.join(save_root,r'%s_'%fn+str(time.time()))+'.json','w')
    json.dump(array.tolist(),json_file)  #将numpy数组转换为列表后存储为json数据格式
    json_file.close()
    
save_root=r'./data'    
save_as_json(themes,save_root,'themes')   
save_as_json(pred,save_root,'themes_pred') 
```

> 第9次课-2021_11_17_Wen_7-8节 :2课时 课程完成时间标识线。

---

<a href=""><img src="./icon/ant_07.png" height="auto" width="200" title="caDesign"></a>













