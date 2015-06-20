
###Step-by-step breakdown of the run_analysis.R

**1. Load the two requried packages**
```
library(dplyr)
library(tidyr)
```

**2. Dowload the file and unzip it**
```
fileURL <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileURL,destfile="UCI HAR Dataset.zip" ,method='curl')
unzip("UCI HAR Dataset.zip")
```
Now, the data is unzipped under the folder "./UCI HAR Dataset/" in your working directory.



**3. Merges the three data (subject id, activity label and features) in the training and test sets respectively to create one data set**

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


**4. Extracts only the measurements on the mean (mean) and standard deviation (std) for each measurement**
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

**5. Uses descriptive activity names to name the activities in the data set**
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



```
write.table(average_data,file="./average_data.txt",sep="\t", row.name=FALSE)
```
