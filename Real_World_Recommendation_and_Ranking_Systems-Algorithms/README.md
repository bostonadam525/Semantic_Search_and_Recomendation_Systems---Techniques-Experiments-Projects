# Real World Recommmendation Systems & Ranking Algorithms
* In "the wild" 2 stage and 4 stage recommendation systems are VERY common in the real-world. 

* The concept of a 2 stage system: 
  * 1) Candidate Retrieval/Generation 
  * 2) Ranking & Recommendations
* The concept of a 4 stage system:
  * 1) Retrieval
  * 2) Filter
  * 3) Score
  * 4) Order

# 2-Stage (2x2) Recommender Systems
* The concept is simple: before we score the items we need to select a reasonably relevant set that contains the items that the user eventually engages with. 
* This stage is commonly referred to as **Candidate Retrieval or Candidate Generation.**
* 2 Stage Retrieval models take many forms including but not limited to:

1. Matrix factorization
2. Two-tower
3. Linear models
4. Approximate nearest neighbor
5. Graph traversal

## 2-Stage System - Environment & Process
1. Environment
  * Online vs. Offline
2. Process
  * Candiate Retrieval vs. Ranking

### 2-Stage -- Offline Environment
* Batch processes handle tasks such as model training, creating embeddings for catalog items, and developing structures like approximate nearest neighbors (ANN) indices or knowledge graphs to identify item similarity. 
* The offline stage may also involve loading item and user data into a feature store, which helps augment input data during ranking.

### 2-Stage -- Online Environment
* Serves individual user requests in real-time, utilizing artifacts generated offline (e.g., ANN indices, knowledge graphs, models). Here, input items or queries are transformed into embeddings, followed by two main steps:
  * 1) **Candidate Retrieval**: Quickly narrows down millions of items to a more manageable set of candidates, trading precision for speed.
  * 2) **Ranking**: Ranks the reduced set of items by relevance. This stage enables the inclusion of additional features such as item and user data or contextual information, which are computationally intensive but feasible due to the smaller candidate set.

### Example of 2 Stage System
* The concept we see here is how to bring together multiple model outputs → Retrieve/Generate Candidates → Rank → Recommend
* This is an example from Google using the Two-Tower Architecture:
1) Candidate Retrieval/Generation
2) Scoring with Dot-Product
3) Ranking

![image](https://github.com/user-attachments/assets/3574a17c-8d72-4a82-b074-fe51100fa4ef)

# 4-Stage Recommender Systems
* Expanding on the 2x2 model, the 4-stage (2x4) recommender system adds additional stages to address more specific requirements commonly encountered in real-world applications. 
* This involves the following 4 stages:

1. **Retrieval**
  * Purpose --> Generates a pool of candidates.
  * Methods often used
    * 1. Matrix factorization
    * 2. Two-tower models
    * 3. ANN
    * 4. Linear models
    * 5. Graph based models
     
2. **Filtering**
  * Purpose --> Applies business rules to further refine the candidate pools. 
  * Filtering ensures the system does not rely solely on retrieval or scoring models to handle business-specific logic, allowing for more precise control over item eligibility.
  * Filtering is most frequently performed following the Retrieval stage.
  * It can be integrated with Retrieval (one of the more complex problems you run into with filtering is ensuring that there are enough candidates after retrieval) or it can even follow scoring in some situations. 
  * Filtering allows you to apply business logic rules that would otherwise be impossible (or at least very hard) to enforce by the model. 
  * In some cases these are simple exclusion queries, but they can be more complex as is the case for Bloom filters which can be used to remove items that have already been interacted with by the users.

3. **Scoring**
  * Purpose:
      * 1. Involves a detailed analysis of the remaining candidates, using models that factor in additional features (e.g., user data, contextual information) to predict user interest. 
      * 2. Scoring can leverage sophisticated models, such as deep learning architectures, to assign interaction likelihoods (e.g., click, purchase probability) to each candidate.
       
4. **Ordering**
  * Purpose: 
    * Arranges items into an ordered list for the user. 
    * Although scoring assigns relevance scores to each item, the ordering stage allows further adjustments to ensure a diverse mix of recommendations or to meet additional criteria like novelty. 
    * This stage addresses the challenge of filter bubbles by promoting item diversity and enhancing user exploration.
 
* Each stage has a unique role in refining and tailoring recommendations to meet both user and business requirements.

![image](https://github.com/user-attachments/assets/76159760-f20f-429e-89ce-ea9aaa7bf494)


## 4-Stage Real-World Examples
* This is a high-level chart showing how the 4 stage recommendation system works in real-world use cases.

![image](https://github.com/user-attachments/assets/3d8789fa-5de1-47b7-9082-2f8f302cf1e8)

### Real-World Example 1 - Instagram

![image](https://github.com/user-attachments/assets/f45c4315-a048-4ef1-b11d-8225ed735b02)

### Real-World Example 2 - Pinterest

![image](https://github.com/user-attachments/assets/24e1a779-7060-480f-8939-32a19d9d79ff)

### Real-World Example 3 - Instacart

![image](https://github.com/user-attachments/assets/42542dd2-f3a6-42de-84f0-bbc633f765be)

