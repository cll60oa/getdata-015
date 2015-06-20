
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
Dimensions of files should look like these:
```
> dim(test_subject_id)
[1] 2947    1
> dim(train_subject_id)
[1] 7352    1
> dim(all_subject_id)
[1] 10299     1
```

Merge the activity label files:
```
# activity label
test_activity_label <- read.table('UCI HAR Dataset/test/y_test.txt')
train_activity_label <- read.table('UCI HAR Dataset/train/y_train.txt')
all_activity_label <- rbind(test_activity_label,train_activity_label)
```
Dimensions of files should look like these:
```
> dim(test_activity_label)
[1] 2947    1
> dim(train_activity_label)
[1] 7352    1
> dim(all_activity_label)
[1] 10299     1
```

Merge the feature files:
```
# features
test_561_features <- read.table('UCI HAR Dataset/test/X_test.txt')
train_561_features <- read.table('UCI HAR Dataset/train/X_train.txt')
all_561_features <- rbind(test_561_features,train_561_features)
```
Dimensions of files should look like these:
```
> dim(test_561_features)
[1] 2947  561
> dim(train_561_features)
[1] 7352  561
> dim(all_561_features)
[1] 10299   561

```


**Extracts only the measurements on the mean (mean) and standard deviation (std) for each measurement**
```
features_ <- read.table('UCI HAR Dataset/features.txt') # read the feature file
mean.sd <- grep('mean\\(|std\\(',features_[,2]) # search for mean( and std(
features_mean.sd <- select(all_561_features,mean.sd) # extract the result
```

```
write.table(average_data,file="./average_data.txt",sep="\t", row.name=FALSE)
```
