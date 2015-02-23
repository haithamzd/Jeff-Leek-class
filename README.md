# Jeff-Leek-class
readme.md:The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.  

You should create one R script called run_analysis.R that does the following. 
1.	Merges the training and the test sets to create one data set.
2.	Extracts only the measurements on the mean and standard deviation for each measurement. 
3.	Uses descriptive activity names to name the activities in the data set
4.	Appropriately labels the data set with descriptive variable names. 
5.	From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.


README.md file for Course Project of the Getting and Cleaning Data module in Coursera.

This file describes the strategy used to devise a script in R called “run_analysis.R” that 
-	Merges the training and test sets from the UCI Human Activity Recognition project to create one data set.
-	Extracts only the measurements of the mean and standard deviation for each measurement and displays them in tables.
-	Uses descriptive activity names to name the activities in the UCI Human Activity Recognition project that are in the data set.
-	Appropriately labels the data set with descriptive variable names.
-	Creates a second, independent tidy data set with the average of each variable for each activity and each subject.
Step 1.	Download data from the UCI Human Activity Recognition project.
Click on the link:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

Step 2.	Move “X_train” and “X_test” data frames into R without headers.
x_train <- read.csv("X_train.csv", header = FALSE)
x_test <- read.csv("X_test.csv", header = FALSE)

Step 3.	Row bind (rbind) “x_train” and “x_test” data frames to create a unified data frame called “bgdf”.
bgdf <- rbind(x_train, x_test)

Step 4. Move the “features” list into R and create a vector of the “features” names.  
features <- read.csv(“features.csv”)
feats <- features[,2]
Step 5.	Create column names (Factors) for both “X_train” and “X_test” data frames using the “feats” vector.
colnames(bgdf) <- feats

Step6.	Move “sub_train” and “sub_test” into R as vectors and concatenate them to create a longer vector called “subject”. 
sub_train <- read.csv(“sub_train”)
sub_test <- read.csv(“sub_test”)
subject <- c(sub_train$sub_train, sub_test$sub_test) 

Step 7.	Column bind (cbind) the “subjects” vector to the “bgdf” data frame, to the right of it, creating a new data frame called “bgdf1”. 
bgdf1 <- cbind(bgdf, subject)

Step 8.	Move “y_train” and “y_test” into R as data frames and combine the activity index value columns of each of these data frames into one vector called “activity_index”.
y_train <- read.csv("y_train.csv")
y_test <- read.csv("y_test.csv")
activity_index <- c(y_train$y_train, y_test$y_test)

Step 9.	Column bind (cbind) the “activity index” vector to the “bgdf1” data frame to the right of it, creating a new data frame called “bgdf2”.
bgdf2 <- cbind(bgdf1, activity_index)

Step 10. Move “activities” into R.  This is a small data frame with one column containing activity_index variable (the integers 1 through 6) and another column containing the descriptive labels for the six physical activities.
act <- read.csv("activities.csv")


Step 11. Merge “BothX_subj_indx” with “activity_names” to create “final_bgdf”.  Then remove the “activity_index” column (which was transferred to the left edge) by subsetting columns 2 through 564 to eliminate the “activity_index column.
final_bgdf <- merge(bgdf2, act, by.x="activity_index")
final_bgdf <- final_bgdf[,2:564]

Step 12. Use the dplyr function to create a new, subset data frame from “final_bgdf”, called “mean_df” made of only variables containing means of measurements.  Do the same for the standard deviation variables from “final_bgdf”, creating a new data frame called “std_df”.  Print summary statistics for each new data frame separately.
library(dplyr)
mean_df <- select(final_bgdf, contains("mean"))
str(mean_df)
std_df <- select(final_bgdf, contains("std"))
str(std_df)

Step 13. Create a new, independent, tidy data set from “final_bgdf”  in which every variable is contained in only one column which is called “tidy1”.


Step 14. Create a new data set called “tidy2” that contains the mean for each variable for each activity and each subject.

