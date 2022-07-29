# simple_AWS_lambda_exercise
A simple exercise on using AWS Lambda functions to convert csv files that are dropped into a S3 bucket into parquet format, taken from Chapter 3 of Gareth Eagar's textbook: Data Engineering with AWS. This exercise had quite a number of steps, which I'll proceed to give a brief summary of.

1. Before creating the lambda function, we'll first have to create a lambda layer containing the AWS Data Wrangler Library. This can be downloaded here [here](https://github.com/awslabs/aws-data-wrangler/releases). In the AWS console when creating the lambda layer, simply upload the zip file you just downloaded.

2. Now, we'll have to create two S3 buckets, one for our files to land in and trigger the lambda function, and another bucket which will be where the clean/transformed
file will be pushed to after being processed by the lambda function.

3. Next, we'll have to create an IAM policy and role for the lambda function, so that the function has the rights to read from the source s3 bucket, write to the 
target S3 bucket, write logs to Amazon CloudWatch, and access to Glue API actions to allow creation of new databases and tables based on data in the files we throw
into the landing bucket. The JSON used to specify permissions should look something like this, but perhaps just replacing my name with yours.

![alt text](https://github.com/BrentTanKian/simple_AWS_lambda_exercise/blob/main/fixtures/AWSpermissions.PNG)

4. To finally create the lambda function, we use the code found in CSVtoParquetLambda.py file, and assign the IAM role we have just created to the function. (Don't forget to replace the bucket names in the file with the actual names of the buckets you've created!)

5. To configure the lambda function to be triggered by an S3 upload, click on the Add trigger button, and proceed to fill in the suffix as .csv (to only trigger the
function when a .csv file is uploaded), and for the bucket, select the landing zone bucket because that is where we'll be dumping files to.

6. And now to finally test that everything is going well, we test the lambda function by uploading the test.csv file to the landing bucket. If everything runs smoothly,
we should see a parquet file being created in the clean bucket like so.

![alt text](https://github.com/BrentTanKian/simple_AWS_lambda_exercise/blob/main/fixtures/Success.PNG)
