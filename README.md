# Supplier Quality and Performance

## Project Overview
Enterprise Manufacturers Ltd has been struggling to assess the quality of materials supplied by vendors and address the impact of defective materials on production processes. The absence of a centralized procurement system has made it challenging to evaluate supplier performance effectively.

To tackle these challenges, the program management team has prioritized centralizing and analyzing supplier quality data. A project has been initiated to leverage Power BI for this analysis. The goal is to identify problem areas and deliver actionable insights to improve supplier quality and minimize production downtime.

## Business Objectives
The primary objectives of this project are:

- To evaluate supplier performance by analyzing defect quantities and downtime across vendors and plant locations.
- To identify specific vendor-material combinations and vendor-plant relationships that contribute to poor performance.
- To uncover trends and patterns in supplier quality data to support data-driven decision-making.
- To provide recommendations for improving supplier quality and optimizing production processes.

## Business Problems and Questions
Enterprise Manufacturers Ltd is facing several challenges due to the lack of a centralized procurement system to monitor supplier quality and performance. These challenges include:

1. Inability to identify which vendors and plants are responsible for the greatest defect quantities.

2. Lack of insight into which vendors and plants are causing the most downtime.

3. Difficulty analyzing specific material-vendor combinations that consistently perform poorly.

4. Challenges in identifying underperforming vendor-plant combinations.

5. Limited ability to compare the performance of the same vendor-material combinations across different plants.

To address these challenges, the following key questions need to be answered:

- Which vendors/plants are causing the greatest defect quantity?
- Which vendors/plants are causing the greatest downtime?
- Is there a particular combination of material and vendor that performs poorly?
- Is there a particular combination of vendor and plant that performs poorly?
- How does the same vendor and material perform across different plants?

## Dataset Overview
### Dataset Details
The dataset provided for this analysis is in Excel format (.xlsx) and contains a single table with 9 columns and 5,226 rows. It provides essential data on supplier performance, capturing key aspects across various dimensions:

- Date: The date the data was recorded (e.g., 3/18/2018).
- Vendor: The name of the supplier (e.g., BrowseBug, TopicZoom).
- Plant Location: The location of the plant receiving the materials (e.g., Westside, Frazer).
- Category: The category of the materials (e.g., Mechanical, Logistics).
- Material Type: The type of material supplied (e.g., Glass, Raw Materials, Cartons).
- Defect Type: The classification of the defect (e.g., Impact, No-Impact, Rejected).
- Defect: A description of the defect (e.g., Bad seams, Wrong material).
- Total Defect Quantity: The number of defective materials recorded.
- Total Downtime (minutes): The total downtime caused by defective materials, measured in minutes.
  
### Dataset Download Link
To access the dataset, click [**here**](https://github.com/Rolakamin/Supplier-Quality-and-Performance/blob/main/supplier%20data.xlsx)

## Data Cleaning and Transformation
The dataset was imported into Power Query Editor in Power BI for cleaning and transformation. Upon inspection, the data was found to be clean and required no additional cleaning steps. The focus of the transformation phase was to prepare the data for analysis by building a data model through data normalization.

### Data Normalization
Data normalization was necessary to improve data integrity and reduce redundancy. The process involved splitting the single table into smaller, related tables categorized as fact and dimension tables. This was appropriate since the dataset primarily consisted of categorical fields (e.g., Vendor, Plant Location, Category, Material Type), two quantitative fields (Total Defect Quantity and Total Downtime) and a date field.

### Steps in the Data Transformation Process

1. **Duplicating the Original Table**
Each dimension table was created by duplicating the original table. This allowed the isolation of specific fields needed for dimensional analysis.

2. **Creating Dimension Tables**
For each dimension table:
- Unnecessary columns were removed, leaving only the relevant field for that dimension.
- Duplicate rows were removed to ensure unique entries.
- An Index Column was added to serve as the Primary Key for the dimension table

3. **Creating the Fact Table**
The original table served as the foundation for the fact table.

To construct the fact table:

- Each dimension table was merged with the original table using the Merge Queries function, applying an Inner Join on the related columns.
- The primary keys from the dimension tables were brought into the fact table as foreign keys.
  
The resulting fact table included:
- Numerical data (metrics): Total Defect Quantity and Total Downtime.
- Foreign keys: Linking the fact table to the respective dimension tables.
- Date column: Retained in the fact table to enable time-based analysis.
This structure ensures the fact table is optimized for analytical purposes, with clear relationships to the dimension tables.

The final data model consisted of:

**Dimension Tables**: Containing unique values for each categorical field, along with their primary keys.

**Fact Table**: Including the date field, metrics (e.g., Total Defect Quantity, Total Downtime), and foreign keys to establish connections with the dimension tables.

Below is a visual representation of the final fact table created:

![Fact Table Screenshot](https://github.com/Rolakamin/Supplier-Quality-and-Performance/blob/main/fact_table.png)

### Data Modeling 
After loading the cleaned and transformed data back into Power BI, a date table was created using the following DAX formula:

Date = ADDCOLUMNS(

    CALENDARAUTO(),
    
    "Year", YEAR([Date]),
    
    "Quarter", QUARTER([Date]),
    
    "Month", MONTH([Date]),
    
    "Month Name", FORMAT([Date], "mmmm"),
    
    "Month Name Short", FORMAT([Date], "mmm"),
    "Day", DAY([Date]),
    "Day of the Week", WEEKDAY([Date]),
    "Day of Week Name", FORMAT([Date], "dddd"),
    "Week of Year", WEEKNUM([Date]),
    "Is Weekend", IF(WEEKDAY([Date]) IN {1, 7}, TRUE, FALSE)
)
