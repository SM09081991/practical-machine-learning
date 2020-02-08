# practical-machine-learning
The goal of this assigment is to build a prediction model to predict the manner in which the
participants in this study perform their exercises. The data for this project come from this 
source: http://groupware.les.inf.puc-rio.br/har. 
  
Task 1: To load the data
#Loading the Data
library(caret) 
library(ggplot2)
library(lattice)
library(rpart)
library(rpart.plot)
library(randomForest)

-------training data------------------------------------------------
trainURL = "http://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv"  
training = read.csv(url(trainUrl), na.strings = c("NA", "", "#DIV0!"))

-------testing data------------------------------------------------------------
testURL = "https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv"
testing = read.csv(url(testUrl), na.strings = c("NA", "", "#DIV0!"))

Task 2: Clean the data to remove Na values and keep the predictors to be  used for building the model
#Pre-Processing/Cleaning data
-------Remove variables with NA values----------
training = training[,colSums(is.na(training)) == 0]
testing = testing[,colSums(is.na(testing)) == 0]

head(colnames(training), 10)
head(colnames(testing), 10)

-------Removing non-predictor columns-----------------------------------------------------------------------
training = training[,-c(1:7)]
testing = testing[,-c(1:7)]

Task 3: Since we have training dataset with high number of samples, I decided to  split my training datsets into 60percent of training and 40percent of testing set to allow for cross-validation. The most accurate model will be choosen and tested on the original Testing dataset. 

# Cross-validation
-------Splitting the data--------------------------------------------------------------------------------------
set.seed(12345)
inTrain = createDataPartition(training$classe, p=0.6, list=FALSE)
trainingCV = training[inTrain,]
testingCV = training[-inTrain,]
dim(trainingCV)
dim(testingCV)
-----Plots----------------------------------------------------------------------------------------------------------
qplot(accel_arm_x, accel_arm_y, col=classe, data=trainingCV)
qplot(accel_forearm_x, accel_forearm_y, col=classe, data=trainingCV)
qplot(accel_dumbbell_x, accel_dumbbell_y, col=classe, data=trainingCV)
qplot(accel_belt_x, accel_belt_y, col=classe, data=trainingCV) 

Task 4: I tested two models Decision Tree and Random forest, model with highest accuracy was chose as final prediction model 

#Prediction models
--------Classification Tree model----------
modelTree <- rpart(classe ~ ., data=trainingCV, method="class")
predictionTree <- predict(modelTree, testingCV, type="class")
Tree <- confusionMatrix(predictionTree, testingCV$classe)
Tree
Confusion Matrix and Statistics

          Reference
Prediction    A    B    C    D    E
         A 1995  246   49   83   51
         B   75  890  111  119  115
         C   44  198 1094  153  143
         D   74  112   79  840   94
         E   44   72   35   91 1039

Overall Statistics
                                          
               Accuracy : 0.7466          
                 95% CI : (0.7368, 0.7562)
    No Information Rate : 0.2845          
    P-Value [Acc > NIR] : < 2.2e-16       
                                          
                  Kappa : 0.6787          
                                          
 Mcnemar's Test P-Value : < 2.2e-16       

Statistics by Class:

                     Class: A Class: B Class: C Class: D Class: E
Sensitivity            0.8938   0.5863   0.7997   0.6532   0.7205
Specificity            0.9236   0.9336   0.9169   0.9453   0.9622
Pos Pred Value         0.8230   0.6794   0.6703   0.7006   0.8111
Neg Pred Value         0.9563   0.9039   0.9559   0.9329   0.9386
Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
Detection Rate         0.2543   0.1134   0.1394   0.1071   0.1324
Detection Prevalence   0.3089   0.1670   0.2080   0.1528   0.1633
Balanced Accuracy      0.9087   0.7600   0.8583   0.7992   0.8414

rpart.plot(modelTree)
 

-----------Random forest model-----------------------------------------------------------------------
modelRF = randomForest(classe ~ ., data=trainingCV, method="class")
predictionRF = predict(modelRF, testingCV, type="class")
RF = confusionMatrix(predictionRF, testingCV$classe)
RF
Confusion Matrix and Statistics

          Reference
Prediction    A    B    C    D    E
         A 2229    4    0    0    0
         B    3 1514    5    0    0
         C    0    0 1359    4    2
         D    0    0    4 1282    4
         E    0    0    0    0 1436

Overall Statistics
                                          
               Accuracy : 0.9967          
                 95% CI : (0.9951, 0.9978)
    No Information Rate : 0.2845          
    P-Value [Acc > NIR] : < 2.2e-16       
                                          
                  Kappa : 0.9958          
                                          
 Mcnemar's Test P-Value : NA              

Statistics by Class:

                     Class: A Class: B Class: C Class: D Class: E
Sensitivity            0.9987   0.9974   0.9934   0.9969   0.9958
Specificity            0.9993   0.9987   0.9991   0.9988   1.0000
Pos Pred Value         0.9982   0.9947   0.9956   0.9938   1.0000
Neg Pred Value         0.9995   0.9994   0.9986   0.9994   0.9991
Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
Detection Rate         0.2841   0.1930   0.1732   0.1634   0.1830
Detection Prevalence   0.2846   0.1940   0.1740   0.1644   0.1830
Balanced Accuracy      0.9990   0.9981   0.9962   0.9978   0.9979

CV <- testingCV
CV$Pred = testingCV$classe == predictionRF
qplot(accel_forearm_x, accel_forearm_y, col=Pred, data=CV)
Low failure points due to high dagree of accuaracy 

Result: Comparing two models Random forest appear to perform well (accuracy=0.9967) as compare to Decision tree (accuracy=0.7466). Therefore, Random forest model was used for final prediction. 

# Testing the model to predict 20 different test cases
Predict = predict(modelRF, testing)
Predict
1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
 B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B  B  B 
Levels: A B C D E
