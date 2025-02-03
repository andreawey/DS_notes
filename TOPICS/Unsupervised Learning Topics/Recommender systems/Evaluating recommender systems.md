[[Recommender Systems]]

If the output of the system is class-like (e.g. positive/neutral/negative): one can use the classification accuracy as a quality measure.
- If the output is a real value (e.g. the predicted user): the MSE could be used as a criterion.
- If the output is a list: one can evaluate how many items match between reference and hypothesis list, or whether the order matches between items score).

These approaches only work if explicit user ratings are available. In cases like targeted advertisement, reference data (do users click an ad banner) have to be acquired implicitly, which makes it far more complicated to get reliable data.

### User satisfaction is more complex:
- Recommendations must be diverse enough to not confine the user in an “information bubble”.
- Users lose confidence in a RS if they do not understand why certain elements have been recommended.
