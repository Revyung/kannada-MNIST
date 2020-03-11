# Kannada MNIST
Kannada is a language spoken predominantly by people of Karnataka in southwestern India. The language has roughly 45 million native speakers and is written using the Kannada script. This competition requires one to specially design a neural network to classifiy handwritten digits. Visit [here](https://www.kaggle.com/c/Kannada-MNIST) for more details.

# Things I have tried:
1. Make sure train and val, test are from the same distribution.
2. Use image augmentation whenever sensible.
3. For small dataset ~ 100k images (28 by 28 by 1) total, start with models with small sets of parameter ~ 50k all layers included.  
   That surely depends on nature of problem, but if one sees overfitting, simplify model structure.
4. Instead of using dropout, use batch normalization as this has proven to be more effective ... [see this link](https://www.kdnuggets.com/2018/09/dropout-convolutional-networks.html).
5. Use smaller learning rate than Adam optimizer's default ~ 0.0001 or a LearningRateScheduler in Keras.
6. Explore the use of average pooling vs max pooling ... [see this link](https://www.quora.com/What-is-the-benefit-of-using-average-pooling-rather-than-max-pooling).
7. Smaller batch size should be used with much smaller momentum for moving means and variance in batch normalisation.
8. Build for utility rather than complexity.
9.  Do error analysis to see why model got some predictions wrong, in this dataset, some of the ground truth labels are wrong.

# Experiment details:
| filter nos in 2 sets of blocks | kernel size in 2 sets of blocks | batch norm usage | dropout usage | (extra) | accuracy |(comment) |
|---|---|---|---|---|---|---|
|16, 32| (5, 5), (3, 3)| yes| yes || 0.987
|16, 32| (5, 5), (3, 3)| yes| yes but only at last FCL || 0.979 |using less dropout has worsen performance
|32, 16| (3, 3), (5, 5)| yes| yes but only at last FCL || 0.985 |learning low lvl features first is better
|32, 16| (3, 3), (5, 5)| yes (added after every conv)| none || 0.9958 |batch norm to replace dropout is better
|32, 16| (3, 3), (5, 5)| yes (added after every activ)| none| added 1 FCL | 0.9967| slightly more complexity, 2-3 FCL at last is better
|32, 16| (3, 3), (5, 5)| yes (added after every activ)| none| added 1 FCL & from max_pool to average pool |  0.9968
|64, 32| (3, 3), (5, 5)| yes (added after every activ)| none| added 1 FCL & from max_pool to average pool | 0.9959 |no performance from increased kernel complexity
|32, 16| (3, 3), (5, 5)| yes (added after every activ)| none| added 1 FCL & from max_pool to average pool | 0.9958 | smaller lr & improvement criterion for early stopping callback

