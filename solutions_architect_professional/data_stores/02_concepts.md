# Concepts

## Persistent Data Store
- Data is durable and sticks around after reboots, restarts, or power cycles
    - Examples:
        - Glacier
        - RDS

## Transiet Data Store
- Data is just temporarily stored and passed along to another process or persistent store
    - Examples:
        - SQS
        - SNS

## Ephemeral Data Store
- Data is lost when stopped
    - Examples:
        - EC2 Instance Store
        - Memcached

## IOPS vs. Throughput
- "Consistency is far better than rare moments of greatness"
- Input/Output Operations per Second (IOPS)
    - Measure of how fast we can read and write to a device
- Throughput
    - Measure of how much dat a can be moved at a time

## Consistency Models - ACID & BASE
- ACID - Relational Databases
    - Atomic: Transactions are "all or nothing"
    - Consistent: Transactions must be valid
    - Isolated: Transactions cannot mess with one another
    - Durable: Completed transactions must stick around

- BASE
    - Basic Availability: Values data availability even if data is old
    - Soft-State: May not be instantly consistent across other data stores
    - Eventual Consistency: Will achieve consistency at some point

- ACID models are accurate and precise, but do not scale well
    - Row locking slows down the database when busy
    - This is because it wants to be consistent all the time, but at scale, it breaks
- Base is easily scalable