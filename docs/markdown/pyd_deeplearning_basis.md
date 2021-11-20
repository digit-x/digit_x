> Created on Sat Nov 20 10/04/50 2021 @author: Richie Bao-python_code_archi_la_design_method_study 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# 深度学习(pytorch)——基础，梯度+多层感知机

## 1. [PyTorch](https://pytorch.org/)的[Tutorials](https://pytorch.org/tutorials/beginner/basics/intro.html)-[quickstart](https://pytorch.org/tutorials/beginner/basics/quickstart_tutorial.html)

### 1.1 Working with data

* 调入库


```python
import torch
from torch import nn
from torch.utils.data import DataLoader
from torchvision import datasets
from torchvision.transforms import ToTensor, Lambda, Compose
import matplotlib.pyplot as plt
```

* 从数据集（datasets）中下载数据（FashionMNIST）


```python
# Download training data from open datasets.
training_data = datasets.FashionMNIST(
    root="data",
    train=True,
    download=True,
    transform=ToTensor(),
)

# Download test data from open datasets.
test_data = datasets.FashionMNIST(
    root="data",
    train=False,
    download=True,
    transform=ToTensor(),
)
```

    Downloading http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/train-images-idx3-ubyte.gz
    Downloading http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/train-images-idx3-ubyte.gz to data\FashionMNIST\raw\train-images-idx3-ubyte.gz
    


      0%|          | 0/26421880 [00:00<?, ?it/s]


    Extracting data\FashionMNIST\raw\train-images-idx3-ubyte.gz to data\FashionMNIST\raw
    
    Downloading http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/train-labels-idx1-ubyte.gz
    Downloading http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/train-labels-idx1-ubyte.gz to data\FashionMNIST\raw\train-labels-idx1-ubyte.gz
    


      0%|          | 0/29515 [00:00<?, ?it/s]


    Extracting data\FashionMNIST\raw\train-labels-idx1-ubyte.gz to data\FashionMNIST\raw
    
    Downloading http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/t10k-images-idx3-ubyte.gz
    Downloading http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/t10k-images-idx3-ubyte.gz to data\FashionMNIST\raw\t10k-images-idx3-ubyte.gz
    


      0%|          | 0/4422102 [00:00<?, ?it/s]


    Extracting data\FashionMNIST\raw\t10k-images-idx3-ubyte.gz to data\FashionMNIST\raw
    
    Downloading http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/t10k-labels-idx1-ubyte.gz
    Downloading http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/t10k-labels-idx1-ubyte.gz to data\FashionMNIST\raw\t10k-labels-idx1-ubyte.gz
    


      0%|          | 0/5148 [00:00<?, ?it/s]


    Extracting data\FashionMNIST\raw\t10k-labels-idx1-ubyte.gz to data\FashionMNIST\raw
    
    


```python
print(training_data)
print("_"*50)
print(test_data)
```

    Dataset FashionMNIST
        Number of datapoints: 60000
        Root location: data
        Split: Train
        StandardTransform
    Transform: ToTensor()
    __________________________________________________
    Dataset FashionMNIST
        Number of datapoints: 10000
        Root location: data
        Split: Test
        StandardTransform
    Transform: ToTensor()
    

* 给定批量大小（batch_size）加载数据


```python
batch_size = 64

# Create data loaders.
train_dataloader = DataLoader(training_data, batch_size=batch_size)
test_dataloader = DataLoader(test_data, batch_size=batch_size)

for X, y in test_dataloader:
    print("Shape of X [N, C, H, W]: ", X.shape)
    print("Shape of y: ", y.shape, y.dtype)
    break
```

    Shape of X [N, C, H, W]:  torch.Size([64, 1, 28, 28])
    Shape of y:  torch.Size([64]) torch.int64
    

* 打印数据查看


```python
image, label=[x[0] for x in iter(train_dataloader).next()]
print(image)
print("_"*50)
print(label)
```

    tensor([[[0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0039, 0.0000, 0.0000, 0.0510,
              0.2863, 0.0000, 0.0000, 0.0039, 0.0157, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0039, 0.0039, 0.0000],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0118, 0.0000, 0.1412, 0.5333,
              0.4980, 0.2431, 0.2118, 0.0000, 0.0000, 0.0000, 0.0039, 0.0118,
              0.0157, 0.0000, 0.0000, 0.0118],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0235, 0.0000, 0.4000, 0.8000,
              0.6902, 0.5255, 0.5647, 0.4824, 0.0902, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0471, 0.0392, 0.0000],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.6078, 0.9255,
              0.8118, 0.6980, 0.4196, 0.6118, 0.6314, 0.4275, 0.2510, 0.0902,
              0.3020, 0.5098, 0.2824, 0.0588],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0039, 0.0000, 0.2706, 0.8118, 0.8745,
              0.8549, 0.8471, 0.8471, 0.6392, 0.4980, 0.4745, 0.4784, 0.5725,
              0.5529, 0.3451, 0.6745, 0.2588],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0039, 0.0039, 0.0039, 0.0000, 0.7843, 0.9098, 0.9098,
              0.9137, 0.8980, 0.8745, 0.8745, 0.8431, 0.8353, 0.6431, 0.4980,
              0.4824, 0.7686, 0.8980, 0.0000],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.7176, 0.8824, 0.8471,
              0.8745, 0.8941, 0.9216, 0.8902, 0.8784, 0.8706, 0.8784, 0.8667,
              0.8745, 0.9608, 0.6784, 0.0000],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.7569, 0.8941, 0.8549,
              0.8353, 0.7765, 0.7059, 0.8314, 0.8235, 0.8275, 0.8353, 0.8745,
              0.8627, 0.9529, 0.7922, 0.0000],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0039, 0.0118, 0.0000, 0.0471, 0.8588, 0.8627, 0.8314,
              0.8549, 0.7529, 0.6627, 0.8902, 0.8157, 0.8549, 0.8784, 0.8314,
              0.8863, 0.7725, 0.8196, 0.2039],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0235, 0.0000, 0.3882, 0.9569, 0.8706, 0.8627,
              0.8549, 0.7961, 0.7765, 0.8667, 0.8431, 0.8353, 0.8706, 0.8627,
              0.9608, 0.4667, 0.6549, 0.2196],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0157, 0.0000, 0.0000, 0.2157, 0.9255, 0.8941, 0.9020,
              0.8941, 0.9412, 0.9098, 0.8353, 0.8549, 0.8745, 0.9176, 0.8510,
              0.8510, 0.8196, 0.3608, 0.0000],
             [0.0000, 0.0000, 0.0039, 0.0157, 0.0235, 0.0275, 0.0078, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.9294, 0.8863, 0.8510, 0.8745,
              0.8706, 0.8588, 0.8706, 0.8667, 0.8471, 0.8745, 0.8980, 0.8431,
              0.8549, 1.0000, 0.3020, 0.0000],
             [0.0000, 0.0118, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.2431, 0.5686, 0.8000, 0.8941, 0.8118, 0.8353, 0.8667,
              0.8549, 0.8157, 0.8275, 0.8549, 0.8784, 0.8745, 0.8588, 0.8431,
              0.8784, 0.9569, 0.6235, 0.0000],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0706, 0.1725, 0.3216, 0.4196,
              0.7412, 0.8941, 0.8627, 0.8706, 0.8510, 0.8863, 0.7843, 0.8039,
              0.8275, 0.9020, 0.8784, 0.9176, 0.6902, 0.7373, 0.9804, 0.9725,
              0.9137, 0.9333, 0.8431, 0.0000],
             [0.0000, 0.2235, 0.7333, 0.8157, 0.8784, 0.8667, 0.8784, 0.8157,
              0.8000, 0.8392, 0.8157, 0.8196, 0.7843, 0.6235, 0.9608, 0.7569,
              0.8078, 0.8745, 1.0000, 1.0000, 0.8667, 0.9176, 0.8667, 0.8275,
              0.8627, 0.9098, 0.9647, 0.0000],
             [0.0118, 0.7922, 0.8941, 0.8784, 0.8667, 0.8275, 0.8275, 0.8392,
              0.8039, 0.8039, 0.8039, 0.8627, 0.9412, 0.3137, 0.5882, 1.0000,
              0.8980, 0.8667, 0.7373, 0.6039, 0.7490, 0.8235, 0.8000, 0.8196,
              0.8706, 0.8941, 0.8824, 0.0000],
             [0.3843, 0.9137, 0.7765, 0.8235, 0.8706, 0.8980, 0.8980, 0.9176,
              0.9765, 0.8627, 0.7608, 0.8431, 0.8510, 0.9451, 0.2549, 0.2863,
              0.4157, 0.4588, 0.6588, 0.8588, 0.8667, 0.8431, 0.8510, 0.8745,
              0.8745, 0.8784, 0.8980, 0.1137],
             [0.2941, 0.8000, 0.8314, 0.8000, 0.7569, 0.8039, 0.8275, 0.8824,
              0.8471, 0.7255, 0.7725, 0.8078, 0.7765, 0.8353, 0.9412, 0.7647,
              0.8902, 0.9608, 0.9373, 0.8745, 0.8549, 0.8314, 0.8196, 0.8706,
              0.8627, 0.8667, 0.9020, 0.2627],
             [0.1882, 0.7961, 0.7176, 0.7608, 0.8353, 0.7725, 0.7255, 0.7451,
              0.7608, 0.7529, 0.7922, 0.8392, 0.8588, 0.8667, 0.8627, 0.9255,
              0.8824, 0.8471, 0.7804, 0.8078, 0.7294, 0.7098, 0.6941, 0.6745,
              0.7098, 0.8039, 0.8078, 0.4510],
             [0.0000, 0.4784, 0.8588, 0.7569, 0.7020, 0.6706, 0.7176, 0.7686,
              0.8000, 0.8235, 0.8353, 0.8118, 0.8275, 0.8235, 0.7843, 0.7686,
              0.7608, 0.7490, 0.7647, 0.7490, 0.7765, 0.7529, 0.6902, 0.6118,
              0.6549, 0.6941, 0.8235, 0.3608],
             [0.0000, 0.0000, 0.2902, 0.7412, 0.8314, 0.7490, 0.6863, 0.6745,
              0.6863, 0.7098, 0.7255, 0.7373, 0.7412, 0.7373, 0.7569, 0.7765,
              0.8000, 0.8196, 0.8235, 0.8235, 0.8275, 0.7373, 0.7373, 0.7608,
              0.7529, 0.8471, 0.6667, 0.0000],
             [0.0078, 0.0000, 0.0000, 0.0000, 0.2588, 0.7843, 0.8706, 0.9294,
              0.9373, 0.9490, 0.9647, 0.9529, 0.9569, 0.8667, 0.8627, 0.7569,
              0.7490, 0.7020, 0.7137, 0.7137, 0.7098, 0.6902, 0.6510, 0.6588,
              0.3882, 0.2275, 0.0000, 0.0000],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.1569,
              0.2392, 0.1725, 0.2824, 0.1608, 0.1373, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000],
             [0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.0000,
              0.0000, 0.0000, 0.0000, 0.0000]]])
    __________________________________________________
    tensor(9)
    


```python
import torchvision

def matplotlib_imshow(img, one_channel=False,figsize=None):
    import matplotlib.pyplot as plt
    import numpy as np    
    
    '''
    function - 定义显示图像函数（一个batch批次）
    '''
    
    if one_channel:
        img = img.mean(dim=0)
    img = img / 2 + 0.5     # unnormalize
    npimg = img.numpy()
    
    plt.figure(figsize = figsize)
    if one_channel:
        plt.imshow(npimg, cmap="Greys")
    else:
        plt.imshow(np.transpose(npimg, (1, 2, 0)))

#提取图像（数量为batch_size） get some random training images
dataiter=iter(train_dataloader)
images,labels=dataiter.next()        

#建立图像格网 create grid of images
img_grid = torchvision.utils.make_grid(images)
#显示图像 show images
matplotlib_imshow(img_grid, one_channel=True,figsize=(8,8))
```


    
<a href=""><img src="./imgs_pyd/061.png" height="auto" width="auto" title="digit-x"></a>

    


### 1.2 Creating Models


```python
# Get cpu or gpu device for training.
device = "cuda" if torch.cuda.is_available() else "cpu"
print(f"Using {device} device")

# Define model
class NeuralNetwork(nn.Module):
    def __init__(self):
        super(NeuralNetwork, self).__init__()
        self.flatten = nn.Flatten()
        self.linear_relu_stack = nn.Sequential(
            nn.Linear(28*28, 512),
            nn.ReLU(),
            nn.Linear(512, 512),
            nn.ReLU(),
            nn.Linear(512, 10)
        )

    def forward(self, x):
        x = self.flatten(x)
        logits = self.linear_relu_stack(x)
        return logits

model = NeuralNetwork().to(device)
print(model)
```

    Using cuda device
    NeuralNetwork(
      (flatten): Flatten(start_dim=1, end_dim=-1)
      (linear_relu_stack): Sequential(
        (0): Linear(in_features=784, out_features=512, bias=True)
        (1): ReLU()
        (2): Linear(in_features=512, out_features=512, bias=True)
        (3): ReLU()
        (4): Linear(in_features=512, out_features=10, bias=True)
      )
    )
    

### 1.3 Optimizing the Model Parameters


```python
loss_fn = nn.CrossEntropyLoss()
optimizer = torch.optim.SGD(model.parameters(), lr=1e-3)
```


```python
def train(dataloader, model, loss_fn, optimizer):
    size = len(dataloader.dataset)
    model.train()
    for batch, (X, y) in enumerate(dataloader):
        X, y = X.to(device), y.to(device)

        # Compute prediction error
        pred = model(X)
        loss = loss_fn(pred, y)

        # Backpropagation
        optimizer.zero_grad() #Sets the gradients of all optimized torch.Tensor s to zero.set_to_none (bool) – instead of setting to zero, set the grads to None. 
        loss.backward()
        optimizer.step()

        if batch % 100 == 0:
            loss, current = loss.item(), batch * len(X)
            print(f"loss: {loss:>7f}  [{current:>5d}/{size:>5d}]")
            
def test(dataloader, model, loss_fn):
    size = len(dataloader.dataset)
    num_batches = len(dataloader)
    model.eval()
    test_loss, correct = 0, 0
    with torch.no_grad():
        for X, y in dataloader:
            X, y = X.to(device), y.to(device)
            pred = model(X)
            test_loss += loss_fn(pred, y).item()
            correct += (pred.argmax(1) == y).type(torch.float).sum().item()
    test_loss /= num_batches
    correct /= size
    print(f"Test Error: \n Accuracy: {(100*correct):>0.1f}%, Avg loss: {test_loss:>8f} \n")            
```

### 1.4 Training and Saving Models


```python
epochs = 5
for t in range(epochs):
    print(f"Epoch {t+1}\n-------------------------------")
    train(train_dataloader, model, loss_fn, optimizer)
    test(test_dataloader, model, loss_fn)
print("Done!")
```

    Epoch 1
    -------------------------------
    loss: 2.307042  [    0/60000]
    loss: 2.288707  [ 6400/60000]
    loss: 2.266480  [12800/60000]
    loss: 2.256476  [19200/60000]
    loss: 2.238141  [25600/60000]
    loss: 2.211003  [32000/60000]
    loss: 2.216235  [38400/60000]
    loss: 2.182089  [44800/60000]
    loss: 2.178703  [51200/60000]
    loss: 2.140869  [57600/60000]
    Test Error: 
     Accuracy: 42.9%, Avg loss: 2.134296 
    
    Epoch 2
    -------------------------------
    loss: 2.152992  [    0/60000]
    loss: 2.132291  [ 6400/60000]
    loss: 2.072634  [12800/60000]
    loss: 2.089680  [19200/60000]
    loss: 2.023405  [25600/60000]
    loss: 1.972375  [32000/60000]
    loss: 1.995246  [38400/60000]
    loss: 1.912020  [44800/60000]
    loss: 1.925423  [51200/60000]
    loss: 1.846543  [57600/60000]
    Test Error: 
     Accuracy: 54.7%, Avg loss: 1.844276 
    
    Epoch 3
    -------------------------------
    loss: 1.888054  [    0/60000]
    loss: 1.845066  [ 6400/60000]
    loss: 1.734562  [12800/60000]
    loss: 1.777394  [19200/60000]
    loss: 1.650039  [25600/60000]
    loss: 1.629869  [32000/60000]
    loss: 1.640070  [38400/60000]
    loss: 1.546894  [44800/60000]
    loss: 1.579702  [51200/60000]
    loss: 1.477962  [57600/60000]
    Test Error: 
     Accuracy: 59.3%, Avg loss: 1.493717 
    
    Epoch 4
    -------------------------------
    loss: 1.571984  [    0/60000]
    loss: 1.528653  [ 6400/60000]
    loss: 1.392575  [12800/60000]
    loss: 1.457019  [19200/60000]
    loss: 1.330526  [25600/60000]
    loss: 1.349623  [32000/60000]
    loss: 1.350353  [38400/60000]
    loss: 1.276905  [44800/60000]
    loss: 1.315421  [51200/60000]
    loss: 1.225695  [57600/60000]
    Test Error: 
     Accuracy: 62.6%, Avg loss: 1.244722 
    
    Epoch 5
    -------------------------------
    loss: 1.330327  [    0/60000]
    loss: 1.304799  [ 6400/60000]
    loss: 1.149812  [12800/60000]
    loss: 1.251025  [19200/60000]
    loss: 1.120557  [25600/60000]
    loss: 1.156347  [32000/60000]
    loss: 1.170870  [38400/60000]
    loss: 1.104533  [44800/60000]
    loss: 1.148049  [51200/60000]
    loss: 1.073095  [57600/60000]
    Test Error: 
     Accuracy: 64.3%, Avg loss: 1.086625 
    
    Done!
    

* Saving models


```python
save_pth="./model/model.pth"
torch.save(model.state_dict(), save_pth)
print("Saved PyTorch Model State to model.pth")
```

    Saved PyTorch Model State to model.pth
    

### 1.5 Loading Models


```python
model = NeuralNetwork()
model.load_state_dict(torch.load(save_pth))
```




    <All keys matched successfully>




```python
classes = [
    "T-shirt/top",
    "Trouser",
    "Pullover",
    "Dress",
    "Coat",
    "Sandal",
    "Shirt",
    "Sneaker",
    "Bag",
    "Ankle boot",
]

model.eval()
x, y = test_data[0][0], test_data[0][1]
with torch.no_grad():
    pred = model(x)
    predicted, actual = classes[pred[0].argmax(0)], classes[y]
    print(f'Predicted: "{predicted}", Actual: "{actual}"')
```

    Predicted: "Ankle boot", Actual: "Ankle boot"
    


```python
import matplotlib.pyplot as plt
plt.figure(figsize =(1.5,1.5))
plt.imshow(x[0,:,:], cmap="Greys")
```




    <matplotlib.image.AxesImage at 0x1a48b8bb7f0>




    
<a href=""><img src="./imgs_pyd/062.png" height="auto" width="auto" title="digit-x"></a>
    


## 2. 张量(tensor)

最常用的数据格式是使用numpy库提供的数组(array)形式（也是机器学习库Sklearn数据集，及数据处理的格式），以及pandas库提供的DataFrame（常配合地理信息系统中的table/表，以及数据库使用）、Series数据格式。在深度学习（[PyTorch](https://pytorch.org/)库，或者[TensorFlow](https://www.tensorflow.org/)）中，引入了张量(tensor)，可以用于GPU计算，并有自动求梯度等更多功能。张量和数组类似，0维（一个数，rank=0,rank为维度数）张量即为标量（Scalar），1维张量（rank=1）即为向量/矢量(Vector)，2维张量(rank=2)即为矩阵(Matrix)，3维张量(rank=3)即为矩阵数组。深度学习中，张量可以看作一个多维数组(multidimentional array)。


```python
import torch
print("_"*50,"A-tensor创建")
#01-未初始化，shape=（5，3）的tensor
t_a=torch.empty(5,3)
print("01-创建未初始化，shape=（5，3）的tensor:\nt_a={},\nshape={}".format(t_a,t_a.shape))
#02-随机初始化shape=（5，3）的tensor
t_b=torch.rand(5,3)
print("02-随机初始化shape=（5，3）的tensor:\nt_b={}".format(t_b))
#03-数据类型为long，全0的tensor
t_c=torch.zeros(5,3,dtype=torch.long)
print("03-数据类型为long，全0的tensor:\nt_c={}".format(t_c))
#04-由给定数据直接创建
t_d=torch.tensor([[3.1415,0],[9,2.71828]])
print("04-由给定数据直接创建:\nt_d={}".format(t_d))
#05-tensor.new_ones()方法重用已有tensor，保持数据类型，及torch.device(CPU或GPU)
t_e=t_c.new_ones(4,3)
print("05-tensor.new_ones()方法重用已有tensor，保持数据类型，及torch.device(CPU或GPU):\nt_e={}\ndtype={},device={}".format(t_e,t_e.dtype,t_e.device))
#06-torch.randn_like()方法重用已有tensor,并可重定义数据类型
t_f=torch.randn_like(t_c,dtype=torch.float)
print("#06-torch.randn_like()方法重用已有tensor,并可重定义数据类型:\nt_f={},\ndtype={}".format(t_f,t_f.dtype))
#07-tensor.size()和tensor.shape方法查看tensor形状
print("07-tensor.size()和tensor.shape方法查看tensor形状:\nt_f.size()={},t_f.shape={}".format(t_f.size(),t_f.shape))
```

    __________________________________________________ A-tensor创建
    01-创建未初始化，shape=（5，3）的tensor:
    t_a=tensor([[0.4891, 0.1479, 0.2193],
            [0.7401, 0.9083, 0.8555],
            [0.9524, 0.3502, 0.3560],
            [0.3341, 0.8411, 0.5545],
            [0.0208, 0.6404, 0.6894]]),
    shape=torch.Size([5, 3])
    02-随机初始化shape=（5，3）的tensor:
    t_b=tensor([[0.4448, 0.9266, 0.1406],
            [0.5206, 0.5046, 0.7984],
            [0.4705, 0.5411, 0.3824],
            [0.6421, 0.9786, 0.6714],
            [0.8425, 0.4285, 0.1281]])
    03-数据类型为long，全0的tensor:
    t_c=tensor([[0, 0, 0],
            [0, 0, 0],
            [0, 0, 0],
            [0, 0, 0],
            [0, 0, 0]])
    04-由给定数据直接创建:
    t_d=tensor([[3.1415, 0.0000],
            [9.0000, 2.7183]])
    05-tensor.new_ones()方法重用已有tensor，保持数据类型，及torch.device(CPU或GPU):
    t_e=tensor([[1, 1, 1],
            [1, 1, 1],
            [1, 1, 1],
            [1, 1, 1]])
    dtype=torch.int64,device=cpu
    #06-torch.randn_like()方法重用已有tensor,并可重定义数据类型:
    t_f=tensor([[-0.4409,  1.1647,  1.8603],
            [ 1.4321, -0.1198,  1.1250],
            [ 1.1956, -1.2576,  1.4611],
            [ 0.9230,  0.4733,  0.7402],
            [-0.0234,  0.9688, -0.6406]]),
    dtype=torch.float32
    07-tensor.size()和tensor.shape方法查看tensor形状:
    t_f.size()=torch.Size([5, 3]),t_f.shape=torch.Size([5, 3])
    


```python
print("_"*50,"B-tensor操作")
#08-加法形式-1-'+'
print("08-加法形式-1-'+':\nt_a+t_b={}".format(t_a+t_b))
#09-加法形式-2-torch.add()
print("09-加法形式-2-torch.add():\ntorch.add(t_a,t_b)={}".format(torch.add(t_a,t_b)))
#10-加法形式-3-原地结果替换inplace/add_(PyTorch原地操作inplace都有后缀_)
print("10-加法形式-3-原地结果替换inplace/add_:\nt_a.add_(t_b)={}，\nt_a={}".format(t_a.add_(t_b),t_a))
#11-索引，共享存储地址
t_g=t_a[0,:]
print("11-索引，共享存储地址:\nt_a[0,:]={}".format(t_g))
t_g+=1
print("t_g+=1后,tg={},t_a[0,:]={}".format(t_g,t_a[0,:]))
#12-view()方法改变tensor形状（shape），但为同一存储地址
print("12-view()方法改变tensor形状（shape），但为同一存储地址:\nt_a.shape={},t_a.view(15).shape={},t_a.view(-1,5).shape={}".format(t_a.shape,t_a.view(15).shape,t_a.view(-1,5).shape))
#13-clone()
print("13-clone():\nid(t_a)==id(t_a.clone())={}".format(id(t_a)==id(t_a.clone())))

print("_"*50,"C-tensor广播机制")
#14-广播机制
t_h=torch.arange(1,3).view(1,2)
t_i=torch.arange(1,4).view(3,1)
print("14-广播机制:\nt_h={}\nt_i={}\nt_h.shape={},t_i.shape={}\nt_h+t_i={}".format(t_h,t_i,t_h.shape,t_i.shape,t_h+t_i))

print("_"*50,"D-inplace原地操作，节约内存")
#15-inplace——out参数
print("15-inplace——out参数:\nid(t_a)==id(torch.add(t_a,t_b,out=t_a))={}".format(id(t_a)==id(torch.add(t_a,t_b,out=t_a))))
#16-inplace——+=(add_())
t_j=t_a
t_a+=1
print("16-inplace——+=(add_()):\nid(t_a)==id(t_j)={}".format(id(t_a)==id(t_j)))
#17-inplace——[:]
t_k=t_a
t_a[:]=t_a+t_b
print("17-inplace——[:]:\nid(t_a)==id(t_k)={}".format(id(t_a)==id(t_k)))

print("_"*50,"E-tensor<-->array(NumPy)")
#18-tensor-->array
print("18-tensor-->array:\ntype(t_a)={},type(t_a.numpy())={}".format(type(t_a),type(t_a.numpy())))
#19-array-->tenssor
import numpy as np
array=np.arange(15).reshape(3,5)
print("19-array-->tenssor:\ntype(array)={},type(torch.from_numpy(array))={}".format(type(array),type(torch.from_numpy(array))))
```

    __________________________________________________ B-tensor操作
    08-加法形式-1-'+':
    t_a+t_b=tensor([[4.2682, 5.8544, 2.7816],
            [3.8225, 3.9269, 5.0491],
            [3.8344, 3.5148, 2.8855],
            [3.9024, 5.7555, 4.2401],
            [4.3908, 3.3543, 2.2018]])
    09-加法形式-2-torch.add():
    torch.add(t_a,t_b)=tensor([[4.2682, 5.8544, 2.7816],
            [3.8225, 3.9269, 5.0491],
            [3.8344, 3.5148, 2.8855],
            [3.9024, 5.7555, 4.2401],
            [4.3908, 3.3543, 2.2018]])
    10-加法形式-3-原地结果替换inplace/add_:
    t_a.add_(t_b)=tensor([[4.2682, 5.8544, 2.7816],
            [3.8225, 3.9269, 5.0491],
            [3.8344, 3.5148, 2.8855],
            [3.9024, 5.7555, 4.2401],
            [4.3908, 3.3543, 2.2018]])，
    t_a=tensor([[4.2682, 5.8544, 2.7816],
            [3.8225, 3.9269, 5.0491],
            [3.8344, 3.5148, 2.8855],
            [3.9024, 5.7555, 4.2401],
            [4.3908, 3.3543, 2.2018]])
    11-索引，共享存储地址:
    t_a[0,:]=tensor([4.2682, 5.8544, 2.7816])
    t_g+=1后,tg=tensor([5.2682, 6.8544, 3.7816]),t_a[0,:]=tensor([5.2682, 6.8544, 3.7816])
    12-view()方法改变tensor形状（shape），但为同一存储地址:
    t_a.shape=torch.Size([5, 3]),t_a.view(15).shape=torch.Size([15]),t_a.view(-1,5).shape=torch.Size([3, 5])
    13-clone():
    id(t_a)==id(t_a.clone())=False
    __________________________________________________ C-tensor广播机制
    14-广播机制:
    t_h=tensor([[1, 2]])
    t_i=tensor([[1],
            [2],
            [3]])
    t_h.shape=torch.Size([1, 2]),t_i.shape=torch.Size([3, 1])
    t_h+t_i=tensor([[2, 3],
            [3, 4],
            [4, 5]])
    __________________________________________________ D-inplace原地操作，节约内存
    15-inplace——out参数:
    id(t_a)==id(torch.add(t_a,t_b,out=t_a))=True
    16-inplace——+=(add_()):
    id(t_a)==id(t_j)=True
    17-inplace——[:]:
    id(t_a)==id(t_k)=True
    __________________________________________________ E-tensor<-->array(NumPy)
    18-tensor-->array:
    type(t_a)=<class 'torch.Tensor'>,type(t_a.numpy())=<class 'numpy.ndarray'>
    19-array-->tenssor:
    type(array)=<class 'numpy.ndarray'>,type(torch.from_numpy(array))=<class 'torch.Tensor'>
    

* tensor在GPU和CPU上互相转换，及在GPU上运算


```python
if torch.cuda.is_available():
    device=torch.device("cuda")
    print("device={}".format(device))
    x=torch.tensor([[3.1415,0],[9,2.71828]],device=device)
    print("x={}".format(x))
    y=torch.rand(2,2)
    print("默认CPU,y={}".format(y))
    y=y.to(device)
    print(".to(device)，y={}".format(y))    
    print("GPU运算,x+y={}".format(x+y))
    print(".to('cpu'),(x+y).to('cpu',torch.double)={}".format((x+y).to("cpu",torch.double)))
```

    device=cuda
    x=tensor([[3.1415, 0.0000],
            [9.0000, 2.7183]], device='cuda:0')
    默认CPU,y=tensor([[0.1915, 0.8456],
            [0.1379, 0.1073]])
    .to(device)，y=tensor([[0.1915, 0.8456],
            [0.1379, 0.1073]], device='cuda:0')
    GPU运算,x+y=tensor([[3.3330, 0.8456],
            [9.1379, 2.8256]], device='cuda:0')
    .to('cpu'),(x+y).to('cpu',torch.double)=tensor([[3.3330, 0.8456],
            [9.1379, 2.8256]], dtype=torch.float64)
    

## 3. 前向(正向)传播(forward propagation)与后向（反向）传播(back propagation)

### 3.1 微积分-链式法则(chain rule)

在'微积分基础的代码表述'部分，求导的基本公式中给出了复合函数的微分公式为：$\{g(f(x))\}' = g'(f(x)) f' (x) $ ，也表示为：$\frac{dy}{dx}= \frac{dy}{du}   .  \frac{du}{dx} $。链式法则表述为，两个函数组合起来的复合函数，导数等于内层函数代入外层函数的导函数，乘以内层函数的导函数。下述验证代码中，定义了复合函数为$5 \times  sin( x^{3}+5 )$，将其分解为外层函数：$5 \times  sin( x)$和内层函数:$ x^{3}+5 $。使用'sympy'库diff方法求导，分别对各个分解函数求导，求导后，外层导函数带入内层函数，最后求积即为应用链式法则求复合函数的结果。其结果与直接对复合函数求导的结果保持一致。

链式法则应用于深度学习神经网络的反向传播计算。


```python
import sympy
from sympy import diff,pprint
import matplotlib.pyplot as plt
import numpy as np

x,x_i,x_o=sympy.symbols('x x_i,x_o')
composite_func=5*sympy.sin(x**3+5)
composite_func_=sympy.lambdify(x,composite_func,"numpy")
print("复合函数：")
pprint(composite_func)

inner_func=x_i**3+5
inner_func_=sympy.lambdify(x_i,inner_func,"numpy")
print("内层函数：")
pprint(inner_func)

outer_func=5*sympy.sin(x_o)
outer_func_=sympy.lambdify(x_o,outer_func,"numpy")
print("外层函数：")
pprint(outer_func)

#求导
d_composite=diff(composite_func,x)
d_composite_=sympy.lambdify(x,d_composite,"numpy")
print("复合函数-求导：")
pprint(d_composite)

d_chain_rule_form_1=diff(outer_func,x_o).subs(x_o,inner_func)*diff(inner_func,x_i)
print("链式法则-求导:")
pprint(d_chain_rule_form_1)

t=np.arange(-np.pi,np.pi,0.01)
y_composite=composite_func_(t)
y_inner=inner_func_(t)
y_outer=outer_func_(t)

fig, ax = plt.subplots(figsize=(10,10))
ax.plot(t,y_composite,label="composite_func")
ax.plot(t,y_inner,label="inner_func")
ax.plot(t,y_outer,label="outer_func")
ax.plot(t,d_composite_(t)/10,label="derivative/5",dashes=[6, 2],color='gray',alpha=0.3)
ax.axhline(y=0,color='r',linestyle='-.',alpha=0.5)

ax.legend(loc='upper left')
ax.spines['right'].set_visible(False)
ax.spines['top'].set_visible(False)
plt.show()
```

    复合函数：
         ⎛ 3    ⎞
    5⋅sin⎝x  + 5⎠
    内层函数：
      3    
    xᵢ  + 5
    外层函数：
    5⋅sin(xₒ)
    复合函数-求导：
        2    ⎛ 3    ⎞
    15⋅x ⋅cos⎝x  + 5⎠
    链式法则-求导:
         2    ⎛  3    ⎞
    15⋅xᵢ ⋅cos⎝xᵢ  + 5⎠
    


    
<a href=""><img src="./imgs_pyd/063.png" height="auto" width="auto" title="digit-x"></a>

    


### 3.2 激活函数(activation function)

在多层神经网络中（多层感知机，multilayer perception,MLP），如果不使用激活函数，则每一层节点（神经元）的输入都是上一层输出的线性函数，那么无论神经网络有多少层，输出都是输入的线性组合，即为原始的感知机(单层感知机，perception)，其网络的逼近能力有限。因此引入了非线性函数作为激活函数，使神经网络可以逼近任意函数。常用的激活函数有ReLU(rectified linear unit)，sigmoid，和tanh函数。

给定元素$x$，ReLU函数的定义为:$ReLU(x)=max(x,0)$，可知ReLU函数只保留正数元素，并将负数元素清零，为两段线性函数。ReLU函数的导数对应在负数时，为0，正数时为1.值为0时，ReLU函数不可导，在此处将其导数配置为0.

sigmoid函数可以将元素的值变换到0-1之间，其公式为：$sigmoid(x)= \frac{1}{1+exp(-x)} $。sigmoid函数在早期的神经网络中较为普遍，目前逐渐被更为简单的ReLU函数取代。当输入接近0时，sigmoid函数接近线性变换。sigmoid函数的导数，在输入为0时，导数达到最大值0.25；当输入偏离0时，导数趋近于0.

tanh（双曲正切）函数可以将元素的值变换到-1到1之间。其公式为：$tanh(x)= \frac{1-exp(-2x)}{1+exp(-2x)} $，当输入接近0时，tanh函数接近线性变换。同时，tanh函数在坐标系原点上对称。依据链式法则，tanh函数的导数为：$ tanh' (x)=1- tanh^{2}(x) $。当输入为0时，tanh的导数达到最大值1；当输入偏离0时，tanh函数的导数趋近于0.


```python
import torch
import matplotlib.pyplot as plt
import torch.nn as nn
fig, axs=plt.subplots(1,3,figsize=(28,5))

#A-ReLU
x=torch.arange(-8.0,8.0,0.1,requires_grad=True)
y_relu=x.relu()
axs[0].plot(x.detach().numpy(), y_relu.detach().numpy(),label="ReLU")

#ReLU函数的导数
y_relu.sum().backward()
axs[0].plot(x.detach().numpy(), x.grad.detach().numpy(),label="grad of ReLU",linestyle='-.')

#B-sigmoid
y_sigmoid=x.sigmoid()
axs[1].plot(x.detach().numpy(), y_sigmoid.detach().numpy(),label="sigmoid")

#sigmoid函数的导函数
x.grad.zero_() #参数梯度置零
y_sigmoid.sum().backward()
axs[1].plot(x.detach().numpy(), x.grad.detach().numpy(),label="grad of ReLU",linestyle='-.')

#C-tanh
y_tanh=x.tanh()
axs[2].plot(x.detach().numpy(), y_tanh.detach().numpy(),label="tanh")

#tanh函数的导函数
x.grad.zero_()
y_tanh.sum().backward()
axs[2].plot(x.detach().numpy(), x.grad.detach().numpy(),label="grad of tanh",linestyle='-.')

axs[0].legend(loc='upper left', frameon=False)
axs[1].legend(loc='upper left', frameon=False)
axs[2].legend(loc='upper left', frameon=False)
plt.show()

```


    
<a href=""><img src="./imgs_pyd/064.png" height="auto" width="auto" title="digit-x"></a>

    


### 3.3 前向(正向)传播(forward propagation)与后向（反向）传播(back propagation)

构建一个典型的三层神经网络，包括输入层(input layer)包含两个神经元(neuron)$i_{1},i_{2}$，和一个偏置（偏差/截距，bias）项$b_{1}$；隐含层(hidden layter)包含两个神经元$h_{1},h_{2}$，和一个偏置$b_{2}$；及输出层(output layer)$o_{1},o_{2}$。假设数据集仅包含一个特征向量(feature)并含两个值，即输入数据：$i_{1} =0.05, i_{2} =0.10$；对应类标(label)，即输出数据：$o_{1}=0.01,o_{2}=0.99$；同时随机初始化权重值：$w_{1} =0.15,w_{2} =0.20,w_{3} =0.25,w_{4} =0.30,w_{5} =0.40,w_{6} =0.45,w_{7} =0.50,w_{8} =0.55$;偏置值为$b_{1} =0.35,b_{2} =0.60$

通过神经网络，输入值经过与权重及偏置值的计算，使得其结果与类标接近，从而最终确定权重值。

<a href=""><img src="./imgs_pyd/066.jpg" height="auto" width="auto" title="digit-x"></a>


> 参考：https://www.cnblogs.com/charlotte77/p/5629865.html

**Step-0：初始化** 

初始化特征值，类标，以及权重值和偏置


```python
i_1,i_2=0.05,0.10
w_1,w_2,w_3,w_4,w_5,w_6,w_7,w_8=0.15,0.20,0.25,0.30,0.40,0.45,0.50,0.55
b_1,b_2=0.35,0.60
o_1,o_2=0.01,0.99
```

**Step-1：前向传播** 
* 1. 输入层--->隐含层

计算神经元$h_{1}$的输入加权和


```python
net_h_1=w_1*i_1+w_2*i_2+b_1*1
print("net_h_1={}".format(net_h_1))
```

    net_h_1=0.3775
    

对h1应用激活函数-sigmoid


```python
def sigmoid(x):
    import math
    '''
    function - sigmoid函数
    '''
    return 1/(1+math.exp(-x))
out_h_1=sigmoid(net_h_1)
print("out_h_1={}".format(out_h_1))

```

    out_h_1=0.5932699921071872
    

同理，计算神经元$h_{2}$的输入加权和，并应用sigmoid函数


```python
net_h_2=w_3*i_1+w_4*i_2+b_1*1
out_h_2=sigmoid(net_h_2)
print("out_h_2={}".format(out_h_2))
```

    out_h_2=0.596884378259767
    

* 2. 隐含层--->输出层

计算神经元$o_{1}, o_{2}$的值


```python
net_o_1=w_5*out_h_1+w_6*out_h_2+b_2*1
out_o_1=sigmoid(net_o_1)
print("out_o_1={}".format(out_o_1))
net_o_2=w_7*out_h_1+w_8*out_h_2+b_2*1
out_o_2=sigmoid(net_o_2)
print("out_o_2={}".format(out_o_2))
```

    out_o_1=0.7513650695523157
    out_o_2=0.7729284653214625
    

随机初始化权重值，逐层计算输入加权和(Summation and Bias):$\sum_{i=1}^m( w_{i} x_{i}  )+bias$，并应用激活函数（sigmoid），获得输出值'out_o_1'和'out_o_2'，与实际值$o_{1}=0.01,o_{2}=0.99$相差还很远，现在对误差进行反向传播，更新权重/权值，重新计算输出。

**Step-2：反向传播** 

* 1. 计算总误差（残差平方误差，Residual square error）

分别计算各个输出（此时有2个输出）的误差，再求和。


```python
E_o_1=1/2*(out_o_1-o_1)**2
E_o_2=1/2*(out_o_2-o_2)**2
E_total=E_o_1+E_o_2
print("E_total={}".format(E_total))
```

    E_total=0.2983711087600027
    

* 2. 隐含层--->输出层的权值更新

以w_5权重值参数为例，计算w_5对整体误差的影响，用整体误差对w_5求偏导，应用微分链式法则，其公式为：$\frac{ \partial  E_{total} }{ \partial  w_{5} } =\frac{ \partial  E_{total} }{ \partial  out_{o1} } \times \frac{ \partial  out_{o1} }{ \partial  net_{o1} } \times \frac{ \partial  net_{o1} }{ \partial  w_{5} }$，分别是对总误差计算公式、激活函数和加权和函数求导。只是各个输入值分别对应w_5计算路径下的各个对应值，可以很容易的从以上神经网络结构图中确定，即函数的输入端值。总误差计算公式导函数对应$ out_{o1}$即`out_o_1`，激活函数导函数对应$net_{o1} $即`net_o_1`，加权和函数导函数对应$out_{h1} $即`out_h_1`。


```python
import sympy
from sympy import diff,pprint

#w_5-链式法则-01-总误差函数（公式）对out_o_1的偏导数
out_o_1_,o_1_,out_o_2_,o_2_=sympy.symbols('out_o_1_ o_1_,out_o_2_ o_2_')
d_E_total_2_out_o_1=diff(1/2*(out_o_1_-o_1_)**2+1/2*(out_o_2_-o_2_)**2,out_o_1_)
d_E_total_2_out_o_1_value=d_E_total_2_out_o_1.subs([(o_1_,o_1),(out_o_1_,out_o_1)])
print("d_E_total_2_out_o_1_value={}".format(d_E_total_2_out_o_1_value))

#w_5-链式法则-02-激活函数对net_o_1的偏导数
from sympy.functions import exp
x_sigmoid=sympy.symbols('x_sigmoid')
d_activation=diff(1/(1+exp(-x_sigmoid)),x_sigmoid)
d_out_o_1_2_net_o_1_value=d_activation.subs([(x_sigmoid,net_o_1)])
print("d_out_o_1_2_activation_value={}".format(d_out_o_1_2_net_o_1_value))

#w_5-链式法则-03-隐藏层h1，加权和函数（公式）对out_h_1的偏导数
w_5_,out_h_1_,w_6_,out_h_2_,b_2_=sympy.symbols('w_5,out_h_1,w_6,out_h_2,b_2')
d_net_o_1_2_w_5=diff(w_5_*out_h_1_+w_6_*out_h_2_+b_2_*1,w_5_)
d_net_o_1_2_w_5_value=d_net_o_1_2_w_5.subs([(out_h_1_,out_h_1)])
print("d_net_o_1_2_w_5_value={}".format(d_net_o_1_2_w_5_value))

#w_5-链式法则-各部分相乘
d_E_total_2_w_5=d_E_total_2_out_o_1_value*d_out_o_1_2_net_o_1_value*d_net_o_1_2_w_5_value
print("d_E_total_2_w_5={}".format(d_E_total_2_w_5))

#w_5-权重值更新
lr=0.5 #学习速率
w_5_update=w_5-lr*d_E_total_2_w_5
print("w_5_update={}".format(w_5_update))

```

    d_E_total_2_out_o_1_value=0.741365069552316
    d_out_o_1_2_activation_value=0.186815601808960
    d_net_o_1_2_w_5_value=0.593269992107187
    d_E_total_2_w_5=0.0821670405642308
    w_5_update=0.358916479717885
    

所有权值的偏导求法基本相同，为了减少重复的代码，可以定义公用的导函数，只是不同的权值根据其在神经网络结构下的路径，调整链式法则中的各个偏导函数，并对应替换值。


```python
#定义总误差偏导
def partialD_E_total_prediction(true_value_,predicted_value_):
    import sympy
    from sympy import diff
    '''
    function - 定义总误差偏导
    '''
    true_value,predicted_value=sympy.symbols('true_value predicted_value')
    partialD_E_total_prediction=diff(1/2*(predicted_value-true_value)**2,predicted_value)
    return partialD_E_total_prediction.subs([(predicted_value,predicted_value_),(true_value,true_value_)])

#定义激活函数偏导
def partialD_activation(x):
    import sympy
    from sympy import diff
    from sympy.functions import exp
    '''
    function -定义激活函数偏导
    '''
    x_sigmoid=sympy.symbols('x_sigmoid')
    partialD_activation=diff(1/(1+exp(-x_sigmoid)),x_sigmoid)
    return partialD_activation.subs([(x_sigmoid,x)])

#定义加权和偏导
def partialD_weightedSUM(w_):
    import sympy
    from sympy import diff
    '''
    fucntion - 定义加权和偏导
    '''
    w,x_w=sympy.symbols('w x_w')
    partialD_weightedSUM=diff(w*x_w,w)
    return partialD_weightedSUM.subs([(x_w,w_)])
```


```python
w_5_update=w_5-lr*partialD_E_total_prediction(o_1,out_o_1)*partialD_activation(net_o_1)*partialD_weightedSUM(out_h_1)
w_6_update=w_6-lr*partialD_E_total_prediction(o_1,out_o_1)*partialD_activation(net_o_1)*partialD_weightedSUM(out_h_2)
w_7_update=w_7-lr*partialD_E_total_prediction(o_2,out_o_2)*partialD_activation(net_o_2)*partialD_weightedSUM(out_h_1)
w_8_update=w_8-lr*partialD_E_total_prediction(o_2,out_o_2)*partialD_activation(net_o_2)*partialD_weightedSUM(out_h_2)

print("w_5_update={}\nw_6_update={}\nw_7_update={}\nw_8_update={}".format(w_5_update,w_6_update,w_7_update,w_8_update))
```

    w_5_update=0.358916479717885
    w_6_update=0.408666186076233
    w_7_update=0.511301270238738
    w_8_update=0.561370121107989
    

* 3. 输入层--->隐含层的权值更新

因为隐含层$ out_{h1}$会受到$ E_{o1},E_{o2}$两个地方传来的误差，因此分别计算并求和，再与其它路径下的导数求积。$ out_{h2}$与之同。


```python
w_1_update=w_1-lr*(partialD_E_total_prediction(o_1,out_o_1)*partialD_activation(net_o_1)*partialD_weightedSUM(w_5)+partialD_E_total_prediction(o_2,out_o_2)*partialD_activation(net_o_2)*partialD_weightedSUM(w_7))*partialD_activation(net_h_1)*partialD_weightedSUM(i_1)
w_2_update=w_2-lr*(partialD_E_total_prediction(o_1,out_o_1)*partialD_activation(net_o_1)*partialD_weightedSUM(w_5)+partialD_E_total_prediction(o_2,out_o_2)*partialD_activation(net_o_2)*partialD_weightedSUM(w_7))*partialD_activation(net_h_1)*partialD_weightedSUM(i_2)
w_3_update=w_3-lr*(partialD_E_total_prediction(o_1,out_o_1)*partialD_activation(net_o_1)*partialD_weightedSUM(w_6)+partialD_E_total_prediction(o_2,out_o_2)*partialD_activation(net_o_2)*partialD_weightedSUM(w_8))*partialD_activation(net_h_2)*partialD_weightedSUM(i_1)
w_4_update=w_4-lr*(partialD_E_total_prediction(o_1,out_o_1)*partialD_activation(net_o_1)*partialD_weightedSUM(w_6)+partialD_E_total_prediction(o_2,out_o_2)*partialD_activation(net_o_2)*partialD_weightedSUM(w_8))*partialD_activation(net_h_2)*partialD_weightedSUM(i_2)

print("w_1_update={}\nw_2_update={}\nw_3_update={}\nw_4_update={}".format(w_1_update,w_2_update,w_3_update,w_4_update))
```

    w_1_update=0.149780716132763
    w_2_update=0.199561432265526
    w_3_update=0.249751143632370
    w_4_update=0.299502287264739
    

至此误差的反向传播全部计算完，并更新权重值，重复前向传播。为了方法计算将前向传播定义为一个函数，计算结果显示其总误差为0.29102777369359933，较之前次0.2983711087600027有所下降。


```python
def forward_func(input_list,weight_list,bias_list,output_list):
    '''
    function - 三层神经网络，各层2个神经元的示例，前向传播函数
    '''
    def sigmoid(x):
        import math
        '''
        function - sigmoid函数
        '''
        return 1/(1+math.exp(-x))
    i_1,i_2=input_list
    w_1,w_2,w_3,w_4,w_5,w_6,w_7,w_8=weight_list
    b_1,b_2,b_3,b_4=bias_list
    o_1,o_2=output_list
    
    net_h_1=w_1*i_1+w_2*i_2+b_1*1
    out_h_1=sigmoid(net_h_1)
    
    net_h_2=w_3*i_1+w_4*i_2+b_2*1
    out_h_2=sigmoid(net_h_2)    
    
    net_o_1=w_5*out_h_1+w_6*out_h_2+b_3*1
    out_o_1=sigmoid(net_o_1)

    net_o_2=w_7*out_h_1+w_8*out_h_2+b_4*1
    out_o_2=sigmoid(net_o_2)

    E_total=1/2*(out_o_1-o_1)**2+1/2*(out_o_2-o_2)**2
    print("out_o_1={}\nout_o_2={}\nE_total={}".format(out_o_1,out_o_2,E_total))
    return out_o_1,out_o_2,E_total
out_o_1,out_o_2,E_total=forward_func(input_list=[0.05,0.10],weight_list=[w_1_update,w_2_update,w_3_update,w_4_update,w_5_update,w_6_update,w_7_update,w_8_update],bias_list=[0.35,0.35,0.60,0.60],output_list=[0.01,0.99])
```

    out_o_1=0.7420881111907824
    out_o_2=0.7752849682944595
    E_total=0.29102777369359933
    

**多层感知机-代码整合**

将上述的分解过程整合，实现迭代计算求取权重值。代码的结构包括三个类，NEURON类定义神经元前向传播和反向传播的函数；NEURAL_LAYER类是基于NEURON的层级别神经元汇总；NEURAL_NETWORK类构建神经网络结构，初始化权值并更新权值。此处的输入值保持不变为[0.05,0.1],输出值修改为[0.01,0.09]，能够更好的观察收敛的过程，如果仍然保持输出值为[0.01,0.99]，因为偏置保持不变，造成损失函数下降的趋势不明显。


```python
class NEURON:
    '''
    class - 每一神经元的输入加权和、激活函数，以及输出计算
    '''
    def __init__(self,bias):
        '''
        function - 初始化权重和偏置
        '''
        self.bias=bias
        self.weights=[]
        
    def activation(self,x_sigmoid):
        import math
        '''
        function - 定义sigmoid激活函数
        '''        
        return 1/(1+math.exp(-x_sigmoid))
        
    def net_input(self):
        import numpy as np
        '''
        function - 每一神经元的输入加权和，inputs，及weights数组形状为(-1,1)
        '''
        y=np.array(self.inputs).reshape(-1,1)*np.array(self.weights).reshape(-1,1)
        return y.sum()+self.bias
            
    def net_output(self,inputs):
        '''
        function - 每一神经元，先计算输入加权和，然后应用激活函数获得输出
        '''
        self.inputs=inputs
        self.output=self.activation(self.net_input())
        return self.output
    
    def neuron_error(self,predicted_output):
        '''
        function - 每一神经元的误差（平方差）
        '''
        return 0.5*(predicted_output-self.output)**2
    
    def pd_activation(self):
        import sympy
        from sympy import diff
        from sympy.functions import exp
        '''
        function -定义神经元激活函数偏导
        '''
        x_sigmoid=sympy.symbols('x_sigmoid')
        partialD_activation=diff(1/(1+exp(-x_sigmoid)),x_sigmoid)
        return partialD_activation.subs([(x_sigmoid,self.output)]) #由sympy的diff方法获取sigmoid导函数
        #return self.output * (1 - self.output) #推到公式
    
    def pd_E_prediction(self,predicted_output):
        import sympy
        from sympy import diff
        '''
        function - 定义神经元误差偏导
        '''
        true_value,predicted_value=sympy.symbols('true_value predicted_value')
        partialD_E_total_prediction=diff(0.5*(predicted_value-true_value)**2,true_value)
        return partialD_E_total_prediction.subs([(predicted_value,predicted_output),(true_value,self.output)])

    
    def pd_net_output(self,predicted_output):
        '''
        function - 每一神经元误差偏导，由链式法则计算，包括神经元激活函数偏导，和神经元总误差偏导
        '''
        return self.pd_activation()*self.pd_E_prediction(predicted_output)
    
    def pd_weightedSUM(self,index):
        import sympy
        from sympy import diff
        '''
        fucntion - 定义神经元加权和偏导
        '''
        w,x_w=sympy.symbols('w x_w')
        partialD_weightedSUM=diff(w*x_w,w)
        return partialD_weightedSUM.subs([(x_w,self.inputs[index])])
    
class NEURAL_LAYER:
    '''
    class - 神经网络每一层的定义
    '''
    def __init__(self,num_neurons,bias):
        '''
        fucntion - 初始化每层偏置，及神经元
        '''
        self.bias=bias if bias else random.random() #同一层各个神经元共享同一个偏置        
        self.neurons=[NEURON(self.bias) for i in range(num_neurons)]
        
    def layer_info(self):
        '''
        function - 查看每层的神经元信息
        '''
        print('neurons:{}'.format(len(self.neurons)))
        for i in range(len(self.neurons)):
            print('neuron-{}'.format(i))
            print('weights:{}'.format([self.neurons[i].weights[j] for j in range(len(self.neurons[i].weights))]))
            print('bias:{}'.format(self.bias))
            
    def layer_output(self,inputs):
        outputs=[neuron.net_output(inputs) for neuron in self.neurons]
        return outputs
    
class NEURAL_NETWORK:
    '''
    class - 构建多层感知机（神经网络）
    '''
    def __init__(self,learning_rate,num_inputs,num_hidden,num_outputs,hiddenLayer_weights=None,hiddenLayer_bias=None,outputLayer_weights=None,outputLayer_bias=None):
        self.lr=learning_rate
        self.num_inputs=num_inputs
        self.hidden_layer=NEURAL_LAYER(num_hidden,hiddenLayer_bias)
        self.output_layer=NEURAL_LAYER(num_inputs,outputLayer_bias)
        self.weights_init(hiddenLayer_weights,self.hidden_layer,self.num_inputs)
        self.weights_init(outputLayer_weights,self.output_layer,len(self.hidden_layer.neurons))       
        
    def weights_init(self,weights,neu_layer,num_previous):
        '''
        fucntion - 初始化权值
        '''
        weights_num=0
        for i in range(len(neu_layer.neurons)):
            for j in range(num_previous):
                if not weights:
                    neu_layer.neurons[i].weights.append(random.random())
                else:
                    neu_layer.neurons[i].weights.append(weights[weights_num])
                weights_num+=1
                
    def neural_info(self):
        '''
        function - 查看神经网络结构
        '''
        print("_"*50)
        print("inputs number:{}".format(self.num_inputs))
        print("_"*50)
        print("hidden layer:{}".format(self.hidden_layer.layer_info()))
        print("_"*50)
        print("output layer:{}".format(self.output_layer.layer_info()))
        print("_"*50)
        
    def forward_propagation(self,inputs):
        '''
        fucntion - 前向传播
        '''
        hiddenLayer_outputs=self.hidden_layer.layer_output(inputs)
        return self.output_layer.layer_output(hiddenLayer_outputs)
    
    def train(self,training_inputs,training_outputs):
        '''
        function - 训练，迭代更新权值
        '''
        self.forward_propagation(training_inputs)        
       
        #1. 输出层神经元的值
        pd_output_values=[0]*len(self.output_layer.neurons)
        for i in range(len(self.output_layer.neurons)):
            # ∂E/∂zⱼ
            pd_output_values[i]=self.output_layer.neurons[i].pd_net_output(training_outputs[i])
       # print(pd_output_values)
        
        
        #2. 隐含层神经元的值
        hidden_values=[0]*len(self.hidden_layer.neurons)
        for i in range(len(self.hidden_layer.neurons)):
            # dE/dyⱼ = Σ ∂E/∂zⱼ * ∂z/∂yⱼ = Σ ∂E/∂zⱼ * wᵢⱼ
            d_errors=0
            for j in range(len(self.output_layer.neurons)):
                d_errors+=pd_output_values[j]*self.output_layer.neurons[j].weights[i]
            # ∂E/∂zⱼ = dE/dyⱼ * ∂zⱼ/∂
            hidden_values[i]=d_errors*self.hidden_layer.neurons[i].pd_activation()
        #print(hidden_values)
        
        #3. 更新输出层权重系数
        for i in range(len(self.output_layer.neurons)):
            for j in range(len(self.output_layer.neurons[i].weights)):
                # ∂Eⱼ/∂wᵢⱼ = ∂E/∂zⱼ * ∂zⱼ/∂wᵢⱼ
                weights_update=pd_output_values[i]*self.output_layer.neurons[i].pd_weightedSUM(j)
                # Δw = α * ∂Eⱼ/∂wᵢ
                self.output_layer.neurons[i].weights[j]-=self.lr*weights_update
                
        #4. 更新隐含层的权重系数
        for i in range(len(self.hidden_layer.neurons)):
            for j in range(len(self.hidden_layer.neurons[i].weights)):
                # ∂Eⱼ/∂wᵢ = ∂E/∂zⱼ * ∂zⱼ/∂wᵢ
                weights_update=hidden_values[i]*self.hidden_layer.neurons[i].pd_weightedSUM(j)
                # Δw = α * ∂Eⱼ/∂wᵢ
                self.hidden_layer.neurons[i].weights[j]-=self.lr*weights_update
                
    def total_error(self,training_sets):
        total_error=0
        for i in range(len(training_sets)):            
            training_inputs, training_outputs=training_sets[i]
            self.forward_propagation(training_inputs)
            for j in range(len(training_outputs)):
                total_error+=self.output_layer.neurons[j].neuron_error(training_outputs[j])
        return total_error

        
learning_rate=0.5        
nn=NEURAL_NETWORK(learning_rate,2, 2, 2, hiddenLayer_weights=[0.15, 0.2, 0.25, 0.3], hiddenLayer_bias=0.35, outputLayer_weights=[0.4, 0.45, 0.5, 0.55], outputLayer_bias=0.6)    
nn.neural_info()

import numpy as np
from tqdm import tqdm
for i in tqdm(range(10000)):
    nn.train([0.05,0.1],[0.01,0.09])
    if i%1000==0:
        print(i,round(nn.total_error([[[0.05, 0.1], [0.01, 0.09]]]),9))
hidenLayer_weights_=[nn.hidden_layer.neurons[i].weights for i in range(len(nn.hidden_layer.neurons))]
output_layer_weights_=[nn.output_layer.neurons[i].weights for i in range(len(nn.output_layer.neurons))]
print("hidenLayer_weights_={}\noutput_layer_weights_={}".format(hidenLayer_weights_,output_layer_weights_))
```

    __________________________________________________
    inputs number:2
    __________________________________________________
    neurons:2
    neuron-0
    weights:[0.15, 0.2]
    bias:0.35
    neuron-1
    weights:[0.25, 0.3]
    bias:0.35
    hidden layer:None
    __________________________________________________
    neurons:2
    neuron-0
    weights:[0.4, 0.45]
    bias:0.6
    neuron-1
    weights:[0.5, 0.55]
    bias:0.6
    output layer:None
    __________________________________________________
    

      0%|          | 6/10000 [00:00<04:00, 41.53it/s]

    0 0.493712428
    

     10%|█         | 1013/10000 [00:12<01:44, 86.33it/s]

    1000 2.4963e-05
    

     20%|██        | 2016/10000 [00:23<01:30, 88.13it/s]

    2000 1.878e-06
    

     30%|███       | 3015/10000 [00:34<01:18, 89.08it/s]

    3000 2.28e-07
    

     40%|████      | 4011/10000 [00:46<01:15, 79.10it/s]

    4000 3.2e-08
    

     50%|█████     | 5017/10000 [00:57<00:56, 88.91it/s]

    5000 5e-09
    

     60%|██████    | 6018/10000 [01:09<00:47, 84.50it/s]

    6000 1e-09
    

     70%|███████   | 7011/10000 [01:20<00:32, 90.65it/s]

    7000 0.0
    

     80%|████████  | 8016/10000 [01:31<00:23, 84.36it/s]

    8000 0.0
    

     90%|█████████ | 9014/10000 [01:42<00:10, 91.56it/s]

    9000 0.0
    

    100%|██████████| 10000/10000 [01:53<00:00, 87.78it/s]

    hidenLayer_weights_=[[0.374907950905178, 0.649815901810356], [0.469098467292651, 0.738196934585302]]
    output_layer_weights_=[[-4.28132748279580, -4.25790301529841], [-2.41120108789009, -2.37807889000506]]
    

    
    

## 4. 梯度下降法（Gradient Descent）+ 自动求梯度(gradient)

### 4.1 梯度下降法（Gradient Descent）
在回归部分使用$\widehat{ \beta } = ( X^{'} X)^{-1} X^{'}Y$矩阵计算的方法求解模型参数值（即回归系数），其中矩阵求逆的计算很为复杂，同时有些情况则无法求逆，因此引入另一种估计模型参数最优值的方法，即梯度下降法。当下对于梯度下降法的解释文献异常繁多，主要包括通过形象的图式进行说明，使用公式推导过程等。仅有图式的说明只能大概的理解方法的过程，却对真正计算的细节无从理解；对于纯粹的公式推导，没有实例，只能空凭象形，无法落实。Udacity在线课程有一篇文章'Gradient Descent - Problem of Hiking Down a Mountain'对梯度下降法的解释包括了图示、推导以及实例，解释的透彻明白，因此以该文章为主导，来一步步的理解梯度下降法，这对于机器学习以及深度学习有很大的帮助。

对于梯度下降法最为通用的描述是下到山谷处，即找到最低点，也就是损失函数的最小值；在下山的过程中每一步也在试图找到该步通往下山路最陡路径，即找到给定点的梯度，梯度的方向就是函数变化最快的方向，超梯度反向，就能让函数值下降最快。然后就是不断的反复这个过程，不断到达局部的最小值，最终到达山谷。快速准确的达到山谷，需要衡量每一步在寻找最陡方向时测量的距离，如果步子过大可能错过最终山谷点，如果步子过小则增加计算的时长，并可能陷入局部最低点，因此每一步的跨度与当前地形的陡峭程度成比例，如果很陡就迈大步，如不很平缓就走小步。梯度下降法是估计函数局部最小值的优化算法。

#### 4.1.1 梯度，微分与导数

对于微分的理解可以拓展为函数图像中，某点切线的斜率和函数的变化率。对于切线的斜率的理解就是导数，导数是函数图像在某点处的斜率，即纵坐标增量（$\triangle y$）和横坐标增量($\triangle x$)，在$\triangle x \mapsto 0$时的比值，其一般定义为，设有定义域和取值都在实数域中的函数$y=f(x)$。若$f(x)$在点$x_{0} $的某个邻域内有定义，则当自变量$x$在$x_{0} $处取得增量$ \triangle x$（点$ x_{0} + \triangle x$仍在该邻域内）时，相应的$y$取得增量$\triangle y=f( x_{0}+  \triangle x )-f( x_{0} )$，如果$\triangle x \mapsto 0$时，$\triangle y$与$\triangle x$之比的极限存在，则称函数$y=f(x)$在点$x_{0} $处可导，并称这个极限为函数$y=f(x)$在点$x_{0} $处的导数，记为$f' ( x_{0} )$，即：$f' ( x_{0} )= \lim_{ \triangle x \rightarrow 0}  \frac{ \triangle y}{ \triangle x} =\lim_{ \triangle x \rightarrow 0} \frac{f( x_{0}+ \triangle x )-f( x_{0} )}{ \triangle x} $，也可记作$y'( x_{0} ), { \frac{dy}{dx} |} _{x= x_{0} } , \frac{dy}{dx} ( x_{0} ),{ \frac{df}{dx} |} _{x= x_{0} }$等。

如果理解为函数的变化率，则就是微分，在微分部分已经对其给与了解释，微分是对函数的局部变化率的一种线性描述。其可以近似的描述当函数自变量的取值足够小的改变时，函数的值是怎样变化的，其定义为，设函数$y=f(x)$在某个了邻域内有定义，对于邻域内一点$x_{0} $，当$x_{0} $变动到附近的$ x_{0} + \triangle x$（也在邻域内）时，如果函数的增量$\triangle y=f( x_{0}+  \triangle x )-f( x_{0} )$可表示为$\triangle y=A \triangle x+ o ( \triangle x)$，其中$A$是不依赖于$\triangle x$的常数，$o ( \triangle x)$是比$\triangle x$高阶的无穷小，那么称函数$f(x)$在点$x_{0} $是可微的，且$=A \triangle x$称作函数在点$x_{0} $相应于自变量增量$\triangle x $的微分，记作$dy$，即$dy=A \triangle x$，$dy$是$\triangle y$的线性主部。通常把自变量$x$的增量$\triangle x$称为自变量的微分，记作$dx$，即$dx=\triangle x$。

微分和导数是两个不同的概念。但是对于一元函数来说，可微与可导是完全等价的。可微的函数，其微分等于导数乘以自变量的微分$dx$，即函数的微分与自变量的微分之上商等于该函数的导数，因此导数也叫微商。函数$y=f(x)$的微分又可记作$dy= f' (x)dx$。

梯度实际上是多元函数导数（或微分）的推广，用$\theta $作为函数$J(  \theta _{1}, \theta _{2},\theta _{3})$的变量，其$J( \Theta )=0.55-(5  \theta _{1}+ 2  \theta _{2}-12 \theta _{3})$，则$ \triangle J( \Theta )=\langle  \frac{ \partial J}{ \partial   \theta _{1} }, \frac{ \partial J}{ \partial   \theta _{2} }, \frac{ \partial J}{ \partial   \theta _{3} }  \rangle=\langle  -5,-2,12 \rangle$，其中$ \triangle $作为梯度的一种符号，$\partial$符号用于表示偏微分（部分的意思），即梯度就是分别对每个变量求偏微分，同时梯度用$\langle  \rangle$括起，表示梯度为一个向量。

梯度是微积分重要一个重要的概念，在单变量的函数中，梯度就是函数的微分（或对函数求导），代表函数在某个给定点切线的斜率；在多变量函数中，梯度是一个方向（向量的方向），梯度的方向指出了函数给定点上升最快的方向，而反方向是函数在给定点下降最快的方向。

> 导数是对含有一个自变量函数（一元）求导；偏导数是对含有多个自变量函数（多元）中的一个自变量求导。偏微分与偏导数类似，是对含有多个自变量函数（多元）中的一个自变量微分。

#### 4.1.2 梯度下降算法数学解释
公式为：$ \Theta ^{1} = \Theta ^{0}- \alpha  \nabla J( \Theta )$，evaluated at $\Theta ^{0}$,其中$J$是关于 $\Theta $的函数，当前所处的位置为$ \Theta ^{0}$，要从这个点走到$J(\Theta)$的最小值点$\Theta ^{1}$，方向为梯度的反向，并用$\alpha$(学习率或者步长，Learning rate or step size)超参数，控制每一步的距离，避免步子过大错过最低点，当愈趋近于最低点，学习率的控制越重要。梯度是上升最快的方向，如果往下走则需要在梯度前加$-$负号。

* 单变量函数的梯度下降

定义函数为：$J(x)= x^{2} $，对该函数$x$微分，结果为：$J' =2x$，因此梯度下降公式为：$x_{next}= x_{current}- \alpha *2x$，同时给定了迭代次数（iteration），通过打印图表可以方便查看每一迭代梯度下降的幅度。


```python
import sympy
import matplotlib.pyplot as plt
import numpy as np
# 定义单变量的函数，并绘制曲线
x=sympy.symbols('x')
J=1*x**2
J_=sympy.lambdify(x,J,"numpy")

fig, axs=plt.subplots(1,2,figsize=(25,12))
x_val=np.arange(-1.2,1.3,0.1)
axs[0].plot(x_val,J_(x_val),label='J function')
axs[1].plot(x_val,J_(x_val),label='J function')

#函数微分
dy=sympy.diff(J)
dy_=sympy.lambdify(x,dy,"math")

#初始化
x_0=1 #初始化起点
a=0.1 #配置学习率
iteration=15 #初始化迭代次数

axs[0].scatter(x_0,J_(x_0),label='starting point')
axs[1].scatter(x_0,J_(x_0),label='starting point')

#根据梯度下降公式迭代计算
for i in range(iteration):
    if i==0:
        x_next=x_0-a*dy_(x_0)
    x_next=x_next-a*dy_(x_next)    
    axs[0].scatter(x_next,J_(x_next),label='epoch=%d'%i)

#调整学习率，比较梯度下降速度
a_=0.2
for i in range(iteration):
    if i==0:
        x_next=x_0-a_*dy_(x_0)
    x_next=x_next-a_*dy_(x_next)    
    axs[1].scatter(x_next,J_(x_next),label='epoch=%d'%i)
    
axs[0].set(xlabel='x',ylabel='y')
axs[1].set(xlabel='x',ylabel='y')
axs[0].legend(loc='lower right', frameon=False)  
axs[1].legend(loc='lower right', frameon=False)  
plt.show()  
```


    
<a href=""><img src="./imgs_pyd/065.png" height="auto" width="auto" title="digit-x"></a>

    


上一部分代码是给定了回归系数的函数，其系数为1，通过梯度下降算法来查看梯度下降的趋势变化，需要注意的是上述所定义的$J(x)= x^{2} $函数，是代表损失函数（可以写成$J( \beta )=  \beta ^{2} $，$ \beta$是回归系数），不是模型（例如回归方程）。是由最初对损失函数求微分，并另结果为0，求回归系数，调整为在损失函数曲线上先求某点微分，找到该点的下降方向和大小（向量），并乘以学习率（调整下降速度），然后根据该向量移到下一个点，以此类推，直至找到下降趋势（梯度变化）趋近于0的位置，这个位置就是所要求的模型系数。根据上述表述完成下述代码，即先定义模型，这个模型是回归模型，为了和上述损失函数的模型区别开来，定义其为：$y= \omega  x^{2} $，并假设$\omega=3$，来建立数据集，解释变量$X$，和响应变量$y$。定义模型的函数为`model_quadraticLinear(w,x)`，使用sympy库辅助表达和计算。有了模型之后，就可以根据模型和数据集计算损失函数，这里用MSE作为损失函数，计算结果为：$MSE=1.75435200000001 (0.333333333333333w-1)^{2} +0.431208 (0.333333333333333w-1)^{2} $。然后定义梯度下降函数，就是对MSE的$\omega$求微分，加入学习率后计算结果为：$G=0.0728520000000003w - 0.218556000000001$，准备好了这三个函数，就可以开始训练模型，顺序一次为定义函数->定义损失函数->定义梯度下降->指定$\omega=5$初始值，用损失函数计算残差平方和即MSE->比较MSE是否满足预设的精度'accuracy=1e-5'，如果不满足开始循环->由梯度下降公式计算下一个趋近于0的点，并在计算MSE，并比较MSE是否满足要求，周而复始->直至'L<accuracy'，达到要求，跳出循环，此时'w_next'即为模型系数$\omega$的值。计算结果为'w=3.008625'，约为3，正是最初用于生成数据集所假设的值。


```python
# 定义数据集，服从函数y= 3*x**2，方便比较计算结果
import sympy
from sympy import pprint
import numpy as np
x=sympy.symbols('x')
y=3*x**2
y_=sympy.lambdify(x,y,"numpy")

X=np.arange(-1.2,1.3,0.1)
y=y_(X)
n_size=X.shape[0]

#初始化
a=0.15 #配置学习率
accuracy=1e-5 #给出精度
w_=5 #随机初始化系数

#定义模型
def model_quadraticLinear(w,x):
    '''定义一元二次方程，不含截距b'''    
    return w*x**2

#定义损失函数
def Loss_MSE(model,X,y):
    '''用均方误差（MSE）作为损失函数'''
    model_=sympy.lambdify(x,model,"numpy")
    loss=(model_(X)-y)**2
    return loss.sum()/n_size/2
    
#定义梯度下降函数，是对损失函数求梯度
def gradientDescent(loss,a,w):
    '''定义梯度下降函数，即对模型变量微分'''
    return a*sympy.diff(loss,w)

#训练模型
def train(X,y,a,w_,accuracy):
    '''根据精度值，训练模型'''
    x,w=sympy.symbols(['x','w'])
    model=model_quadraticLinear(w,x)
    print("定义函数：")
    pprint(model)
    loss=Loss_MSE(model,X,y)
    print("定义损失函数：")
    pprint(loss)
    grad=gradientDescent(loss,a,w)
    print("定义梯度下降：")
    pprint(grad)
    print("_"*50)
    grad_=sympy.lambdify(w,grad,"math")
    w_next=w_-grad_(w_)
    loss_=sympy.lambdify(w,loss,"math")
    L=loss_(w_next)
    
    i=0
    print("迭代梯度下降，直至由损失函数计算的结果小于预设的值，w即为权重值（回归方程的系数）")
    while not L<accuracy:
        w_next=w_next-grad_(w_next)
        L=loss_(w_next)
        if i%10==0: 
            print("iteration:%d,Loss=%.6f,w=%.6f"%(i,L,w_next))
        i+=1
        #if i%100:break
    return w_next
w_next=train(X,y,a,w_,accuracy)
```

    定义函数：
       2
    w⋅x 
    定义损失函数：
                                              2                                   
    1.75435200000001⋅(0.333333333333333⋅w - 1)  + 0.431208⋅(0.333333333333333⋅w - 
    
      2
    1) 
    定义梯度下降：
    0.0728520000000003⋅w - 0.218556000000001
    __________________________________________________
    迭代梯度下降，直至由损失函数计算的结果小于预设的值，w即为权重值（回归方程的系数）
    iteration:0,Loss=0.717755,w=4.719207
    iteration:10,Loss=0.158109,w=3.806898
    iteration:20,Loss=0.034829,w=3.378712
    iteration:30,Loss=0.007672,w=3.177746
    iteration:40,Loss=0.001690,w=3.083424
    iteration:50,Loss=0.000372,w=3.039154
    iteration:60,Loss=0.000082,w=3.018377
    iteration:70,Loss=0.000018,w=3.008625
    

### 4.2 自动求梯度(gradient)
在上述前向和反向传播中，反向传播的梯度计算是应用sympy函数的diff方法求得导函数计算。因为在深度学习中经常要对函数求梯度，因此pytorch提供了`autograd`，可以根据输入和前向传播过程自动构建计算图，并执行反向传播。在定义张量时，如果配置参数`requires_grad=True`，将追踪其上的所有操作，因此可以应用链式法则进行梯度传播计算。完成前向传播计算后，可以调用`.backward()`来完成梯度计算。梯度将累积到.grad属性中。如果不想被追踪，可以执行`.detach()`将其从追踪记录中剥离开来，或者使用`with torch.no_grad()`将不想追踪的操作代码包裹起来（例如在评估模型时）。

注意：grad在反向传播过程中是累加的(accumulated)，一般在反向传播之前把应用`x.grad.data.zero_()`梯度清零。


> 梯度,gradient，一种关于多元导数的概况。一元（单变量）函数的导数是标量值函数，而多元函数的梯度是向量值函数。多元可微函数$f$在点$P$上的梯度，是以$f$在$P$上的偏导数为分量的向量。一元函数的导数表示这个函数图形切线的斜率，如果多元函数在点$P$上的梯度不是零向量，则它的方向是这个函数在$P$上最大增长的方向，它的量是在这个方向上的增长率。


```python
import torch
#01-定义包含梯度追踪的张量
x = torch.tensor([3.0, 2.0, 1.0, 4.0], requires_grad=True) #x.requires_grad_(True)的方式可以将未指定requires_grad = False的张量转换为追踪梯度传播
print("01-定义包含梯度追踪的张量:")
print("x={}\nx.grad_fn={}\nx.requires_grad={}".format(x,x.grad_fn,x.requires_grad))
print("_"*50)

#02-追踪运算的梯度
print("02-追踪运算的梯度:")
y=(x+1)**3+2
z=y.view(2,2)
print("z={}\nz.grad_fn={}".format(z,z.grad_fn))
print("x.is_leaf={},y.is_leaf={},z.is_leaf={}".format(x.is_leaf,y.is_leaf,z.is_leaf))
print("_"*50)

#03-z(张量)关于x的梯度
print("03-z(张量)关于x的梯度:")
v=torch.tensor([[1.0, 0.1], [0.01, 0.001]],dtype=torch.float)
z.backward(v) #因为z不是标量，在调用backward时需要传入一个和z同形状的权值向量（权值）进行加权求和得到一个标量
print("dz/dx={}".format(x.grad)) #x.grad是和x同形的张量
print("_"*50)

#04-中断梯度追踪
print("04-中断梯度追踪:")
x_1=torch.tensor(1.0,requires_grad=True)
y_1=x_1**2
with torch.no_grad():
    y_2=x_1**3
y_3=y_1+y_2
print("x.requires_grad={}\ny_1={};y_1.requires_grad={}\ny_2={};y_2.requires_grad={}\ny_3={};y_3.requires_grad={}\n".format(x.requires_grad,y_1,y_1.requires_grad,y_2,y_2.requires_grad,y_3,y_3.requires_grad))
y_3.backward()
print("dy_3/dx={}".format(x_1.grad)) #y_2梯度并未被追踪，与y_2有关的梯度并不会回传
print("_"*50)

#05-不影响反向传播下修改张量值，tensor.data
print("05-不影响反向传播下修改张量值，tensor.data:")
x_2=torch.ones(1,requires_grad=True)
print("x_2.data={}\nx_2.data.requires_grad={}".format(x_2,x_2.data.requires_grad))
y_4=2*x_2
x_2.data*=100 #只改变了值，不会记录在计算图中（autograd记录），不影响梯度传播
y_4.backward()
print("x_2={}\nx_2.grad={}".format(x_2,x_2.grad))
```

    01-定义包含梯度追踪的张量:
    x=tensor([3., 2., 1., 4.], requires_grad=True)
    x.grad_fn=None
    x.requires_grad=True
    __________________________________________________
    02-追踪运算的梯度:
    z=tensor([[ 66.,  29.],
            [ 10., 127.]], grad_fn=<ViewBackward0>)
    z.grad_fn=<ViewBackward0 object at 0x000001A51EF00A60>
    x.is_leaf=True,y.is_leaf=False,z.is_leaf=False
    __________________________________________________
    03-z(张量)关于x的梯度:
    dz/dx=tensor([48.0000,  2.7000,  0.1200,  0.0750])
    __________________________________________________
    04-中断梯度追踪:
    x.requires_grad=True
    y_1=1.0;y_1.requires_grad=True
    y_2=1.0;y_2.requires_grad=False
    y_3=2.0;y_3.requires_grad=True
    
    dy_3/dx=2.0
    __________________________________________________
    05-不影响反向传播下修改张量值，tensor.data:
    x_2.data=tensor([1.], requires_grad=True)
    x_2.data.requires_grad=False
    x_2=tensor([100.], requires_grad=True)
    x_2.grad=tensor([2.])
    

## 5. 用PyTorch构建多层感知机（多层神经网络）
#### 5.1-自定义激活函数、模型、损失函数及梯度下降法
PyTorch自动求梯度的方式，让神经网络模型的构建变得轻松，可以将更多的注意力放在模型的构建上，而不是梯度的计算上。上述实现了典型三层神经网络的逐步计算、以及代码整合，对应输入输出值，权值和偏置、激活函数、损失函数、模型结构以及权值更新（优化算法：梯度下降法）保持不变，应用PyTorch分别重新定义。从迭代计算结果来看，因为偏置值也得以更新，损失函数下降的比较快，迅速的收敛。


```python
import torch
import numpy as np

#A-训练数据
X=torch.tensor([0.05,0.1])
y=torch.tensor([0.01,0.9])

#B-定义模型参数
num_inputs,num_outputs,num_hiddens=2,2,2

W1=torch.tensor([[0.15, 0.2], [0.25, 0.3]]) #hiddenLayer_weights
b1=torch.tensor([0.35,0.35]) #hiddenLayer_bias
W2=torch.tensor([[0.4, 0.45], [0.5, 0.55]]) #outputLayer_weights
b2=torch.tensor([0.6,0.6]) #outputLayer_bias
params=[W1, b1, W2, b2]
for param in params:
    param.requires_grad_(requires_grad=True)

#C-定义激活函数
def sigmoid(X):
    return 1/(1+torch.exp(-X))

#D-定义模型
def net(X):
    X=X.view((-1,num_inputs))
    H=sigmoid(torch.matmul(X,W1)+b1)
    return torch.matmul(H,W2)+b2

#F-定义损失函数
def loss(y_hat,y):
    return 0.5*(y_hat-y)**2

#G-优化算法
def sgd(params,lr):
    for param in params:
        param.data-=lr*param.grad

#H-训练模型
def train(net,X,y,loss,num_epochs,params=None,lr=None,optimizer=None):
    train_l_sum, train_acc_sum, n = 0.0, 0.0, 0
    for epoch in range(num_epochs):
        y_hat=net(X)
        l=loss(y_hat,y).sum()
        #梯度清零
        if params is not None and params[0].grad is not None:
            for param in params:
                param.grad.data.zero_()        
        l.backward()
        sgd(params,lr)
        print('epoch %d, loss %.9f'%(epoch+1,l))
    return net,params
        
num_epochs, lr=5, 0.5
net_,params_=train(net,X,y,loss,num_epochs,params,lr)
print("应用训练好的模型验证预测值：\nnet_(x)={}".format(net_(X)))
print("参数(权值+偏置)更新结果：\nparams_={}".format(params_))
```

    epoch 1, loss 0.677512288
    epoch 2, loss 0.013031594
    epoch 3, loss 0.000361601
    epoch 4, loss 0.000010227
    epoch 5, loss 0.000000290
    应用训练好的模型验证预测值：
    net_(x)=tensor([[0.0101, 0.9000]], grad_fn=<AddBackward0>)
    参数(权值+偏置)更新结果：
    params_=[tensor([[0.1463, 0.1954],
            [0.2427, 0.2907]], requires_grad=True), tensor([0.2770, 0.2573], requires_grad=True), tensor([[0.0102, 0.3532],
            [0.1094, 0.4529]], requires_grad=True), tensor([-0.0585,  0.4366], requires_grad=True)]
    

#### 5.2-直接使用PyTorch提供的函数
PyTorch已经内置了多种激活函数、损失函数、优化算法以及各类模型算法，可以直接调用参与计算。因为延续了以上的典型三层神经网络，假设的输入输出值比较简单，因此损失函数仍然采取自定义的平方误差形式。


```python
import torch
from torch import nn

#A-训练数据
X=torch.tensor([0.05,0.1])
y=torch.tensor([0.01,0.9])

#B-定义模型参数:权值和偏置此次随机初始化
num_inputs,num_outputs,num_hiddens=2,2,2
    
#C-定义模型
net=nn.Sequential(
    nn.Linear(num_inputs,num_hiddens),
    nn.Sigmoid()
    )

#D-初始化参数：可以省略，pytorch定义模型时，已经初始化
#for params in net.parameters():
    #nn.init.normal_(params,mean=0,std=0.01)

#E-定义损失函数
def loss(y_hat,y):
    return 0.5*(y_hat-y)**2

#F-优化算法
optimizer=torch.optim.SGD(net.parameters(),lr=0.5)

#G-训练模型
def train(net,X,y,loss,num_epochs,params=None,lr=None,optimizer=None):
    train_l_sum, train_acc_sum, n = 0.0, 0.0, 0
    for epoch in range(num_epochs):
        y_hat=net(X)
        l=loss(y_hat,y).sum()
        #梯度清零
        if optimizer is not None:
            optimizer.zero_grad()
        elif params is not None and params[0].grad is not None:
            for param in params:
                param.grad.data.zero_()        
        #自动求梯度
        l.backward()
        optimizer.step()
        
        if epoch%1000==0:
            print('epoch %d, loss %.9f'%(epoch+1,l))
    return net,params


num_epochs=10000
net,params=train(net,X,y,loss,num_epochs,params,lr,optimizer)
print("应用训练好的模型验证预测值：\nnet_(x)={}".format(net_(X)))
print("参数(权值+偏置)更新结果：\nparams_={}".format(params_))
```

    epoch 1, loss 0.243969083
    epoch 1001, loss 0.000377767
    epoch 2001, loss 0.000147044
    epoch 3001, loss 0.000081489
    epoch 4001, loss 0.000052245
    epoch 5001, loss 0.000036312
    epoch 6001, loss 0.000026573
    epoch 7001, loss 0.000020157
    epoch 8001, loss 0.000015700
    epoch 9001, loss 0.000012480
    应用训练好的模型验证预测值：
    net_(x)=tensor([[0.0101, 0.9000]], grad_fn=<AddBackward0>)
    参数(权值+偏置)更新结果：
    params_=[tensor([[0.1463, 0.1954],
            [0.2427, 0.2907]], requires_grad=True), tensor([0.2770, 0.2573], requires_grad=True), tensor([[0.0102, 0.3532],
            [0.1094, 0.4529]], requires_grad=True), tensor([-0.0585,  0.4366], requires_grad=True)]
    

## 6. [Ecosystem Tools](https://pytorch.org/ecosystem/)+[开放神经网络交换-ONNX(Open Neural Network,Exchage)](https://onnx.ai/supported-tools.html)
