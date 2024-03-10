## How Does Image Classification Work
![[Pasted image 20240226090321.png]]
- normally split data into two sets:
	- Training set (for training the classifier)
	- Test set (for evaluating performance on unseen data)
![[Pasted image 20240226090432.png]]
### MNIST Example
![[Pasted image 20240226090510.png]]
![[Pasted image 20240226090530.png]]
#### We Could then Extract Features and Classify Using Many Techniques:
![[Pasted image 20240226090616.png]]
### K-NN Algorithm
![[Pasted image 20240226090644.png]]
![[Pasted image 20240226090652.png]]
### How Might We Use KNN for MNIST?
![[Pasted image 20240226090723.png]]
#### How Do We Define Neighbours?
![[Pasted image 20240226090824.png]]
![[Pasted image 20240226090949.png]]
![[Pasted image 20240226090958.png]]
![[Pasted image 20240226091009.png]]
#### The Tradeoff: Cost
- KNN is pretty expensive at inference time due to the lack of any training
- We could instead explore alternatives that involve more training in favour of faster evaluation
### Features
![[Pasted image 20240226091142.png]]
- What features shall we learn for a given dataset
- What classifier should we use?
![[Pasted image 20240226091247.png]]
#### RECAP: HOG
![[Pasted image 20240226091322.png]]
#### Support Vector Machine (SVM)
![[Pasted image 20240226091405.png]]
### Linear Classifier
![[Pasted image 20240226091429.png]]
![[Pasted image 20240226091442.png]]
- We go from calculating distance at eval time (expensive) to calculating which side of a line a point lies on (cheap)
![[Pasted image 20240226091541.png]]
![[Pasted image 20240226091618.png]]
- There are support vectors alongside the decision boundary; this is integral to the SVM architecture
#### Maximum Margin
![[Pasted image 20240226092258.png]]
- $m \bf{w} \cdot \bf{n}$ comes from substitution
- the margin is $2$ * distance between hyperplane and closest support vector; support vectors are equidistant if the dataset is linearly separable
![[Pasted image 20240226093016.png]]
![[Pasted image 20240226093031.png]]
![[Pasted image 20240226093054.png]]
- $y_i$​ is the true class label of the �ith data point (��=±1yi​=±1 for binary classification).
- $w$ is the weight vector (normal to the decision boundary).
- $x_i$​ is the feature vector of the �ith data point.
- $b$ is the bias term.
- $max⁡(0,⋅)$ denotes the hinge loss function, which penalises misclassifications. If the data point is correctly classified, the loss is zero. Otherwise, the loss is proportional to the distance from the correct classification.
![[Pasted image 20240226093536.png]]
![[Pasted image 20240226093656.png]]
### Dealing with MultiClass
![[Pasted image 20240226093723.png]]
![[Pasted image 20240226093746.png]]
![[Pasted image 20240226093823.png]]
