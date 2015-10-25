# GACD-Activity-Recognition-Using-Smartphones
an exercise using R to create tidy data

The R script called "run_analysis.R" executes the following steps.
Test data are run through the scripted commands first, 
then training data.

Reads activity labels from  "activity_labels.txt".
Reads column names from "features.txt".
Reads the data (x, y and subject) 
  For test data:  from 'test/X_test.txt', 'test/y_test.txt', "test/subject_test.txt".
  For training data:  from 'train/X_train.txt', 'train/y_train.txt', "train/subject_train.txt".

For the test and training data:
  Identifies data to subset out from features:  Extract_features gets only column names containing "mean" or "std" 
  Uses "features.txt" to supply column names for the test and training datasets
  Adds a new column "activity_label" next to activity IDs in the test and training datasets, then populates the column with    the labels from "activity_labels.txt".

Pulls all test data together, column-wise, and puts "subject" into first column.
Pulls all training data together, column-wise, and puts "subject" into first column.

Adds new columns "subject", "activity_id", "activity_label" into the test and training datasets.

Merges test and training data.

Melts data so that each subject has one line of data.

Stores tidy_data in plain text file called "tidy_data.txt".
