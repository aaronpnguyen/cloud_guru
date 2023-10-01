# Database options

| Database        | Use if you need...                                                                                       |
| --------------- | ----------------------------------------------------------------------------------------------- |
| Database on EC2 | - Ultimate control over database <br/> - Other textPreferred DB not available under RDS         |
| RDS             | - Need traditional relational database for online transactional processing (OLTP, essentially orders for users) <br/> - Your data is well-formed and structured |
| DynamoDB        | - Key pair data or unpredictable data structure <br/> - In-memory performance with persistence  |
| Redshift        | - Massive amounts of data <br/> - Primarily online analytical processing (OLAP) workloads                                      |
| Neptune         | - Relationships between objects are a major portion of the data value                                     |
| Elasticache     | - Fast temporary storage for small amounts of data <br/> - Highly volatile data                 |
