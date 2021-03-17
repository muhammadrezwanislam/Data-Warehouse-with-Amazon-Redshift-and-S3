# Project Description

Sparkify, a music streaming app startup, wants to leverage songs and user data that they have been collecting from the app by analyzing and finding relevant patterns. In particular, the analytics team wants to know what are the songs that the users are listening to. However, within the current setup, it is difficult to make sense of the data. In recent times, the app has grown its user base as well as song database and want to move their processes and data onto the cloud. Up until now, their data resides in Amazon s3 Bucket, in directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app, which is not suitable for quering at all. The goal of this project is to create an ETL pipeline that extracts data from S3, stages them in Redshift, and transforms data into a set of dimensional tables for their analytics team to continue finding insights in what songs their users are listening to. Here is a short overview of the project:

![ETL in context](ETL in Context.png)

# Project Steps:

a) Design schemas for fact and dimension tables
b) Write a SQL CREATE statement for each of these tables and SQL DROP statements to drop tables in the beginning of the table creation process if it already exists 
c) Launch a redshift cluster and create an IAM role that has read access to S3 using python SDK
d) Add redshift database, secret keys and IAM role info to dwh.cfg
e) Test by running the creating sql code and checking the table schemas in redshift database. One option is to use Query Editor in the AWS Redshift console for this
f) Write code to load data from S3 to staging tables on Redshift
g) Write code to load data from staging tables to analytics tables on Redshift
h) Test these data loading code after creating tables and running the analytic queries on the Redshift database to compare results with the expected results
i) Delete the Redshift cluster when done

# Schema Design


For this project, we have implemented this following star schema:

![Database ER diagram (crow's foot)](Database ER diagram (crow's foot)_v2.0.jpeg)

As the fact table contains a 1 to many relationship to each of the dimensions in the data, we thought that it is appropriate to use a star schema. This database will enable the Analytics team to create recommendations to the users based on query data. Especially, they can capture live playback behavior such as song plays, artist plays, time when it is played, and more. Ultimately, this would enable the analytics team to use this behavioral data to recommend personalize playlists.

# Project Files:
Here is the list of project files and related descriptions:

1) create_tables.py -  creates fact and dimension tables for the star schema in Redshift.
2) etl.py -  load data from S3 into staging tables on Redshift and then process that data into analytics tables on Redshift.
3) sql_queries.py - definition of SQL statements, which will be imported into the two other files above.
4) dhw.cfg - Configuration file used that contains info about Redshift, IAM and S3
5) Creating Redshift Cluster using the AWS python sdk.ipynb - infrastructure as code for creating Redshift clusters in AWS 
6) Images - all relevant images related to this project 


# Query Result:

Here is a sample query result from AWS Redshift Console:

![Query Result](Query_One.png)

# How to run this project

a) Configure the [AWS] section of the dhw.cfg file with access keys from AWS
b) Create a Redshift cluster using "Creating Redshift Cluster using the AWS python sdk.ipynb" and take a note of IAM role and host. Insert these two piece of information in dwf.cfg file
c) After opening terminal session, set your filesystem on project root folder and run create_tables.py and then etl.py