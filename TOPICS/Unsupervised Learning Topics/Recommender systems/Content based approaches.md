[[Recommender Systems]]

We have features for users and items. Cast the whole recommendation task as a classification
problem (does user X like movie Y or not?) or regression problem (predict the rating of movie Y
by user X). Any classifier or regressor can in principle be used.

### Properties
- Always assume a model,
- Easy to use advanced algorithms,
- Cold-start problem greatly alleviated,
- If the user or item features are not good enough, content-based systems might blur differences
  and lack personalization.

#### Item-centered
- User features are used,
- Model tries to answer the question whether a user likes or dislikes a particular item.

#### User-centered
- Item features are used,
- Model tries to answer the question which items fit to a particular user.

It is often easier to collect item features than to collect user features!
