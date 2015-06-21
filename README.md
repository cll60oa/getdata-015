
###Step-by-step breakdown of the run_analysis.R

**1. Load the two requried packages.**
```
library(dplyr)
library(tidyr)
```

**2. Dowload the file and unzip it.**
```
fileURL <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileURL,destfile="UCI HAR Dataset.zip" ,method='curl')
unzip("UCI HAR Dataset.zip")
```
Now, the data is unzipped under the folder "./UCI HAR Dataset/" in your working directory.



**3. Merges the three data (subject id, activity label and features) in the training and test sets respectively to create one data set.**

Merge the subject id files:
```
# subject id
test_subject_id <- read.table('UCI HAR Dataset/test/subject_test.txt')
train_subject_id <-  read.table('UCI HAR Dataset/train//subject_train.txt')
all_subject_id <- rbind(test_subject_id, train_subject_id)
```

Merge the activity label files:
```
# activity label
test_activity_label <- read.table('UCI HAR Dataset/test/y_test.txt')
train_activity_label <- read.table('UCI HAR Dataset/train/y_train.txt')
all_activity_label <- rbind(test_activity_label,train_activity_label)
```

Merge the feature files:
```
# features
test_561_features <- read.table('UCI HAR Dataset/test/X_test.txt')
train_561_features <- read.table('UCI HAR Dataset/train/X_train.txt')
all_561_features <- rbind(test_561_features,train_561_features)
```


**4. Extracts only the measurements on the mean (mean) and standard deviation (std) for each measurement.**
Load the feature file:
```
features_ <- read.table('UCI HAR Dataset/features.txt')
```
Structure of the feature file looks like this:
```
> tbl_df(features_)
Source: local data frame [561 x 2]

   V1                V2
1   1 tBodyAcc-mean()-X
2   2 tBodyAcc-mean()-Y
3   3 tBodyAcc-mean()-Z
4   4  tBodyAcc-std()-X
5   5  tBodyAcc-std()-Y
.. ..               ...
```

Use grep() to search for rows contain the text "mean(" or "std(" in the colume V2. This return a logical vector.
```
mean.sd <- grep('mean\\(|std\\(',features_[,2])
```

Use select() to extract the results.
```
features_mean.sd <- select(all_561_features,mean.sd)
```

**5. Uses descriptive activity names to name the activities in the data set.**
```
activity_labels <- read.table('./UCI HAR Dataset/activity_labels.txt')
all_activity_label[,1] <- activity_labels[all_activity_label[,1],2]
```
Before this step, activity data set looks like:
```
> tbl_df(all_activity_label)
Source: local data frame [10,299 x 1]
   V1
1   5
2   5
3   5
4   5
5   5
.. ..
```

After this step, activity data set looks like::
```
> tbl_df(all_activity_label)
Source: local data frame [10,299 x 1]
         V1
1  STANDING
2  STANDING
3  STANDING
4  STANDING
5  STANDING
..      ...
```

**6. Appropriately labels the data set with descriptive variable names.** 
Manually name the column names in each data set:
```
names(all_subject_id) <- "subject_id"
names(all_activity_label) <- "activity_label"
names(features_mean.sd) <- gsub("\\(\\)","", features_[mean.sd,2]) %>% gsub(pattern="-", replacement="_")
```
**7. Creates a second, independent tidy data set with the average of each variable for each activity and each subject.**
```
all_data_sets <- cbind(all_subject_id,all_activity_label,features_mean.sd)
average_data <- all_data_sets %>%
gather(features,values,tBodyAcc_mean_X:fBodyBodyGyroJerkMag_std) %>%
group_by(features,subject_id,activity_label) %>%
summarize(mean=mean(values)) %>%
spread(features,mean)
```
The following blockquotes show how the data frame changes after each step:
```
> all_data_sets
Source: local data frame [10,299 x 68]

   subject_id     activity_label tBodyAcc_mean_X tBodyAcc_mean_Y tBodyAcc_mean_Z tBodyAcc_std_X
1           2 WALKING_DOWNSTAIRS       0.2571778     -0.02328523     -0.01465376     -0.9384040
2           2 WALKING_DOWNSTAIRS       0.2860267     -0.01316336     -0.11908252     -0.9754147
```
gather(features,values,tBodyAcc_mean_X:fBodyBodyGyroJerkMag_std)
```
Source: local data frame [679,734 x 4]

   subject_id     activity_label        features    values
1           2 WALKING_DOWNSTAIRS tBodyAcc_mean_X 0.2571778
2           2 WALKING_DOWNSTAIRS tBodyAcc_mean_X 0.2860267
```
group_by(features,subject_id,activity_label)
```
Source: local data frame [679,734 x 4]
Groups: features, subject_id, activity_label

   subject_id     activity_label        features    values
1           2 WALKING_DOWNSTAIRS tBodyAcc_mean_X 0.2571778
2           2 WALKING_DOWNSTAIRS tBodyAcc_mean_X 0.2860267
```
summarize(mean=mean(values))
```
Source: local data frame [11,880 x 4]

          features subject_id     activity_label      mean
1  tBodyAcc_mean_X          1             LAYING 0.2554617
2  tBodyAcc_mean_X          1            SITTING 0.2773308
```
spread(features,mean)
```
Source: local data frame [180 x 68]

   subject_id     activity_label tBodyAcc_mean_X tBodyAcc_mean_Y tBodyAcc_mean_Z tBodyAcc_std_X
1           1             LAYING       0.2554617    -0.023953149      -0.0973020    -0.35470803
2           1            SITTING       0.2773308    -0.017383819      -0.1111481    -0.28374026
```

**8. Write the result to the average_data.txt**
```
write.table(average_data,file="./average_data.txt",sep="\t", row.name=FALSE)
```
