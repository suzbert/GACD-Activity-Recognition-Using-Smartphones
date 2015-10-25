GACD-Activity-Recognition-Using-Smartphones
an exercise using R to create tidy data
Goal: Prepare a tidy dataset to use for later analysis 
from data collected by Samsung Galaxy S smartphone accelerometers 

#Sources:
data: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
background details: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

#Data:
30 subjects
10299 cases
561 attributes
6 activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING)
70% of subjects generate training data, 30% generate test data (subjects randomly assigned to test or train condition)

#Files utilized from UCI HAR Dataset:
Training data files
train/X_train.txt: Training set - A 561-feature vector with time(t) and frequency(f) domain variables
train/y_train.txt: Training labels - Its activity label ++ match with activity_labels.txt for descriptive labels
train/subject_train.txt: subject identifier

Test data files
test/X_test.txt’: Test set- A 561-feature vector with time and frequency domain variables
test/y_test.txt’: Test labels - Its activity label 
test/subject_test.txt: subject identifier

#Labels and names
features_info.txt: Shows information about the variables used on the feature vector
features.txt: Contains the list of 561 features to map to the test and training datasets
activity_labels.txt: Links the class labels with their activity name (6 activities)

#Objective: Create one R script called run_analysis.R that does the following: 
    1. Merges the training and the test sets to create one data set. 
    2. Extracts only the measurements on the mean and standard deviation for each measurement. 
    3. Uses descriptive activity names to name the activities in the data set 
    4. Appropriately labels the data set with descriptive activity names. 
    5. Creates a second, independent tidy data set with the average of each variable for each activity and each subject. 

install data.table and reshape2 packages. 
http://www.r-bloggers.com/library-vs-require-in-r/

```{r}
if (!require("data.table")) { 
  install.packages("data.table") 
} 
```

```
if (!require("reshape2")) { 
  install.packages("reshape2") 
} 
```

```
require("data.table") 
```

```
require("reshape2") 
```

#Start with test data Read: activity labels

activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt")[,2]

# Read: data column names
features <- read.table("./UCI HAR Dataset/features.txt")[,2]

# get only column names with "mean" or "std"
extract_features <- grepl("mean|std", features)

# Read X_test & y_test data. Apply names.
X_test <- read.table("./UCI HAR Dataset/test/X_test.txt")
y_test <- read.table("./UCI HAR Dataset/test/y_test.txt")
subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt")
names(X_test) = features

#subset data on those selected mean, std column names
X_test = X_test[,extract_features]

# Read activity labels. map activity labels to column next to Activity ID
y_test[,2] = activity_labels[y_test[,1]]
names(y_test) = c("Activity_ID", "Activity_Label")
names(subject_test) = "subject"

# Bring test data together column-wise (horizontal). put subject into first column
test_data <- cbind(as.data.table(subject_test), y_test, X_test)

# Continue with train data
# Read X_train & y_train data.  Apply names.

X_train <- read.table("./UCI HAR Dataset/train/X_train.txt")
y_train <- read.table("./UCI HAR Dataset/train/y_train.txt")
subject_train <- read.table("./UCI HAR Dataset/train/subject_train.txt")
names(X_train) = features

#subset data on those selected mean, std column names
X_train = X_train[,extract_features]

# Read activity data
# map activity labels to column next to Activity ID
y_train[,2] = activity_labels[y_train[,1]]
names(y_train) = c("Activity_ID", "Activity_Label")
names(subject_train) = "subject"

# Bring train data together column-wise (horizontally)
#put subject into first column
train_data <- cbind(as.data.table(subject_train), y_train, X_train)

# Merge test and train data row-wise (vertically)
# ID_labels is , data_labels is everything else
data = rbind(test_data, train_data)

# define columns for IDs and labels and for rest of the outcome data collected
id_labels = c("subject", "Activity_ID", "Activity_Label")
data_labels = setdiff(colnames(data), id_labels)

# Melt datasets such that each subject has one line of data 
melt_data = melt(data, id = id_labels, measure.vars = data_labels)

# Apply mean function to all values in melted dataset
tidy_data = dcast(melt_data, subject + Activity_Label ~ variable, mean)

# write table with row.name = FALSE
write.table(tidy_data, file = "./tidy_data.txt", row.name=FALSE)
