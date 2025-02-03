One-out-of-N [[Encoding]] replaces a categorical variable with k different possible values
We can represent any number of categories by introducing one new feature per category
When using this encoding in a ML algo we would drop the original feature and only keep the new features from the encoding

![[OneHotEncoding.png]]

Numbers can encode categorical:
- Often categorical are encoded as integers
- They are numbers but should not be treated as continuous features
- If there is no ordering between the semantics that are encoded the feature musts be treated as discrete.
- For other cases like ratings the better encoding depends on the particular task and data and which ML algo  is used.


