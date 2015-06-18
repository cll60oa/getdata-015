#Code book

**Human Activity Recognition Using Smartphones Dataset**

he experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

**The variables found in the tidy data set (average_data.txt) are the followings:**

| Variable| Description| Unit |
|--- | --- | --- |
| subject_id | Subject identification | |
| activity_label | Activities: WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING | |
| tBodyAcc | Time domain, the body acceleration signal  | standard gravity units 'g'|
| tGravityAcc | Time domain, the angular acceleration vector of gravity | standard gravity units 'g'|
| tBodyAccJerk | Time domain, the body acceleration signal (Jerk) | standard gravity units 'g'|
| tBodyGyro | Time domain, the angular velocity vector from the gyroscope | radians/second |
| tBodyGyroJerk | Time domain, the angular velocity vector from the gyroscope (Jerk) | radians/second |
| tBodyAccMag | Time domain, the signal magnitude of the body acceleration signal | standard gravity units 'g'|
| tGravityAccMag | Time domain, the signal magnitude of the angular acceleration vector of gravity | standard gravity units 'g'|
| tBodyAccJerkMag | Time domain, the signal magnitude of body acceleration signal (Jerk) | standard gravity units 'g'|
| tBodyGyroMag | Time domain, the signal magnitude of angular velocity vector from the gyroscope | radians/second |
| tBodyGyroJerkMag | Time domain, the signal magnitude of angular velocity vector (Jerk) from the gyroscope | radians/second |
| fBodyAcc |	Frequency domain,the body acceleration signal  | standard gravity units 'g'|
| fBodyAccJerk |	Frequency domain, the body acceleration signal (Jerk) | standard gravity units 'g'|
| fBodyGyro |	Frequency domain, the angular velocity vector from the gyroscope | radians/second |
| fBodyAccMag |	Frequency domain, the signal magnitude of the body acceleration signal | standard gravity units 'g'|
| fBodyBodyAccJerkMag |	Frequency domain, the signal magnitude of body acceleration signal (Jerk) | standard gravity units 'g'|
| fBodyBodyGyroMag |	Frequency domain, The signal magnitude of angular velocity vector from the gyroscope | radians/second |
| fBodyBodyGyroJerkMag |	Frequency domain, the signal magnitude of angular velocity vector (Jerk) from the gyroscope | radians/second |
| SUFFIX | _mean (The average), _std (standard deviation), _X/_Y/_Z (X, Y or Z axis)| |

[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. A Public Domain Dataset for Human Activity Recognition Using Smartphones. 21th European Symposium on Artificial Neural Networks, Computational Intelligence and Machine Learning, ESANN 2013. Bruges, Belgium 24-26 April 2013. 
