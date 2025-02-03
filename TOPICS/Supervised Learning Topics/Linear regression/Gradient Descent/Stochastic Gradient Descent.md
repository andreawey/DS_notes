[[Batch Gradient Descent]] uses the whole training set to compute the gradients at each step -> very slow when training set is large

Stochastic [[Gradient Descent]] picks a random instance in the training set at every step and computes the gradients based only on that single instance

Much faster since it has a very little data to manipulate at every iteration
It allows to train on huge training sets, since only instance needs to be in memory at each iteration.

Randomness is good to escape from local optima, but bad because it means that the algo can never settle at the minimum 

-> solution
Gradually reduce the learning rage
Start with large steps, then get smaller and smaller allowing the algo to settle at the global minimum 

This is called simulated annealing
Learning schedule is the function that determines the learning rate at each iteration

If it is reduced too quickly 
- it might get stuck in a local min
- or it might end up frozen halfway to the min

if the learning rate is reduced too slowly:
- It might jump around the minimum for a long time and end up with a suboptimal solution if we stop training too early