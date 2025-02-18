# Graph Neural Networks (GNNs) for Rec Systems

## Background
* Rec systems are used to generate a list of recommended items for a given user(s) also known as candidate generation.
* Recommendations are pooled from available set of items (e.g., movies, groceries, webpages, etc...) and are personalized to individual users, based on:

  * 1. user’s preferences (implicit or explicit),
  * 2. item features
  * 3. user<->item past interactions.

* The quantity and the quality of the user and item data determine the quality of the recommendations. Most current state-of-the-art Rec Systems use deep learning. 

## Why GNNs?
* From what we see above, we can easily model rec system data as a graph:
    * users and items as the nodes and the edges representing the relation between the nodes
    * relationships can be extracted and inferred from input data of most rec systems

* Depending on how we model the rec system data as a graph, there are multiple types of GNN models we can use to solve the recommendation problem. These models are usually classified into 3 main types:
```
1. GNNs using user-item interaction information
2. GNNs enhanced with social network information
3. GNNs enhanced with knowledge graphs
```



# References
* Building Rec Systems with GNN in PyTorch
* [Graph Neural Network (GNN) Architectures for Recommendation Systems](https://towardsdatascience.com/graph-neural-network-gnn-architectures-for-recommendation-systems-7b9dd0de0856/)
* [Graph Neural Networks for Social Recommendation](https://arxiv.org/abs/1902.07243)
* [GNN Rec Systems on Github](https://github.com/tsinghua-fib-lab/GNN-Recommender-Systems)
* [Recommendation Systems using Graph Neural Networks](https://github.com/sm823zw/Recommendation-System-Using-GNNs?tab=readme-ov-file)
* [TorchRec from PyTorch](https://github.com/pytorch/torchrec?tab=readme-ov-file)
