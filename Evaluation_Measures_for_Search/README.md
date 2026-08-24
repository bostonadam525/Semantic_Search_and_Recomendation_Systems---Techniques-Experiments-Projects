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

## 2. BM25 + Vector Search == HYBRID SEARCH
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
# 1. How does a retrieval pipeline works end-to-end?
Indexing
Retrieval
Ranking
Re-ranking
Evaluation






---
# 2. BM25 + Vector Search == HYBRID SEARCH
- Why use both?
- How do they complement each other?
- Tradeoffs

## BM25
- BM25 works by scoring documents using term frequency, inverse document frequency, and document length normalization. BM25 is still the default ranking algorithm in Elasticsearch and OpenSearch.
- How it works:
  - **Term Frequency**: how often a term appears in a document (e.g. "bank" appearing 100 times) -- however just because a term appears most frequently does not mean it is most important.
  - **Inverse Document Frequency (IDF)**: How rare is the term across all documents? Stop words which are very common (is, the, a) carry small weight vs. rare words will carry more weight
  - **Document Length Normalization**: BM25 penalizes longer documents to prevent them from dominating the search results

- BM25 is really good at:
  - Exact keyword matching (SKUs, error codes, CLI flags)
  - High-precision queries (exact keywords "hits")
  - Transparent debuggable ranking -- more easy to debug
  - Latency: this is very fast and efficient (e.g. ElasticSearch can handle billions of documents efficiently)

- BM25 will break and fail at:
  - Vocabulary mismatch ("cancel membership" vs "terminate subscription") --> out of vocabulary problem (OOV)
  - Semantic intent
  - Multi lingual queries
  - Similarity of concepts

- BM25 is really just a classical NLP bag-of-words model -- It has no understanding of meaning.

## Vector Embeddings
- Vector search transforms text into dense numerical vectors where semantically similar content is closer in the same vector space.
- Retrieval is thus easier because it uses nearest-neighbor search within the vector space.

- Semantic Vector search is really good at:
  - semantic equivalence
  - natural language questions
  - fuzzy concept matching
  - cross-lingual retrieval
  - RAG pipelines where LLMs generate natural language queries

- Semantic Vector search will break and fail at:
  - exact term matching (NullPointerException, order IDs)
  - infrastructure cost (GPU for embedding, RAM for HNSW indexes),
  - staleness when documents change frequently
  - scaling --> this is why quantization and methods such as Matryhoska embeddings are used today. 

- Quality of vector search is entirely dependent upon the embedding model you use — domain fit, dimensionality, and max token length all matter.

## Tradeoffs and Balance


## Hybrid search usage
- often times running both pipelines in parallel and combining their scores is the most efficient and precise way to do this.
- **Reciprocal Rank Fusion (RRF)** is the most common way to do this
  - RRF works by merging ranked lists from multiple retrievers without needing normalization.
  - RRF is RANK-BASED not score-based (e.g. you don't need cosine similarity vs. BM25 scores before you merge them)

```
Query
  │
  ├──► BM25 Retriever (Elasticsearch / OpenSearch)
  │         └── Top-K candidates
  │
  └──► Vector Retriever (Pinecone / Weaviate / pgvector / Qdrant / ChromaDB / FAISS)
            └── Top-K candidates
                    │
                    ▼
            RRF Merge (or weighted score fusion)
                    │
                    ▼
            Re-ranker (optional — cross-encoder for precision)
                    │
                    ▼
            Final Top-N Results → LLM Context / Response

## Re-Rankers
- After the initial retrieval (RECALL), a cross-encoder/re-ranker is often used to improve PRECISION.
- However, unlike bi-encoders (encode query and document separately), **cross-encoders process the query-document pair in the same space**, producing a much more accurate relevance score — at higher compute cost (cross-encoders are usually slower):
- This is the typical Re-Ranker pattern:
  - Retrieval (high recall, lower precision): BM25 + vector search, top 50–100 candidates
  - Re-ranking (high precision, higher cost): cross-encoder on the top candidates, select top 5–10
  - Output/Generation: pass final candidates as context to the LLM or user

### Why do we even need Re-Rankers?
- Most semantic search systems use a single bi-encoder which is a dense embedding model. The problem? We are condensing information into the embedding space the bi-encoder provides -- into a SINGLE VECTOR.
- This high --> low dimensionality often causes information loss.
- In addition, we don't know the context of the users query until it happens -- so we are creating two separate embeddings (1 of the query, 1 of the document to search) then comparing them. This results in "proximity" to vectors and often loses precision. 
- This is where re-rankers come in. Less information is lost. We embed BOTH the user query AND the document into the same vector space similarity score.


#### Bi-encoders
- Below we can see the typical bi-encoder workflow separates the vectors between the query and the document resulting in "proximity" [source](https://www.pinecone.io/learn/series/rag/rerankers/)

<img width="2760" height="1420" alt="image" src="https://github.com/user-attachments/assets/56499a55-bfde-4ed3-89e6-230f436c38da" />


#### Cross-Encoders
- With the cross encoder both are compared in the same space [source](https://www.pinecone.io/learn/series/rag/rerankers/)

<img width="2440" height="1100" alt="image" src="https://github.com/user-attachments/assets/63aeae74-7a45-442d-9b16-176fbf6d93ba" />





```


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
- [BM25 vs. Vector Search: Choosing the Right Retrieval Strategy for Production Systems](https://dev.to/aloknecessary/bm25-vs-vector-search-choosing-the-right-retrieval-strategy-for-production-systems-599n)
- [BM25 vs. Vector Search: Choosing the Right Retrieval Strategy for Production Systems](https://aloknecessary.in/blogs/bm25_vs_vector_search/?utm_source=devto&utm_medium=referral&utm_campaign=blog_syndication&utm_content=bm25-vs-vector-search)
- [Pinecone - Rerankers](https://www.pinecone.io/learn/series/rag/rerankers/)
- [ElasticSearch - What is semantic reranking and how to use it](https://www.elastic.co/search-labs/blog/elastic-semantic-reranker-part-1)
