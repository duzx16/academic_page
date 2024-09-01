---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "从零学习 Vector Quantization"
subtitle: ""
summary: "从零学习 Vector Quantization"
authors: 
- Zhengxiao Du
tags: []
categories: [技术]
date: 2024-09-01T00:00:00+08:00
lastmod: 2024-09-01T00:00:00+08:00

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one **or** more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

最近因为研究需要学习了 vector quantization（VQ）并且实际训练了一些带有 VQ bottleneck 的模型。在这里记录一下学习和训练的心得。

## 什么是 VQ

我们知道神经网络强大的表示能力很大程度上来自于其使用的连续向量表示，以及依赖于连续向量表示的梯度下降优化算法。而 vector quantization 则是将神经网络中某一层的连续向量表示替换为离散变量。具体方式为有一个词表（codebook） $e\in R^{K\times D}$，其中$K$ 是词表大小，$D$ 是向量维度。对于神经网络中某一层的输出 $z\in R^{D}$，我们希望找到一个词表中的向量 $e_i$ 使得 $e_i$ 与 $z$ 的距离最小，即 $i = \arg\min_j \|x - e_j\|_2$。这样，我们就可以将 $e_i$ 作为 $z$ 的近似，称为 $z_q$，继续参与后面的运算。这种替换方式可以看作是一种量化（quantization）操作，因此得名 vector quantization。

## 为什么需要 VQ

VQ 最为知名的应用就是 VQVAE，常常被用于将原始图片编码为离散的 token 序列。但是实际上 VQ 可以用于任何神经网络中，并不局限于 AutoEncoder。

VQ 可以看作是 Transformer 中 embedding 层的逆运算。Embedding 层的作用就是将神经网络不好处理的离散变量转化为连续向量表示，为什么我们还需要反过来呢？离散变量的优点在于：

1. 更适合作为生成任务的目标。我们已经知道自回归 Transformer 能够很好地建模自然语言这样的离散序列。而连续向量序列，比如原始的图片、音频等，往往需要 diffusion model 这样更复杂的生成模型来建模。
2. 节省存储空间。连续变量 $z$ 需要存储 $D$ 个浮点数，而离散变量 $e_i$ 只需要存储一个整数。


## VQ 的训练

可以看出带有 VQ bottleneck 的优化并不容易。原因在于将 $z$ 转化为 $z_q$ 时存在一个 $\arg\min$ 操作，它是不可导的。

为了方便，我们把神经网络在 VQ bottleneck 前的部分看作 encoder，其作用是将输入 $x$ 转化成连续向量 $z$，把 VQ bottleneck 后的部分称为 decoder，其作用是从量化后的向量 $z_q$ 中解码出需要的输出。带有 VQ bottleneck的神经网络的参数分为三部分，即 encoder 的参数 $\theta_{\text{enc}}$，decoder 的参数 $\theta_{\text{dec}}$，以及词表 $e$。decoder 的参数的优化不存在任何问题，因为它是以 $z_q$ 作为输入的，$z_q$ 是怎么来的与它无关，只需要优化神经网络原来的 loss即可。而 encoder 的参数则问题很大，因为它以 $z$ 作为输出， decoder 的梯度回传只能到 $z_q$，而 $z$ 到 $z_q$ 是不存在梯度的。在VQVAE中提出了一个近似方法，即用 $z_q$ 的梯度作为 $z$ 的梯度。原因在于两者的维度都是 $D$，并且 $z_q$ 是 $z$ 的近似，因此它们的梯度应该是相似的。给定 decoder 的输出目标 $y$，假设原来的 loss 为 $l(y, \text{Dec}(z))$，则加上 VQ bottleneck 之后的 loss 为

$$
l(y,\text{Dec}(z+sg(z_q-z)))
$$

其中 $sg$ 表示 stop gradient，即在 backward 的时候不计算着一部分的梯度。在 PyTorch 中实现这个近似也比较容易，只要用 `z+(z_q-z).detach()` 替代 `z_q` 进行后续的 forward 计算即可。

通过以上方法我们优化了 encoder 和 decoder 部分，但是还没有优化 VQ 过程。VQ 过程的优化目标是使量化后的表示 $z_q$ 尽可能地接近 $z$。因此我们直接优化 $z_q$ 和 $z$ 的 mean-squared-error loss，即 $\|z_q-z\|_2^2$。这个 loss 可以进一步拆分为两部分，即 $\|z_q-sg(z)\|_2^2$ 和 $\|z-sg(z_q)\|_2^2$。前者使得 codebook 中离 $z$ 最近的向量进一步接近 $z$，后者使得 $z$ 进一步接近 codebook 中离它最近的向量。相对来说，我们更希望 codebook 中的向量去接近 $z$，因此前者的权重更大一些。

最终我们优化的 loss 为
$$
l(y, \text{Dec}(z+sg(z_q-z)))+\alpha \|z_q-sg(z)\|_2^2+\beta \|z-sg(z_q)\|_2^2
$$

在整个 loss 中，只有第二项的 loss 与 codebook 的更新有关，其实也可以直接得到一个 closed-form optimal。假设 $\{z_{i,1},z_{i,2},\cdots,z_{i,n_i}\}$ 为 encoder 输出中离散近似为 $e_i$ 的 $n_i$个向量，则根据 MSE loss 的性质 $e_i$ 最优解为
$$
e_i = \frac{1}{n_i}\sum_{j=1}^{n_i}z_{i,j}
$$

不过实际中我们都是使用 minibatch 进行训练，在每一步优化时只能看到 batch 中的样本。因此我们使用 exponential moving average（EMA）来进行更新
$$
N_i^{(t)}=N_i^{(t-1)}*\gamma+n_i^{(t)}(1-\gamma)\\
m_i^{(t)}=m_i^{(t-1)}*\gamma+\sum_jz_{i,j}^{(t)}(1-\gamma)
$$

使用EMA更新 codebook 时，loss可以改为
$$
l(y, \text{Dec}(z+sg(z_q-z))+\beta \|z-sg(z_q)\|_2^2
$$



## VQ 的训练心得

1. codebook 的初始化非常重要

无论是否使用 EMA，codebook 中都只有被激活的向量才会被更新。因此如果 codebook 中某些向量从一开始就没有激活，那么这些向量就一直不会被更新，导致使用的 codebook 大小过小。之前很多研究表明预训练模型中连续向量的分布并不是在整个欧几里得空间上均匀的，而是集中分布在一个较小的区域。因此完全随机初始化的 codebook 往往只有一小部分向量会被激活。


2. 超参数选择的意义

使用 EMA 更新的方法相当于直接计算了 codebook 最优解，收敛速度会更快。$\gamma$ 决定了 codebook 更新的速率，$\gamma$ 越小 codebook 更新越快。

系数为 $\beta$ 的 loss 项其实是对 $z$ 的一个正则项。$z$ 的复杂度是由 encoder 的参数量决定的，而 $z_q$ 的复杂度是由 codebook 的大小决定的，实际中 $z$ 的复杂度一般远大于 $z_q$。如果不加约束的话 $z_q$ 永远也追不上 $z$。而用 $z_q$ 的梯度近似 $z$ 的梯度要求 $z$ 和 $z_q$ 必须非常接近。因此需要用 $\beta$ 来约束 $z$ 的复杂度。

3. 如何防止 codebook collapse
   
在学习过程中 $z$ 的分布是始终在变化中的，即使初始化的时候 codebook 中的大部分向量都能被激活，随着训练的进行，codebook 中被激活的向量也可能会越来越少。这个现象被称为 codebook collapse。通过仔细设置 $\gamma$, $\beta$ 和 学习率可以缓解这一问题，但是会非常麻烦。

另一个想法是，我们可以重新初始化 codebook 中长时间没有被激活的向量，这个想法我是在 [Jukebox: A Generative Model for Music](http://arxiv.org/abs/2005.00341) 中看到的。使用 EMA 更新的时候 $N_i^{(t)}$ 实际上已经记录了向量 $e_i$ 在过去被激活次数的 exponential moving average，可以用来判断有哪些向量长期没有被激活。至于如何重新初始化这些向量，我们希望重新初始化之后这些向量能够被激活，因此我们可以用当前 batch 中的 $z$ 来重新初始化这些向量。
