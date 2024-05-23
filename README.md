Tokyo Olympics Data Analysis: Microsoft Azure End-to-End Pipeline Project


This repository provides a comprehensive guide on how to build and implement an end-to-end data engineering pipeline for the analysis of Tokyo Olympics data. This project takes advantage of several Microsoft Azure services, including Azure Data Lake Gen2, Azure Data Factory (ADF), Azure Synapse Analytics, Databricks, and Power BI.
Power BI link
Databricks Notebook link

Project Overview
The goal of this project is to develop an end-to-end data pipeline to ingest, transform, analyze, and visualize historic data from the Tokyo Olympics. Initially, data is ingested into an Azure Data Lake Gen2 storage account using Azure Data Factory pipelines. Following ingestion, the data is processed and curated using Databricks Notebooks. Azure Synapse Analytics is then deployed to create a SQL database from the curated data, which is subsequently integrated into PowerBI for sophisticated analytics and visualizations.

Project Architecture
The architecture of this data pipeline has been designed to be robust and scalable. It consists of the following Azure services:

Azure Databricks for data transformation.
Azure Data Factory for data ingestion.
Azure Storage (Data Lake Gen2) for data storage.
Azure Synapse Analytics for creating a SQL database.
Power BI for data visualization.

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/9d4cb2e6-81d1-47dc-aab6-eee71adb5333)

Technologies and Services Used

The project utilizes the following technologies and services from Microsoft Azure:

Azure Databricks: A Fast, easy, and collaborative Apache Spark–based analytics service.
Azure Data Factory: A fully managed, serverless data integration solution for ingesting, preparing, and transforming data.
Azure Storage (Data Lake Gen2): A set of capabilities dedicated to big data analytics, built into Azure Blob storage.
Azure Synapse Analytics: An integrated analytics service for data integration, enterprise data warehousing, and big data analytics.
Power BI: A business analytics tool that delivers insights to enable fast, informed decisions.


Getting Started
To implement this project, ensure you have an active Azure subscription and have access to the services defined above and a Databricks Workspace.

Data Ingestion Process
We employ ADF pipelines to ingest data into our Azure Data Lake Gen2 object storage.To do this we would first have to create a LinkedService to connect AFD to our Storage Account. Linked-service-placeholder

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/028f41e0-60a4-4f63-b8be-17027c46530a)


Once we have established a connection we can create an Ingestion Pipeline on Azure Data Factory to pull the data from online sources and save it to our storage container.Our online data source is primarily raw files available under the data tab on GitHub.

![image](https://github.com/Reshmarathod/Azure-Olympics-Analysis-Data-Engineering-End-to-End-Project1/assets/146751905/1e508014-4e6e-4e5d-858c-97041b101b97)

Data Transformation Process
After the successful ingestion of data into the Data Lake, we deploy Azure Databricks notebooks. These notebooks attach to our storage account and perform the necessary data transformations. This process involves creating an app under the App Registration Service and providing it with access control to the storage account.

Creating the app:


