# Spotify ETL Pipeline Project

A comprehensive ETL (Extract, Transform, Load) pipeline for processing Spotify music data using Azure Data Lake Storage Gen2, Azure Data Factory, and Databricks, implementing the Medallion Architecture.

## Table of Contents
- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Medallion Architecture](#medallion-architecture)
- [Setup and Configuration](#setup-and-configuration)
- [Key Components](#key-components)
- [Visualizations](#visualizations)
- [Data Model](#data-model)
- [Future Enhancements](#future-enhancements)

## Project Overview

This project implements a robust data pipeline that extracts Spotify music data, processes it through multiple layers of transformation, and produces analytical insights. The pipeline follows modern data engineering best practices, including the Medallion Architecture (Bronze, Silver, Gold layers) to ensure data quality, reliability, and usability.

## Architecture

Spotify ETL Architecture

The project utilizes the following technologies:
- **Azure Data Factory (ADF)**: For orchestrating the data ingestion process
- **Azure Data Lake Storage Gen2 (ADLS Gen2)**: For securely storing data at different stages of processing
- **Databricks**: For processing and transforming data using Apache Spark
- **Delta Lake**: For ensuring ACID transactions and reliable data storage
- **OAuth Authentication**: For secure access to Azure resources

## Medallion Architecture

The project implements the classic Medallion Architecture with three layers:

### 1. Bronze Layer
- Raw data ingestion from source systems
- Minimal to no transformations
- Data preservation in its original form
- Provides a foundation for reprocessing if needed

### 2. Silver Layer
- Data validation and quality checks
- Schema enforcement and data cleaning
- Transformation of raw data into a more structured format
- Enrichment with additional metadata and calculations

### 3. Gold Layer
- Business-level aggregations and transformations
- Creation of dimensional models (Star Schema)
- Generation of KPIs and analytical datasets
- Optimized for analytical queries and reporting

## Setup and Configuration

### Prerequisites
- Azure subscription with access to:
  - Azure Data Factory
  - Azure Data Lake Storage Gen2
  - Azure Databricks
- Databricks workspace
- Service Principal with appropriate permissions

### Storage Mounting

The project uses OAuth authentication to securely mount ADLS Gen2 containers to Databricks:

```python
container = "raw"
storage_account = "etlprojectspotify"
mount_name = "raw"

result = dbutils.notebook.run("/Workspace/Users/suman.kr.ghorai@gmail.com/Spotify-ETL-Databricks/utils/mount_utils", 60, {
    "container": container,
    "storage_account": storage_account,
    "mount_name": mount_name
})
```

## Key Components

### 1. Data Ingestion
- Uses Azure Data Factory to extract data from Spotify APIs
- Stores raw data in the ADLS Gen2 "raw" container

### 2. Data Processing
- Bronze Layer: Stores raw data without transformation
- Silver Layer: Validates, transforms, and enriches data
- Gold Layer: Creates dimensional models and aggregates

### 3. Dimensional Model
The Gold layer implements a Star Schema with:
- Dimension tables:
  - dim_track
  - dim_artist
  - dim_album
  - dim_date
- Fact tables:
  - fact_track_performance

### 4. KPI Tables
- kpi_top_10_tracks: Tracks ranked in the top 10 by country and date
- kpi_avg_popularity_by_artist: Average popularity scores for artists
- kpi_explicit_track_counts: Count of explicit vs clean tracks by country
- kpi_energy_distribution: Distribution of tracks by energy level categories

## Visualizations

The project includes several visualizations using Databricks' native visualization tools, matplotlib, seaborn, and Plotly:

1. **Top 10 Tracks by Country and Date**
   - Track popularity trends over time
   
2. **Artist Popularity Analysis**
   - Average popularity scores for top artists
   
3. **Explicit vs Clean Track Distribution**
   - Proportion of explicit content across different countries

4. **Audio Feature Analysis**
   - Correlation between audio features like energy, danceability, valence
   - Distribution of tracks by key audio characteristics

5. **Track Performance Metrics**
   - Relationship between track age and popularity
   - Popularity distribution by country

6. **Music Mood Analysis**
   - Distribution of tracks by valence category (emotional tone)
   - Mode distribution (Major vs Minor keys)

## Data Model

### Star Schema Design
The Gold layer implements a dimensional model with:

```
fact_track_performance
├── track_key (FK → dim_track)
├── artist_key (FK → dim_artist)
├── album_key (FK → dim_album)
├── date_key (FK → dim_date)
├── country
├── popularity
└── various metrics
```

### Key Metrics
- Track popularity
- Audio features (energy, danceability, etc.)
- Track age and its relationship to popularity
- Geographic popularity differences

## Future Enhancements

Potential improvements for the project:
1. Real-time data processing using Structured Streaming
2. Machine learning models for popularity prediction
3. Integration with additional music data sources
4. Automated data quality monitoring
5. Advanced visualization dashboards using Power BI or Tableau

---

Created by Suman Kumar Ghorai