# DocumentDB
- AWS-native document data storage service
- Stores NoSQL JSON document database server
- Compatible with MongoDB APIs
- Has AWS-managed auto scaling
- Supports write-then-read consistency
- For better performance, read replicas can be used, which have eventual consistency

## What is a Document?
- A document is a JSON blob
- Is designed to be retrieved all at once
- Can contain data that is a natural schema and is intuitively used in applications
- Documents are flexible and are similar to other NoSQL schemas
    - This allows developers the ability to add/remove fields without worrying about breaking a rigid schema

## High Availability and Reliability
- Architecture is similar to Aurora
    - Has one main instance and up to 15 read replicas
- Actual document data has redundant copies stored in each availability zone where an instance lives
- Main instsances handles all of the writing, and the replicas in addition to the main instance, can handle reading
- Has a similar fail-safe as Aurora
    - If a main instance fails, a read replica is promoted to the main instance
- Applications accessing the database to *read* data can do so through a reader endpoint that points to all read replicas
- Applications accessing the database to *write* data can do so through a writer endpoint that that points to the main instance
- Instance endpoints can be configured to target a certain subset of instances

## Exploring Use Cases
- If you need a NoSQL document database
- Can store user profiles in an intuitive format in a single document
- Can easily handle real-time big data, that can be deliver in documents
- If you need content management
    - Content management applications use document data and document data stores

## Things to Take Away
- Built to scale
    - Like Amazon urora, DocumentDB is built from the ground up to scal data storage and handle multi-AZ and multi-region read replicas
- Stores JSON document data
    - JSON document databases allow  for flexible and intuitive schemas, which allows for iterative and natural development
- MongoDB compatability
    - Watch for scenarios asking you to emulate MongoDB or migrate document databases
    - DocumentDB may be a great choice
