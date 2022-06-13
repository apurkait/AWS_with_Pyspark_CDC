# AWS_with_Pyspark_CDC

Tools used in this Project:  
Python 3  
PySpark  
Mysql Workbench  
AWS RDS with MySql  
AWS S3  
AWS DMS  
AWS Lambda  
AWS Glue  

Description:  
This is a CDC (Change Data Capture) Project where a pipeline has been created with below steps will be happening internally on FULL LOAD:  
1) Insert few records in RDS instance using Mysql Workbench.
2) As soon as data has been inserted, all data are shifted to an input S3 bucket using a DMS migration task where RDS instance acts as a source endpoint and S3 acts as target endpoint.
3) After successful transfer to S3 bucket, a lambda function were triggered (code uploaded).
4) The triggered lambda function in turn starts a glue job (code uploaded) to make some transformations on the data using PySpark.
5) After successful transformation, the transformed data will be moved to a separate S3 output bucket.


Functionality on Data changes inside RDS (any UPDATE/INSERT/DELETE operations) after FULL LOAD:
1) After updating few data in the database, Process (2), (3) and (4) were followed just like above except the changed/modified data will be saved as a separate file in S3 bucket so that DMS ONLY migrates the changed data, not the whole data.
2) After successful transformation, transformed data will be merged with the previous saved output file and sent to S3 bucket.
