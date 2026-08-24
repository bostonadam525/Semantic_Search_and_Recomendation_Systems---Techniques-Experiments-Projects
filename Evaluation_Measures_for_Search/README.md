# Evaluation Measures for Search - Overview 
- In this repo we will cover at a high level:
```
Search systems
BM25
Retrieval architecture
Ranking and re-ranking
Image search
Relevance optimization
A/B Testing
```

## 1. How does a retrieval pipeline works end-to-end?
Indexing
Retrieval
Ranking
Re-ranking
Evaluation

## 2. BM25 + Vector Search
Why use both?
How do they complement each other?
Tradeoffs

## 3. Image Search
How you approached photo retrieval
CLIP embeddings
Semantic image search
Ranking results

## 4. Search Quality
NDCG
MRR
Precision@K
Relevance measurement
A/B testing

---
# 3. Image Search
- [source](https://www.elastic.co/search-labs/blog/multimodal-image-retrieval-with-roboflow)
- [CLIP with Pinecone](https://www.pinecone.io/learn/clip-image-search/)
- To build a semantic image search system using CLIP embeddings, you would usually do the following:
  - process images and text into a shared vector space,
  - index them with a vector database, and
  - rank results using cosine similarity scores to match user queries with the most visually and conceptually relevant pictures

- Ranking and Retrieval
  - Cosine Similarity: Measure the angle between vectors to score how close the text meaning matches the image content.
  - Score Thresholding: Filter out low scores to keep only relevant matches.
  - Re-ranking: Apply secondary models or metadata filters to fine-tune the final display order.



---
# 4. Search Quality -- Online vs. Offline Evaluation Measures
- IR measures are commonly split into online vs. offline as we see below, [source](https://www.pinecone.io/learn/offline-evaluation/)

<img width="921" height="561" alt="image" src="https://github.com/user-attachments/assets/591605a5-835b-4f60-bf21-23a932211ac1" />


- Key takeaway: You should always consider using BOTH online and offline metrics. However, offline metrics should be your starting point. 
---
# Offline Metrics

## Order Unaware Metrics
- order does not make a difference

1. **Precision@k** -- measures proportion of relevant items within the top k results returned.
   - Precision@k = num of relevant items in top K results / K
   - Advantages: focuses on the top items retrieved; easy to interpret
   - Disadvantages: does not consider the entire pool size (does not penalize a system for missing relevant items outside top k rank).
    
2. **Recall@k** - measures how many **relevant items** were returned vs. how many relevant items exist in the entire dataset
   - Recall@K = truePositives / (truePositives + falseNegatives)


3. **F1@k**
   - F1 score just like in classical ML is the harmonic mean of Precision@k and Recall@k


## Order AWARE Metrics 
- order DOES make a difference

1. **MRR@k**
   - Mean Reciprocal Rank: difference from Recall@k is that MRR is order AWARE so it considers the order of the returned results.
   - MRR also considers multiple queries not just one query.
   - Advantage of MRR: consider rank of first relevant item but no others -- advantage where the first relevant result is most important.
   - Disadvantages of MRR: if you want to consider other returned hits then this is not ideal; less readily interpretable compared to recall@k.
2. **MAP@k**
   - Mean average precision @k.
   - Precison@k = truePositives / (truePositives + falsePositives)
   - Advantage: allows us to consider ORDER of returned items (ideal when we retrieve multiple relevant items)
   - Disadvantage: relK parameter is BINARY -- this means items are either classified as relevant or irrelevant but does not allow for multi-label (cant be both). 
     
3. **NDCG@k**
   - Normalized Discounted Cumulative Gain @k (ORDER AWARE metric).
   - This allows us to rank every item based on its true relevance to a query: Least Relevant (0) --> Most relevant (4)
   - NDCG@k is most popular in IR systems because it is easily interpretable and optimizes for highly relevant documents.
   - Disadvantage: it only tells us which items are relevant to the query --- it doesn't rank items relevance based on relevance to each other. 

---
## A/B Testing Search Engines
- [Source](https://typesense.org/docs/guide/ab-testing.html)

### How A/B testing works (usually)
- Set hypothesis and primary KPI
- Split traffic evenly between A and B
- Run test long enough to get statistically significant results
- Compare results --> estimate lift --> select "winner"

### Setup and Scoping
1. Randomization -- assign user session ID or user ID level rather than individual queries to keep search consistent.
2. Traffic allocation -- split traffic evenly (50% control, 50% treatment)
3. Duration -- need to run test long enough to achieve statistical significance

### A/B Test Metrics
- Click Through Rate (CTR) -- % of results where user clicked through at least 1 result
- Mean Reciprocal Rank (MRR) -- how high up the clicked results appeared (higher up is better)
- Zero-result rate -- % of queries that returned not results
- Conversation/Success Rate -- downstream actions like selecting an item, completing purchase, etc...
- Query refinement rate -- how often did users have to refine their queries to find the optimal results? 
---
## Multi-Armed Bandits (MAB)
- Start by trying all versions
- As results come in, shift more traffic to the leader
- Keep a small amount of traffic on the others so you don’t miss a late winner
- Use sticky bucketing, where you make sure that a returning user keeps seeing the same version

### MAB Sampling methods
1. Epsilon-greedy
2. Upper Confidence Bound (UCB)
3. Thompson Sampling -- bayesian approach

### key differences between MAB and A/B
- SPEED
- A/B testing needs a lot of quality data .
- MAB adapts while the test is running.
- [source](https://www.braze.com/resources/articles/multi-armed-bandit-vs-ab-testing)

---
# Resources
- [PineCone article](https://www.pinecone.io/learn/offline-evaluation/)
