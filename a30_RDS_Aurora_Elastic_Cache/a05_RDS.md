

# Available DB

1. Postgres
2. MySQL
3. MariaDB
4. Oracle
5. Microsoft SQL server
6. IBM DB2
7. Aurora

# Advantage of RDS

1. Is managed service
   1. Automated provisioning, OS Patching
   2. Monitoring dashboards
   3. Read replicas
   4. Storage backed by EBS
   5. Multi AZ setup for disaster recovery
2. Disadvantage
   1. No access to EC2 instance through SSH

# Features

1. **Storage Autoscaling** 
   1. Helps to increase storage automatically
   2. Set using max-threshold
   3. Automatically increase
      1. If free storage is less then 5%
      2. If low storage lasts for 5 minutes
      3. 6 hours have passed since last modification
   4. Useful for apps with unpredictable laods
   5. Supports all db engines
2. **Read replicas**
   1. Allowes up to 15 read replicas
   2. Can be within same AZ, Cross AZ or Cross Region
   3. Read replicas are **ASYNC** replication, so eventually consistent
   4. Replicas can be promoted to their own DB
   5. Application should update connection string to read from read replicas
   6. Note: Read replicas is only for reads (ie SELECT)
   7. Network cost:
      1. There is no n/w cost if the replicas are within the same region
      2. Cross region replicas, there will be a n/w cost
3. **RDS Multi AZ (Disaster recovery / Standby DB)**
   1. Is done through **SYNC** replication
   2. Application talks to one DNS and in case of failure of the master, app failovers to `standby` database
   3. This increase availability
   4. Failure happens when: lost of AZ, loss of n/w, instance or storage failure
   5. No manual intervention to the app
   6. Not used for scaling
   7. Single AZ to Multi AZ Conversion
      1. Zero down time to move from single AZ to MultiAZ
      2. Process
         1. Snapshot taken of master
         2. Restored as a new standby db
         3. Sync enabled to standby

# RDS Custom

1. Is managed Oracle & Microsoft SQL server
2. Provides OS & Database customization
3. Provides ability to,
   1. Configures settings
   2. Install patches
   3. Enable native features
   4. Access underlying ec2 instance using SSH or SSM Session manager
4. To perform customization,
   1. Deactive automation mode, so not automation, scaling etc... is performed

# Aurora

1. Proprietary from AWS
2. Is MySQL & Postgres compatible
3. Claims to be 5X performance improvement over MySQL on RDS, Over 3x improvement on Postgres on RDS
4. Has `Shared storage volume` which grows in increment of 10GB upto 125 TB
5. Can have 15 read replicas
6. Failover is instantenous. Its HA native
7. Cost 20% more than RDS
8. High availability & Read scaling
   1. 6 Copies of data across 3 AZ
   2. 4 Copies out of 6 needed for writes
   3. 3 Copies out of 6 needed for reads
   4. Self healing with peer-peer replication
   5. Storage is striped across 100s of volumes
   6. Failover for master is less than 30 seconds
   7. Any read replica can become master vis-a-vis Other RDS based db, where we need standby 
   8. Read replicas support cross region replication
9. Endpoints
   1. Writer endpoint
      1. Points to the master
      2. If if the master fails, the failover will get this endpoint
   2. Reader endpoint
      1. One single point, which will connect to all read replicas in a transparent way
      2. (Note: Load balancing happens at connection level and not at statement level)
10. `Backtrack`
    1. Restore data at any point of time without using backups
11. Aurora instance type
    1. Serverless V2: Provides instant scaling
12. Other Features
    1. Provide reader auto scaling
    2. `Custom endpoints`
       1. Can define a custom endpoint on large (which might provide better performance for analytical queries) instance read replicas (Generally we will avoid reader endpoint at this time, and use c.endpoints for different set of workloads)
    3. `Aurora serverless`
       1. Automated db instantiation & autoscaling based on usage
       2. Use case: Good for unpredictable workloads
       3. Is pay per second
    4. `Global Aurora`
       1. **Cross region read replicas**
          1. Has cross region read replicas
          2. Good for disaster recovery
       2. **Aurora Global db**
          1. 1 Primary region (For both read / write)
          2. Upto 5 secondary region (Replication lag is less then 1 second)
          3. Upto 16 read replica / secondary region
          4. Recovery time objective (To promote a secondary region as primary) of < 1 min
    5. `Aurora Machine learning`
       1. Enables ML based predictions to application via SQL
       2. Secure integration b/w Aurora & AWS ML Services
       3. Supported Services
          1. Amazon Sage Maker (Use with any ML model)
          2. Amazon Comprehend (For sentiment analysis)
       4. Use cases: Fraud detection, ads targeting, sentiment analysis

# Backups

## RDS Backup

1. Automated Backup
   1. Daily full backup of db
   2. Transactional logs are backed by RDS every 5 minutes
   3. Ability restore any point in time (From oldest backup to 5 mins ago)
   4. Retention: 1 to 35 days (Set 0 to disable backup)
2. Manual DB Snapshots
   1. Benefits: Retention of backup as long as we want
   2. When to use? A stopped RDS still cost money for storage. Take a snapshot & restore instead (Costs way less)

## Aurora Backups

1. Automated backup
   1. Is 1 to 35 days (Cannot be disabled)
   2. Point in time recovery
2. Manual snapshots
   1. Retention of backup as long as we want

# Restore Options

1. Restoring RDS or Aurora creates a new db
2. Restore MySQL RDS from S3
   1. On Premise > S3 > Restore into RDS (MySQL)
3. Restore MySQL Aurora cluster from S3
   1. Backup on Premise (Create backup using `Percona Xtra backup`) > S3 > New Aurora Cluster running MySQL

# Aurora db cloning

1. Used to create a new cluster from existing one (Faster then Snapshot & restore)
2. Why faster?
   1. Uses `Copy-on-write` protocol
      1. Initially new DB uses same data volume as original DB
      2. When updates are made to new db cluster, additional storage is allocated and data is copied to be separated
3. Advantage
   1. Fast & Cost effective
4. Use case
   1. Useful to create staging db from prod

# RDS & Aurora Security

1. **At rest encryption**
   1. If master is not encrypted, read replica cannot be encrypted
   2. How to encrypt unencrypted db?
      1. Take snapshot and restore as unencrypted
2. **In flight encryption**
   1. TLS ready by default b/w client & db (Use the AWS TLS root certificates client side)
3. IAM Authentication
   1. Can also use IAM roles to connect (instead of username/pwd)
4. Security groups
   1. Can be used to control n/w access
5. No SSH available (Except for RDS custom)
6. Audit logs
   1. Can be enabled & sent to cloud watch
   2. Provides info about what queries are made on Aurora + Happenings in db
   3. How to keep audit log for long period of time (as those logs will be deleted in c.watch)?
      1. Send the log to `Cloudwatch log service` for retention


# RDS Proxy

1. Is fully managed, Serverless and highly available (ie Multi AZ)
2. Allows apps to pool & share connections established with db
   1. App > RDS Proxy > db
3. Advantage
   1. Reduce stress of resources (CPU, RAM)
   2. Minimize Open connections & timeouts
   3. Reduced failover time by upto  66% (for both RDS & Aurora)
4. Supports RDS & Aurora
5. No code change required for most apps
6. Can do **IAM based authentication** for clients & secrets can be stored in AWS Secret manager
7. Is not publicly accessible (ie not over internet) (Must be accessed from within VPC)
8. Super useful when used with Lambda functions (As they can share the connection pool from RDS proxy)

# Note
1. Read replica is asyc whereas MultiAZ is sync replication