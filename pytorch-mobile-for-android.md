---
title: 'PyTorch mobile for android'
date: 2020-07-11 20:22:44
tags: []
published: false
hideInList: false
feature: 
isTop: false
---
There are some bugs in videos not shown!
## My github repo:


I'm a Chinese guy! I'll try to tell you all my research process. And I will tell you how to install a demo app in English.

1. 资料收集(not so related about the app, so I'll be in Chinese)：

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