---
title: "GLM-130B: An Open Bilingual Pre-Trained Model"
date: 2022-10-05
publishDate: 2022-10-05
authors:
- Aohan Zeng
- Xiao Liu
- admin
- Zihan Wang
- Hanyu Lai
- Ming Ding
- Zhuoyi Yang
- Yifan Xu
- Wendi Zheng
- Xiao Xia
- Weng Lam Tam
- Zixuan Ma
- Yufei Xue
- Jidong Zhai
- Wenguang Chen
- Peng Zhang
- Yuxiao Dong
- Jie Tang
author_notes:
- "Equal contribution"
- "Equal contribution"
publication_types: ["1"] # 1 = Conference paper, 2 = Journal article, 3 = Preprint / working paper
abstract: "We introduce GLM-130B, a bilingual (English and Chinese) pre-trained language model with 130 billion parameters. It is an attempt to open-source a 100B-scale model at least as good as GPT-3 and unveil how models of such a scale can be successfully pre-trained. Over the course of this effort, we face numerous unexpected technical and engineering challenges, particularly on loss spikes and disconvergence. In this paper, we introduce the training process of GLM-130B including its design choices, training strategies for both efficiency and stability, and engineering efforts. The resultant GLM-130B model offers significant outperformance over GPT-3 175B on a wide range of popular English benchmarks while the performance advantage is not observed in OPT-175B and BLOOM-176B. It also consistently and significantly outperforms ERNIE TITAN 3.0 260B -- the largest Chinese language model -- across related benchmarks. Finally, we leverage a unique scaling property of GLM-130B to reach INT4 quantization, without quantization aware training and with almost no performance loss, making it the first among 100B-scale models. More importantly, the property allows its effective inference on 4×RTX 3090 (24G) or 8×RTX 2080 Ti (11G) GPUs, the most ever affordable GPUs required for using 100B-scale models. The GLM-130B model weights are publicly accessible and its code, training logs, related toolkit, and lessons learned are open-sourced at https://github.com/THUDM/GLM-130B"
featured: false
publication: "**ICLR 2023**"
links:
  - icon_pack: fab
    icon: github
    name: Code
    url: 'https://github.com/THUDM/GLM-130B'
  - icon_pack: ai
    icon: arxiv
    name: Preprint
    url: 'https://arxiv.org/abs/2210.02414'
---

