Getting and Cleaning Data Course Project

Instructions for project The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.
One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

Here are the data for the project:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

You should create one R script called run_analysis.R that does the following.
Merges the training and the test sets to create one data set. Extracts only the measurements on the mean and standard deviation for each measurement. Uses descriptive activity names to name the activities in the data set Appropriately labels the data set with descriptive variable names. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

set working directory
setwd(“F:/1-DataScientistCertification/GettingandCleaningData/PeerAssessmentAsgnmnt/test/UCI HAR Dataset”)

library(knitr)
## Warning: package 'knitr' was built under R version 3.2.2

Files in folder ‘UCI HAR Dataset’ that will be used are as following:

1.SUBJECT FILES test/subject_test.txt train/subject_train.txt
2.ACTIVITY FILES test/X_test.txt train/X_train.txt
3.	DATA FILES test/y_test.txt train/y_train.txt
4.features.txt - Names of column variables in the dataTable
5.activity_labels.txt - Links the class labels with their activity name.


Read the above files and create data tables.

y_train <- read.table(“train/y_train.txt”, quote=“"”) y_test <- read.table(“test/y_test.txt”, quote=“"”)
features <- read.table(“features.txt”, quote=“"”) activity_labels <- read.table(“activity_labels.txt”, quote=“"”)
subject_train <- read.table(“train/subject_train.txt”, quote=“"”) subject_test <- read.table(“test/subject_test.txt”, quote=“"”)
X_train <- read.table(“train/X_train.txt”, quote=“"”) X_test <- read.table(“test/X_test.txt”, quote=“"”)
colnames(activity_labels)<- c(“V1”,“Activity”)


to merge the y_train with the activity label

subject<- rename(subject_train, subject=V1) train<- cbind(y_train,subject) train1<- merge(train,activity_labels, by=(“V1”))
colnames(X_train)<- features[,2]
train2<- cbind(train1,X_train)


Eliminate the train2 1st column in order to avoid error “duplicate column name”
train3<- train2[,-1]

to select only the columns that contains means and standard deviation
train4<- select(train3,contains(“subject”), contains(“Activity”), contains(“mean”), contains(“std”))
colnames(activity_labels)<- c(“V1”,“Activity”)


merge the y_test with the activity label
subjecta<- rename(subject_test, subject=V1) test<- cbind(y_test,subjecta) test1<- merge(test,activity_labels, by=(“V1”))
colnames(X_test)<- features[,2]


Combining y_test, activity labels, X_test
test2<- cbind(test1,X_test)


Eliminate the train2 1st column in order to avoid error “duplicate column name”
test3<- test2[,-1]


select only the columns that contains means and standard deviation
test4<- select(test3,contains(“subject”), contains(“Activity”), contains(“mean”), contains(“std”))


Combining Train data with Test data
run_analysis1<- rbind(train4,test4)

