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

<img width="1098" alt="image" src="https://github.com/user-attachments/assets/2706b2cf-87ac-4fe7-bc2c-45d27c601010" />

Then click the checkbox and `agree and continue` in the bottom right.

<img width="563" alt="image" src="https://github.com/user-attachments/assets/86fddc7c-c853-423a-ad8f-d1d8c66808a8" />

Then click the "Databricks" box and then on the next page click "Subscribe". 
<img width="808" alt="image" src="https://github.com/user-attachments/assets/79862dbd-cab7-48b9-8644-cbb76e9879a7" />

<img width="754" alt="image" src="https://github.com/user-attachments/assets/3c2a7742-11c2-4469-ba7c-90437c295ef2" />



After hitting subscribe, you'll be brought to a new page. Scroll down, under "Purchase details" select your billing account, then click the box at the bottom and click "Subscribe".
<img width="940" alt="image" src="https://github.com/user-attachments/assets/6b6766c3-a5db-44a5-b3ce-ef3d2da46117" />


After this, we're going to need to make an account with Databricks through their portal. Click the "Sign Up With Databricks" button. 

<img width="1507" alt="image" src="https://github.com/user-attachments/assets/7f288f1f-b9f2-423e-b7ce-29ba88c844dd" />

You want to create an organization name (this can be anything) and then click Login with Google and then select the Google account you're using for the Google Cloud Platform setup. This will process for a little bit, and it's possible it may require you to refresh the pop-up window and re-enter the information. Once it's done the pop-up window should close and you can go back to the other GCP tab you had open. 

<img width="1214" alt="image" src="https://github.com/user-attachments/assets/0dc1a461-c2c3-4998-9fb7-dbd62c474b64" />

On that tab you should see that it has changed (or refresh it first) to have a button that says "Manage on Provider." Click this button to be taken to the Databricks site. 

<img width="1509" alt="image" src="https://github.com/user-attachments/assets/43e420e2-7297-41e6-adff-248f34f2a3ad" />

Once on the databricks site, you can once again login with Google. It will prompt you select a subscription plan. This comes with a 14-day free trial, which will be enough to cover all the usage we need. 

<img width="1511" alt="image" src="https://github.com/user-attachments/assets/a2cdf245-5dce-46e7-8300-3582b42d6fea" />

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/9d236cd8-ff22-49d4-856f-8326c50c4c50" />

## Create Workspace
Next, we're going to go back to our GCP page. In the upper part of the screen, you'll see a box with your project name. In the example image, there is a red-box drawn around this. Click that box and you'll be shown an overlay that lists the project and it's project ID. Copy this project ID as it will be needed to create a Databricks Workspace. This ID is also labelled with a red box. in the second image below.

<img width="881" alt="image" src="https://github.com/user-attachments/assets/57b27960-dd24-43b3-9531-3f9f1e30df84" />

<img width="762" alt="image" src="https://github.com/user-attachments/assets/57905d80-84f1-4443-9298-25c40831679c" />

Once you're logged in, you'll see a workspaces tab on the left hand nav bar. Select that and select "Create Workspace." You will be brought to a page which asks you to give the workspace a name ***make this data342 to fit with the pre-written SQL workload***, pick a GCP region (I chose us-east-1), and paste the project ID you copied in the previous step. Then click "Create workspace."

<img width="1511" alt="image" src="https://github.com/user-attachments/assets/de404b8d-1235-45bf-bdef-42256f265146" />

<img width="1029" alt="image" src="https://github.com/user-attachments/assets/40afaa0e-f445-404a-960b-77cf0df49347" />

## Create Table
Finally, we want to create a table! Go back to the Workspaces page and wait for the workspace to be created and for the status to be "Running". Once that's the case, hit the "Open" button labelled in the image below.

<img width="1311" alt="image" src="https://github.com/user-attachments/assets/25223b77-7869-4fd0-b75e-354bb5a1ef06" />

You will be taken to a page with a nav-bar in the left hand side. Select Catalog, then select the workspace name you chose earlier (for me it was Data342) and click default. Then click "Create > Create Table" as shown below. 

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/aa0ced77-4048-4135-b63c-6def0b715a6c" />

You will be brought to a page where you are asked to upload files. Upload the yellow-cab data Parquet files you downloaded for February through December 2023 you downloaded earlier from the NYC taxi site into a single table. Once these are done uploading, you'll be taken to the following page, where you can rename the table under Table Name.

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/96b554e3-8123-4122-89c1-0dd41a7caf71" />

Once the table is done, you are ready to query data!

## Run SQL Queries and Record Performance

Go to the SQL Editor in the nav-bar on the left. This will open a window where you can write or paste SQL queries. You can access the table by using the format `WORKSPACE_NAME.default.TABLE_NAME`, i.e. `data342.default.taxi_data` as shown below. You can then run the query and see the results and the runtime as well in that same page. 

<img width="1510" alt="image" src="https://github.com/user-attachments/assets/94b3d54f-5b48-4130-aab6-24f5f830e795" />

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




