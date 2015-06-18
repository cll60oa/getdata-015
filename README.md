
###Step-by-step breakdown of the run_analysis.R

**Load the two requried packages**
```
library(dplyr)
library(tidyr)
```

**Dowload the file and unzip it**
```
fileURL <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileURL,destfile="UCI HAR Dataset.zip" ,method='curl')
unzip("UCI HAR Dataset.zip")
```
Now, the data is unzipped under the folder "./UCI HAR Dataset/" in your working directory.

**Merges the three data (subject id, activity label and features) in the training and test sets respectively to create one data set**
```
# subject id
test_subject_id <- read.table('UCI HAR Dataset/test/subject_test.txt')
train_subject_id <-  read.table('UCI HAR Dataset/train//subject_train.txt')
all_subject_id <- rbind(test_subject_id, train_subject_id)

# activity label
test_activity_label <- read.table('UCI HAR Dataset/test/y_test.txt')
train_activity_label <- read.table('UCI HAR Dataset/train/y_train.txt')
all_activity_label <- rbind(test_activity_label,train_activity_label)

# features
test_561_features <- read.table('UCI HAR Dataset/test/X_test.txt')
train_561_features <- read.table('UCI HAR Dataset/train/X_train.txt')
all_561_features <- rbind(test_561_features,train_561_features)
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
