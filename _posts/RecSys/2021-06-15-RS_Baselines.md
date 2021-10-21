---
layout: post
title: "Baseline methods of RecSys"
subtitle: 'This note introduces baseline methods about Recommender Systems'
author: "Yiqiao Jin (Ahren)"
header-style: text
mathjax: true
tags:
  - Recommender Systems
  - Explainable Recommendation
---
**About dataset**
Number of users: 5356
Average actions of users: 1001.5

Number of repo: 60189
Average events of repo: 89.1

Number of interactions: 5362881
The sparsity of the dataset: 98.3%

Fields: ['user_id', 'repo_id', 'timestamp', 'rating (binary)']

Currently not including user features and repo features (e.g. textual information like README)


## LightGCN
User embedding: (|U|, 32)
Item embedding: (|I|, 32)

LightGCN learns user and item embeddings by linearly propagating them on the user-item interaction graph

It uses the weighted sum of embeddings in all layers as the final embedding

**NGCF**

$e _{u}^{(k+1)}=\sigma\left( W _{1} e _{u}^{(k)}+\sum_{i \in N_{u}} \frac{1}{\sqrt{\left| N _{u} \| N _{i}\right|}}\left( W _{1} e _{i}^{(k)}+ W _{2}\left( e _{i}^{(k)} \odot e _{u}^{(k)}\right)\right)\right)$
$e _{i}^{(k+1)}=\sigma\left( W _{1} e _{i}^{(k)}+\sum_{u \in N_{i}} \frac{1}{\sqrt{\left| N _{u} \| N _{i}\right|}}\left( W _{1} e _{u}^{(k)}+ W _{2}\left( e _{u}^{(k)} \odot e _{i}^{(k)}\right)\right)\right.$

**LightGCN**

$e _{u}^{(k+1)}=\sum_{i \in N _{u}} \frac{1}{\sqrt{\left| N _{u}\right|} \sqrt{\left| N _{i}\right|}} e _{i}^{(k)}$
$e _{i}^{(k+1)}=\sum_{u \in N _{i}} \frac{1}{\sqrt{\left| N _{i}\right|} \sqrt{\left| N _{u}\right|}} e _{u}^{(k)}$

### Features

Model complexity is same as standard MF.

Model prediction is the inner product of user and item final representations
$\hat{y}_{u i}= e _{u}^{T} e _{i}$

### Issue

Both user and item are modeled as homogeneous nodes

i.e. the features are propagated with an adjacency matrix of (|U|+|I|, |U|+|I|)

$A =\left(\begin{array}{cc} 0 & R \\ R ^{T} & 0 \end{array}\right)$

### Experiments

Best accuracy is reached when using 2 layers in most cases, while after that
it drops quickly to the worst point of layer 4. 

This indicates that smoothing a node's embedding with its first-order and second-order neighbors is very useful for CF, but will suffer from over-smoothing issues when higher-order neighbors are used.

### Results


```
Trainable parameters: 2097440 ((|U|+|I|) * |E|)
Epoch 0
recall@10 : 0.1566    mrr@10 : 0.2923    ndcg@10 : 0.1939    hit@10 : 0.3454    precision@10 : 0.0942    

Epoch 5
recall@10 : 0.2546    mrr@10 : 0.491    ndcg@10 : 0.3208    hit@10 : 0.5111    precision@10 : 0.118    

Epoch 10
recall@10 : 0.2552    mrr@10 : 0.4958    ndcg@10 : 0.324    hit@10 : 0.5109    precision@10 : 0.1198    

Epoch 15
recall@10 : 0.2557    mrr@10 : 0.4985    ndcg@10 : 0.3261    hit@10 : 0.5113    precision@10 : 0.1209    

Epoch 20
recall@10 : 0.256    mrr@10 : 0.4994    ndcg@10 : 0.3272    hit@10 : 0.5113    precision@10 : 0.1217    

Epoch 25
recall@10 : 0.2562    mrr@10 : 0.4995    ndcg@10 : 0.3274    hit@10 : 0.5117    precision@10 : 0.1219
```


## BPR
### Results
```
Epoch 0
recall@10 : 0.1061    mrr@10 : 0.2466    ndcg@10 : 0.1373    hit@10 : 0.2938    precision@10 : 0.0618    

Epoch 5
recall@10 : 0.2569    mrr@10 : 0.5025    ndcg@10 : 0.3282    hit@10 : 0.5119    precision@10 : 0.1224    

Epoch 10
recall@10 : 0.2581    mrr@10 : 0.5073    ndcg@10 : 0.3324    hit@10 : 0.5132    precision@10 : 0.1242    

Epoch 15
recall@10 : 0.2585    mrr@10 : 0.5091    ndcg@10 : 0.3338    hit@10 : 0.5139    precision@10 : 0.125    

Epoch 20
recall@10 : 0.2588    mrr@10 : 0.5095    ndcg@10 : 0.3347    hit@10 : 0.5136    precision@10 : 0.1255    

Epoch 25
recall@10 : 0.2588    mrr@10 : 0.5093    ndcg@10 : 0.335    hit@10 : 0.5136    precision@10 : 0.1259    

test result
{'recall@10': 0.2586, 'mrr@10': 0.5097, 'ndcg@10': 0.3348, 'hit@10': 0.5136, 'precision@10': 0.1256}

```

## NGCF

### Results
```
Trainable parameters: 2118304
epoch 0 
recall@10 : 0.2388    mrr@10 : 0.4668    ndcg@10 : 0.2982  

epoch 5
recall@10 : 0.2464    mrr@10 : 0.4796    ndcg@10 : 0.306    hit@10 : 0.5072    precision@10 : 0.1101   

epoch 10
recall@10 : 0.2477    mrr@10 : 0.4835    ndcg@10 : 0.307    hit@10 : 0.5106    precision@10 : 0.1103    

Epoch 15
recall@10 : 0.2552    mrr@10 : 0.4997    ndcg@10 : 0.3235    hit@10 : 0.5119    precision@10 : 0.1196    

Epoch 20
recall@10 : 0.2573    mrr@10 : 0.5064    ndcg@10 : 0.3307    hit@10 : 0.5119    precision@10 : 0.1229    

Epoch 25
recall@10 : 0.2588    mrr@10 : 0.5082    ndcg@10 : 0.3332    hit@10 : 0.5121    precision@10 : 0.1246    
best valid : {'recall@10': 0.2588, 'mrr@10': 0.5082, 'ndcg@10': 0.3332, 'hit@10': 0.5121, 'precision@10': 0.1246}
test result: {'recall@10': 0.2588, 'mrr@10': 0.5082, 'ndcg@10': 0.3332, 'hit@10': 0.5121, 'precision@10': 0.1246}

```
