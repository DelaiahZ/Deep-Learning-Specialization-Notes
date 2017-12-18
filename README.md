# Coursera-Deep-Learning-Specialization
Things about the DL specialization from Coursera.

# Improving Deep Neural Networks: Hyperparameter tuning, Regularization and Optimization

## Week 1 - Practical Aspects of Deep Learning

### Setting up your Machine Learning Application

#### Train/Dev/Test sets
1. What are train/dev/test sets?
1. How do you choose train, test, and dev sets? (Previous era vs. this era; smaller dataset vs. larger dataset)
1. What happens when the data distribution of train, test, and dev sets are different? (test and dev. data distribution have to be same)

#### Bias/Variance 
1. Describe bias and variance intuitively. What is the bias variance trade-off? (Trick question since people don't talk about the bias variance trade-off nowadays. All 4 scenarios can be seen.)
1. Give examples for each scenario.
1. How do you decide which 4 of the possible scenarios you are in? (Check train error, compare with Bayes error - decide high bias/low bias; then check dev error, compare with train error)

#### Basic recipe for Machine Learning
1. How to approach in reducing bias and variance? (Talk about the basic ML recipe)
1. Why is the bias-variance tradeoff not important anymore in the era of DL? (Because in the earlier era of ML, almost all techniques that increased one decreased the other. Now we have techniques that can almost solely affect one of them. For example, bigger network solely reduces bias and more data solely reduces variance)

### Regularizing your neural network

#### Regularization

1. What is L2 regularization? 
1. Do we have to regularize both weights are biases? (For each neuron, most parameters are in the weight vector since it's high dimensional. So regularizing bias is not required. Can be omitted.)
1. What is the L1 regularization? What are the consequences? (sparse network - most weights zero) When is it typically used? (compressing neural networks, but doesn't work much in pratice)
1. What is the regularization parameter? How do you set it? (another hyperparameter - so use dev set)
1. What is the formula for L2 regularization for a multi-layer neural network?
1. What is Frobenius norm? (L2 norm of a matrix; basically sum of squares of all elements)
1. How does the weight update rule get changed when L2 regularization is added?
1. Why is L2 regularization called the weight decay?

### Why regularization reduces overfitting?

1. Intuitively why does regularization prevent overfitting? (automatic pruning of network; classicial reason of operating in the linear region of an activation function)
1. Difference between cost function and loss. (loss is a part of CF)

### Dropout regularization

1. What is dropout? Why does it help in regularization?
1. How do you implement dropout? (Inverted dropout implementation)
1. How does backprop happen in a dropout layer?
1. Why is the inversion with keep_prob required during training? (Reducing the computation at test time- just no dropout network is fine)

### Understanding dropout

1. Why does dropout work? (From the network perspective - smaller network at each iteration; from a neuron's perspective - spreading out its weights since inputs can randomly go away -> effect is shrinking the L2 norm of weights i.e. adaptive form of L2 regularization
1. How do you choose the dropout factor for layers? (keep prob smaller for layers with more parameters i.e. layers that have more chance of overfitting)
1. What is a downside of dropout? (cost function of J is not well-defined)

### Other regularization methods

1. What are other regularization tricks? (data augmentation, early stopping, 
1. What is data augmentation and why is it used? (flip horizontal, random zooming., random rotations, distortions depending on the problem; Doesn't add much informarion but might regularize)
1. What is early stopping and why is it used? Why does it work? (weights start from close to zero and then increases; early stopping chooses weights at the mid range)
1. What is the advantage and disadvantage of early stopping? (adv. = unlike L2 norm which requires multiple passes of SGD to find the hyperparameter lamda early stopping requires only 1 pass. disadv. = combines the optimization and regularization part)
1. What is orthogonalization in the context of ML?

## Setting up your optimization problem

### Normalizaing inputs

1. How to normalize input? (for each dimension subtract mean and dividie by std. dev., remember that test set has to see the same transformation)
1. Why do we normalize the input data? (otherwise elongatated cost function so magnitude of weights for differnt dimensions are very dissimilar -> slower to optimize )
1. When do we normalize the input data? (When i/p features come from very different ranges, although doing it doesn't do any harm)

### Vanishing/Exploding gradients

1. Explain the phenomenon in case of vanishing/exploding gradients. (explain the phenomenon and talk about why it is not very important anymore in case of feed-forward networks. But still important for RNNs.) (Very interesting read - https://medium.com/@karpathy/yes-you-should-understand-backprop-e2f06eab496b for the case of sigmoids and tanh)(It is better to talk about the classical problems first and then come to modern probelms and talk about the random block initialization paper and why it is less of an issue.)(Networks facing this problem in order of severity -> RNN, FFN w sigmoid/tanh, Mod. ReLU).


### Weight initialization for deep networks

1. How do you intialize the weight of any layer of a deep network? Why? Discuss the changes for different activation functions (ReLU and tanh).
1. What is He intialization? Xavier intialization? Glorot and Bengio initialization? (Also an interesting paper: https://arxiv.org/pdf/1704.08863.pdf)

### Numerical approximation of gradients

1. Should you take a two sided difference or one sided difference while calculating gradients? (Accuracy of calculation: two-sided error is in O(ephsilon^2) whereas one-sided is in O(ephsilon). Although, two-sided is much more accurate, one-sided is faster.)

### Gradient checking

1. Why is gradient checking necessary?
1. How to perform gradient checking for a neural network? (Give formula) 
1. What is a good ballpark value of epshilon that can be used for calculating gradients?

### Gradient checking implementation notes

1. What to do if your algorithm fails gradient checking?
1. Should you using grad check throughout training? If not, why not?
1. How do you handle regularization during grad check? (IF L2 or additive regularzation just add that; but doesn't work with dropout)
1. How do you do grad check for a neural network that has dropout? (Just turn off dropout, check and it the algo passes grad check, turn on dropout)
1. Do we do grad check before training? (Yes, and also after some training epochs have passed.)


## Week 2 - Optimization Algorithms

### Mini-batch gradient descent

1. What is mini-batch gradient descent? Compare it with batch gradient descent (stable but too long per iteration)? Also compare with stochastic gradient descent (very noise, doesn't converge, and loose speed up from vectorization) (Also remember this paper - https://arxiv.org/pdf/1609.04836.pdf)
1. What are the advantages of mini-batch GD? (Can exploit vectorized implementations, more gradient descent steps than batch GD, less noisy than Stochastic GD)
1. How to choose the size of the mini-batch? (In between 1 and m; Guidelines: If small training set (<2000) just use m i.e. batch GD, for bigger training set typical mini-batch sizes: 64, 128, 256, 512 (power of 2 since makes use of computer architecture), Make sure each minibatch fits GPU/CPU memory, mini-batch can also be a hyperparameter - explore powers of 2).

### Exponentially weighted averages

1. Explain the general exponential moving average formula. (V_t = beta * (V_t-1) + (1-beta) * theta_t))
1. Approximately, how many previous steps does it average upon? (1/(1-beta))

### Understanding exponentially weighted averages

1. Why the name "exponentially"?
1. What is the advantage of this technique? (Computationally efficient)

### Bias correction in exponentially weighted averages

1. Why is bias correction required? (Starts with zero, the estimate for initial phases are inaccurate)
1. How is bias correction done? 

### Gradient descent with momentum

1. What is the core idea of this algorithm? (calculate exponentially weighted moving averages of gradients and use this to update weights)
1. Why can't larger learning rates might sometime create problem in GD? (If oscillations happen in some directions, they would get intensified)
1. What is the formula for this algorithm?
1. Intuitively, why does momentum work? (oscillations are averaged out to some extent; so larger learning rates can be used -> bigger steps towards the minimum -> speeding up the optimization process.)
1. What is a common value of beta that is used?

### RMSprop

1. What is the RMSprop formula? Explain it intuitively.
1. How to add numerical stability to the RMSprop algorithm?
1. What is the advantage of RMSprop? (Averaging out oscillations -> allows us to use a larger learning rate -> speeding up the optimization process)

### Adam optimization algorithm

1. What is the core idea of Adam? (Combining gradient descent with momentum and RMSprop)
1. Write the formula.
1. How are the hyperparameters selected for Adam? (alpha = needs to be tuned, beta_1 = 0.9, beta_2 = 0.999, ephsilon = 1e-8; typically only alpha is tuned)

### Learning rate decay

1. What is the intuition behind slowly reducing the learning rate decay?
1. How to implement the learning rate decay? (staircase or formulae as a function of epoch - some of them introduce new hyperparameters)

### The problem of local optima

1. How has the concept of local minima in the context of optimization evolved in modern deep learning? (lots of intuitions on lower dimensional spaces doesn't translate to higher dimensions; For example, local optima rarely occur in high dimensional spaces, saddle points are more likely . Derivatives are also zero in a saddle point.)
1. What is the challenge faced by optimization in deep learning? (Problem of plateau - makes learning pretty slow - Adam, RMSprop can really help speed up learning here.)


