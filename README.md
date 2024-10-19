# Seattle-Airbnb-Data

This project focuses on analyzing the feasibility and profitability of establishing an Airbnb accommodation in Seattle, Washington. The analysis aims to determine whether such an endeavor would be economically advantageous and align with strategic business objectives.

---

## Objective

To conduct a comprehensive analysis of the Seattle Airbnb market to assess the economic feasibility and profitability of hosting Airbnb properties in the region.

---

## Dataset

The dataset used for this project consists of:
- `calendar.csv`
- `listings.csv`
- `reviews.csv`

These files provide key insights into Airbnb bookings, property listings, and user reviews, which are critical for the analysis.

---

## Steps to Execute the Project

### Step 1: Uploading Dataset to Azure Storage Account

1. Created a new container named `airbnb-data` in the **Azure Storage Account**.
2. Uploaded the following CSV files to the container path `airbnb-data/Data/`:
   - `calendar.csv`
   - `listings.csv`
   - `reviews.csv`

---

### Step 2: Creating External Data Source and External Tables in Azure Synapse Studio

To analyze the dataset in **Power BI**, external tables were created in **Azure Synapse Studio** using the following steps.

#### 1. Creating an External Data Source

```sql
CREATE EXTERNAL DATA SOURCE airbnb_data_source
WITH (
    LOCATION = 'abfss://airbnb-data@newworkspace.dfs.core.windows.net/Data'
);

IF NOT EXISTS (SELECT * FROM sys.external_file_formats WHERE name = 'csv_file_format')
CREATE EXTERNAL FILE FORMAT csv_file_format
WITH (
    FORMAT_TYPE = DELIMITEDTEXT,
    FORMAT_OPTIONS (
        FIELD_TERMINATOR = ',',
        STRING_DELIMITER = '"',
        FirstRow = 2,
        USE_TYPE_DEFAULT = FALSE,
        Encoding = 'UTF8',
        PARSER_VERSION = '2.0'
    )
);

CREATE SCHEMA airbnb;
GO
Calendar Table
CREATE EXTERNAL TABLE airbnb.calendar (
    location_id SMALLINT,
    borough VARCHAR(15),
    zone VARCHAR(50),
    service_zone VARCHAR(15)
)
WITH (
    LOCATION = 'Data/calendar.csv',
    DATA_SOURCE = airbnb_data_source,
    FILE_FORMAT = csv_file_format,
    REJECT_VALUE = 10,
    REJECTED_ROW_LOCATION = 'rejections/taxi_zone'
);
Listings Table
CREATE EXTERNAL TABLE airbnb.listings (
    location_id SMALLINT,
    borough VARCHAR(15),
    zone VARCHAR(50),
    service_zone VARCHAR(15)
)
WITH (
    LOCATION = 'Data/listings.csv',
    DATA_SOURCE = airbnb_data_source,
    FILE_FORMAT = csv_file_format,
    REJECT_VALUE = 10,
    REJECTED_ROW_LOCATION = 'rejections/taxi_zone'
);
Reviews Table
CREATE EXTERNAL TABLE airbnb.reviews (
    location_id SMALLINT,
    borough VARCHAR(15),
    zone VARCHAR(50),
    service_zone VARCHAR(15)
)
WITH (
    LOCATION = 'Data/reviews.csv',
    DATA_SOURCE = airbnb_data_source,
    FILE_FORMAT = csv_file_format,
    REJECT_VALUE = 10,
    REJECTED_ROW_LOCATION = 'rejections/taxi_zone'
);
```
### Step 3: Connecting Dataset to Power BI
Connecting Power BI to Azure Synapse Studio
After creating the external tables in Synapse Studio, I established a connection between Power BI and the Azure Synapse Analytics service.

Loading the Dataset into Power BI
The datasets (calendar.csv, listings.csv, and reviews.csv) were successfully loaded into Power BI for further analysis and visualization.

### Step 4: Power BI Dashboard
In Power BI, I created a dashboard to visualize the key insights from the Seattle Airbnb data. The dashboard highlights various metrics such as:

Booking trends from the calendar.csv file.
Property listings by neighborhood from the listings.csv file.
Review sentiment analysis from the reviews.csv file.

## Conclusion
This project provides a detailed analysis of the Airbnb market in Seattle using Azure Synapse and Power BI. By uploading the dataset to Azure Storage, creating external tables in Synapse Studio, and visualizing the data in Power BI, the project effectively demonstrates the feasibility and profitability of operating Airbnb accommodations in Seattle.
