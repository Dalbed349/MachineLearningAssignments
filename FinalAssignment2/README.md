## Machine Learning Fall 2020.

### Final Assignment 2.

#### Image Recognition 

1. The goal for this assignment was to build and extract features from photographs in order to train a model to detect the presence of airplanes. In line [5] a Histogram of Oriented Gradient (https://scikit-image.org/docs/dev/auto_examples/features_detection/plot_hog.html) feature detector is used. The parameter for pixels_per_cell is set to (12,12) and the parameter for cells_per_block is set to (3,3). These affect the granularity of the transformed images that are returned. As pixels per cell is increased (to 15,15) the images became more identifiable to the human eye at the expense of performance during test. 
2. In line [7] the images are converted to a numpy nd array and then reshaped to a flattened so that there is one image per row. A train and test set are created. 
3. In line [23] a Multilayer Perceptron Neural Network model is selected for training. 
	``` 
	nn = neural_network.MLPClassifier(alpha=.000000001,max_iter=100000, hidden_layer_sizes=(4,8,16))
	```
	The alpha value, max iterations, and hidden layer sizes were adjusted to have optimal performance. 
4. Performance was measured based on classifications of True Positives, True Negatives, False Positives, and False Negatives. Accuracy, precision, and recall were all calculated. 
```
TRAINING SET: 
{'Pos': 76, 'Neg': 4992, 'TP': 76, 'TN': 4991, 'FP': 1, 'FN': 0, 'Accuracy': 0.999802683504341, 'Precision': 0.987012987012987, 'Recall': 1.0, 'desc': 'nn', 'set': 'train'}
```
```
TEST SET: 
{'Pos': 25, 'Neg': 1665, 'TP': 24, 'TN': 1665, 'FP': 0, 'FN': 1, 'Accuracy': 0.9994082840236687, 'Precision': 1.0, 'Recall': 0.96, 'desc': 'nn_test', 'set': 'test'}
```
Ideally a combination of features and neural network parameters would have been selected with a result of 0 false positives and 0 false negatives. Unfortunately after many iterations there was always a single instance of each. Upon further inspection the False Positive on the training set was a cloud that had a striking resemblance to the form of an airplane. On the test set an image of a plane with a heavy shadow through the middle and poor wing visibility was responsible for the False Negative. 

5. One issue with the current features and neural network parameters is that they have not been very generalizable for different random seed numbers. The random seed determines a subset of the images to use as train and test data. It is possible that some 'draws' of the random seed may contain images not ideal for training and thus produce poor test performance. It is also possible that the number and quantity of neurons in the hidden layers could use adjustment before future use. Overall this project is a solid introduction to image recognition in the world of machine learning and the models performance is acceptable. 