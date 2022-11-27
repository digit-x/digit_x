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