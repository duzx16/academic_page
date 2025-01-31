---
title: "GPT Understands, Too"
date: 2021-03-18
publishDate: 2021-03-18
authors:
- Xiao Liu
- Yanan Zheng
- admin
- Ming Ding
- Jiezhong Qiu
- Zhilin Yang
- Jie Tang
author_notes:
- "Equal contribution"
- "Equal contribution"
publication_types: ["2"] # 1 = Conference paper, 2 = Journal article, 3 = Preprint / working paper
abstract: "While GPTs with traditional ﬁne-tuning fail to achieve strong results on natural language understanding (NLU), we show that GPTs can be better than or comparable to similar-sized BERTs on NLU tasks with a novel method P-tuningwhich employs trainable continuous prompt embeddings. On the knowledge probing (LAMA) benchmark, the best GPT recovers 64% (P@1) of world knowledge without any additional text provided during test time, which substantially improves the previous best by 20+ percentage points. On the SuperGlue benchmark, GPTs achieve comparable and sometimes better performance to similar-sized BERTs in supervised learning. Importantly, we ﬁnd that P-tuning also improves BERTs’ performance in both few-shot and supervised settings while largely reducing the need for prompt engineering. Consequently, Ptuning outperforms the state-of-the-art approaches on the few-shot SuperGlue benchmark."
featured: false
publication: "AI Open"
links:
  - icon_pack: ai
    icon: arxiv
    name: Preprint
    url: 'https://arxiv.org/abs/2103.10385'
  - icon_pack: fab
    icon: github
    name: Code
    url: 'https://github.com/thudm/p-tuning'
---