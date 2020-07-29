# Physical_Activity_Recognition


[Link_to_notebook](https://github.com/MahsaShokouhi/Physical_Activity_Recognition/blob/master/Physical_Activity_Recognition.ipynb)


<br>

## Motivation:
Remote patient monitoring allows for supervision of the health and well-being for disabled and elderly patients while they perform regular daily activities. The goal of this project was to verify the feasibility of recognizing basic physical activities from body motion parameters using a simple, wearable sensing device such as a smartphone.

<br>

## Data
The data (from the ‘Human Activity Recognition Using Smartphones Data Set’ study [1]) was acquired comes from 30 volunteers (19-48 years) wearing a smartphone on the waist during 12 different physical activities, consisting of 6 basic activities: static postures (standing, sitting, lying) and dynamic activities (walking, walking downstairs and walking upstairs), as well as 6 postural transitions between the static postures: stand-to-sit, sit-to-stand, sit-to-lie, lie-to-sit, stand-to-lie, and lie-to-stand.
The provided data was already divided in train and test sets which included 7767 and 3162 observations, respectively. For each set the following files were provided:
*	subject_id.txt: ranging 1 to 30, identifying each of the 30 subjects
*	X.txt: with 561 features representing body motion parameters for each observation.
*	y.txt: target variable ranging from 1 to 12, each corresponding to an activity group.


<img src="/images/Table1.png" width=500>


The goal was to build the classification model which gives highest accuracy with cross-validation on the train set, and then use this model to predict the activity type on the test set.

#### <ins>Features Description</ins>
Body motion parameters (3-axial (X, Y, Z) acceleration and 3-axial angular velocity) were extracted from the embedded accelerometer and gyroscope of the device. Body acceleration (BodyAcc) and gravity (GravityAcc) were extracted from the sensor acceleration signal, and the angular velocity was extracted from the gyroscope (BodyGyro). Jerk signals (tBodyAccJerk and tBodyGyroJerk) and the magnitude of these three-dimensional signals (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag) were also calculated. 
A set of statistics on these parameters were used to obtain a 561-feature vector with time and frequency domain variables (prefixed by ‘t’ and ‘f’, respectively). These statistics included Mean, STD, Median absolute deviation (Mad), Min, Max, Signal magnitude area (SMA), Energy, Autorregresion coefficients (ARCoeff), Interquartile range (IQR), Signal entropy (Entropy), Correlation coefficient between two signals (Correlation), and Angle between to vectors (Angle), in addition to frequency-specific statistics: Index of the frequency component with largest magnitude (MaxInds), Skewness and Kurtosis of the frequency domain signal, Weighted average of the frequency components (MeanFreq), and Energy of a frequency interval (BandsEnergy).

<br>

## Exploratory Data Analysis
There were no common subjects between the train and test sets and all records of each subject were only included in one set. 
There were no missing data.
Features were already normalized and did not need rescaling. The following two tables show the distribution statistics for each feature and the range of these statistics accross all features.
![Table 2](/images/Table2.png)
<img src="/images/Table3.png" width=400>
The dataset was imbalanced with only small percentage of total observations (1% or less) in each transitional activity group. Therefore, the modeling problem can be defined as multi-class classification with imbalanced data.
<img src="/images/Table4.png" width=600>
The distribution of observations across different classes were similar between the train and test set.
![Figure 1](/images/fig1.png)
A number of features were highly correlated with each other. The following figure shows features with correlations above 0.60.
![Figure 2](/images/fig2.png)
The following figure shows the clusters of activity groups using the first two components of linear discriminant analysis for dimensionality reduction. As can be seen, there is a considerable overlap between clusters corresponding to "Sitting" and "Standing". Also, clusters corresponding to "Walking", "Walking-downstairs", and "Walking-upstairs" considerably overlap, Whereas the "Laying" cluster is separate. The clusters of transitional postures are seen between the above-mentioned, basic activities.
[Figure 3](/images/fig3.png)

<br>

## Training Classification Models
Models were evaluated using 5-fold cross-validation with 3 repeats. The mean accuracy was then used to compare different models.

### Baseline Model
For comparison, a dummy classifier that used the most-frequent class as the basis for classification was selected as the baseline model and achieved the mean accuracy of 0.183.

### Spot-Check Classification Algorithms
A set of linear, non-linear, and ensemble algorithms were fitted on imbalanced data for a preliminary model comparison. The algorithms which resulted in highest accuracies were selected for further model optimization and hyperparameter tuning.

### Balanced Algorithms with Data Resampling
To account for imbalanced data, data resampling (upsampling of the minority classes) was performed with SMOTE and ADYSAN algorithms from imblearn package. The resampled data was then used with the algorithms chosen from previous section. However, resampling did not improve the accuracy of the selected models.

### Dimensionality Reduction
The effect of dimensionality reduction using Linear Discriminant Analysis (LDA) and Principal Components Analysis (PCA) was also evaluated.
The following table shows the model accuracies with imbalanced data (column 1), after resampling (columns 2-3), and after dimensionality reduction (columns 4-5).

![Table 5](/images/Table5.png)

### Hyperparameter Tuning for Cost-Sensitive Algorithms
The following cost-sensitive models (by setting the class_weight parameter of the model to 'balanced', to account for the imbalance) were tuned for model parameters and compared:

<img src="/images/Table6.png" width=400>

Eventually Support Vector Classifier (with linear kernel) was selected for prediction on the test set.

<br>

## Prediction for Test Set
The model achieved an overall accuracy of 0.95 on the test set. The classes with largest number of observations showed best performance scores. Group 9 (‘sit_to_lie’ activity) had lowest classification scores.

![Table 7](/images/Table7.png)

<br>

## Citation
1. Jorge-L. Reyes-Ortiz, Luca Oneto, Albert SamÃ , Xavier Parra, Davide Anguita. Transition-Aware Human Activity Recognition Using Smartphones. Neurocomputing. Springer 2015. 
