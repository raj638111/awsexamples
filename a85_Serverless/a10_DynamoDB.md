

# DynamoDB: Features

1. Available with replication across multiple AZ
2. NoSQL with transaction support
3. Scales to
   1. Millions of requests / sec
   2. 100s of TB of storage
4. **Can rapidly evolve schemas**
5. Fast & consistent performance
   1. Single digit millisecond
6. Integrates with IAM
7. Autoscaling capabilities
8. Available table class
   1. Standard
   2. Infrequent Access (IA)

# 101

1. Table
   1. Has primary key (Partition key + Sort key (Optional))
   2. Can have infinite no of items (Rows)
      1. Each item has attributes(ie fields) (Can be added over time)
      2. Max item size: 400KB
   3. Available data types
      1. Scalar
         1. String, Number, Binary, Boolean, Null
      2. Document Types
         1. List, Map
      3. Set types
         1. String set, Number set, Binary set
2. Read / Write Capacity Modes
   1. **Provisioned Mode (Default)**
      1. Capacity specified in advance (ie read/writes / sec)
      2. Cost
         1. Pay per provisioned `Read Capacity Units (RCU)` & `Write Capacity Units (WCU)`
      3. Scaling
         1. Can add auto scaling mode for `RCU` and `WCU`
   2. **On Demand Mode**
      1. Read/Write scales with workload
         1. ie, No capacity planning needed
      2. Cost
         1. Pay per use


# DynamoDB Accelerator (DAX)

1. Is a fully available in-memory cache for DynamoDB
2. Features
   1. Microsecond latency
   2. No code change required (ie Compatible with existing DynamoDB APIs)
   3. Cache TTL default: 5 minutes
3. How to enable this?
   1. We need to create a DAX cluster
4. Request flow from application
   1. application -> DAX Cluster -> DynamoDB
5. Use case
   1. DAX
      1. Individual object cache
      2. Query and scan cache
   2. ElastiCache
      1. Store aggregation results (ie very big computation results)

# Stream Processing

1. Features
   1. Provides ordered stream of `item-level` modifications (Create/Update/Delete) in a table
2. Use case
   1. Welcome email to new user
   2. Realtime usage analytics
   3. Implement cross-region replication
   4. Insert into derivative table
   5. Invoke Lambda on any change made to the table
3. Two available stream types
   1. **DynamoDB Streams**
      1. 24 hour retention
      2. Limited no of consumers
      3. Process using Lambda triggers or DynamoDB Stream Kinesis Adapter(KCL Adapter)
   2. **Kinesis Data Streams**
      1. 1 year retention
      2. High no of consumers
      3. Process using Lambda, Kinesis Data Analytics, Kinesis Data Firehose, AWS Glue streaming ETL
   

# Global tables

1. Features
   1. Is a table replicated across **multiple regions**
   2. Provides two way replication b/w same table in different regions
   3. Active-Active replication (Applicaiton can read & write from the table in any region) 
2. Use case
   1. Provides low-latency in multiple regions
3. Pre-requisite
   1. Need to enable DynamoDB Streams

# TTL

1. Features
   1. Automatically delete items after an expiry timestamp
2. Use case
   1. Delete old items
   2. Web session handling

# Backups for disaster recovery

1. PITR (Point in time recovery)
   1. Provides continous backup using PITR
   2. Optional: can be enabled for last 35 days
   3. Note: Recovery creates a new table
2. On-Demand backups
   1. Full backups for long terms retention, until explicitly deleted
   2. Do not affect performance or latency
   3. Can be configured and manged in `AWS Backup` service (enables cross-region copy)
   4. Recovery creates a new table

# Integrations

1. S3
   1. Export to S3
      1. Export to S3 (Need PITR enabled)
      2. Point in time until PITR max (ie 35 days)
      3. Does not affect read capacity
      4. Can retain snapshots for auditing
      5. Data format
         1. DynamoDB JSON (or)
         2. ION format
   2. Import to S3
      1. Can import
         1. CSV, DynamoDB JSON or ION
      2. Does not consume any write capacity
      3. Creates a new table
      4. Import erros are logged in `CloudWatch` logs

