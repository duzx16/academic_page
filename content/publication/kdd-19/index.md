---
title: "Sequential Scenario-Specific Meta Learner for Online Recommendation"
date: 2019-07-20
publishDate: 2019-07-20T13:59:10.997616Z
authors: ["Zhengxiao Du", "Xiaowei Wang", "Hongxia Yang", "Jingren Zhou", "Jie Tang"]
publication_types: ["2"]
abstract: "Cold-start problems are long-standing challenges for practical recommendations. Most existing recommendation algorithms rely on extensive observed data and are brittle to recommendation scenarios with few interactions. This paper addresses such problems using few-shot learning and meta learning. Our approach is based on the insight that having a good generalization from a few examples relies on both a generic model initialization and an effective strategy for adapting this model to newly arising tasks. To accomplish this, we combine the scenario-specific learning with a model-agnostic sequential meta-learning and unify them into an integrated end-toend framework, namely Scenario-specific Sequential Meta learner (or $s^2$ Meta ). By doing so, our meta-learner produces a generic initial model through aggregating contextual information from a variety of prediction tasks while effectively adapting to specific tasks by leveraging learning-to-learn knowledge. Extensive experiments on various real-world datasets demonstrate that our proposed model can achieve significant gains over the state-of-the-arts for cold-start problems in online recommendation. Deployment is at the Guess You Like session, the front page of the Mobile Taobao."
featured: false
publication: "25th *ACM SIGKDD* International Conference on
               Knowledge Discovery & Data Mining, **KDD 2019**"
links:
  - icon_pack: fab
    icon: github
    name: Code
    url: 'https://github.com/THUDM/ScenarioMeta'
  - icon_pack: ai
    icon: arxiv
    name: Preprint
    url: 'https://arxiv.org/abs/1906.00391'
---

