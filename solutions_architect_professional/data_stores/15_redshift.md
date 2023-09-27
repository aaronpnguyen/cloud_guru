# Redshift
- Fully managed, clustered peta-byte scale data warehouse
- Extremely cost-effective as compared to some other on-premises data warehouse platforms
    - Compared to Teradata and Netezza
- PostgreSQL compatibability with JDBC and ODBC drivers is available
    - Compatible with most BI tools out of the box
- Features parallel processing and columnar data stores which are optimized for complex queries and data analytics
- Option to query directly from data files on S3 via Redshift Spectrum

## Data Lake
- Data lakes are simply a large repository for a variety of data, that uses a framework or technology to make use of the data
- Query raw data without extensive pre-processing
- Lessen time latency from data collection to data value
- Identify corelation between disparate data sets

## Use Case
- We can dump large data sets into S3 such as:
    - Transaction logs
    - Sensor readings
    - Social media stream
    - Weather data
- Then we can use analytic tools such as Excel or Quicksight to point to Redshift Spectrum to query the S3 data
