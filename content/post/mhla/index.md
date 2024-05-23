---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "关于 MHLA（Multi-Head Latent Attention）的一些分析"
subtitle: ""
summary: "关于 MHLA的一些分析"
authors: 
- Zhengxiao Du
tags: []
categories: [技术]
date: 2024-05-09T21:26:09+08:00
lastmod: 2024-05-10T17:55:09+08:00

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

原始的 Multi-Head Attention 的计算公式为（单个head，暂时先不考虑 RoPE）

$$
Q_i=W^Q_iH
$$

$$
K_i=W_i^KH
$$
$$
V_i=W^V_iH
$$
$$
O_i=\text{Softmax}(\frac{Q_i^TK_i}{\sqrt{d_h}})V_i
$$
其中$H$ 是 $d\times l$ 的矩阵（$l$ 为序列长度），表示 hidden states， $W^Q_i, W^K_i, W^V_i$ 是 $d_h\times d$的矩阵。

Multi-Query Attention 的计算公式为（单个head，暂时先不考虑 RoPE）
$$
Q_i=W^Q_iH
$$

$$
K=W^KH
$$
$$
V=W^VH
$$
$$
O_i=Softmax(\frac{Q_i^TK}{\sqrt{d_h}})V_i
$$
最大的区别在于 $K,V$ 是所有 head共享的，因此能够减少KV Cache的显存占用。其中
$$
Q_i^TK=H^T(W_i^Q)^TW^KH
$$
Multi-Head Latent Attention 的计算公式为（单个head，暂时先不考虑 RoPE）
$$
C^Q=W^{DQ}H
$$
$$
Q_i=W^{UQ}_iC^Q
$$
$$
C^{KV}=W^{DKV}H
$$
$$
K_i=W_i^{UK}C^{KV}
$$
$$
V_i=W_i^{UV}C^{KV}
$$
$$
O_i=Softmax(\frac{Q_i^TK_i}{\sqrt{d_h}})V_i
$$
其中 $W^{DQ}, W^{DKV}\in \mathbb{R}^{d_c\times d}$，$W_i^{UQ},W_i^{UK},W_i^{UV}\in\mathbb{R}^{d_h\times d_c}$

单独看 Attention 计算的前一部分，其中 
$$
Q_i^TK_i=H^T(W^{DQ})^T(W_i^{UQ})^TW^{UK}_iW^{DKV}H
$$
令 $W_i^Q=(W_i^{UK})^TW_i^{UQ}W^{DQ}$，我们有
$$
Q_i^TK_i=H^T(W_i^Q)^TW^{DKV}H
$$
可以看到这一计算公式和 Multi-Query Attention 其实是一样的，都是使用的单独的 $Q$ 和共享的 $K$。区别在于，这里 $W_i^QH,W^{DKV}H\in\mathbb{R}^{d_c\times l}$。也就是说在进行 attention 计算的时候，向量点积的维度是 $d_c$  而不是 $d$。在论文中实际设置的是 $d_c=4d$。**也就是说 Multi-Head Latent Attention 其实是 head dimension 提高到4倍的 Multi-Query Attention**。在论文中也提到了在 inference 的时候 absorb $W^{UK}$ into $W^{UQ}$，其实就代表了这里的结合方式。因为每个head的维度提高了，所以能够计算出更加复杂的 attention分布，从而相比起 Multi-Query Attention 取得性能提升。相比起直接提高 head dimension，其优点在于所有head的 $W^{DQ},W^{UQ},W^{UK}$的总参数量是 $d\cdot d_c+d \cdot d_c+ d \cdot d_c=3d\cdot d_c=12d\cdot d_h$，而所有 head 的 $W^Q$ 的参数量是 $d \cdot d_c\cdot n_h=4d^2$，节省了参数量。也就是说对 $W^Q$ 做了一个低秩分解。

但是这个提升并不是 free lunch，因为 head dimension 提高意味着 attention 的计算量也提高，而 attention 的计算量是 $O(l^2)$ 的。为了处理长文本，现在大家一般都倾向于尽可能降低 attention 计算量的常数，而这个方法是会增加常数的。

以上分析没有考虑 RoPE，如果考虑 RoPE 的话，每个 head 的维度会从 $4d$ 变成 $4.5d$，其中$4d$是没有 positional encoding的，$0.5d$ 是使用 RoPE encoding的。其实 ChatGLM2-6B 中已经使用过类似的[做法](https://huggingface.co/THUDM/chatglm2-6b/blob/main/modeling_chatglm.py#L161)，即只在一半的 head dimension 上使用 RoPE ，目的是为了把 attention 计算分成位置相关和位置无关的两部分，与性能提升的关系并不大。