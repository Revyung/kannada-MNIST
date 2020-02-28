This is a solution for the Kannada-MNIST competition on kaggle.com implemented in Keras, visit kaggle.com/revyung2017.

Things I have done:
	make sure train and val, test are from the same distribution
    use image augmentation whenever sensible
	for small dataset ~ 100k images (28 by 28 by 1) total
		start with models with small sets of parameter ~ 50k all layers included
		of coz, that depends on nature of problem, but if u see overfitting, simplify model structure
    instead of using dropout, use batch normalization as this has proven to be more effective
        [see this link](https://www.kdnuggets.com/2018/09/dropout-convolutional-networks.html)
    taken some inspiration from resnet 34
	use smaller learning rate than Adam optimizer's default ~ 0.0001
    explore the use of average pooling vs max pooling
        [see this link](https://www.quora.com/What-is-the-benefit-of-using-average-pooling-rather-than-max-pooling)
	smaller batch size should be used with much smaller momentum for moving means and variance in batch normalisation
	build for utility rather than complexity
    do error analysis to see how model got some predictions wrong, sometimes, the ground truth label is wrong
    use early stopping or learning rate plateau callbacks in Keras API for improving performance

Experiment details:
    filter nos in 2 sets of blocks, kernel size in 2 sets of blocks, batch norm usage, dropout usage, extra -> accuracy, (comment)
    16, 32, (5, 5), (3, 3), yes, yes -> 0.987
    16, 32, (5, 5), (3, 3), yes, yes but only at last FCL -> 0.979 (using less dropout has worsen performance)
    32, 16, (3, 3), (5, 5), yes, yes but only at last FCL -> 0.985 (learning low lvl features first is better)
    32, 16, (3, 3), (5, 5), yes (added after every conv), none -> 0.9958 (batch norm to replace dropout is better)
    32, 16, (3, 3), (5, 5), yes (added after every activ), none, added 1 FCL -> 0.9967, (slightly more complexity (2-3 FCL at last is better))
    32, 16, (3, 3), (5, 5), yes (added after every activ), none, (from max_pool to average pool) ->  0.9968
    64, 32, (3, 3), (5, 5), yes (added after every activ), none, (from max_pool to average pool) -> 0.9959 (no performance from increased kernel complexity)
    32, 16, (3, 3), (5, 5), yes (added after every activ), none, (from max_pool to average pool) -> 0.9958 (smaller lr & improvement criterion for callbacks)



