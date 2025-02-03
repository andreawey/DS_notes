another [[Discretization]] algorithm is entropy based

1. Calculate entropy for your data
2. For each potential split in your data
	1. calculate entropy in each potential bin
	2. find net entropy for your split
	3. Calculate entropy gain $Gain(E_{new})= E_{initial} - E_{new}$
3. Select the split with the highest entropy gain
4. recursively perform the partition on each split until a termination criteria is met
	1. Terminate once you reach a specified number of bins
	2. terminate once entropy gain falls below a certain threshold

The  information across the bins $Info_{Split}(D)$ is equal to the proportion of the bin's size multiplied by that bin's entropy 

Entropy Gain $Gain(E_{new})$ = is a measure of the improvement to the information we get from the data, resulted from the split

The algorithm maximizes the entropy gain

> Termination Criteria
> - Terminate when a specified number of bins has been reached
> 	- This makes sense when you value the degree to which you understand your data
> 	- A data set with 3 bins is much easier to think about than one with 12
> - Terminate when information gain falls below a certain threshold 
> 	- If information gain drops below a certain value, the algo can terminate early
> 	- both of these criteria could be used in conjunction to yield optimal results

Final considerations on discretization
- binning has generally no beneficial effect for #tree -based models
- #linear models benefits greatly in expressiveness from the transformation of the data
