---
title: "P-Tuning v2: Prompt Tuning Can Be Comparable to Fine-tuning Universally Across Scales and Tasks"
date: 2021-10-14
publishDate: 2021-10-14
authors:
- Xiao Liu
- Kaixuan Ji
- Yicheng Fu
- admin
- Zhilin Yang
- Jie Tang
author_notes:
- "Equal contribution"
- "Equal contribution"
- "Equal contribution"
publication_types: ["3"]
abstract: "Prompt tuning, which only tunes continuous prompts with a frozen language model, substantially reduces per-task storage and memory usage at training. However, in the context of NLU, prior work reveals that prompt tuning does not perform well for normal-sized pre-trained models. We also find that existing methods of prompt tuning cannot handle hard sequence tagging tasks, indicating a lack of universality. We present a novel empirical finding that properly optimized prompt tuning can be universally effective across a wide range of model scales and NLU tasks. It matches the performance of fine-tuning while having only 0.1%-3% tuned parameters. Our method P-Tuning v2 is not a new method, but a version of prefix-tuning optimized and adapted for NLU. Given the universality and simplicity of P-Tuning v2, we believe it can serve as an alternative to fine-tuning and a strong baseline for future research."
featured: false
publication: "Under review"
links:
  - icon_pack: ai
    icon: arxiv
    name: Preprint
    url: 'https://arxiv.org/abs/2110.07602'
  - icon_pack: fab
    icon: github
    name: Code
    url: 'https://github.com/THUDM/P-tuning-v2'
---