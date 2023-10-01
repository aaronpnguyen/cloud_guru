# Database Options

## Athena
- SQL Engine overlaid on S3 based on Presto
- Can query raw data objects as they sit in an S3 Bucket, like JSON or CSV
- Data converted into Parquet format can increase performance
- Similar to Redshift Spectrum but...
    - Athena is used for data that mostly lives in S3 without the need to perform joins with other data sources
    - If you need to join S3 data with existing RedShift tables or create union products, using Redshift Spectrum would be a better option

## Quantum Ledger Database
- Based on blockchain concepts
- Provides an immutable and transparent journal as a service without having to setup and maintain an entire blokchain framework
- Centralized design (as opposed to decentralized consensus-based design for common blockchain frameworks) allows for higher perforamnce and scalability
    - Downsides of a decentralized framework is that it takes some time for the transactions to replicate amongst other nodes
    - Centralized frameworks usually have pretty good performance
- Append-only concept where each record contributes to the integrity of the chain
- Ledger databases should be immutable, records in the chain cannot be overwritten

> [!NOTE]
> Explaining the append-only concept: A record produces a hash, which is a mathematical fingerprint of the data in that record.
> The following record, will incorporate the hash from the previous record, to create its own hash.
> Simply put, the hash of the previous record, is used to build the hash for the next record.
> If someone in the future were to change a previous record, the hashes would no longer be accurate and this effect would cascade to records following the current record being changed.

## Managed Blockchain
- Fully managed blockchain framework supporting open source frameworks of:
    - Hyperledger Fabric
    - Ethereum
- Distributed consensus-based concept consisting of a network, members (other AWS accounts), nodes (instances), and potentially external applications
- Uses QLDB ordering service to maintain complete history of all transactions to ensure the transactions are immutable

## Timestream Database
- Fully managed database service specifically built for storing and analyzing time-series data
- Alternative to DynamoDB or Redshift and includes some built-in analytics like interpolation and smoothing
- Use cases:
    - Indsutrial machinery
    - Equipment telemetry
    - Sensor networks

## DocumentDB
- Has MongoDB compatability
- AWS' invention that emulated the MongoDB API so it acts like MongoDB to existing clients and drivers
- Fully managed with:
    - Multi-AZ, high availability
    - Scalability per instance
    - Integrated with KMS
    - Backed up to S3
- An option if you currently use MongoDB and want to get out of the server management business without needing to refactor codebases

## OpenSearch
- ElasticSearch has been renamed to OpenSearch because the service now supports OpenSearch 1.0
- It is NOT ElastiCache
- Mostly a search engine but kind of a document store (caution)
    - Indexes the documents that it serves up and stores them as documents in the form of a JSON
    - Is not an alternative to other document databases, but more useful as an analytics tool
- OpenSearch service components are sometimes referred to as an ELK stack and ES on the exam

- OpenSearch has multiple layers:
    - Kibana (Visualization dashboard software for ElasticSearch/OpenSearch)
    - Intake: LogStash, CloudWatch, FireHose, IoT (GreenGrass)
    - Search and storage: OpenSearch
