---
title: 'Teach you PyTorch mobile for android by creating a demo app hand-by-hand'
date: 2020-07-11 20:22:44
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
## There are some ** bugs** in videos not shown!
## My github repo:
https://github.com/StevenJokes/pytorch-andriod-greatdemo
## My blog:
https://blog.dltech.xyz/post/pytorch-android/

# Part One:
I'm a Chinese guy and a Win10 user! So if you can't understand my sentences, give me an issue and I will do my best to help you. Linux users can also get help from my article too.

I'll share you all my research process. (It cost me two days and two nights..) And I will tell you **how to have a demo app for android in English**.

I'm new to Android and PyTorch too. If you are learning or have mastered in them, you can telegram me and maybe we will become good friends. And I'm applying for a job now, please email me at llgg8679@qq.com if you like me.

It's my first share about a newest tech. *Please please please!* give my repo a **star** and **fork** if you find it helpful.


## What we need:

1. Anaconda(you could use miniconda to `conda install` too).
2. Git (bet you guys all have it)for `git clone https://github.com/pytorch/android-demo-app`
3. Android Studio 3.5.1+ ，Android SDK and Android NDK (For win10 users, I suggest to use chocolaty to install, see the following "Install android dev".) which are required from https://pytorch.org/mobile/android/.
4. Watch again: https://www.youtube.com/watch?v=O_2KBhkIvnc by 0.1 speed.

## Create pytorch-nightly environment

In your cmd, create pytorch-nightly environment called pytorch_n(It is necessary to use `pytorch.jit`)

```cmd
conda create -n pytorch_n 
conda activate pytorch_n
```

I only have cpu (if you are rich, please help me to get my own GPU workstation.) You can find GPU pytorch-nightly version to install in https://pytorch.org/get-started/locally/.
```cmd
conda install pytorch torchvision cpuonly -c pytorch-nightly -y
```

## Git clone

In your cmd,
```cmd
git clone https://github.com/pytorch/android-demo-app
```
## Open folder

Then open your "HelloWorldApp" (It is necessary to use `pytorch.jit.save`, or you will get error.)  folder in android-domo-app by `cd` or clicking.

Here, I used VScode to open "HelloWorldApp" folder.
(Thanks for "you need to have the same module/file/dir heirarchy or else it will crash." in https://github.com/pytorch/pytorch/issues/31620)

## Save my model

Open the trace_model.py, and save it as trace_model_me.py.

I read https://pytorch.org/mobile/android/ and  https://pytorch.org/docs/master/generated/torch.jit.save.html#torch.jit.save.I use `m` to replace `traced_script_module`. I named my model as model_me.pt.

```python
import torch
import torchvision

model = torchvision.models.resnet18(pretrained=True)
model.eval()
example = torch.rand(1, 3, 224, 224)
m = torch.jit.trace(model, example) #  
m.save("app/src/main/assets/model_me.pt")
```

Must in "HelloWorldApp"!![](https://blog.dltech.xyz/post-images/1594475049132.png)
We can see how many times I tried!(Cost me 6 hours. cry!)
![tried](https://blog.dltech.xyz/post-images/1594474096806.png)

If you are in android-demo-app,error wil get you crazy.
![Error](https://blog.dltech.xyz/post-images/1594474241776.png)

## Install android dev

 `choco install  android-sdk, android-ndk, androidstudio` 

My version: android-sdk 26.1.1, android-ndk 21.4, androidstudio 4.0.0.16.

Then open "android Studio". I'm using proxy to get quicker network.

![proxy](https://blog.dltech.xyz/post-images/1594475620751.png)
![jre](https://blog.dltech.xyz/post-images/1594475671153.png)
![components](https://blog.dltech.xyz/post-images/1594475715350.png)

Here, I didn' install Virtual Device. We can install it when we need. I used it in my following step.

## Create New Project

![empty activity](https://blog.dltech.xyz/post-images/1594475886535.png)
![configure](https://blog.dltech.xyz/post-images/1594475910656.png)
![enable](https://blog.dltech.xyz/post-images/1594475946465.png)

---

#Part Two:
## build.gradle appear!

Then I found I don't have "build.gradle(Module: app)"

Then I learnt some knowledge : https://developer.android.com/training/basics/firstapp/creating-project
Gradle Scripts > build.gradle
There are two files with this name: one for the project, "Project: My First App," and one for the app module, "Module: app." Each module has its own build.gradle file, but this project currently has just one module. Use each module's build.file to control how the Gradle plugin builds your app. For more information about this file, see Configure your build.

![sync](https://blog.dltech.xyz/post-images/1594795763502.png)
![sync_success](https://blog.dltech.xyz/post-images/1594795946268.png)
![appear](https://blog.dltech.xyz/post-images/1594795976147.png)

## add implementation

`jcenter()` has already appear

```java
implementation'org.pytorch:pytorch_android:1.5.0'
implementation'org.pytorch:pytorch_android_torchvision:1.5.0'
```

![implementation](https://blog.dltech.xyz/post-images/1594796106557.png)

## move some files and rename

![new_dir](https://blog.dltech.xyz/post-images/1594796500856.png)

move
![](https://blog.dltech.xyz/post-images/1594796415709.png)

# Part Three: TODO:tired again. If you need the tutorial, please star and watch the repo. I'll update Part Three (all 5 part), if >20 stars or watch.

## What we got:
![demo app](https://blog.dltech.xyz/post-images/1594476102427.png)



## For more:  资料收集(not so related about the app, so it'll be in Chinese)：

搭个model server，远程call api就好了，目前javascript没有很好的植入，做的还行的就是tf javascript。Torchserve可用来host pt模型
torch mobile模式
来自 <https://www.zhihu.com/question/393499221> 

However if you’re aiming for edge deployment or you want to squeeze as much raw performance as possible, your best bet right now is to use ONNX 32. You can export 5 your PyTorch model in ONNX format and then use another framework (like caffe2, MXNet, CNTK, etc.) to actually run the model, these other frameworks do support edge and/or have specialized deployment extensions (e.g. the MXNet model server 20 which can also serve ONNX models directly).
来自 <https://discuss.pytorch.org/t/pytorch-models-for-production/18115> 

libtorch是深坑，还是onnx靠谱
graphpipe了解一下
PyTorch模型到Android
1.2，使用onnx-tensorflow 项目，再从tensorflow转；
不支持空洞卷积、prelu...放弃了。
1.3，使用MMdnn项目转化为IR，再从IR转换为tensorflow或者keras，再到tflite；
使用命令：
mmtoir -f pytorch -d gemfieldnet --inputShape 3,512,1024 -n model.pth
解决无望！放弃了。
2，Pytorch到NCNN
通过onnx转换，刚开始（2019年1月25日）ncnn不支持upsample，在合并了Gemfield的一些PR后，终于可以转换成功了。详细使用方法，请参考NCNN官方。
推荐方法是使用PytorchConverter github地址：https://github.com/starimeL/PytorchConverter
3，PyTorch到小米的MACE
通过onnx转换，不过目前（2019年1月25日）不支持卷积核的group参数，不支持upsample，放弃。
Pytorch的模型不能直接被MNN进行解析，所以我们这里需要选定一个媒介。参考之前专栏的一篇文章《整合mxnet和MNN的嵌入式部署流程》，这里也采用ONNX进行pytorch和MNN之间的桥梁。
方案一：利用腾讯开源的ncnn库(nihui大神牛皮！！)；但这个适合移动端部署，特别是针对andriod的极致优化。
编译ncnn的准备工作：g++, cmake，protobuf,opencv，
前两个不必说，很容易。其中安装protobuf和opencv都可以分别写一篇博客了。

来自 <https://www.zhihu.com/search?type=content&q=pytorch%20%E9%83%A8%E7%BD%B2%20%E5%B0%8F%E7%A8%8B%E5%BA%8F> 

```
	Use compatible model saving & loading method, e.g.
	# Saving, notice the difference on DataParallel
net_for_saving = net.module if use_nn_DataParallel else net
torch.save(net_for_saving.state_dict(), path)
	# Loading
net.load_state_dict(torch.load(path, map_location=lambda storge, loc: storage))
```
来自 <https://github.com/starimeL/PytorchConverter> 

Pytorch 模型部署到服务器有没有什么好的方案?
Lanking
​
AWS AI算法工程师
torchserve了解一下。也可以用DJL，如果你的服务器是java的。
DJL.ai​DJL.ai

PyTorch项目移植到移动端目前有两种方案：
1. 把PyTorch模型转成专门的部署框架模型，比如NCNN、TVM之类的，可能需要经过ONNX中转。优点是：NCNN、TVM这种专门的部署框架下的模型推理速度比较快；缺点是：转换过程中可能会出现各种操作不支持，需要有解决这些问题的工程能力。
2. 使用PyTorch Mobile，即将PyTorch模型转成TorchScript模型，可以利用PyTorchMobile的java接口调用。这个方法的有点是：TorchScript支持控制流，所以不需要对项目代码做多大修改，就可以迁移到移动端；缺点是：速度拼不过专门的部署框架，如果模型比较大的话，转换之后可能会发现明显的速度下降。
发布于 03-08
来自 <https://www.zhihu.com/search?type=content&q=pytorch%20mobile> 

TorchServe库同时支持Python和TorchScript模型；它可以同时运行一个模型的多个版本，甚至可以在模型存档中回滚到过去的版本。亚马逊工程师在一篇博文中表示，超过80%的使用PyTorch的云机器学习项目是在AWS上进行的。
来自 <https://www.zhihu.com/search?type=content&q=pytorch%20mobile> 

在2019年10月举行的年度PyTorch开发者大会上，Facebook首次介绍了Google Cloud TPU支持和量化以及PyTorch Mobile。
来自 <https://www.zhihu.com/search?type=content&q=pytorch%20mobile> 

另外10分感谢小伙伴的友情指导。（突然发现我司的框架编译难度已经很容易了.......233）
OAID/Tengine​github.com
Tencent/ncnn​github.com
Tencent/FeatherCNN​github.com
XiaoMi/mace​github.com
alibaba/MNN​github.com
已经提前git clone完毕Paddle-Lite的repo，配置好ANDROID_NDK路径（ncnn、mnn、mace、Tengine均能识别）
来自 <https://www.zhihu.com/question/341980046> 

没有人敢用不维护的开源项目，即使你曾经速度最快，功能最强，技术最先进
没有人敢用可能不到一年便不维护的开源项目，即使你现在速度最快，功能最强，技术最先进
来自 <https://www.zhihu.com/question/341980046> 

