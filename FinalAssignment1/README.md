
## Machine Learning Fall 2020.

### Final Assignment 1.

#### Natural Language Data

  

1. Feature Extraction part 1: The first step for this project was to create a sparse matrix from the Wikipedia comments. A bag of words approach is taken. In line [3] a hashing vectorizer is used to create the sparse matrix. 
	```       
	  hv = HashingVectorizer( n_features=2 ** 16, alternate_sign=False, strip_accents='ascii', lowercase=True, ngram_range=(1,2))
	  ```
	  This code sets the number of features to 2**16, strips any accents from words, converts all words to lowercase, and extracts bigrams (pairs of words).  
2. Feature Extraction part 2: in the same cell, additional quantitative features are added. These are the number of words, the number of sentances, number of '!', number of '?', and the count of Uppercase letters. 
	```
	toxic_data['word_count'] = toxic_data['comment_text'].str.split(' ').str.len()
    toxic_data['punc_count'] = toxic_data['comment_text'].str.count("\.")
    toxic_data['punc_count2'] = toxic_data['comment_text'].str.count("\!")
    toxic_data['punc_count3'] = toxic_data['comment_text'].str.count("\?")
    toxic_data['upper_case'] = toxic_data['comment_text'].str.count(r'[A-Z]')

    X_quant_features = toxic_data[["word_count", "punc_count", "punc_count2", "upper_case"]]```
3. In line [4] these quantitative features are calculated and the shapes of the Train and Test sets are printed. In this case the train is (127656, 14) and the test is (31915, 14).
4. Various models are tested in the code cells that follow. Ultimately a Ridge Regression Classifier is chosen to be the model for the final test fitting. The following changes are made to the default RDG parameters. 
```
rdg = linear_model.RidgeClassifier(alpha=30,fit_intercept=True,normalize=True,copy_X=True, class_weight='balanced',solver='sag')
rdg.fit(X_train, y_train)
```
a. An alpha value of 30 was determined to be optimal, at least within the iterations tested. This value could be shifted anywhere between 20 and 40 to produce similar results and it is possible that the optimal value is not 30. From sklearn: "Regularization strength; must be a positive float. Regularization improves the conditioning of the problem and reduces the variance of the estimates. Larger values specify stronger regularization."

b. fit_intercept= True: Default value. Whether to calculate the intercept for this model. If set to false, no intercept will be used in calculations (e.g. data is expected to be already centered).

c. normalize=True: If True, the regressors X will be normalized before regression by subtracting the mean and dividing by the l2-norm. This changed provided significant improvements to the model. 

d. copy_X=True: Default value. X is copied and not overwritten. 

e. class_weight='balanced': The “balanced” mode uses the values of y to automatically adjust weights inversely proportional to class frequencies in the input data as `n_samples  /  (n_classes  *  np.bincount(y))`. This change from the default value of None provided significant improvements on the train and test sets. 

f. solver='sag': The biggest shift in performance with the RDG model occurred when the solver was set to 'sag' when compared to the default value of 'auto'. 'sag’ uses a Stochastic Average Gradient descent.

5. The test results for the model are as follows: 
		``` 
		{'Pos': 3246, 'Neg': 28669, 'TP': 2712, 'TN': 26072, 'FP': 2597, 'FN': 534, 'Accuracy': 0.9018956603477989, 'Precision': 0.5108306649086457, 'Recall': 0.8354898336414048, 'desc': 'rdg_test'}
		```
		![Test_Performance.PNG](https://github.com/Dalbed349/MachineLearningAssignments/blob/main/FinalAssignment1/Test_Performance.PNG "Test_Performance.PNG")
6. Final CSV of test performance ratings can be found in /dataIterations/[toxiccomments_submission3.csv](https://github.com/Dalbed349/MachineLearningAssignments/blob/main/FinalAssignment1/dataIterations/toxiccomments_submission3.csv "toxiccomments_submission3.csv")
