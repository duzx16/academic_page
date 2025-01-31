---
title: "P-Tuning: Prompt Tuning Can Be Comparable to Fine-tuning Across Scales and Tasks"
date: 2022-03-15
publishDate: 2022-03-15
authors:
- Xiao Liu
- Kaixuan Ji
- Yicheng Fu
- Weng Lam Tam
- admin
- Zhilin Yang
- Jie Tang
author_notes:
- "Equal contribution"
- "Equal contribution"
- "Equal contribution"
publication_types: ["1"] # 1 = Conference paper, 2 = Journal article, 3 = Preprint / working paper
abstract: "Prompt tuning, which only tunes continuous prompts with a frozen language model, substantially reduces per-task storage and memory usage at training. However, in the context of NLU, prior work reveals that prompt tuning does not perform well for normal-sized pretrained models. We also find that existing methods of prompt tuning cannot handle hard sequence labeling tasks, indicating a lack of universality. We present a novel empirical finding that properly optimized prompt tuning can be universally effective across a wide range of model scales and NLU tasks. It matches the performance of finetuning while having only 0.1%-3% tuned parameters. Our method P-Tuning v2 is an implementation of Deep Prompt Tuning (CITATION) optimized and adapted for NLU. Given the universality and simplicity of P-Tuning v2, we believe it can serve as an alternative to finetuning and a strong baseline for future research."
featured: false
publication: "**ACL 2022**"
links:
  - icon_pack: fab
    icon: github
    name: Code
    url: 'https://github.com/THUDM/P-tuning-v2'
  - icon_pack: ai
    icon: arxiv
    name: Preprint
    url: 'https://arxiv.org/abs/2110.07602'
  - icon_pack: ai
    icon: acm
    name: Paper
    url: 'https://aclanthology.org/2022.acl-short.8'
---