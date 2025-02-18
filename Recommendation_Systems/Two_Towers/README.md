# Two-Tower Recommender Systems
* The main concept of the two-tower model system is that there are 2 separate neural networks, called towers.
* You can think of these two towers as separate models:
  * 1. "Query Tower" which represents the users
  * 2. "Candidate Tower" which represents the items.
* During training, each tower learns to transform an arbitrary set of input features into vectors known as embeddings.
* The dimension of these embeddings must be the same for both the users and the items as finally the similarity between them is measured using the dot product.

![image](https://github.com/user-attachments/assets/25ab0d4c-c5e7-4187-a6dc-ada2e1d50955)





## Cool resources to check out:
* google search --> “nvidia merlin course”
* Outerbounds end-to-end recommender systems
* End-to-end rec sys with NVIDIA MERLIN
* Mastering multilingual rec sys with nvidia merlin
* Build Scalable Retrieval Systems with Two Tower Models
* How Uber Eats Built a Rec System using Two Towers




# References
* [Video Recommendations at Joyn: Two Tower or Not to Tower, That Was Never a Question](https://medium.com/tech-p7s1/video-recommendations-at-joyn-two-tower-or-not-to-tower-that-was-never-a-question-6c6f182ade7c)
