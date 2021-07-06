# ETL-DAG-cloud-blankspace-week2
Running batch ETL DAG using Google Cloud Composer

# Data Source
1. CSV files which later will be store on Google Cloud Storage
2. BigQuery Table

# Prerequisite
1. Python 3.9 (at least using 3.6 version)
2. Google Cloud Platform
  - Composer
  - Dataflow
  - BigQuery
  - Google Cloud Storage
 
# Google Cloud Composer Environment Set Up
Composer Environment is a place to manage and run your airflow on Google Cloud Platform. With this tools, you can focus on writing your DAGs without spending more time on managing airflow.
Here are the steps of set it up:
1. Activate the Composer API (if you have, feel free to skip this step)
2. Open https://console.cloud.google.com/
3. Go to Big Data Section on Navigation Menu, select Composer. Wait until the Composer page shows up
4. Click on CREATE Button to create a new environment
5. Fill the required parts there. For this project, I use these configuration:
   - Name : cloud-etl-blankspace (you can name it per your project name)
   - Location : us-central1
   - Node Count : 3
   - Zone : us-central1
   - Machine Type : n1-standard-1
   - Disk size (GB) : 20 GB
   - Service account: choose your Compute Engine service account
   - Image Version: composer-1.17.0-preview.1-airflow-2.0.1
   - Python Version: 3
   - Cloud SQL machine type : db-n1-standard-2
   - Webserver machine type : composer-n1-webserver-2
  6. Click Create Button
  7. Please note that it will take several minutes for your environment to be created
  ![image](https://user-images.githubusercontent.com/59094767/124632868-ea35c400-deae-11eb-94dd-a147e8f9454a.png)


# Airflow on Composer
After the environment is successfully created, the next step is to access airflow webpage on composer and run the dag. Here are the detail steps:
1. Click the Airflow webserver link option on Composer Environments list (This will show us the     same airflow page as http://localhost:8080 when we use local airflow.
2. Create json file on your project to store airflow variables. Let's named the file as variable.json, with minimum key-values are:
   {
   "PROJECT_ID": "",
   "GCS_TEMP_LOCATION": "",
   "BUCKET_NAME": "",
   "ALL_KEYWORDS_BQ_OUTPUT_TABLE": "",
   "GCS_STG_LOCATION": "",
   "MOST_SEARCHED_KEYWORDS_BQ_OUTPUT_TABLE": "",
   "EVENTS_BQ_TABLE": ""
   }
   with the details below:
   - PROJECT_ID : your GCP project ID
   - GCS_TEMP_LOCATION : temporary location of storing data before loaded it to BigQuery. The     format is gs:// followed by your bucket and object name
   - BUCKET_NAME : your GCS bucket name
   - ALL_KEYWORDs_BQ_OUTPUT_TABLE : output table in BigQuery to store keyword search data. The value format is dataset_id.table_id
   - GCS_STG_LOCATION: staging location to store data before loaded to BigQuery. The format is gs:// followed by your bucket and object name
   - MOST_SEARCHED_KEYWORDS_BQ_OUTPUT_TABLE : output table in BigQuery to store most searched keyword data. The value format is dataset_id.table_id
   - EVENTS_BQ_TABLE: output table in BigQuery to store event data from reverse engineering result. The value format is dataset_id.table_id
  3. Go to Admin > Variables
  4. Choose File then click Import Variables

# BigQuery
1. On your GCP navigation, go to BIg Data > BigQuery
2. Create your dataset there
3. Fill the Dataset form. In this project, we only need to set Data set ID and Data location
4. Select CREATE DATA SET option
5. Ensure that your dataset has been created successfully

# Google Cloud Storage
1. Back to your GCP console, go to Storage section > Cloud Storage
2. Click CREATE BUCKET button, and fill some fields there (the bucket name and where to store the data, for the rest of it, leave it as default)
3. Click CREATE
4. Your bucket will be created and displayed on GCS browser

# Output 
This ETL DAG cloud process will produce 3 BigQuery tables as the output:
1. keyword_search
   This table contains the information about user's keyword search on platform
2. most-searched_keyword
   This one is about most search keyword from keyword_search table
3. reverse-te table 
  This table fills with information of events table data that has been experienced reverse engineering process due to some missing data
