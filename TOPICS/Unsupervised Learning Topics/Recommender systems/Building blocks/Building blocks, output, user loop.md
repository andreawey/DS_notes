[[Recommender Systems]]
## Input Features
- User interaction data stems from the user’s behavior and can be explicit (e.g. ratings, comments, search logs, etc.) or implicit (order history, clicks, etc.).
- Item data are the item's features (e.g. video metadata, keywords; property location and amenities, etc.).
- Furthermore, Recommender Systems could make use of external data sources (e.g. expert reviews).

## Building Blocks, Output, User Loop
A Recommender System could output a single valuation for a given user and item (e.g. like/dislike, numerical ranking). It could also output a (sorted) list of possible candidates. Some real-life systems use multiple processing steps (e.g. candidate selection, ranking). All practical recommender systems influence the user behavior and thus their own future training data. Therefore, care must be taken to avoid a “rich-get-richer” effect (only popular items are recommended, so these items get even more popular). Also, systems must take into account which items have already been presented to the user.
