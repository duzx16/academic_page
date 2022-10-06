---
title: "GLM: General Language Model Pretraining with Autoregressive Blank Infilling"
date: 2022-03-15
publishDate: 2022-03-15
authors:
- admin
- Yujie Qian
- Xiao Liu
- Ming Ding
- Jiezhong Qiu
- Zhilin Yang
- Jie Tang
author_notes:
- "Equal contribution"
- "Equal contribution"
publication_types: ["1"] # 1 = Conference paper, 2 = Journal article, 3 = Preprint / working paper
abstract: "There have been various types of pretraining architectures including autoencoding models (e.g., BERT), autoregressive models (e.g., GPT), and encoder-decoder models (e.g., T5). On the other hand, NLP tasks differ in nature, with three main categories being natural language understanding (NLU), unconditional generation, and conditional generation, while none of the pretraining frameworks performs the best for all tasks. We propose a General Language Model (GLM)  based on autoregressive blank infilling to address this challenge. The proposed architecture has two major benefits: (1) It improves pretrain-finetune consistency via cloze-style finetuning and naturally handles variable-length blank infilling which is crucial for many downstream tasks. Empirically, GLM substantially outperforms BERT on the SuperGLUE natural language understanding benchmark with the same amount of pretraining data and steps. (2) It is flexible enough to handle various NLP tasks with a single pretrained model. GLM with 1.25x parameters of BERT-Large achieves the best performance in NLU, conditional and unconditional generation at the same time, demonstrating its generalizability to different downstream tasks."
featured: false
publication: "**ACL 2022**"
links:
  - icon_pack: fab
    icon: github
    name: Code
    url: 'https://github.com/THUDM/GLM'
  - icon_pack: ai
    icon: arxiv
    name: Preprint
    url: 'https://arxiv.org/abs/2103.10360'
  - icon_pack: ai
    icon: acm
    name: Paper
    url: 'https://aclanthology.org/2022.acl-long.26'
---

