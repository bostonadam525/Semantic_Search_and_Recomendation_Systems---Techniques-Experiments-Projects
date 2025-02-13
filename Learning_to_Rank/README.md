# Learning to Rank
* This repo goes over “Learning to Rank” Algorithms a very well known concept used to build Recommendation systems in Data Science and Machine Learning. 
* For any recommendation or personalization system you may consider for simplicity going with a Point-wise classification (e.g. logistic regression, SVM) or regression algorithms.
* There is also a Pair-wise algorithm called Factorization Machines that allows using either classification or regression algorithms and can easily be implemented OOTB on AWS. 


# Scoring vs. Ranking
* In recommender systems, scoring and ranking are two key concepts that are used to determine the relevance of items to recommend to a user.
  * **Scoring**
    * The process of assigning a score or a rating to each item in the candidate pool based on its similarity to the user’s preferences or past behavior. 
    * A scoring function can be based on different factors such as content similarity, collaborative filtering, or a combination of both. 
    * A scoring function is used to determine the relevance of each item to the user, and items with higher scores are considered to be more relevant.

* **Ranking**
  * Process of ordering the items based on their scores.
  * Items with the highest scores are ranked at the top of the recommendation list, while the items with the lowest scores are ranked at the bottom.
  * A ranking process ensures that the most relevant items are presented to the user first.
  * Example
    * Suppose a user is looking for music recommendations based on their past listening history. 
    * A scoring function will assign a score to each song in the candidate pool based on factors such as genre, artist, duration, featured artists, release date, and tempo. Songs with higher scores would be considered more relevant to the user.
    * A ranking process would then order songs based on their scores, with the highest-scored songs appearing at the top of the recommendation list.
 
 
## In summary:
* Scoring is the relevance of each item in a candidate pool.
* Ranking orders the items based on their scores to present the most relevant items to the user.

# Ranking
* “Learning to Rank” (LTR) or Candidate Ranking methods aim to predict the probability that a user will interact with an item, given their previous interactions and other contextual information. 
* LTR methods can be classified into 3 categories: 
  * **(i) Point-wise**
  * **(ii) Pair-wise**
  * **(iii) List-wise methods**
* Each method offers unique advantages and disadvantages. The choice of method should be guided by the specific requirements of a ranking task, the nature of the data, and the desired ranking performance.
* The table below gives a high-level overview of the 3 most common Ranking methods.

![image](https://github.com/user-attachments/assets/1b55b00c-f533-484b-9d38-1ea61e57ae15)


## Point-wise Ranking
* Point-wise methods evaluate items independently, ignoring rank order of other items. 
* Predict the relevance of a single document for a given query by training a classifier or regressor to score each item based on a set of features, such as those used in a linear model like logistic regression. 
* Final ranking is obtained by sorting list of results based on individual document scores.
* Most important is the concept that relevance scores assigned to each document are independent of the scores assigned to other documents in the same query result set.
* These methods often utilize binary/multi-class classification or regression models, where task is to predict relevance label for each item in isolation relative to other items (e.g., relevant vs. non-relevant in binary classification or multiple relevance levels in multi-class settings). 
* Classification examples of point-wise models include:
  * 1) Logistic Regression
  * 2) Gradient Boosted Decision Trees (e.g. XGBoost, LightGBM, MART)
“MART” - Multiple Additive Regression Trees
  * 3) SVM
  * 4) Random Forest
  * 5) Neural networks

* Regression examples of point-wise models include:
  * 1) Linear Regression
  * 2) Ordinal Regression
  * 3) Polynomial Regression 

### Pros of Pointwise Ranking
1. **Simplicity:** Relatively straightforward to implement.

2. **Flexibility:** Wide range of regression and classification algorithms can be employed, allowing the use of various features to predict relevance.

3. **Scalability:** Methods are computationally less intensive compared to pair-wise and list-wise methods, making them suitable for large-scale applications.

### Cons of Pointwise Ranking
1. **Lack of Contextual Awareness:** Point-wise methods do not consider interactions between documents, potentially leading to suboptimal rankings where relative importance among documents is crucial.

2. **Limited Optimization:** They may not capture complex dependencies or the competitive nature of documents vying for the same query, which can result in less accurate ranking performance.


## Pair-wise Ranking
* Pair-wise methods compare items in pairs, aiming to learn a function that assigns a higher score to the preferred item in each pair. How does this work?
  * A loss function focuses on pairs of documents. 
  * Given a pair, the objective is to learn the optimal ordering and minimize the number of inversions, which occur when the pair’s order is incorrect relative to the ground truth. 
  * Pair-wise methods align more closely with the nature of ranking tasks, as they directly model the relative order of documents.
* Pair-wise methods often employ **binary classification** models, where the task is to classify whether one document in the pair should be ranked higher than the other. 
  * The classifier outputs the relative preference between two items, guiding the ranking order.
* Algorithms such as **Factorization Machines, RankNet, LambdaRank, and LambdaMART** are prominent examples of pair-wise approaches

### Pros of Pair-wise Ranking
1. **Alignment with Ranking Tasks:** Naturally align with the goal of ranking, focusing on the relative order of documents rather than absolute relevance scores.

2. **Improved Accuracy:** By modeling pairwise preferences, these methods often provide more accurate rankings compared to point-wise approaches.

3. **Versatility:** Can be adapted to different ranking tasks and evaluation metrics, making them broadly applicable.

### Cons of Pair-wise Ranking

1. **Complexity:** Need to evaluate and compare pairs which often leads to increased computational complexity, especially for large datasets with many potential document pairs.

2. **Resource Intensity:** Training pair-wise models can require significant computational resources and time, particularly when dealing with extensive query-document pairs.


## List-wise Ranking
* Treats entire ranked list of items as a single unit and aims to optimize a scoring function that directly maps from the item set to a ranking score.
  * Focuses on optimizing entire list of documents rather than individual or pairs of documents.
* Offers a comprehensive approach to ranking by considering entire list of documents. 
  * While they provide the benefit of directly optimizing ranking metrics and often result in improved ranking accuracy, they are computationally demanding and require careful implementation.
  * Depending on the size of a dataset and the complexity of the ranking task, trade-offs between higher accuracy and resource intensity must be considered when choosing between list-wise methods and simpler point-wise or pair-wise approaches.
* Two main techniques used in list-wise ranking: 

1) Direct optimization of information retrieval (IR) measures, such as:
 Normalized Discounted Cumulative Gain (NDCG), used by algorithms like SoftRank and AdaRank

2) Minimizing a loss function defined based on the unique properties of the target ranking, as seen in ListNet and ListMLE.

* Examples of list-wise methods include:
  * **ListNet**
  * **ListMLE**
  * **LambdaMART** (used for both pair-wise and list-wise ranking methods), which optimize ranking metrics like NDCG to improve the overall quality of the ranked list.
* List-wise methods can extend beyond binary or multiclass classification into learning complex ranking functions, where models are trained to directly optimize ranking metrics across an entire list, improving the overall ranking quality in contrast to just focusing on individual pairwise comparisons.

### Pros of List-wise Ranking
* 1. **Comprehensive Optimization:** Account for the entire list of documents, leading to more holistic optimization of the ranking function.

* 2. **Direct Metric Alignment:** Can directly optimize metrics of interest, such as NDCG, resulting in better performance on those metrics.

* 3. **Enhanced Ranking Quality:** By considering an entire list, list-wise methods can capture complex interdependencies between documents, potentially leading to superior ranking accuracy.

### Cons of List-wise Ranking
1. **Complexity:** Inherently more complex and computationally demanding compared to point-wise and pair-wise methods.

2. **Resource Requirements:** Optimization process for entire lists can be resource-intensive, requiring significant computational power and memory.

3. **Implementation Challenges:** Implementation and fine-tuning of list-wise methods can be difficult, necessitating expertise in optimization and ranking algorithms.




# References
* [Personalized search with learning-to-rank (LTR)](https://www.elastic.co/search-labs/blog/personalized-search-elasticsearch-ltr)
