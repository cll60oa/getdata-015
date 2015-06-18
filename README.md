**Load the requried packages**
> library(dplyr)
> library(tidyr)

# dowload the file and unzip it
fileURL <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileURL,destfile="UCI HAR Dataset.zip" ,method='curl')
unzip("UCI HAR Dataset.zip")

# Merges the training and the test sets to create one data set.
test_subject_id <- read.table('UCI HAR Dataset/test/subject_test.txt')
train_subject_id <-  read.table('UCI HAR Dataset/train//subject_train.txt')
all_subject_id <- rbind(test_subject_id, train_subject_id)

test_activity_label <- read.table('UCI HAR Dataset/test/y_test.txt')
train_activity_label <- read.table('UCI HAR Dataset/train/y_train.txt')
all_activity_label <- rbind(test_activity_label,train_activity_label)

test_561_features <- read.table('UCI HAR Dataset/test/X_test.txt')
train_561_features <- read.table('UCI HAR Dataset/train/X_train.txt')
all_561_features <- rbind(test_561_features,train_561_features)


# Extracts only the measurements on the mean and standard deviation for each measurement. 
features_ <- read.table('UCI HAR Dataset/features.txt')
mean.sd <- grep('mean\\(|std\\(',features_[,2])
features_mean.sd <- select(all_561_features,mean.sd)

# Uses descriptive activity names to name the activities in the data set
activity_labels <- read.table('./UCI HAR Dataset/activity_labels.txt')
all_activity_label[,1] <- activity_labels[all_activity_label[,1],2]

# Appropriately labels the data set with descriptive variable names. 
names(all_subject_id) <- "subject_id"
names(all_activity_label) <- "activity_label"
names(features_mean.sd) <- gsub("\\(\\)","", features_[mean.sd,2]) %>% gsub(pattern="-", replacement="_")

# From the data set in step 4, creates a second, independent tidy data set with the average
# of each variable for each activity and each subject.
all_data_sets <- cbind(all_subject_id,all_activity_label,features_mean.sd)
average_data <- all_data_sets %>%
gather(features,values,tBodyAcc_mean_X:fBodyBodyGyroJerkMag_std) %>%
group_by(features,subject_id,activity_label) %>%
summarize(mean=mean(values)) %>%
spread(features,mean)

write.table(average_data,file="./average_data.txt",sep="\t", row.name=FALSE)
