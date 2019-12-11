---
title: "Fair Learning-to-Rank from Implicit Feedback"
date: 2019-11-19
publishDate: 2019-11-19T02:45:42.998650Z
authors: ["Himank Yadav*", "**Zhengxiao Du***", "Thorsten Joachims"]
publication_types: ["3"]
abstract: "Addressing unfairness in rankings has become an increasingly important problem due to the growing influence of rankings in critical decision making, yet existing learning-to-rank algorithms suffer from multiple drawbacks when learning fair ranking policies from implicit feedback. Some algorithms suffer from extrinsic reasons of unfairness due to inherent selection biases in implicit feedback leading to rich-get-richer dynamics. While those that address the biased nature of implicit feedback suffer from intrinsic reasons of unfairness due to the lack of explicit control over the allocation of exposure based on merit (i.e, relevance). In both cases, the learned ranking policy can be unfair and lead to suboptimal results. To this end, we propose a novel learning-to-rank framework, FULTR, that is the first to address both intrinsic and extrinsic reasons of unfairness when learning ranking policies from logged implicit feedback. Considering the needs of various applications, we define a class of amortized fairness of exposure constraints with respect to items based on their merit, and propose corresponding counterfactual estimators of disparity (aka unfairness) and utility that are also robust to click noise. Furthermore, we provide an efficient algorithm that optimizes both utility and fairness via a policy-gradient approach. To show that our proposed algorithm learns accurate and fair ranking policies from biased and noisy feedback, we provide empirical results beyond the theoretical justification of the framework.
"
featured: false
publication: "*CoRR*, under review as a conference submission"
links:
  - icon_pack: ai
    icon: arxiv
    name: Preprint
    url: 'https://arxiv.org/abs/1911.08054'
---

