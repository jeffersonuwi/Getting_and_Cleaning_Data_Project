# Getting and Cleaning Data Project John Hopkins Coursera
# Author: Jefferson Bien-Aime

# 1. Merges the training and the test sets to create one data set.
# 2. Extracts only the measurements on the mean and standard deviation for each measurement.
# 3. Uses descriptive activity names to name the activities in the data set
# 4. Appropriately labels the data set with descriptive variable names.
# 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.



# Load Packages
library(reshape2)
library(data.table)

# Creating the necessary directory

setwd("~/RStudioProjects/Getting and Cleaning Data/Getting_and_Cleaning_Data_Project")
path <- getwd()
if(!file.exists("data")) {
  file.create("data")
}

# Load the data
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
downloadDirectory = "./data"
download.file(url, file.path(path, "./data/dataFiles.zip"))
unzip(zipfile = "./data/dataFiles.zip", exdir = downloadDirectory)


# load test data files X_test.txt and y_test.txt 
test_x <- read.table("./data/UCI HAR Dataset/test/X_test.txt")
test_y <- read.table("./data/UCI HAR Dataset/test/y_test.txt")
subject_test <- read.table("./data/UCI HAR Dataset/test/subject_test.txt")

# load training data files X_train.txt and y_train.txt
train_x <- read.table("./data/UCI HAR Dataset/train/X_train.txt")
train_y <- read.table("./data/UCI HAR Dataset/train/y_train.txt")
subject_train <- read.table("./data/UCI HAR Dataset/train/subject_train.txt")


# Load activity labels + features
activityLabels <- fread("./data/UCI HAR Dataset/activity_labels.txt"
                        , col.names = c("classLabels", "activityName"))
allfeatures <- fread("./data/UCI HAR Dataset/features.txt"
                  , col.names = c("index", "featureNames"))
featuresDesired <- grep("(mean|std)\\(\\)", allfeatures[, featureNames])
measurements <- allfeatures[featuresDesired, featureNames]
measurements <- gsub('[()]', '', measurements)




# 1. Merges the training and the test sets to create one data set.

combined_x <- rbind(test_x, train_x)
combined_y <- rbind(test_y, train_y)
combined_subject <- rbind(subject_test, subject_train)

colnames(combined_x)  <- allfeatures$featureNames


# 2. Extracts only the measurements on the mean and standard deviation for each measurement. 

combined_mean_std <- combined_x[,featuresDesired]

# 3. Uses descriptive activity names to name the activities in the data set

combined_y$activity     <- activityLabels[combined_y$V1, 2]

# 4. Appropriately labels the data set with descriptive variable names. 

names(combined_y)  <- c("ActivityID", "ActivityLabel")
names(combined_subject) <- "Subject"

# 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

combined_all <- cbind(combined_subject, combined_y, combined_x)
allLabels <- c("Subject", "ActivityID", "ActivityLabel")

data_labels = setdiff(colnames(combined_all), allLabels)
melted_data = melt(combined_all, id = allLabels, measure.vars = data_labels, na.rm=TRUE)
tidy_data = dcast(melted_data, Subject + ActivityLabel ~ variable, mean)

write.table(tidy_data, file = "./data/tidy_data.txt", row.names = FALSE)

