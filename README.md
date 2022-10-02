# Getting and Cleaning Data

# Problem statement

The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 
1) a tidy data set as described below, 
2) a link to a Github repository with your script for performing the analysis, and 
3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. 

You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.

# Project requirements

Create one R script called run_analysis.R that does the following: 

1.	Merges the training and the test sets to create one data set.
2.	Extracts only the measurements on the mean and standard deviation for each measurement. 
3.	Uses descriptive activity names to name the activities in the data set
4.	Appropriately labels the data set with descriptive variable names. 
5.	From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

# Solution:

# Tidy data set

1. Merge the training and the test sets to create one data set.  
This is done using the functions `cbind` (to merge x_test, y_test 
and subject_test for both train and test data sets) and the `rbind` 
to merge the two data sets together. 

2. Extracts only the measurements on the mean and standard deviation for each measurement.  
This is done by using the `grep` function extracting every variable containing "mean" 
OR "std". 

3. Uses descriptive activity names to name the activities in the data set  
It was easy to define the columns names with the function `colnames`.

4. Appropriately labels the data set with descriptive activity names.  
This step was achieved by using the ```gsub``` function.  

5. Created an independent tidy data set with the average of each variable for each activity and each subject.  

A combination of the `melt` and the `dcast` function permits to calculate the mean for every activity/subject. 
