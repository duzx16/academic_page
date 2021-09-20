---
title: "Policy-Gradient Training of Fair and Unbiased Ranking Functions"
date: 2021-07-11
publishDate: 2021-07-11
authors:
- Himank Yadav
- admin
- Thorsten Joachims
author_notes:
- "Equal contribution"
- "Equal contribution"
publication_types: ["1"]
abstract: "While implicit feedback (e.g., clicks, dwell times, etc.) is an abundant and attractive source of data for learning to rank, it can produce unfair ranking policies for both exogenous and endogenous reasons. Exogenous reasons typically manifest themselves as biases in the training data, which then get reflected in the learned ranking policy and often lead to rich-get-richer dynamics. Moreover, even after the correction of such biases, reasons endogenous to the design of the learning algorithm can still lead to ranking policies that do not allocate exposure among items in a fair way. To address both exogenous and endogenous sources of unfairness, we present the first learning-to-rank approach that addresses both presentation bias and merit-based fairness of exposure simultaneously. Specifically, we define a class of amortized fairness-of-exposure constraints that can be chosen based on the needs of an application, and we show how these fairness criteria can be enforced despite the selection biases in implicit feedback data. The key result is an efficient and flexible policy-gradient algorithm, called FULTR, which is the first to enable the use of counterfactual estimators for both utility estimation and fairness constraints. Beyond the theoretical justification of the framework, we show empirically that the proposed algorithm can learn accurate and fair ranking policies from biased and noisy feedback.
"
featured: false
publication: "44th International **ACM SIGIR** Conference on Research and Development in Information RetrievalJuly 2021, **SIGIR 2021**"
links:
  - icon_pack: fab
    icon: github
    name: Code
    url: 'https://github.com/him229/fultr'
  - icon_pack: ai
    icon: arxiv
    name: Preprint
    url: 'https://arxiv.org/abs/1911.08054'
---

