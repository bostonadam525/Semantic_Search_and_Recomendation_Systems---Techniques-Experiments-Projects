# Bandits for Recommendation Systems

## Overview
* Rec systems work well when you have a lot of data on **user-item preferences**.
* With a lot of data, you will obviously have high certainty about what your users like.
* However, very often we have very little data which results in low certainty.
    * Despite the low certainty, rec systems tend to greedily promote items that received higher engagement in the past.
    * And because they influence how much exposure an item gets, potentially relevant items that aren’t recommended continue getting no to low engagement, perpetuating the feedback loop.

## Bandits Solution
* Bandits can address this problem by modeling uncertainty and exploration.
* By acknowledging the uncertainty in the data and deliberately exploring to reduce it, bandits learn about the relevance of unexplored items.
* This is VERY applicable when your item set changes quickly, such as for news, ads, and tweets, or when the rate of traffic is low.
* If new items are constantly added, waiting to collect user batch data before retraining the model **can be too slow**.
* Bandits are a good fit as they can incrementally update with new data and adaptively focus on items with higher reward.
* This reduces regret, which is the opportunity cost while recommending suboptimal items.


# Contextual Bandits
* These bandits are a class of reinforcement learning algorithms used in decision-making models where a learner must choose actions that result in the most reward.
* Contextual Bandits are named after the classical "one-armed bandit" problem of gambling, where a player needs to decide which slot machines to play, how many times to play each machine, and in what order to play them.
* The decision-making process is informed by the context which is what makes this unique.
  * The context, in this case, refers to a **set of observable variables that can impact the result of the action**.
  * This addition makes the bandit problem closer to real-world applications, such as personalized recommendations, clinical trials, or ad placement, where the decision depends on specific circumstances.





# References
* [Bandits for Recommender Systems](https://eugeneyan.com/writing/bandits/)
* [Building a Multi-Armed Bandit System from the Ground Up: A Recommendations and Ranking Case Study, Part I](https://medium.com/udemy-engineering/building-a-multi-armed-bandit-system-from-the-ground-up-a-recommendations-and-ranking-case-study-b598f1f880e1)
* [Contextual Multi-Armed Bandit Problems in Reinforcement Learning](https://hackernoon.com/contextual-multi-armed-bandit-problems-in-reinforcement-learning)
* [How Netflix, Lyft, and Yahoo use Contextual Bandits for Personalization](https://www.geteppo.com/blog/netflix-lyft-yahoo-contextual-bandits)
