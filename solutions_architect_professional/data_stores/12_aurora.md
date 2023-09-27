# Aurora
- Most of the questions used to revolve around RDS
- Aurora is built ontop of RDS
- RDS is a starting point for relational data solutions
- Aurora has leveled up
    - Fully managed RDS service for:
        - MySQL
        - PostgreSQL
    - Aurora will does not support other databases
- Has multi-az availability by design
    - Helps with performance & availability
- Automatically scales and is managed by AWS
    - Storage is automatically scaled
    - Instances themselves that handle the compute and networking is automatically scaled
- Easy multi-region replication (relatively new)

## Read Replicas
- When provisioning Aurora, you can provision multiple instances
    - Can span multiple availability zones
    - These instances handle compute and networking aspects of Aurora

- You can have up to one main instance and 15 read replicas
    - Data is held on data stores distributed across these availability zones
    - Independent from the instances that you provision
    - Can scale independently based on the data being stored

- Main instance handles all write traffic
    - Replication is extremely fast
    - Writes to all data copies
        - Available after 100ms of being written
    - All instances are able to support read traffic after being written to

- Natively handles high-availability
    - Read replica can be automatically promoted in case the main instances goes down
    - Do not need to worry about updating the endpoint to change the the instance target
        - This is done behind the scenes by Aurora

**Example**: In an example with three different Aurora instances A (Main), B (Replica), C (Replica).
There is a cluster endpoint that allows write traffic only to the Main cluster, and a read endpoint that allows read access to the replicas.
If instance A were to go down, B (or C) would automatically promote to become the Main instance and the endpoint would automatically point
towards the newly promoted main instance. When instance A eventually comes back online, it will take the place of the promoted instance
and become a read replica. An elastic load balancer would automatically point/distribute traffic evenly between the new and old instances.


## Global Databases
- In the main region, Aurora allows for a single main instance as well as 15 read replicas
    - The read replicas are available in different availability zones
- Read replicas can be created in different regions
    - These replicas are asynchronously copied and may be a few seconds behind replicas within the main region
- Has one primary region and up to five secondary regions
- Data replicates from primary to secondary regions with low latency
- Leverages storage-level replication[^1] for transferring data
- Secondary region can be promoted in case of an outage

> [!NOTE]
> When thinking of Aurora as a global database, it has high availability and performant disaster recovery

## Amazon Aurora Serverless
- Aurora Standard
    - Used for very predictable or consistent workloads

- Aurora Serverless
    - Used for highly variable workloads
    - Allows instances to scale seamlessly without impacting performance
    - Min/max computer resources (ACU[^2]) can be configured for a cluster

**Example:**
By configuring min/max ACU for a cluster, Aurora can closely match the compute power that is needed for the current demand.
Aurora will usually take a conservative step-wise back off cycle once traffic has been reduced.

## Exploring Use Cases
- Used for most MySQL and PostgreSQL needs
- Need high priority, availability, and resistance to failure in a database (handled by AWS)
- Dealing with inconsistent demands/workloads
- Low operational overhead
    - Setup in Aurora can be much faster than RDS or EC2 based databases

## Things to takeaway
- Designed for high availability
    - Stores data redundantly within and across availability zonesa
    - Failover and data storage is handled automatically (this is done by AWS)

- Read replicas allow for low latency
    - Read replicas can be provisioned across availability zones via ELB to distribute read traffic
    - Global databases can be used to span across regions

- Use serverless for variable workloads
    - Aurora allows users the ability to provision instances that automatically scale memory, CPU, and network capacity
    - Both main and read replicas can be configured to be serverless instances
    - When loads decrease, Aurora takes a conservative step=wise back off cycle

[^1]: Storage-Level Replication - Replicates underlying block storage and not the whole instance
[^2]: Aurora Capacity Units (ACU) - Equates to ~2 GiB of memory, CPU, and networking. The min/max ACU configuration for a cluster is 0.50 to 128 (both in GiB)
