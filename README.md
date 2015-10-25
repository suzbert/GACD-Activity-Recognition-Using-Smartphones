# GACD-Activity-Recognition-Using-Smartphones
an exercise using R to create tidy data


#The R script called "run_analysis.R" executes the following steps: 
Test data are run through the scripted commands first, then training data.

1.Reads activity labels from "activity_labels.txt". Reads column names from "features.txt". Reads the data (x, y and subject) 
  For test data: from 'test/X_test.txt', 'test/y_test.txt', "test/subject_test.txt". 
  For training data: from 'train/X_train.txt', 'train/y_train.txt', "train/subject_train.txt".

2.For the test and training data: Identifies data to subset out from features: Extract_features gets only column names containing "mean" or "std" 

3.Uses "features.txt" to supply column names for the test and training datasets 

4.Adds a new column "activity_label" next to activity IDs in the test and training datasets, then populates the column with the labels from "activity_labels.txt".

5. Pulls all test data together, column-wise, and puts "subject" into first column. 

6. Adds new columns "subject", "activity_id", "activity_label" into the test and training datasets.

7. Merges test and training data.

8. Melts data so that each subject has one line of data.

9. Stores tidy_data in plain text file called "tidy_data.txt".
