# Uni_project

Collection of different Machine Learning models and neural network structures applied to a particle dataset using Keras, Tensorflow and Sklearn.
The aim of the project is to focus on how to make the better choice of a suitable ML algorithm.
Complexity and learning curve analyses are part of the visual analytics tools that help for comparing the merits of various ML algorithms.


# Table of Contents
- [About_the_project](#About)
    - [Dataset](#Dataset)
- [Implementation](#Implementation)
    - [ML model comparison](#MLmodelcomparison)
    - [ML comparison variating training dataset size](#MLcomparisonvariatingtrainingdatasetsize)
    - [Neural Network performance](#NeuralNetworkperformance)
    - [Neural Network performance variating training dataset size](#NeuralNetworkperformancevariatingtrainingdatasetsize)
- [Conclusion](#Conclusion)

## About the project <a name="About"></a>

### Dataset <a name="Dataset"></a>
The Dataset used, 'pid-5M' is a dataset available online and downloaded from Kaggle's dataset page. (https://www.kaggle.com/naharrison/particle-identification-from-detector-responses)

That's a simplified dataset of a GEANT based simulation for electron-proton inelastic scattering measured by a particle detector system.
It simulates in the final state four particle types with an id number associated - positron (-11), pion (211), kaon (321), and proton (2212); six detector responses. Some detector responses are zero due to detector inefficiencies or incomplete geometric coverage of the detector.
Is composed of 5000000 rows and 7 columns:
+ ID
+ Momentum(GeV/c)
+ Theta angle(rad)
+ Beta value
+ Number of photoelectrons
+ Inner energy(GeV)
+ Outer energy(GeV)

![alt text](https://github.com/nico0407/Uni_project/blob/main/images/Data_visualisation/Data_hinsto.png)

Is also reported the following plot feaguring the beta value(v/c) of a particle against the momentum of this one.

![alt text](https://github.com/nico0407/Uni_project/blob/main/images/Data_visualisation/Data_betavsmomentum.png)

It's easy to see how in this plot the pion trace is very different from the kaon and the proton one. In HEP is a widley used plot for making cuts in the domain and discriminate particles one from the others. Particular difficulties is between electron and pion, in that case also for lack of statistics.

The rest of the images are left into the specific folder of Data visualization

## Implementation <a name="Implementation"></a>
A juppiter notebook was used for a better manipolation of the script. Due to the possibility to run the code piece by piece it was possible to do more test on the models accuracy without run everytime the whole file given the huge structure of this one.
The library used are:
+ Pandas
+ Numpy
+ Matplotlib
+ Seaborn
+ Sklearn
+ Keras
+ Tensorflow

The aim of the project was to discriminate pions respect to the other particles, so first of all was done a data manipulation on the id values, the pion id was changed into 1 and the other particles'(electrons, protons and kaons) id into 0.
Then given the extreme number of rows in first analysis were considered just the first 50000 ones.

In such a way the dataframe was more usable for a machine learning implementation, in addition the dataframe was splitted into an 'x' and 'y' part. The latter one is the id modified column whose role is as a target to understand is the classification is done in the right way or not, instead the other one whas inside all the other six remanent columns.
Then was implemented a data splitting into test and train with a test_size equal to 0.30. Soon after, an other splitting was done, dividing the test dataset into a validation and a test one, both with the same dimension.

The validation set is a set of data, separate from the training set, that is used to validate our model performance during training.
This validation process gives information that helps us tune the model’s hyperparameters and configurations accordingly.
The model is trained on the training set, and, simultaneously, the model evaluation is performed on the validation set after every epoch.
The main idea of splitting the dataset into a validation set is to prevent our model from overfitting i.e., the model becomes really good at classifying the samples in the training set but cannot generalize and make accurate classifications on the data it has not seen before. 

The test set is a separate set of data used to test the model after completing the training.

### ML model comparison <a name="MLmodelcomparison"></a>
The models utilized were the following:
+ Decision Tree Classifier, used with both gini and entropy criterion
+ Ada Boost Classifier
+ Logistic Regression
+ K neighbors Classifier
+ SGDC Classifier
+ Rnadom Forest Classifier

For each model a variation of hyperparameters was implemented, and after each trial the learning curves were plotted. In particular at each change the plot has two curves, a red one representing the learning curve for the training dataset, and in blue the validation one.
The learning curve represent the variation of the accuracy of the model in function of some paramethers(e.g. the minimum sample leaf or the maximum depth in the case of a decison tree classifier)

Confronting the two curves one can check if, at the end of the time of training, the model predict well the data samples.

In particular one can have cases of overfitting, when the model approximate too well the training set, and has a low predictive power in any other poxible data sample. A possible way to avoid that is by looking at the behaviour of the learning curves of training and validation. If the accuracy on the training set starts to became higher than the one on the validation set and the discrepancy do not vanish it can be a case of overfitting.
At the end of the fitting process the discrepance between the two has to be quite low and has to decrease with the rinning time in order to have a good working model.

put the bias and variance

For each model is also provided the ROC curve 


At the end the accuracy was computed via the function "accuracy_score" and also via the "f1_score", the results are reported below.
Was choosen to use also the latter one because combine inside two other metrix such as recall_score and precision_score, so the result of f1_score will be high only if both recall and precision are high.
 


accuracy_score                                                                                                  |  f1_score
:--------------------------------------------------------------------------------------------------------------:|:---------------------------------------:
![alt](https://github.com/nico0407/Uni_project/blob/main/images/model_comparison/comparison/compare1.png "left")  |  ![alt](https://github.com/nico0407/Uni_project/blob/main/images/model_comparison/comparison/compare2.png "right")


### ML comparison variating training dataset size <a name="MLcomparisonvariatingtrainingdatasetsize"></a>

The project present also an other comparison between models. In this case was applied a cut in the dataset available for training each model.
In that case the model was fed by step of 10% of the data at the time, and at each step was calculated and plotted the learning curve for the training and validation data set, until the model reach the 100% of the data fed for training. 
For semplicity is reported just an example of this, the other images are available in the specific repository section.

![alt-text](https://github.com/nico0407/Uni_project/blob/main/images/model_comparison/training_variations/knn10.png "text")

Moreover at each step it's benn computed the ROC curve relative to the percentage of the training data fed.
Also in that case is presented just one case, the rest are available in the images folder section of the repository.

![alt-text](https://github.com/nico0407/Uni_project/blob/main/images/model_comparison/ROC_curve_training_changing/ROC_RFC10.png)


### Neural Network performance <a name="NeuralNetworkperformance"></a>
The study was done also using neural network models, and in thi section some functions were defined.

+ A function for building the model. In particular this provides two dense layer interprised by dropout layers. This function allows to choose the number of nodes for both of the hidden layers, the learing rate, the dropout probability and even the oprimizer function. At the end it also compile the model.
+ An other section for running the model. Has the fit function inside and provide the arbitrary choose of the number of epochs to run and the batch size. Moreover make a checkpoint model, saving the model step by step. While the model is fitted on the training data at the same time is also validated with the validation data set. At the end are ploted together learning curves for training and validation, both for the accuracy and for the loss.
+ In the last section is implemented the testing for the model with the "evaluate" function that provide a score of the model on the testing data set. That's much useful for determine if a model overfit or underfit, if has an enough good predictive power.

Here is reported an example of the Adam optimizer with the corrispective ROC curve.

ACCURACY                                                                                                  |  LOSS |ROC
:--------------------------------------------------------------------------------------------------------------:|:---------------------------------------:|:--:|
![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/first_models/Adam50.png "left")  |  ![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/first_models/Adam50loss.png "right") | ![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/first_models/Adam50ROC.png)



After some trial of esecution was done a comparison between the oprimizer, both with the use of the evaluate function described in the testing section above and with the f1_score function.

![alt text](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/accuracy/Accuracy1.png)

Moreover was applied a change contemporary in the number of nodes for each layer and for the number of epochs of run using the Adam optimizer. In that case an improvement in the learning curve shows up, and is reported in the following gif, where the plots are refered to a number of epoch of run respectevely of 5,10 and 25.

![alt text](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/Variating_numepoch_and_neurons/5zvq2b.gif)

This is followed by a section in wich there is a variation of the hyperparameters of the model(expecially for the SGD one), such as the number of nodes in each hidden layer and the learning rate, trying to evidentiate some specific values for reduce the complexity of the model built.

There is also the implementation of an other Neural Network to improve the performance of the previous one, composed with tree hydden layer devided by a drop out one.
Also here it's been done a study on the better value for the hyperparameters, testing the model variating the number of neuron for each layer and the number of epoch of run. That model was executed for a growing number of epoch, until it came up into overfitting of training data.

ACCURACY                                                                                                  |  LOSS |ROC
:--------------------------------------------------------------------------------------------------------------:|:---------------------------------------:|:--:|
![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/Sequence/Seq200.png "left")  |  ![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/Sequence/Seq200loss.png "right") | ![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/Sequence/Seq200ROC.png)

ACCURACY                                                                                                  |  LOSS |ROC
:--------------------------------------------------------------------------------------------------------------:|:---------------------------------------:|:--:|
![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/Sequence/Seq100.png "left")  |  ![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/Sequence/Seq100loss.png "right") | ![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/Sequence/Seq100ROC.png)

### Neural Network performance variating training dataset size <a name="NeuralNetworkperformancevariatingtrainingdatasetsize"></a>

Also in the neural network section was done a variation in the training dataset at step af 10% of the totality of the data given. Eì VERO?!?!?
For each step was implemented the computation and the conseguent plot of validation and training curve and the construction of the ROC curve. Concerning the latter ones is evident how it improves increasing the trainig data set.
First of all what changed was the fraction of the whole data for training the model, that goes from 10% to 100%, leaving validation and testing data sets unaltered.

something | somenthing
:-------:|:----------:|
![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/variation_training_set/variation_trainingset_lr0001.png)    | ![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/variation_training_set/variation_trainingset_lr0001ROC.png)


After this trial then, the fraction of the total data available for training, testing, and validation was changed simultaneously, manteining the proportions decided at the beginning the same(training : validation : testing = 70 : 15 : 15).

So in that case is not only changing the training set, but all the available amount of data for building the model.

something | somenthing
:-------:|:----------:|
![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/variation_training_set/toality_fitting_modification.png)    | ![alt](https://github.com/nico0407/Uni_project/blob/main/images/NN_model/variation_training_set/toality_fitting_modificationROC.png)


## Conclusion <a name="Conclusion"></a>
After many trial and variations of the hyperparamethers of the various model, the main ones that seems to work better are the RFT and the Adaboost, they give as output the highest value of the f1_score, so contemporaneusly a pretty high value of recall and prediction at the same time, and they can be used with a high rate of efficiency for discriminating pion from the rest of the particles in this specific dataset.


In this section, we have begun to explore the concept of model validation and hyperparameter optimization, focusing on intuitive aspects of the bias–variance trade-off and how it comes into play when fitting models to data. In particular, we found that the use of a validation set or cross-validation approach is vital when tuning parameters in order to avoid over-fitting for more complex/flexible models.
In later sections, we will discuss the details of particularly useful models, and throughout will talk about what tuning is available for these models and how these free parameters affect model complexity. Keep the lessons of this section in mind as you read on and learn about these machine learning approaches!

--------

The model on the left attempts to find a straight-line fit through the data. Because the data are intrinsically more complicated than a straight line, the straight-line model will never be able to describe this dataset well. Such a model is said to underfit the data: that is, it does not have enough model flexibility to suitably account for all the features in the data; another way of saying this is that the model has high bias.
The model on the right attempts to fit a high-order polynomial through the data. Here the model fit has enough flexibility to nearly perfectly account for the fine features in the data, but even though it very accurately describes the training data, its precise form seems to be more reflective of the particular noise properties of the data rather than the intrinsic properties of whatever process generated that data. Such a model is said to overfit the data: that is, it has so much model flexibility that the model ends up accounting for random errors as well as the underlying data distribution; another way of saying this is that the model has high variance.


-------


For high-bias models, the performance of the model on the validation set is similar to the performance on the training set.

For high-variance models, the performance of the model on the validation set is far worse than the performance on the training set.

