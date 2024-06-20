Visibility-Prediction-ML-Model

Problem Statement:

Based on different weather conditions build a ml model to predict the visibility distance.

Data Description -

The data is in multiple sets of files in batches at a given location. Data contains information about different weather conditions, and a visibility column that has values ranging from 0 to 10.
Apart from training files, we need a "schema" file, which contains all the relevant information about the training files such as: Name of the files, Length of Date value in FileName, Number of Columns, Name of the Columns, and their datatype.

Data Validation -

In this step, we perform different sets of validation on the given set of training files.

Name Validation- We validate the name of the files based on the given name in the schema file, using a regex pattern as per the name given in the schema file to use for validation. After validating the pattern in the name, we check for the length of date in the file name. If all the values are as per requirement, we move such files to "Good_Data_Folder" else we move such files to "Bad_Data_Folder."

Number of Columns - We validate the number of columns present in the files, and if it doesn't match with the value given in the schema file, then the file is moved to "Bad_Data_Folder."

Name of Columns - The name of the columns is validated and should be the same as given in the schema file. If not, then the file is moved to "Bad_Data_Folder".

The datatype of columns - The datatype of columns is given in the schema file. This is validated when we insert the files into Database. If the datatype is wrong, then the file is moved to "Bad_Data_Folder".

Null values in columns - If any of the columns in a file have all the values as NULL or missing, we discard such a file and move it to "Bad_Data_Folder".

Data Insertion in Database

1.	Database Creation and connection - Create a database with the given name passed. If the database is already created, open the connection to the database.

2.	Table creation in the database - Table with name - "Good_Data", is created in the database for inserting the files in the "Good_Data_Folder" based on given column names and datatype in the schema file. If the table is already present, then the new table is not created and new files are inserted in the already present table as we want training to be done on new as well as old training files.

3.	Insertion of files in the table - All the files in the "Good_Data_Folder" are inserted in the above-created table. If any file has invalid data type in any of the columns, the file is not loaded in the table and is moved to "Bad_Data_Folder".

Model Training -

1.	Data Export from Db - The data in a stored database is exported as a CSV file to be used for model training.

2.	Data Preprocessing

a) Check for null values in the columns. If present, impute the null values using the KNN imputer.

b) Check if any column has zero standard deviation, remove such columns as they don't give any information during model training.


3.	Clustering - KMeans algorithm is used to create clusters in the preprocessed data. The optimum number of clusters is selected by plotting the elbow plot, and for the dynamic selection of the number of clusters, we are using "KneeLocator" function. The idea behind clustering is to implement different algorithms To train data in different clusters. The Kmeans model is trained over preprocessed data and the model is saved for further use in prediction.

4.	Model Selection - After clusters are created, we find the best model for each cluster. We are using two algorithms, "Random Forest" and "XGBoost". For each cluster, both the algorithms are passed with the best parameters derived from GridSearch. We calculate the AUC scores for both models and select the model with the best score. Similarly, the model is selected for each cluster. All the models for every cluster are saved for use in prediction.

5.	Once the prediction is made for all the clusters, the predictions along with the Wafer names are saved in a CSV file at a given location and the location is returned to the client.


Language Used – Python

Framework- Flask

Tools- VsCode

Algorithms - Random Forest Regressor and XGBoost Regressor







