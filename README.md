## Improving Movie Recommendations with Spark and the Surprise Library

1.	Problem
2.	The Dataset
3.	The Models
4.	Recommendations

**1.	The Motivation**

Company X has used production recommenders for many years now. The current recommender, called **Mean of Means**, relies on per-user and per-item rating means and incorporates the global mean for smoothing. Mean of Means provides a significant revenue stream so product managers are hesitant to touch it. The issue is that these systems have been around a long time. The task is to explore new solutions, with the main goals of improving the Root Mean Square Error on the predictions, creating a working recommender written in **Spark**, and using the **surprise** library to explore other models that could potentially be prototyped for production.

**2.	The Dataset**

A classic recommendation dataset, **MovieLens** is one of the built-in datasets in the surprise. (For more information, see: https://grouplens.org/datasets/movielens/.) The dataset comes in two versions:
* Small: 100,000 ratings and 2,488 tag applications applied to 8,570 movies by 706 users. Last updated 4/2015.
* Full: 21,000,000 ratings and 470,000 tag applications applied to 27,000 movies by 230,000 users. Last updated 4/2015.

**3.	The Models**

I compared the current recommender with several collaborative filtering engines using the **surprise** library. These engines all use past item-user ratings to predict the ratings a user would give an unrated item. Here’s a great animation from Wikipedia on how collaborative filtering works:
<p><a href="https://commons.wikimedia.org/wiki/File:Collaborative_filtering.gif#/media/File:Collaborative_filtering.gif"><img src="https://upload.wikimedia.org/wikipedia/commons/5/52/Collaborative_filtering.gif" alt="Collaborative filtering.gif"></a><br>By <a href="//commons.wikimedia.org/w/index.php?title=User:Moshanin&amp;action=edit&amp;redlink=1" class="new" title="User:Moshanin (page does not exist)">Moshanin</a> - <span class="int-own-work" lang="en">Own work</span>, <a href="https://creativecommons.org/licenses/by-sa/3.0" title="Creative Commons Attribution-Share Alike 3.0">CC BY-SA 3.0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=24097346">Link</a></p>
<br>
Surprise is a Python library for building and analyzing recommender systems. It offers several recommender options, plus the ability to build new recommender systems, and the option to gridsearch for the best tuning parameters. 
<br>
Surprise’s recommenders fall into 4 basic types: 
* baseline, 
* k-Nearest Neighbors (with pearson and cosine similarity distance metric options), 
* matrix factorization-based recommendation engines (SVD and NMF, plus an extension of SVD for implicit ratings), 
* co-clustering, and
* slope one.
<br>
The best performing models were:
k-Nearest Neighbors

![k-Nearest Neighbors](https://www.jeremyjordan.me/content/images/2017/06/Screen-Shot-2017-06-17-at-9.30.39-AM-1.png) 

and matrix factorization
![matrix factorization](https://www.altoros.com/blog/wp-content/uploads/2018/03/collaborative-filtering-with-tensorflow-for-recommender-systems-v1.png) 
<br>
The Spark ML machine learning library currently only has one recommendation engine.

**4.	Conclusion**

Many of the collaborative filtering options in **surprise** predict ratings more accurately than the current recommender. The one recommender option available in Spark also lowered the RMSE once implemented with the optimal parameters found through GridSearch. Overall, the Root Mean Squared Error was reduced from 1.0175 (**Mean of Means**) to 0.6254 (**Spark ML**).

