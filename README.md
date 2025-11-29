# :gear: Earthquake Azure Data Pipeline

## Overview and Architecture

### Business Case

Earthquake data is incredibly valuable for understanding seismic events and mitigating risks. Government agencies, research institutions, and insurance companies rely on up-to-date information to plan emergency responses and assess risks. With this automated pipeline, we ensure these stakeholders get the latest data in a way that’s easy to understand and ready to use, saving time and improving decision-making.

### Architecture Overview

Creating a scalable data pipeline in Azure, transforming raw data into meaningful insights using Databricks, Azure Data Factory (ADF), and Synapse Analytics.

![Data Engineering vs Software Engineering (6)](https://github.com/user-attachments/assets/bdadd2e0-89be-4683-b53b-fe331be6f6bf)

This pipeline follows a modular architecture, integrating Azure’s powerful data engineering tools to ensure scalability, reliability, and efficiency. The architecture includes:

1. **Data Ingestion**: Azure Data Factory orchestrates the daily ingestion of earthquake data from the USGS Earthquake API.
2. **Data Processing**: Databricks processes raw data into structured formats (bronze, silver, gold tiers).
3. **Data Storage**: Azure Data Lake Storage serves as the backbone for storing and managing data at different stages.
4. **Data Analysis**: Synapse Analytics enables querying and aggregating data for reporting.
5. **Optional Visualization**: Power BI can be used to create interactive dashboards for stakeholders.

### Data Modeling

We implement a **medallion architecture** to structure and organize data effectively:

1. **Bronze Layer**: Raw data ingested directly from the API, stored in Parquet format for future reprocessing if needed.
2. **Silver Layer**: Cleaned and normalized data, removing duplicates and handling missing values, ensuring it’s ready for analytics.
3. **Gold Layer**: Aggregated and enriched data tailored to specific business needs, such as adding in country codes.

### Understanding the API

- The earthquake API provides detailed seismic event data for a specified start and end date.
- **Start Date**: Defines the range of data. This is dynamically set via Azure Data Factory for daily ingestion.
- **API URL**: `https://earthquake.usgs.gov/fdsnws/event/1/`

### Integrating Azure Synapse Analytics
1. **Create a Synapse Workspace**:
   - Link it to the existing Storage Account.
   - Configure a file system and assign necessary permissions.
2. **Query Data Using Serverless SQL**:
   - Use `OPENROWSET` to query Parquet files stored in `bronze`, `silver`, and `gold` containers.
   - Example query:
     ```sql
     SELECT
         country_code,
         COUNT(CASE WHEN LOWER(sig_class) = 'low' THEN 1 END) AS low_count,
         COUNT(CASE WHEN LOWER(sig_class) IN ('medium', 'moderate') THEN 1 END) AS medium_count,
         COUNT(CASE WHEN LOWER(sig_class) = 'high' THEN 1 END) AS high_count
     FROM
         OPENROWSET(
             BULK 'https://<storage_account>.dfs.core.windows.net/gold/earthquake_events_gold/**',
             FORMAT = 'PARQUET'
         ) AS [result]
     GROUP BY
         country_code;

---

### Key Benefits

- **Automation**: Eliminates manual data fetching and processing, reducing operational overhead.
- **Scalability**: Handles large volumes of data seamlessly using Azure services.
- **Actionable Insights**: Provides stakeholders with ready-to-use data for informed decision-making.


## **Technologies Used**
- Azure Databricks
- Azure Storage Account
- Azure Data Factory
- Azure Synapse Analytics

## Key Considerations
- **Linked Services**: Ensure reusable and secure connections between Azure services.
- **Scalability**: Use Synapse for querying large datasets efficiently.
- **Data Engineering Focus**: Maintain an emphasis on structured pipelines and optimized workflows.

This guide provides a comprehensive approach to setting up a professional-grade Azure Databricks and Synapse workflow for data engineering.

---

## 🏳️ License
This project is licensed under the [MIT License](LICENSE). You are free to use, modify, and share this project with proper attribution.

## 🔖 About Me 
Hi there! I'm **Houssem Gharbi**, i'm a Junior Data Engineer passionate about building scalable data solutions and deriving actionable insights. I enjoy transforming raw data into meaningful stories that drive decision-making. This project reflects my skills and interest in creating efficient data pipelines and analytics platforms.

