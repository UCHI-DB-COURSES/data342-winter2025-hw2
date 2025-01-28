# HW2 (Due 2/5 11:59pm)

In this assignment, you will be using Databricks on Google Cloud Platform (GCP), creating a table with data from the yellow cab NYC Taxi Dataset [NYC Taxi Data](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page), and then running a set of SQL queries against that table. You will then import this same data into BigQuery (if you still have it imported from HW1 you can reuse that) and run the same queries against that table. You will measure the runtimes of each and compare the performance of the SQL workload on BigQuery and Databricks.

**As a note, this lab will likely trigger some pop-up windows prompting login, and if you are using Safari pop-ups are sometimes blocked. Just check the URL box to see if this is happening and you can open pop-ups from there, or use another browser like Firefox or Chrome.**
Additionally, in this process you may be redirected and asked to login by selecting your google account a few times. That is normal, just click through that process, and allow Databricks the permissions it requests for. 

# Submission 
## Where to Submit
Accept the Github classroom link for HW2 posted on Ed. This will create a Github repo for you. Then, push your writeup and any code you wrote to that repo in the `HW2` folder. 

## Writeup Requirements 
Include the following information:
1. Performance information for each query run in both BigQuery and Databricks
2. Some analysis of this data. What queries seem to have the same performance? What have different? For those that are different, do you have hypotheses on what is making the performance different?
3. How you found deploying Databricks, building tables, and running queries compared to the same process natively in GCP and BigQuery
4. Any feedback you have on the assignment, relative difficulty, or other information.

# Assignment Description 
## Prepare Databricks
Go to the link for [Databricks on GCP](https://cloud.google.com/databricks?hl=en) and then click "Try on Marketplace".

<img width="1098" alt="Screenshot 2025-01-24 at 5 20 29 PM" src="https://github.com/user-attachments/assets/b5661508-5b88-4578-b74b-051a8a7200c6" />

Then click the checkbox and `agree and continue` in the bottom right.

<img width="563" alt="Screenshot 2025-01-24 at 5 25 04 PM" src="https://github.com/user-attachments/assets/246d31b6-8f6d-492f-98da-b65b8f583079" />

Then click the "Databricks" box and then on the next page click "Subscribe". 
<img width="808" alt="Screenshot 2025-01-24 at 5 27 13 PM" src="https://github.com/user-attachments/assets/e02370c1-984a-48d0-b597-0ec61b7c8094" />
<img width="754" alt="Screenshot 2025-01-24 at 5 27 39 PM" src="https://github.com/user-attachments/assets/dba5c7d4-a6bf-4164-9285-d7bd0f3ebf33" />

After hitting subscribe, you'll be brought to a new page. Scroll down, under "Purchase details" select your billing account, then click the box at the bottom and click "Subscribe".
<img width="940" alt="Screenshot 2025-01-24 at 5 33 49 PM" src="https://github.com/user-attachments/assets/a60f4b9a-a9ad-4bf3-b4a5-998586f06568" />

After this, we're going to need to make an account with Databricks through their portal. Click the "Sign Up With Databricks" button. 

<img width="1507" alt="Screenshot 2025-01-24 at 5 34 44 PM" src="https://github.com/user-attachments/assets/cf869089-4593-4fd5-a9ba-97b34737dbec" />

You want to create an organization name (this can be anything) and then click Login with Google and then select the Google account you're using for the Google Cloud Platform setup. This will process for a little bit, and it's possible it may require you to refresh the pop-up window and re-enter the information. Once it's done the pop-up window should close and you can go back to the other GCP tab you had open. 

<img width="1214" alt="Screenshot 2025-01-24 at 5 36 15 PM" src="https://github.com/user-attachments/assets/732df1c8-9184-486d-80ab-87c192c5a3ad" />

On that tab you should see that it has changed (or refresh it first) to have a button that says "Manage on Provider." Click this button to be taken to the Databricks site. 
<img width="1509" alt="Screenshot 2025-01-24 at 5 37 08 PM" src="https://github.com/user-attachments/assets/68e30f55-b9e0-4c87-98ac-8c9d3b39c65a" />
Once on the databricks site, you can once again login with Google. It will prompt you select a subscription plan. This comes with a 14-day free trial, which will be enough to cover all the usage we need. 
<img width="1511" alt="Screenshot 2025-01-24 at 5 37 26 PM" src="https://github.com/user-attachments/assets/2a18a3f6-bd5f-4262-8c51-5777508a9b5c" />
<img width="1512" alt="Screenshot 2025-01-24 at 5 37 40 PM" src="https://github.com/user-attachments/assets/2b128edc-41f9-4a4e-a74e-f8adcfce2ae6" />

## Create Workspace
Next, we're going to go back to our GCP page. In the upper part of the screen, you'll see a box with your project name. In the example image, there is a red-box drawn around this. Click that box and you'll be shown an overlay that lists the project and it's project ID. Copy this project ID as it will be needed to create a Databricks Workspace. This ID is also labelled with a red box. in the second image below.
<img width="881" alt="Screenshot 2025-01-24 at 5 39 03 PM" src="https://github.com/user-attachments/assets/1f476014-de22-442f-9ee9-1a88b6f2d7c1" />
<img width="762" alt="Screenshot 2025-01-24 at 5 40 22 PM" src="https://github.com/user-attachments/assets/d353054a-77e5-47e7-af38-3abed499e09d" />

Once you're logged in, you'll see a workspaces tab on the left hand nav bar. Select that and select "Create Workspace." You will be brought to a page which asks you to give the workspace a name ***make this data342 to fit with the pre-written SQL workload***, pick a GCP region (I chose us-east-1), and paste the project ID you copied in the previous step. Then click "Create workspace."
<img width="1511" alt="Screenshot 2025-01-24 at 5 38 02 PM" src="https://github.com/user-attachments/assets/c8a1bb5f-0229-4f43-9e8c-8068c92d78f8" />
<img width="1029" alt="Screenshot 2025-01-24 at 5 41 17 PM" src="https://github.com/user-attachments/assets/632ba2ee-18fb-47f2-9428-460904d18731" />



## Create Table
Finally, we want to create a table! Go back to the Workspaces page and wait for the workspace to be created and for the status to be "Running". Once that's the case, hit the "Open" button labelled in the image below.
<img width="1311" alt="Screenshot 2025-01-24 at 5 48 41 PM" src="https://github.com/user-attachments/assets/a0be0031-a986-4c25-a4af-8681bcc56da6" />
You will be taken to a page with a nav-bar in the left hand side. Select Catalog, then select the workspace name you chose earlier (for me it was Data342) and click default. Then click "Create > Create Table" as shown below. 
<img width="1512" alt="Screenshot 2025-01-24 at 5 49 43 PM" src="https://github.com/user-attachments/assets/7f86da80-e259-4820-b7ea-e9ce890e4c82" />
You will be brought to a page where you are asked to upload files. Upload the yellow-cab data Parquet files you downloaded for February through December 2023 you downloaded earlier from the NYC taxi site into a single table. Once these are done uploading, you'll be taken to the following page, where you can rename the table under Table Name.
<img width="1512" alt="Screenshot 2025-01-24 at 5 52 56 PM" src="https://github.com/user-attachments/assets/300baf8a-7dcb-4e10-af83-359af118a556" />

Once the table is done, you are ready to query data!

## Run SQL Queries and Record Performance

Go to the SQL Editor in the nav-bar on the left. This will open a window where you can write or paste SQL queries. You can access the table by using the format `WORKSPACE_NAME.default.TABLE_NAME`, i.e. `data342.default.taxi_data` as shown below. You can then run the query and see the results and the runtime as well in that same page. 
<img width="1510" alt="Screenshot 2025-01-24 at 6 11 53 PM" src="https://github.com/user-attachments/assets/bfd3cfd0-6c56-441d-8fc9-110cf50b4abe" />

## Setup BigQuery
Setup BigQuery the same as in HW1, but be sure to import data from February to December 2023 yellow-cab Parquet files into a single BigQuery **INTERNAL** table. Repeat this, but do it for an **EXTERNAL** table as well. You will run each query against both table options. Name your dataset within BigQuery **Data342** to avoid having to edit the pre-written SQL workload.

## SQL Workload 
Now that we have this setup, we're going to run a SQL workload in Databricks. There are 4 SQL query files in the `HW2/SQL` folder in your repo, each containing a single SQL query. You're going to run each query first against the Databricks table, and then against both BigQuery table versions (using standard, built-in tables that are stored internally as well as tables which are externally stored). You can copy and paste the queries into the editors, or read documentation online to setup APIs to execute SQL queries via APIs.

The queries are written to assume that in BigQuery your **dataset** is named
**DATA342** and all data is imported into a single table entitled **TAXI**. In
Databricks, the queries assume you created a workspace entitled **DATA342**, and
within the **default** catalog you create a table called **TAXI**. If you have
alternate names, you must tweak the queries to change the tables listed in the
FROM clause. We advise **not** changing column names from the default definitions in the Parquet
files as this  would require many more SQL changes.

There is a BigQuery and Databricks version for each of the four queries, in the `HW2/SQL/BQ`
and `HW2/SQL/Databricks` folders respectively. 

Run each query, record the runtime in each system, and present this in your writeup, either in a table or other format. 




