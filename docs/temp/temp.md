$ sudo apt update
1. apt-get update
2. apt-get upgrade

sudo apt install python3-pip	

$ sudo apt install nvidia-cuda-toolkit
$ nvcc --version

> sudo apt-get -y install cmake
> which cmake
/usr/bin/cmake
> cmake --version
cmake version 2.8.12.2


rm -rf directory
rm file

cat /etc/os-release



curl -O https://repo.anaconda.com/archive/Anaconda3-2020.07-Linux-x86_64.sh
bash ~/Downloads/Anaconda3-2020.02-Linux-x86_64.sh
source ~/.bashrc

conda create --name py37 python=3.7  
conda install --name py37 spyder -c conda-forge
conda activate py37
spyder

conda create -n myenv python=3.6
conda env list
conda info --envs
conda env list

conda activate your_env_name
conda deactivate

pip3 install tensorflow-gpu
#conda create -n tf-gpu tensorflow-gpu
#conda activate tf-gpu


$ sudo apt-get remove package-name


conda install --name cloudPts spyder -c conda-forge

git clone --recurse-submodules https://github.com/intel-isl/Open3D-PointNet2-Semantic3D

$ python -c "import torch; print(torch.__version__)"
>>> 1.6.0
$ python -c "import torch; print(torch.version.cuda)"
>>> 10.2


$ pip install torch-scatter==latest+cu102 -f https://pytorch-geometric.com/whl/torch-1.6.0.html
$ pip install torch-sparse==latest+cu102 -f https://pytorch-geometric.com/whl/torch-1.6.0.html
$ pip install torch-cluster==latest+cu102 -f https://pytorch-geometric.com/whl/torch-1.6.0.html
$ pip install torch-spline-conv==latest+cu102 -f https://pytorch-geometric.com/whl/torch-1.6.0.html
$ pip install torch-geometric

（源）参数化设计实践-清华城市规划设计研究院（清华同衡） 2009 - 2010.07            :done,    des1, 2009-01,2010-07

            * [IUR_1.4 CATERPILLAR 参数化编程设计模组-IDEAS](https://richiebao.github.io/parametric_design_coding_module)



 to deprecate:

 Detectron2 Beginner's Tutorial : https://colab.research.google.com/drive/16jcaJoc6bCFAQ96jDe2HwtXj7BMD_-m5#scrollTo=QHnVupBBn9eR

https://pytorch.org/ecosystem/ 


C 项目实践
报告人的参数化设计探索历程
o 踩过的坑（人生苦短）
你我以为的参数化设计领域是否相同？

interrobang Microsoft 365究竟是为谁服务的？为格式（样式）困扰值得吗？和做笔记的重要性

做笔记的目的：
快速记录，不用过度关心样式格式；
代码格式高亮显示，具有易读性；
方便文件管理，可有序无限扩展；
易于搜索，快速定位搜索主题；
可以发布在线，在线更新知识积累，随时随地查看及分享；
方便不同平台、格式转换，具有广泛的弹性，易于迁移；
自主性。
轻量级标记语言	解释器	集成式	在线版
digit-x
markdown	digit-x
Visual Studio Code（VSC）	digit-x
jupyterlab	digit-x Colab
digit-x kaggle
interrobang 做了那么多设计，写了那么多逻辑代码，为什么新的设计任务还要重新思考写逻辑？

解决的方法⟶建立模组库|包+案例的必要性 🠊 拥有自行构建可不断扩展的工具库（代码库、模组库、算法库）——不一样的设计库
对于GH，建立模组；
digit-x digit-x
对于Python，建立包。
例如：城市空间数据分析方法-USDA 库手册

interrobang 你在做别人已完成的工作，为什么要从0开始及必须从0开始？

避免重复造轮子！1个地球，1个世界！

互联网改变了很多事，这尤其在代码世界，无以计数的研究成果很多都已代码形式存在，让研究成果/实验更容易再现分享，甚至研究的过程就是代码书写的过程（工具构建的过程）。这非常容易的研究实验再现方式，和无以计数集“世界之力”的数据处理方法，都加快了知识的迭代更新。

已有扩展或库不稳定时，需要自行写代码。...

interrobang 什么都需要藏着掖着吗？版权与分享