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



# Convolutional Neural Networks (CNN) vs. Graph Neural Networks (GNN)
* CNNs uses kernel matrix filtering methods to slide over each pixel in a fixed manner as a "sliding window". The insight allowing us to reach our goal is that each convolution takes a little sub-patch of the image (a little rectangular part of the image), applies a function to it, and produces a new part (a new pixel). [source](https://neptune.ai/blog/graph-neural-network-and-some-of-gnn-applications)

![image](https://github.com/user-attachments/assets/3e51dc96-c7a4-4db7-bd44-b96c06cb2606)

* We can’t use a “moving” filter with a GNN though. The kernel filter in a CNN is instead abstracted as a central node with X neighbors.
* However, the concept of aggregation is similar. Each node in a GNN has X nearest neighbors which are encoded as aggregate embeddings similar to how in a CNN you would take the max, min, sum or average of each kernel to get your final matrix. 

![image](https://github.com/user-attachments/assets/44b0fa18-a188-4aac-a796-56bf89b4fec6)


# Graph Neural Networks Functionality
* A computational graph is formed by aggregation using neural networks. And this is forward propagation [source](https://neptune.ai/blog/graph-neural-network-and-some-of-gnn-applications)

![image](https://github.com/user-attachments/assets/0d615327-9f72-484c-83a1-3e7acd1f8f38)



# LightGCN
* LightGCN is an embedding-based model, which means that it attempts to find optimal embeddings (vectors) for the users and items.
* At the same time, it is also searching for the optimal scoring function `f`, whose purpose is:
  1. assigning high scores to **new user-item pairs** --> which are good recommendations
  2. assigning low scores otherwise

* As a result, the **embeddings of users with similar preferences are expected to be similar in the most optimal case**.
* And thus the embeddings of users with more dissimilar preferences are expected to be more different [source](https://medium.com/@jn2279/better-recommender-systems-with-lightgcn-a0e764af14f9)

![image](https://github.com/user-attachments/assets/77484e01-40a3-4e45-8533-6712636f5ce7)

## Matrix Factorization -- The base layer
* [source](https://medium.com/@jn2279/better-recommender-systems-with-lightgcn-a0e764af14f9)
* Matrix factorization captures **only the first-order edge connectivity structure of the graph (only the information from immediate neighbours of a given node)**
* However, we would want a model to be able to capture a **higher-order graph structure**.
* This is where LightGCN comes in, which starts training with node embeddings which are initialized with matrix factorization:

![image](https://github.com/user-attachments/assets/46a73096-31d1-4b2f-bbc9-2c4054e6bf76)


* Once the embeddings are initialized we reach the most crutial part of the entire model: How do we get the embeddings to interact with each other in such a way that encourages the embeddings of similar users/items to be similar?
* LightGCN is able to do this with 3 layers, where for each layer, each node obtains its new embedding by combining the embeddings of it’s neighbours.
* This is very similar to the CNN or convolutional neural network mentioned above and is essentially a graph convolution (see comparison with image convolution below)

![image](https://github.com/user-attachments/assets/d92419b7-98e3-4142-8d9c-9e50648acbde)



# References
* Building Rec Systems with GNN in PyTorch
* [Graph Neural Network (GNN) Architectures for Recommendation Systems](https://towardsdatascience.com/graph-neural-network-gnn-architectures-for-recommendation-systems-7b9dd0de0856/)
* [Graph Neural Networks for Social Recommendation](https://arxiv.org/abs/1902.07243)
* [GNN Rec Systems on Github](https://github.com/tsinghua-fib-lab/GNN-Recommender-Systems)
* [Recommendation Systems using Graph Neural Networks](https://github.com/sm823zw/Recommendation-System-Using-GNNs?tab=readme-ov-file)
* [TorchRec from PyTorch](https://github.com/pytorch/torchrec?tab=readme-ov-file)
