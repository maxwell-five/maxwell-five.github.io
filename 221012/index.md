# 深入理解ResNet(一)



## 为什么引入ResNet

在深度学习当中，网络的深度对模型效果影响非常大，如下图（但是越好的模型不只和深度有关）。因为CNN能够提取 low / mid / high-level 的特征，网络的层数越多，意味着能够提取到不同level的特征越丰富。并且，越深的网络提取的特征越抽象，越具有语义信息。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="\images\221012_2.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图一 ImageNet分类排名</div>
</center>

**为什么不能简单的增加网络层数？**

- 网络深度增加时，网络准确度出现饱和，甚至出现下降。也就是网络退化问题（Degradation problem），这不同于`overfitting`。

- 解决方案：对输入数据和中间层的数据进行`归一化`操作(加一个`Batch Normalization`操作。但是，这个方法仅对几十层的网络有用，当网络再往深处走的时候就无能为力了。模型的训练提高需要层数的增加，ResNet的模型方案就很好解决退化问题。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="\images\221012_3.png">
    <br>
</center>

## ResNet是如何解决退化问题

`ResNet`是一种残差网络(Deep residual network),下面是ResNet的一个`Basic block`结构。其中，曲线是加入的`shortcut`。



<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="\images\221012_1.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图三 残差网络单元</div>
</center>

我们基于一个假设：现在你有一个浅层网络，通过向上堆积新层来建立深层网络，一个极端情况是这些增加的层什么也不学习，仅仅复制浅层网络的特征，即这样新层是恒等映射（Identity mapping）。在这种情况下，深层网络应该至少和浅层网络性能一样，也不应该出现退化现象。

残差学习单元中,输出为 `H(x) = F(x) + x`，即 残差函数为`F(x) = H(x) - x` 。只要`F(x)=0`，就构成了一个恒等映射`H(x) = x.`

>假设 输入x=5 ,图中直线部分把5映射为5.1，即 H(x) =5.1,  H(5)=F(5)+5, F(5)=0.1
>
>引入残差后的映射对输出的变化更敏感，比如原来是从5.1到5.2，映射的输出增加了1/51=2%，，而对于残差结构从5.1到5.2，映射F是从0.1到0.2，增加了100%。明显后者输出变化对权重的调整作用更大，所以效果更好。**残差的思想都是去掉相同的主体部分，从而突出微小的变化**。



第一次写DL，对ResNet理解可能不胜到位，待我读一下`Kaiming He`大神的paper [Identity Mappings in Deep Residual Networks](https://arxiv.org/pdf/1603.05027.pdf "Identity Mappings in Deep Residual Networks") 再来补充。


