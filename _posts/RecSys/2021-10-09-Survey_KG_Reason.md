---
layout: post
title: "Survey on Knowledge-Graph-based RecSys"
subtitle: 'This note introduces Knowledge-Graph-based Recommender Systems'
author: "Yiqiao Jin (Ahren)"
header-style: text
mathjax: true
tags:
  - Recommender Systems
  - Explainable Recommendation
  - Knowledge Graph
---

## Resources

* [Summary of KG-related works](https://shaoxiongji.github.io/knowledge-graphs/) ([GitHub](https://github.com/shaoxiongji/knowledge-graphs))
* [Stanford CS 520 on KG](https://www.youtube.com/playlist?list=PLDhh0lALedc7LC_5wpi5gDnPRnu1GSyRG)


## Title
* Title: Faithfully Explainable Recommendation via Neural Logic Reasoning ([paper]()) ([code](https://github.com/orcax/LOGER))
* Conference: ACL 2021
* Author: from Rutgers University

Highlights
* New evaluation metrics: faithfulness

### Motivation

* None of existing explainable recommendation models based on KGs considered faithfulness in explainable reasoning process, e.g. the generated explanations and evaluation of reasoning paths.

### Approach
The model consists of:

* KG Encoder
* Neural Logic Model

Neural Logic Model
* Goal: Given a set of logic rules extracted from KG, we aim to obtain their importance scores and make predictions.
* Use MLN to model joint distribution:

$$p\left(X_{\mathcal{G}}, X_{H} \mid w\right)=\frac{1}{Z} \exp \left(\sum_{/ \in L} w_{/} n_{l}\right)$$

* Hard to directly maximize the log likelihood $\rightarrow$ Variational EM
* Composition rule:

$\forall u \in \mathcal{U}, v \in \mathcal{V}, e_{1}, \ldots, e_{j-1} \in \mathcal{E}$

$(u, r_{1}, e_{1}) \wedge \cdots \wedge(e_{j-1}, r_{j}, v) \Rightarrow(u, r_{u i}, v)$

#### Contribution

* Introduces **faithfulness**: rule or pattern should be consistent with user's historical behaviors
* Introduce logical rules and let the weights reflect historical behaviors

EM-step
* Motivation: Markov Logic Network (MLN) is inefficient while knowledge graph embedding (KGE) is not faithful
* Solution: use variational EM so MLN and KGE can mutually benefit each other.
* E-Step: fix weight $w$ of each logical rule, update $q\left(X_{H} \mid \theta\right)$ by minimizing the $\mathrm{KL}$ divergence: $\mathbb{E}_{q\left(X_{H} \mid \theta\right)}\left[\log p\left(X_{H} \mid X_{\mathcal{G}}, w\right)-\log q\left(X_{H} \mid \theta\right)\right]$
The object of $\theta$ is:
$$\ell(\theta)=\sum_{\left(e_{h}, r, e_{t}\right) \in \mathcal{G} \cup H^{+}} \log q\left(X_{h r t}=1 \mid \theta\right)$$
M-Step: fix $q\left(X_{H} \mid \theta\right)$, update $w$ by maximizing the log-likelihood function of each of all triplets: $\mathbb{E}_{q\left(X_{H} \mid \theta\right)}\left[\log p\left(X_{H} \mid X_{\mathcal{G}}, w\right)\right]$
the optimal is acquired by gradient ascent:
$\nabla_{w_{l}} \ell_{P L}\left(w_{l}\right)=\sum_{\left(e_{h}, r, e_{t}\right) \in \mathcal{G}} \frac{1-p_{h r t}}{\left|L_{h r t}\right|}$

$+\sum_{\left(e_{h}, r, e_{t}\right) \in H} \frac{q\left(X_{h r t}=1 \mid \theta\right)-p_{h r t}}{\left|L_{h r t}\right|}$

Rule-guided Path Reasoner


#### Baseline
* MLN

#### Input / Output

#### Training 


### Experiments
* Dataset: Amazon, including Cellphone, Grocery, Automotive
* Conventinal Metrics: Precision, Recall, NDCG, Hit Rate

* New metric: faithfulness

$\mathrm{JS}_{f} &=\mathbb{E}_{u \sim \mathcal{U}}[D_{\mathrm{JS}}(Q_{f}(u) \| F(u))]$
$\mathrm{JS}_{w} &=\mathbb{E}_{u \sim \mathcal{U}}[D_{\mathrm{JS}}(Q_{w}(u) \| F(u))]$

$F(u)$: rule distribution over 1000 paths sampled from each selected user

### Related

## A review: Knowledge reasoning (KR) over knowledge graph (KG)

### Introduction
* Definition of Reasoning: the process of deducing new judgements (conclusions) from one or several existing judgements (premises)
* Significance of KG (with examples): KG are able to mine, organize, and effectively manage knowledge from large-scale data to improve the quality of information services ...
* Goal of reasoning over KG: identify errors and infer new conclusions from existing data
* Contribution: Number of papers surveyed, 

### Methodology
* Descriptions of how papers are collected. Sources, criteria, ...

### Knowledge Reasoning

#### Introduction of commonly used knowledge graphs
* Introduction of WordNet, Freebase, YAGO, Wikidata, NELL

#### Knowledge reasoning (KR) oriented knowledge graph (KG)
* Definition: Given KG = $<E, R, T>$ and relation path $P$, generate a triplet that does not exist in the KG $G' = \{(h,r,t)|h \in E,r \in R,t \in T, (h,r,t) \in G\}$

* Definition of KG: KG is basically a semantic network and a structured semantic knowledge base which can formally interpret concepts and their relations in the real world

#### Rule-based KR

* reason over KG by applying simple rules or statistical features

* $($YaoMing, wasBornIn, Shanghai$) \cap ($Shanghai, locatedIn,
China$) \Rightarrow ($YaoMing, nationality, China$)$

#### Ontology-based KR
