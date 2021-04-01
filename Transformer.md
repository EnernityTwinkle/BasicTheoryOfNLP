# Transformer的基本架构

![Transformer](image/Transformer%20framework.png)


# Transformer的细节问答

* Transformer的位置编码
  
    \>>方法一：Position Embeddings（可选的一种方法）

    参照词向量矩阵的方式，可以简单通过位置向量矩阵将位置（1,2,...,n)转换为对应的位置向量。这种方式存在一个问题，即在训练的过程中需要让模型见过所有的需要在预测阶段使用的位置向量，否则模型就不知道相应位置的向量。

    方法二：Position Encodings（Transformer中使用的方法）

    位置编码的方法不通过位置向量矩阵实现位置到对应向量的转换，它通过选择一个函数来生成每个位置的向量。这样做的好处是，对于一个选择的比较好的函数，网络模型能够处理那些在训练阶段没有见过的序列位置的向量，即比较通用。Transformer中使用的位置编码函数为：

    $PE_{(pos, 2i)} = sin(pos/10000^{2i/dim})$

    $PE_{(pos, 2i + 1)} = cos(pos/10000^{2i/dim})$

    其中pos是对应的词序列位置，i是对应的词向量的下标，dim是词向量维度的大小。


* Transformer结构中残差连接的作用是什么？

    \>\>缓解梯度消失问题。

* Norm层的作用是什么？

    \>>防止梯度弥散（梯度爆炸或梯度消失）。



* Transformer注意力权重如何计算的？
