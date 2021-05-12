# Transformer的基本架构

![Transformer](image/Transformer%20framework.png)


## 多头注意力
自注意力：$Attention(Q,K,V)=softmax(\frac{QK^{T}}{\sqrt{d_k}})V$

这里的$\sqrt{d_k}$是缩放操作，目的是防止某个输入比较大导致softmax几乎将全部的概率分布都分配给了最大值对应的标签，而其它标签的概率几乎为0，导致参数更新变得困难。

多头注意力机制：将$Q,K,V$进行多次线性变化，得到多个不同的$Q',K',V'$,然后分别进行自注意力编码过程，并将输出进行拼接；最后通过一个线性变换输出。

## Add & Norm
Add操作是引入残差结构，这种结构能够缓解梯度消失，便于模型优化。Norm是Layer Normalization过程，能够缓解梯度弥散，同样是为了更好地进行网络训练的常用手段。

## Feed Forward
Transformer的前馈神经网络采用了两个线性变换，激活函数为Relu,公式如下：
$$ FFN(x)=Relu(0,xW_1+b_1)W_2+b_2 $$


# Transformer QA

* Transformer的位置编码
  
    \>>方法一：Position Embeddings（可选的一种方法）

    参照词向量矩阵的方式，可以简单通过位置向量矩阵将位置（1,2,...,n)转换为对应的位置向量。这种方式存在一个问题，即在训练的过程中需要让模型见过所有的需要在预测阶段使用的位置向量，否则模型就不知道相应位置的向量。

    方法二：Position Encodings（Transformer中使用的方法）

    位置编码的方法不通过位置向量矩阵实现位置到对应向量的转换，它通过选择一个函数来生成每个位置的向量。这样做的好处是，对于一个选择的比较好的函数，网络模型能够处理那些在训练阶段没有见过的序列位置的向量，即比较通用。Transformer中使用的位置编码函数为：

    $PE_{(pos, 2i)} = sin(pos/10000^{2i/dim})$

    $PE_{(pos, 2i + 1)} = cos(pos/10000^{2i/dim})$

    其中pos是对应的词序列位置，i是对应的词向量的下标，dim是词向量维度的大小。

* 为什么Position Embedding采用正余弦函数？[<sup>1</sup>](#参考文献)

    \>>因为有
    $$PE(pos+k)=PE(pos)+PE(k)$$
    ,这样使得模型能够记住相对位置信息。


* Transformer结构中残差连接的作用是什么？

    \>\>缓解梯度消失问题。

* Transformer中的残差连接为什么能缓解梯度消失问题？[<sup>2</sup>](#残差网络-1)
  
    \>>通过在一个浅层网络基础上叠加$y=x$的层（称为恒等映射），可以让网络随深度增加而不退化。同时，由于浅层的输入值直接连接到了端部的位置，避免了在层层映射过程中，由于权重小于1而导致的梯度消失现象。

* Norm层的作用是什么？

    \>>防止梯度弥散（梯度爆炸或梯度消失）。

* Transformer中mask操作的使用方式？
\>> Mask操作是对某些值进行掩盖，使得其在更新参数时不产生效果。在Transformer中，通过将需要被mask的位置设置为负无穷，这样在后面的Softmax后，这些位置的概率就接近于0，就不会对结果产生影响了。

    Transformer本身的mask操作分为两部分，padding mask与sequence mask.

    Padding mask:考虑到每个批次中输入序列的长度是不一样的，而为了进行批量化训练，我们往往对长度不足的文本进行padding，而这些padding的词其实是没有意义的，因此我们在进行self-attention的时候应该忽略它。

    Sequence mask:在Decoder中，当前的输出应该只依赖于t时刻之前的输入，而不应该依赖于t时刻之后的输入，此时也需要进行mask。具体的做法是产生一个上三角矩阵，上三角的值全为1，下三角的值全为0，对角线也为0.



# 参考文献

<div id="position-encoding-1"></div>

[1] [Transformer为什么选用正余弦函数作为位置编码函数](https://www.zhihu.com/question/347678607)


<div id="残差网络-1"></div>

[2] [残差网络原理](https://blog.csdn.net/LEEANG121/article/details/104171683)

<div id="transformer-1"></div>

[3] [Transformer原理解析](https://mp.weixin.qq.com/s/PtDZqM6IEGrX6juHBrh9uA)