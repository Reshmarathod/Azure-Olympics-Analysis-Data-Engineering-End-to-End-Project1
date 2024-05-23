# **Tokyo Olympics Data Analysis: Microsoft Azure End-to-End Pipeline Project**



This repository provides a comprehensive guide on how to build and implement an end-to-end data engineering pipeline for the analysis of Tokyo Olympics data. This project takes advantage of several Microsoft Azure services, including Azure Data Lake Gen2, Azure Data Factory (ADF), Azure Synapse Analytics, Databricks, and Power BI.
Power BI link
Databricks Notebook link.

### **Project Overview**

The goal of this project is to develop an end-to-end data pipeline to ingest, transform, analyze, and visualize historic data from the Tokyo Olympics. Initially, data is ingested into an Azure Data Lake Gen2 storage account using Azure Data Factory pipelines. Following ingestion, the data is processed and curated using Databricks Notebooks. Azure Synapse Analytics is then deployed to create a SQL database from the curated data, which is subsequently integrated into PowerBI for sophisticated analytics and visualizations.

# **Project Architecture**


The architecture of this data pipeline has been designed to be robust and scalable. It consists of the following Azure services:

Azure Databricks for data transformation.
Azure Data Factory for data ingestion.
Azure Storage (Data Lake Gen2) for data storage.
Azure Synapse Analytics for creating a SQL database.
Power BI for data visualization.

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/9d4cb2e6-81d1-47dc-aab6-eee71adb5333)

# **Technologies and Services Used**


### **The project utilizes the following technologies and services from Microsoft Azure:**


Azure Databricks: A Fast, easy, and collaborative Apache Spark–based analytics service.
Azure Data Factory: A fully managed, serverless data integration solution for ingesting, preparing, and transforming data.
Azure Storage (Data Lake Gen2): A set of capabilities dedicated to big data analytics, built into Azure Blob storage.
Azure Synapse Analytics: An integrated analytics service for data integration, enterprise data warehousing, and big data analytics.
Power BI: A business analytics tool that delivers insights to enable fast, informed decisions.


# **Getting Started**
To implement this project, ensure you have an active Azure subscription and have access to the services defined above and a Databricks Workspace.

### **Create Resource Group**:

 Lets start by creating the Resource Group which will hold all our resources that we will use for this project.

Azure > Resource Group > Create Resource Group
![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/2ec00554-d6d3-482e-a384-089376b7063b)


### **Create Storage Account, Containers and Folders**:


From the above created resource group and we will now create a storage account which will hold our Azure Data Lake Gen2.

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/b67c1901-32dc-417f-9b8f-76db7bfe8a43)



Once a storage account is created we can now proceed to create container and folders inside the container which will hold our raw and transformed data.




![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/568ca4b3-0f44-42c9-901e-79f1f89ee8a5)






Go to the container that was just created and we will create folders inside the container as below:






![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/ab9908a6-de40-4d6a-adb8-7dfd938c567b)


## **Data Ingestion Process**:


Now the first step with our data is to ingest the data from our github repository to the ADLS2 using Azure Data Factory and the linked Services.

We will create a Azure Data Factory resource inside the resource group.

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/9dfae247-6442-422a-a64d-6a622752674f)

Once ADF is created, launch the ADF Studio. Now we need to ingest data into Azure Data Factory, to do that we will use Linked Services of Azure for each dataset.

Creating required Linked Services: Two Linked Services need to be created for each dataset. These linked services will establish the connection between Azure Data Factory and diverse sources of data.

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/efafbe71-3570-420e-a015-2622173e1d16)

As our source data resides on Github repository for Source Linked Service we will be using HTTP Data store. Continue with the process after selecting the data store type.

Provide the below details while creating the HTTP Linked Service



![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/50ea7f86-9f9f-4acf-9b1b-6137d9a68e11)

After provide all above details we will move forward and click on create to create the Source Linked Service, which will serve as a connection between Github Repo and Azure Data Factory.

Now that we have created a source connection, we will also need our ADF to connect to our destination i.e., ADLS2, so let’s create a Destination Linked Service for the same dataset as above.

As our destination is ADLS2, we will configure the Linked Service with ADLS2 data store.

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/78530156-e602-4b8b-8b3c-bc24d14d2764)

Same as we did in Source Linked Service, we will also provide the details here for Destination Linked Service.

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/c0fc89df-e749-497f-8a28-4f5a59b93bd7)

We will use authentication type as Account Key, then use our created storage account name to authenticate our Linked Service.

Similary for all other datasets we will be creating Source and Destination Linked Services. There are 5 datasets in our data, hence we will have 10 Linked Services in total.

Now we need to create source and destination datasets for each of the 5 datasets. To create datasets in ADF Studio, go to “Author” then Click on Datasets > “New Dataset”

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/fb51bd5b-eaa1-4f61-a158-0f935e32f75c)

Select the datastore, for Source we will use HTTP and for destination we will use DataLake Gen 2. Then we will need to select the file type i.e., CSV, then give name to dataset and select the linked service that is to be used. For example if we are creating a source dataset for Coaches.csv, we will use the Source Linked Service for the same.

Now that we have created the datasets, it’s time to create an ADF activity using pipelines. We will use copy data activity to Ingest the raw data from the GitHub Repo to ADLS raw layer



Once created, we can click on Debug to run the debugger on our pipeline. Similarly lets create copy activities for all other datasets.

Once we have created all the copy activities, we can link each activity in the pipeline to run one after another upon the success of the previous activity as below. We could also create the multiple pipeline for copy of each datasets, that would run independent of other copy activites

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/dc457215-4a66-4585-ba56-a53dd01381f4)

Once the data pipeline is run, you will see the datasets added into the raw folder of our container.

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/e813af76-6b20-4ae7-bef2-37fd41525647)




## **Data Transformation Process**:

After the successful ingestion of data into the Data Lake, we deploy Azure Databricks notebooks. These notebooks attach to our storage account and perform the necessary data transformations. This process involves creating an app under the App Registration Service and providing it with access control to the storage account.

Once completed, we can connect and mount our storage container to the Databricks notebook. We then activate a Spark session to execute transformations.



<img width="622" alt="image" src="https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/26ba1927-9d0a-4ebd-af16-031d79b68081">



<img width="645" alt="image" src="https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/342f0762-cd4a-4e25-a9c2-550b573d6800">






Following the execution of transformations, we store these files back into our Data Lake, but this time in the transformed_data folder.

<img width="649" alt="image" src="https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/1c35588a-199e-4447-9e99-ae87d43dd098">

<img width="671" alt="image" src="https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/b0beec9c-d7ca-44a4-84a4-9567866e003f">

<img width="622" alt="image" src="https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/8d8bdc84-cacc-4155-a039-ce2ccbdaa511">

<img width="580" alt="image" src="https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/361d0836-d62c-4fa0-b782-b31e3d35d467">

<img width="583" alt="image" src="https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/b72e877f-057e-48f5-a45c-bc67d4bd3d40">





## **Creation of the Database using Azure Synapse Analytics**:

Load Data from ADLS2 to Azure Synapse Analytics for Data Analysis:

From Azure Portal, search for Azure Synapse Analytics and Create a Synapse WorkSpace.

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/416df8b2-2ed6-4b18-973e-cb6de296b8a6)

Once the workspace is created we will proceed and launch the Synapse Studio. Go to the “Data Hub” and add a Lake Database to add the data into Synapse

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/23b6abce-dca0-4eef-8742-cd7de08cc019)

Once the database is created, we can create the table and load the data by choosing create table from data lake:

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/82d7e68e-abc4-4447-a302-84ced4e599de)

Similary we shall create the tables for all the datasets by inferring the column names from the dataset. Once created we will have our tables in Synapse Workspace as below:

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/3de49354-ffe0-44d8-90de-dcdfe6a90ca0)



Once the tables are created we can use SQL to perform the analytics on the data and answer few questions like

#calculate total number of athletes for each country.
#Avg number of entries by Gender for each discipline
#Total number of coaches for each discipline
#Total Number of events per country
#Percentage of Participants won the medals and many more.






<img width="635" alt="image" src="https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/866616e5-1343-425f-a241-380ea10cc225">



## **Generating a report in PowerBI**:

Next i created interactive dashboards using various charts and visualizations provided by PowerBI.


![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/77af5976-c313-4d7d-979d-fe9bade851e6)



![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/57acc276-b2a7-4aeb-b683-16e1b47241cf)



# **Conclusion**


This project showcases an entire data pipeline built using prominent Microsoft Azure and Databricks services. The focus is to demonstrate an end-to-end process of ingesting raw data, performing transformations, and creating visualizations using PowerBI.

Throughout this entire journey, we gain insights from the Olympics data and present a high-level analytical overview. This pipeline can be used as a template for other similar large scale data engineering projects. Happy coding!




