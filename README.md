*Getting and Cleaning Data*
*Programming Assignment*

library(tools)
library(tidyverse)

*Load Test data*

X_test <- read.table("./data/UCI HAR Dataset/test/X_test.txt", sep = "")

*Load Training data*

X_train <- read.table("./data/UCI HAR Dataset/train/X_train.txt", sep = "")

*combine test and training data*

X <- rbind(X_test, X_train)

*load features*

features <- read.table("./data/UCI HAR Dataset/features.txt")

*find featurs containing "mean" or "std"*

matchnames <- features\$V2 %>% str_subset("mean|std") 
matchnums <- matchnames %>% match(features$V2)

*subset X for mean and std columns*

X_extract <- X %>% select(matchnums)
names(X_extract) <- matchnames

*add activity description*

X_extract\$activity <- rbind(read.table("./data/UCI HAR Dataset/test/y_test.txt"), read.table("./data/UCI HAR Dataset/train/y_train.txt"))\$V1
X_extract\$subject <- rbind(read.table("./data/UCI HAR Dataset/test/subject_test.txt", sep = ""), read.table("./data/UCI HAR Dataset/train/subject_train.txt", sep = ""))\$V1

*load the activiy labels*

activity_labels <- read.table("./data/UCI HAR Dataset/activity_labels.txt", sep = "")
X_extract\$activitylabel <- unlist(lapply(X_extract\$activity, function(x) activity_labels\$V2[x]))

*bring activity label to first column*

X_extract <- X_extract %>% select(activitylabel, subject, everything())

*Task 4*
*group by label and subject and calculate mean*

new_df <- X_extract %>% dplyr::group_by(activitylabel, subject) %>% summarize_all(list(mean))


                                   
