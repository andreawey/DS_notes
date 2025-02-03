in [[Clustering]] we need distance measures especially in [[Flat Clustering]]

Distance measures for feature represented by numerical vectors x and y:
• the (squared) Euclidean distance $$ d_2^2(x, y) = \|x - y\|_2^2 = \sum_{i=1}^{n} (x_i - y_i)^2 $$• the maximum distance $$ d_\infty(x, y) = \|x - y\|_\infty = \max_{i} |x_i - y_i| $$• the Manhattan distance $$ d_1(x, y) = \|x - y\|_1 = \sum_{i=1}^{n} |x_i - y_i| $$• the mahanolobis distance$$ d_{\text{Mah}}(x, y) = \sqrt{(x - y)^T S^{-1} (x - y)} $$where S is the data covariance matrix.

Domain specific distances
• the hamming distance for strings of equal length comparisons
• the levenshtein distance for unequal strings
• the Jaccard distance between non-empty sets $$ d_J(A, B) = |A \cup B| - |A \cap B| $$
