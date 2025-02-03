[[Clustering Anomaly Detection]]

Since we build [[Decision Trees]] the method is related to [[Clustering]], but it does not model normality. Instead, it explicitly isolates anomalies. It is a very efficient method with linear time complexity and low memory requirements.

## Key Assumption

IFOR assumes anomalies are rare events that differ from the nominal class (normal points). Anomalies lie in leaves with shallow depth.

## Training Process

1. Build a forest for a given dataset and, for each train set, generate a tree.
2. Select an axis to split, determine a split point on the axis. The procedure stops when:
    - Tree depth limit is reached.
    - Only one point is left in a leaf.
    - Points in a leaf share the same value.
   
## Operational Phase

- Compute the average path length among all the trees in the forest for a given point.
- A point is defined as anomalous if its anomaly score surpasses a certain threshold, which represents how shallow the point ended up being in the tree. The shallower the point, the more likely it is to be anomalous, as the tree could easily isolate them (they are far from the leaves).
