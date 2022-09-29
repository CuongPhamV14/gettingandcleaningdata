# gettingandcleaningdata
Data science course


# Define some vairiables 

library(dplyr)

setwd("/Users/..../Data Science/Course 3 - Getting and cleaning data/")

fpath <- "UCI HAR Dataset"
fname <- 'getdata_dataset.zip'


# Check if the data is already downloaded and unzipped in destination folder. If not, download 
# and unzip the data

if(!file.exists(fpath)) {
  # Do they at least have the zip file?
  if(!file.exists(fname)) {
    
    # They don't have the zip file, so downlaod it
    download.file(
      'https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip',
      fname
    )
  }
  # unzip the file
  unzip(fname)
}

# Q1. Merges the training and the test sets to create one data set.

# Read in the data into the test and training sets

features <- read.table(file.path(fpath, 'features.txt'),col.names = c("n","functions"))
activities <- read.table(file.path(fpath,  'activity_labels.txt'), col.names = c("code", "activity"))

test.subjects <- read.table(file.path(fpath, 'test', 'subject_test.txt'),  col.names = "subject")
xtest.data <- read.table(file.path(fpath, 'test', 'X_test.txt'), col.names = features$functions)
ytest.data <- read.table(file.path(fpath, 'test', "y_test.txt"), col.names = "code")

train.subjects <- read.table(file.path(fpath, 'train', 'subject_train.txt'), col.names = "subject")
xtrain.data <- read.table(file.path(fpath, 'train', 'X_train.txt'), col.names = features$functions)
ytrain.data <- read.table(file.path(fpath, 'train', 'y_train.txt'), col.names = "code")


# Bind the rows for each of the data sets together

x.data <- rbind(xtrain.data, xtest.data)
y.data <- rbind(ytrain.data, ytest.data)
subject <- rbind(train.subjects, test.subjects)

# Combine all data into one table 

full.data <- cbind(subject, y.data, x.data)


# Q2. Extracts only the measurements on the mean and standard deviation for each measurement. 

tidy.data <- full.data %>% select(subject, code, contains("mean"), contains("std"))


# Q3. Uses descriptive activity names to name the activities in the data set

tidy.data$code <- activities[tidy.data$code, 2]


# Q4. Appropriately labels the data set with descriptive variable names. 

names(tidy.data)[2] = "activity"
names(tidy.data)<-gsub("Acc", "Accelerometer", names(tidy.data))
names(tidy.data)<-gsub("Gyro", "Gyroscope", names(tidy.data))
names(tidy.data)<-gsub("BodyBody", "Body", names(tidy.data))
names(tidy.data)<-gsub("Mag", "Magnitude", names(tidy.data))
names(tidy.data)<-gsub("^t", "Time", names(tidy.data))
names(tidy.data)<-gsub("^f", "Frequency", names(tidy.data))
names(tidy.data)<-gsub("tBody", "TimeBody", names(tidy.data))
names(tidy.data)<-gsub("-mean()", "Mean", names(tidy.data), ignore.case = TRUE)
names(tidy.data)<-gsub("-std()", "STD", names(tidy.data), ignore.case = TRUE)
names(tidy.data)<-gsub("-freq()", "Frequency", names(tidy.data), ignore.case = TRUE)
names(tidy.data)<-gsub("angle", "Angle", names(tidy.data))
names(tidy.data)<-gsub("gravity", "Gravity", names(tidy.data))

# Q5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

fdata<-aggregate(. ~subject + activity, tidy.data, mean)
fdata<-fdata[order(fdata$subject,fdata$activity),]
write.table(fdata, "tidy.txt", row.name=FALSE)
