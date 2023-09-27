# DynamoDB
- Managed multi-AZ NoSQL data store with cross-region replication option
- Defaults to eventual consistency reads (BASE model)
    - Can request strongly consistent reads via SDK parameter (ACID model)
        - Returns the absolute latest update regardless if it has propogated in other AZs
        - Read replicas can fail if:
            - The AZ hosting the latest updates fail
            - Network delays occur between the latest and read replicas
    - Usually takes about a second for written data to propagate

- Priced on throughput rather than compute
    - Can provision read and write capacity in anticipation of need

- Autoscale capacity adjusts per configured min/max level
    - Scaling up steps:
        1. This works by tracking the table for consistent elevated stream of requests, either read/write
        2. This can then trigger a CloudWatch alarm
        3. CloudWatch alarm triggers a scale-up for the read/write capacity of the table
    - Scaling down:
        - DynamoDB does not automatically scale back down
        - Documentation has guides to scale the capacity down by putting synthetic transactions to taper off the load

- On-Demand capacity for flexible capacity at a small premium cost
    - Best to use if we don't know the read/write capcity needed
    - Scales to the size needed automatically
        - Cost effective if read/write capacity is provisioned in advance vs on-demand capacity

- Can achieve ACID compliance with DynamoDB Transactions
    - SDK has a method to put and get records that implement ACID compliance
    - Opens DynamoDB to new use cases and is not restricted to a RDBMS[^1] system

## Partition Key vs Sort Key
**Example JSON:**
```JSON
{
    "salesOrderNum": "34234324", // Primary Key + Local Secondary Index
    "timestamp": "2018-06-11T20:13:47Z", // Sort Key
    "salesOrder" : {
        "salesOrderType": "schedule agreement",
        "salesOrderLine": [
            {
                "lineItem": "1",
                "material": {
                    "materialNumber": "HYDJF234", // Local Secondary Index
                    "materailDescription": "Flange, 8cm, Iron",
                }
            }
        ],
        "customer": {
            "customerNum": "345535", // Global Secondary Index
            "customeName": "ABC Company",
            "customerAddress1": " 564 Main Street"
        }
    }
}
```
### Primary Key
- Each record requires a primary key that is unique to identify a record
- To access the record in the example above, you are required to know the `salesOrderNum`
    - DynamoDB uses this primary key to create an internal hash[^2]
    - DynamoDB uses this hash to decide which partition, or underlying physical storage, should be used to store the value

### Sort Key
- A composite key is comprised of a partition key and sort key
- We can have occurances of the same partition Key as long as the sort key is different
- The same hash is done to the primary key, but records are stored in the order of the sort key order
    - Useful if you need to pull out subsets of data based on some sort key

**Example:**
In the case that a sales order was never updated, but a new version was created each time, querying for the `salesOrderNum` would give back all versions of that record.
By passing in and querying by the max `timestamp` of the order, we would be given a single record, indicating the latest order.

## Secondary Indexes
|         Index Type         |                             Description                             |                             How to Remember                              |                                                             When to use                                                              |                                                  Example                                                   |
| :------------------------: | :-----------------------------------------------------------------: | :----------------------------------------------------------------------: | :----------------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------: |
| Global Secondary Key Index | Partition key and sort key can be different from those on the table |   Not restricted to just the partitioning set forth by the partion key   | When you want a fast query of attributes outside the primary key without having to do a table scan (reading everything sequentially) |             "I'd like to query Sales Orders by Customer number rather than Sales Order Number              |
|   Local Secondary Index    |       Same partition key as the table but different sort key        | Key must respect the table's partition key, but can be whatever sort key |                       When you already know the partition key and want to quciky query on some other attribute                       | "I have the Sales Order Number, but I'd like to retrieve only those records with a certain Material Number |

- There is a limit to the number of indexes and attribtues per index
- Indexes take up storage space

| If you need...                                   | Consider...                                                                      | Cost                                         | Benefit                                                |
| ------------------------------------------------ | -------------------------------------------------------------------------------- | -------------------------------------------- | ------------------------------------------------------ |
| access just a few attributes as fast as possible | Projecting just those few attributes in a global secondary index                 | Minimal                                      | Lowest possible latency access for non-key items       |
| frequently access some non-key attributes        | Projecting those attributes in a global secondary index                          | Moderate; aims to offset cost of table scans | Lowest possible latency access for non-key items       |
| frequently access most non-key attribtues        | Projecting those attributes or even the entire table in a global secondary index | Up to Double                                 | Maximum flexibility                                    |
| rarely query but write or update frequently      | Projecting keys only for the global secondary index                              | Minimal                                      | Very fast write or updates for non-partition-key items |

## Attribute Projections
- When creating an index, users must select which attribute will be projected onto that index
- Using the same example above, you can imagine a secondary index as a view
    - Projects are a subset of attributes to be a part of the secondary index (no more than 20)

**Example:**
We create a global secondary index with customer number as a key, and project `customerName`, `customerAddress1`, `salesOrderNum`, and `timestamp`

## Design Best Practicess
```JSON
{
    "CustomerId": "123",
    "SortKey": "Details",
    "FirstName": "Pumpkin",
    "SurName": "Escobar",
    "Contact": "pumpkin.escobar@gmail.com"
},
{
    "CustomerId": "123",
    "SortKey": "Purchases-2019-03",
    "Period": "2019-03",
    "TotalPurchases": "45000.00",
    "Currency": "USD"
},
{
    "CustomerId": "123",
    "SortKey": "order-45670",
    "IsOpen": "True",
    "TotalValue": "2300.00",
    "Currency": "USD"
}
```

- The JSON above shares the same partition key, but has different values for the sort key
- SortKey is the same attribute, but has a different value

**Examples:**
- If we wanted to see total purchases for all customers, we could define a global secondary index with `period` as the primary key and `TotalPurchases` as an attribute. This allows quick aggregations based on date
- Using sparse indexes, not every record has an attribute of `period`. A neat trick of DynamoDB is that these indexes only exist for those items that have this attribute. This incurs fewer reads/writes
- Global secondary indexes can create table replicas by using the same partition and sort key as the original table, as well as one or more attributes
    - Use cases for this example:
        - We have two tiers of customers: Premium and Free-Tier
            - If we want to make sure that premium customers don't run into performance issues, we can allow them to use a table that has a higher RCU/WCU limit
            - For free-tier customers, we can provision a global secondary index and put a lower RCU/WCU limit on these indexes
        - Performance reasons:
            - We want high write capacity onto a table to stream data in quickly and a replica table with a high read capacity limit
            - This would prevent doing analysis on the same table that is being continuously written into

> [!NOTE]
> DynamoDB replicas are *eventually* consistent, meaning there is a delay between the source table and the replica table. In most cases this is ~1 second

[^1]: Short-hand for relational database management systems
[^2]: Internal hash: Mapping a data value of arbitrary size to one of fixed size
