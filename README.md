# practicalmachinelearning.github.io

Assignment Background
Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal 
activity relatively inexpensively. These type of devices are part of the quantified self movement - a group of enthusiasts who take
measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. 
One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it.
In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. 
They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. More information is available from the website 
here: http://groupware.les.inf.puc-rio.br/har (see the section on the Weight Lifting Exercise Dataset).


Expected Output
The goal of your project is to predict the manner in which they did the exercise. 

Task 1: To load the datasets and packages required to perform analysis and plot the outcomes

Task 2: To clean the data to remove Na values and keep the predictors to be  used for building the model. This leave us with 19622 observations and 53 predictors in training dataset.

Task 3: Our outcome variable classe is an unordered factor variable. Thus, we can choose our error type as 1-accuracy. Since we have training dataset with high number of samples, I set.seed(12345) and decided to  split my training datsets into (60:40) 60percent of training and 40percent of testing set to allow for cross-validation. The most accurate model will be choosen and tested on the original Testing dataset. Giving 11776 observations in the trainingCV group, and 7846 in the testingCV group.

Task 4: I tested two models Decision Tree and Random forest, model with highest accuracy was chose as final prediction model.

Results: Comparing two models Random forest appear to perform well (accuracy=0.9967, error rate=0.0033) as compare to Decision tree (accuracy=0.7466,error rate=0.2534). Therefore, Random forest model was used for final prediction. 

Conclusions: Prediction evaluation was base on model with high accuracy and low out of sample error rate. All other available variables after cleaning were used for prediction. Two models (ecision tree and random forest algorithms) were tested The model with the highest accuracy was chose as final prediction model.


