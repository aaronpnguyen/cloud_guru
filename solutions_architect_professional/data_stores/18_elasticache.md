# Elasticache

- Fully managed implementations of two popular in-memory data stores
    - Redis
    - Memcached
- Push-button scalability for memory, writes, and reads
- In memory key/value store - not persistent in the traditional sense and is not tied by disk I/O
- Is not a persistent data data like Dynamo or RDS, but is faster
- Billed by node size and hours of usage

|            Use            |                                                                                 Benefit                                                                                  |
| :-----------------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|      Web Session Use      | In cases with load-balanced web servers, store web session information in Redis so if a server is lost, the session info is not lost and another web server can pick-up. |
|     Database Caching      |                          Use Memcache in front of AWS RDS to cache popular queries to offload work from RDS and return results faster to users.                          |
|       Leaderboards        |                                                Use Redis to provide a live leaderboard for millions of users of your app.                                                |
| Streaming Data Dashboards |                           Provide a landing spot for streaming sensor data on the factory floor, providing live real-time dashboard displays.                            |

## Use Memcached if you...
- Need to scale in and out as demand changes
- Need to run multiple CPU cores and threads
- Need to cache objects (i.e. like database queries)
- Think of simple, no-frills, straight forward work horse

## Use Redis if you...
- Need encryption
- Need HIPAA compliance
- Need support for clustering
- Need complex data types
- Need high-availability (replication)
    - Has persistence options
- Pub/sub capability
- Geospacial indexing
- Backup and restore
- Think of Redis as a feature-rich option

> [!NOTE]
> A cache is a cache that is meant to hold temporary data. It is not meant to do the job of RDS or S3, which stores persistent data.
