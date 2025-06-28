Similarity can be measured by various metrics, the choice depending on your application, data and model. Similarity can be based on distance (L1, Lp, ...) or on orientation (dot prduct) or on intersection/overlap (Jaccard dist.) or on distributions (KL divergence)

# Distance based metrics
in High dimensions, distance is tricky, data tends to be concentrated around the surface, not the center, data is spread out and sparse.
- Reason: the ration of the volume near the center to the rest of the volume becomes very small
The ration of the distances of nearest and farthest neighbors to a given point is approx. 1 for a wide variety of data distributions and distance functions.
Implication: relative difference between distances becomes less significant in Euclidean terms, data points are effectively equidistant from one another

## Orientation based metrics
### Cosine similarity 
Cosine similarity is determined by the orientation of vectors, not the Euclidean distance of data points
- Values of cosine similarity lie between -1 and 1
- Cosine similarity increases if the two vectors point in the same direction
- Interpretability of cosine similarity is clearer than for unbound L1 and L2 metrics
Note: occasionally cosine distance is implemented $Cos distance(x,y) = 1 - Cos(x,y)$

## Distance vs Orientation based metrics
Euclidean norm (L2) and cosine similarity (cos) time complexity is both $O(n)$ where n is the dimensionality of the vector space
Dot product uses simpler operations than the euclidean norm

Note: if vectors are normalized to length 1, 
- Cosine similarity can be computed faster via dot product
- Cosine similarity and Euclidean distance are closely related 
- Similarity based ranking of vectors under both L2 and Cos metrics are equivalent!

## Application of VSM and similarity metrics for IR
- Compute vector representation $v(D_i)$ for each document $D_i$. This can be applied off-line ($v(D_i)$ can be pre-computed)
- Similarly, compute a vector representation $v(Q)$ for a given query $Q$
- Identify most relevant documents, e.g by highest cosine similarity for a given query 
![[TFIDF OKAPI.png]]

>[!FAQ] Conclusion
>So far we have been representing text as Bag of Words with the sparse Vector Space Model and applying similarity metrics
>Advantages of this approach are the following:
>- concept is simple and well-founded
>- Documents can be ranked according to relevance using similarity with the query (continuous scale)
>- Matching of terms between query and document can be partial (subset)
>Limitations of this approach include
>- Documents/queries with similar content but different term vocabularies (e.g .synonyms or plurals) will not be associated
>- Word order is ignored ("parking fine" vs "fine parking" )

