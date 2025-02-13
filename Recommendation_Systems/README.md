# Recommendation Systems
* By Adam Lang
* A repo devoted to all things related to recommendation systems. 


# What a recommender system is NOT
* This is NOT a system that recommends “arbitrary” values or predictions, that is a machine learning problem in general. 


# So What is a recommender system?
* Generally speaking it predicts ratings or preferences a user might give to an item. 
* These are often a list of items sorted, ranked or presented to a user based on Top-K or Top-n recommendations
* These are also known as:
    * Recommender engines
    * Recomendation systems
    * Recommendation platorms 
    * Personalization systems

* A good recommendation system consists of: **candidate generation, scoring, and re-ranking.**
* Architecture forms the foundation for the data pipeline for any algorithm(s) you use. 
* It is important to have a well planned approach to data management and implementation of any algorithm(s) and always consider: 
   * Offline vs. Online environments
   * Candidate retrieval vs. Ranking steps
 
# Recommendation Systems - Core Concepts
* Recommendations are machine learning (ML) models that determine a users preference based on their past history, or by analyzing similar users preferences.
* These systems typically have:
  * **documents**
    * These are entities a system recommends, like movie, videos etc..
  * **queries**
    * information you need to make recommendations, like user info, location etc.
  * **embeddings**
    * mapping from a set (of queries or documents to recommend) to a vector space called embedding space.
 
* The **most COMMON architecture** for recommendation systems consists of the following:
  * 1. Candidate generation
  * 2. Scoring
  * 3. Re-Ranking

* [image source](https://developers.google.com/machine-learning/recommendation/overview/types)
   
![image](https://github.com/user-attachments/assets/65b73f8d-5c57-4d2f-bf0f-8d0d1ecd0f2d)

## Candidate Generation
* This is where a recommendation system model starts. 
* It has a huge corpus (database) and generates a smaller subset of “candidates”.
    * Example: Netflix has thousands of movies, but has to pick which ones to show you on your personal home page!
* A model needs to evaluate queries at rapid speed on a large corpus and keep in mind, a model may provide multiple candidate generators which each having different candidate subsets.

## Scoring
* This is usually the next step in a recommendation system architecture. 
* Here, we simply rank generated candidates in order to select a set of documents.
* Here a model runs on a smaller subset of documents, so we can aim for higher precision in terms of ranking.


## Re-ranking
* This is very often the final step.
* The goal is to Re-Rank recommendations obtained during Candidate Generation and Scoring.
    * Example: Continuing our Netflix example from above, if a user explicitly dislikes some movies, Netflix will need to move those out of the user’s main home page.
* Many large scale recommendation systems are implemented in two steps: 
1) Retrieval
2) Ranking

### Retrieval
* Retrieving more items will result in better results but will slow computations for recommendations.
* Often times it is best to carry out offline experiments to see if retrieving additional items results in more relevant recommendations.
  * From Andrew Ng’s machine learning notes (most Data Scientists have seen this before):
 
![image](https://github.com/user-attachments/assets/0705d595-6eaa-40e6-a47c-4ba722ba72b3)


### Ranking
* Ranking is a core task in recommender systems, which aims at providing an **ordered list of items to users.**
* A ranking function is learned from the labeled dataset to optimize the global performance, which produces a ranking score for each individual item.
* The **quality of the ranked list** given by a ranking algorithm has a great impact on users’ satisfaction as well as the revenue of the recommender systems.
  * From Andrew Ng’s machine learning notes (most Data Scientists have seen this before):
 
![image](https://github.com/user-attachments/assets/45d8b5b9-6d7c-4941-a9f1-c738794ac51d)

# Architecture Design for Industrial Recommendation Systems
* Specific to discovery systems (i.e., recommendations and search), most implementations follow a similar paradigm—components and processes are split into:
  * 1. Offline vs. Online environments
  * 2. Candidate retrieval vs. Ranking steps
 
* The 2 x 2 chart below simplifies this. Where does your system fit in?

![image](https://github.com/user-attachments/assets/7e21e7a6-50ad-4f44-91d5-84363111189f)

Source: Yang, 2021

## 1. Offline environment
* Hosts batch processes such as:
  * Machine learning model training (e.g., representation learning, ranking)
  * creating embeddings for catalog items
  * building an approximate nearest neighbors (ANN) index or knowledge graph to find similar items. 
* This may also include loading item and user data into a feature store that is used to augment input data during ranking.

## 2. Online environment
* This uses generated artifacts (e.g., ANN indices, knowledge graphs, models, feature stores) to serve individual requests. 
* A typical approach is converting an input item or search query into an embedding, followed by candidate retrieval and ranking. 

## 3. Candidate retrieval
* fast-refined—step to narrow down millions of items into hundreds of candidates. 
* We trade off precision for efficiency to quickly narrow the retrieval space
  * (e.g., from millions to hundreds, a 99.99% reduction) for the downstream ranking task. 
* Most contemporary retrieval methods convert the input (i.e., item, search query) into an embedding before using ANN to find similar items. 
* In some examples, we’ll also see systems using graphs (DoorDash) and decision trees (LinkedIn).

## 4. Ranking 
* Slower—but more precise—step to score and rank top candidates. 
* As we’re processing fewer items (i.e., hundreds instead of millions), we have room to add features that would have been infeasible in the retrieval step (due to compute and latency constraints).
* Such features include item and user data, and contextual information. You can also use more sophisticated models with more layers and parameters.
* Ranking can be modeled as either a:
  * 1. Learning-to-rank task
  * ...or...
  * 2. Classification task (more common)

* If deep learning is applied, the final output layer is either a softmax over a catalog of items, or a sigmoid predicting the likelihood of user interaction (e.g., click, purchase) for each user-item pair.

# Implementation of Online vs. Offline & Retrieval vs. Reranking

![image](https://github.com/user-attachments/assets/d5f1a0d6-e9bd-419e-8690-d61f79974b61)

## Offline environment: data flows bottom → up
* Here we use training data and item/user data to create artifacts such as models, ANN indices, and feature stores. 
* These artifacts are loaded into the online environment (via the dashed arrows).

## Online environment: each request flows left → right
* Through retrieval and ranking steps before returning a set of results (e.g., recommendations, search results).

## Additional details:
1. With a trained representation learning model, embed items in the catalog.
2. With item embeddings, build the ANN index that allows retrieval of similar embeddings and their respective items.
3. Get (historical) features to augment training data for the ranking model. Use the same feature store in offline training and online serving to minimize train-serve skew. Might require “time travel”.
4. Use input query/item(s) embedding to retrieve k similar items via ANN.
5. Add item and user features to candidates for downstream ranking.
6. Rank candidates based on objectives such as click, conversion, etc.


# Candidate Generation vs. Candidate Retrieval in Recommendation Systems
* Candidate generation is the first stage of recommendations and is typically achieved by finding features for users that relate to features of the items.
* Candidate generation is for recommender systems like a search engine where the candidates are not particularly based on time (Eg: searching for how to tie a tie should roughly return the same results in every instance).
* The two most common candidate generation algorithmic approaches:
  * 1) Content-based filtering
  * 2) Collaborative filtering
* Candidate retrieval is for news-feed or home-page based systems where time is important, such as Instagram or Facebook, to make sure you don’t keep seeing the same content again and again. 

# Two common candidate generation approaches
1. **Content-based filtering**
  * Uses similarity between content to recommend new content.
  * Example: if a user watches corgi videos, the model will recommend more corgi videos.

2. **Collaborative filtering**
  * Uses similarity between queries (2 or more users) and items (videos, movies) to provide recommendations.
  * A common method for candidate generation. 
  * Example: if user A watches corgi videos and user A is similar to user B (in demographics and other areas), then a model can recommend corgi videos to user B even if user B has never watched a corgi video before.
 
## Content-Based Filtering
* A common method for candidate generation. 
* Mechanism of action
  * By examining attributes or features of items, such as textual descriptions, images, or tags, and constructing a profile of the user’s preferences based on the attributes of items they have engaged with. 
  * The system then recommends items similar to those the user has previously interacted with, drawing on the similarity between the item attributes and the user’s profile.

## Types of Content-Based Filtering
1) Item-Based Filtering
  * system suggests new items based on similarity to items previously selected by the user, which are treated as implicit feedback. 
  * Example:

![image](https://github.com/user-attachments/assets/7930a9a7-55dc-4c72-a32a-fa4acedadece)

2) User-Based Filtering
  * Collecting user preferences through explicit feedback mechanisms, and building a user profile. 
  * The gathered information is then used to recommend items with features similar to those of items the user has previously liked.
  * Example:

![image](https://github.com/user-attachments/assets/eadf8c99-ac3f-4f12-881f-2538132dbaf2)

### Content-Based Filtering Algorithmic Example
* Let’s say we want to implement an algorithm that matches users with items.

![image](https://github.com/user-attachments/assets/ac86f82a-8f48-4ca2-9577-db30e2537146)

![image](https://github.com/user-attachments/assets/6cd5b9ce-ab23-47e4-b23c-63215f43f7f5)

![image](https://github.com/user-attachments/assets/25eee755-317d-444e-ba40-e1e5bb89bc4a)

* Above we can see we have the user and the items.
  * The dimensions of the vector space for each are different starting out. 
  * Using a neural network (which excels at finding patterns in unstructured data!!) we can choose our layers, dimensions, weights and biases, run them through 2 networks and using an activation function scale the raw logits (predictions) to get the final prediction(s).
  * One activation function we can use is ReLU which outputs the same value as the input if the input is greater than or equal to zero, and outputs zero if the input is negative. 
  * Softmax is another common activation function used as it scales all logits between 0 and 1. 

# High-Level "Types" of Recommendation Systems
1. Recommending “things”
   * (e.g. Amazon purchases) based on historical patterns
2. Recommending “content”
   * (e.g. Social Media, NY times articles, Books)
3. Recommending “music”
   * (e.g. Spotify, Pandora) — this is content based as it is based on the properties of the things you are recommending—> song tempo, length, etc. 
4. Recommending people
   * online dating apps
5. Recommending Search Results “personalization”
   * Predict your own personal search results based on historical searches. 
  * where you live
  * what sites you visit
  * your personal interests
  * …etc…



# How do recommendation systems work?
* Always always always starts with user data!!!!

## Wait, so where does the data come from?
* Generally speaking there are 2 methods of collecting user data and interactions:
  
1. **Explicit feedback**
   * A user rates content with thumbs up or down or star reviews
   * This requires extra work on the part of the users to actually give feedback. 
   * This can be highly biased of skewed as everyone has different standards (e.g. 4 star review may not mean the same thing to everyone)

2. **Implicit feedback**
   * These are things you click on such as a website link —> is this a positive rating?
   * Users click on an ad —> we assume that you like this product or ad
   * Problems with this approach?
       * People often can/will click on things by accident and the click data may not be very accurate!
       * Click data can also be highly susceptible to FRAUD due to bots. 
   * Things you BUY or purchase seem to be more accurate as they are more resistant to fraud and also more accurate as we can often assume if you bought this —> then you will buy it again or something like it.
   * Things you consume
        * YouTube for example looks at minutes you spent watching a video and assume you liked it. 
   * Other social media websites may assume if you spend time on a page with an “impression” than you like it. 

3. Bottom line you need lots of good data!
   * Granted most recommendation systems suffer from the "cold start" problem, but any data is good data when building recommendation systems.
  

# Terminology
1. **“Top-N” recommenders**
   * These always return a list of the top-n results (e.g. N is 100)
   * Customers don’t care about your ability to predict an item a user has not seen before. 
   * Customers care about things that are relevant to them! 

* One way a top-n recommender works:
```
(item-based collaborative filtering)

individual interests —> candidate generation <—> item similarities
						    |
					candidate ranking (combine candidates to top-n)
						    |
                |
          filtering (remove non-relevant items)
                |
           display

* Another method
```
candidate generation <—> rating predictions database
               |
 	      candidate ranking
		           |
	      filtering
	             |
	         display
 ```

# Examples of Real-World Recommender systems
* Netflix homepage 
* Google search
* Amazon’s “people who bought also bought”
* Pandora music
* YouTube

# Examples of Top-N Recommenders
* Netflix recommender widgets (—> usually limited to 100)
* Amazon “people who bought also bought” (—> usually limited to 100)
* .....(Google is not, as it can be infinite unless you limit)
	

# Which are components of top-N recommenders?
1. Candidate generation
2. Filtering
3. Ranking
