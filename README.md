# Assignment-9.1

#Problem

#1. Use the below given data set

Data Set
#2. Perform the below given activities:

#a. Create classification model using logistic regression model

#b. verify model goodness of fit

#c. Report the accuracy measures

#d. Report the variable importance

#e. Report the unimportant variables

#f. Interpret the results

#g. Visualize the results

#Answers

#a)

#using dataset cs2m

#reading the dataset

cs2m <- read.csv("C:/Users/SIMATIT/Documents/cs2m.csv")

View(cs2m)

#logistic regression

model<- glm(DrugR~BP+Chlstrl+Age+Prgnt+AnxtyLH, data = cs2m ,family= binomial)

model

summary(model)

#classification

library(caTools)

library(tree)

#splitting

set.seed(1)

split<- sample.split(cs2m$DrugR,SplitRatio = 0.70)

cs2mTrain <- subset(cs2m,split == TRUE)

cs2mTest<- subset(cs2m, split == FALSE)

modelClassTree<- tree(DrugR~BP+Chlstrl+Age+Prgnt+AnxtyLH,data = cs2mTrain)

plot(modelClassTree)

text(modelClassTree,pretty = 0 ,cex=0.75)

pred<- predict(modelClassTree,newdata= cs2mTest)

predict<- predict(model,type="response")

head(predict,3)

cs2m$predict <- predict

cs2m$predictROUND<- round(predict,digits = 0)

#confusion matrix

table(cs2m$DrugR,predict>= 0.5)

sum<- sum(table(cs2m$DrugR,predict>= 0.5))

#f) & b) & c)

#Answers

#interpretation, Accuracy and model goodness of our model

summary(model)

#accuracy of our model

accuracy<- (13+12)/(30)

accuracy

#0.8333333333

install.packages("verification")

library(verification)

predictTrain<- predict(model,cs2m,type="response")

table(cs2m$DrugR,predictTrain >=0.5)

head(predictTrain,3)

auc(cs2m$DrugR,predictTrain)

#model goodness

#NOTE

#Area under the curve: 0.9333333

#also our AIC is less which is measure of good model

#NULL deviance is also less which is good for model

#Residual deviance is also less model

#by this all things we conclude that our model is good and fit

#d)

install.packages("caret")

library(caret)

varImp(step_fit)

#g)

#plot the fitted model

plot(model$fitted.values)

#plot glm

library(ggplot2)

ggplot(cs2mTrain, aes(x=Age, y=DrugR)) + geom_point() +

stat_smooth(method="glm", family="binomial", se=FALSE)

#e)

library(MASS)

step_fit<- stepAIC(model,method ="backward")

summary(step_fit)

confint(step_fit)

#thus by this method we get our best model and variable bp is not as much important y this method

#some test

#ANOVA on base model

anova(model,test = 'Chisq')

#ANOVA from reduced model after applying the Step AIC

anova(step_fit,test = 'Chisq')

#check for multicollinearity

library(car)

vif(model)

vif(step_fit)
