

---

## EBS (Elastic Block store) Volume

1. Is a n/w drive
2. Allows to persist data
3. Bound to AZ
4. Can be attached to one instance at a time
5. Free tier: 30 GB / per month
6. Advantage
   1. Can be detached & attached to another instance
   2. Capacity can be increased
7. Limit
   1. AZ Bound (But can be done through snapshot)
8. Configuration
   1. Size in GB
   2. IOPS (IO Operations / second)
9. Default
   1. Root volume is deleted with EC2 instance termination
   2. Other volumes are not deleted

## EBS Snapshots

1. Is a backup at any point in time of EBS volume
2. Features
   1. Snapshot archive: Is 75% cheaper
      1. Takes 24 to 72 hours to restore
   2. Recycle bin: Used to recover snapshot if needed
      1. Retention: 1 day to 1 year
   3. Fast snapshot restore
      1. Full init of snapshot to have no latency on first use
3. Recommended
   1. Do not run snapshot backup when application is using EBS (as backups also use IO) 
   2. Not necessary to detach, but recommended
4. Advantage
   1. Can copy snapshot across AZ or Region

## Volume types

1. **Has 6 types**
   1. **gp2 / gp3 (SSD):** 
      1. General purpose
      2. Low latency
      3. Upto 16 TB
      4. gp2
         1. upto burst 3000 IOPS,
      5. gp3
         1. 3000/16,000 IOPS and throughput 123MiB/s / 1000MiB/s
   2. **io1 / io2-Block express (SSD) (Provisoned IOPS / PIOPS)**
      1. High througput
      2. Used for sustained performance (More than 16000 IOPS)
      3. Use case: Database workloads
      4. io1
         1. 4 - 16TB
         2. 64000 IOPS max
      5. io2
         1. 4 - 64TB
         2. 256,000 IOPS max
         3. IOPS: GB > 1000:1
      6. Supports EBS multi attach feature
   3. **st1**
      1. Low cost HDD, frequent access
      2. 125 GB to 16TB
      3. Use case: Big data, log processing
      4. 500 MB/s max t.put / 500-max-IOPS
   4. **sc1**
      1. Low cost HDD, less frequent access
      2. 250 MB/s max t.put / 250-max-IOPS
2. **Limitations**
   1. Boot volume
      1. Only gps, io can be used


## EBS multi attach

1. io1 / io2
   1. Can be attached to multiple instance in same AZ
   2. Each instance will have full read / write permission
   3. Use case
      1. High application availability
      2. Application doing concurrent writes
   4. Limitation
      1. Upto 16 instances at a time
      2. Must use filesystem that is cluster aware (Not XFS, EXT4)

## EBS encryption

1. Store / flight are all encrypted
2. Minimal impact on latency
3. Uses keys from KMS (Key management store)
4. Copying an unencrypted snapshot allows encryption
5. Snapshots of encrypted volumes are encrypted
6. Encrypt an unencrypted EBS volume
   1. Create snapshot
   2. Encrypt using copy
   3. Create new EBS volume from snapshot

----

# EC2 instance store

1. Why? Instead of EBS (which is n/w drive), high performance hardware disk can be used (through Instance store)
2. Advantage
   1. Better IO performance (as this is not a n/w drive)
3. Bad
   1. Is ephemeral: ie storage will be lost of EC2 stop
   2. Risk of data loss if hardware fails
   3. Backup and replication is our responsibility
4. Use case
   1. Buffer, Cache

----

# EFS (Elastic File System)

Is a managed network file system

## Features
   1. Can be mounted on multiple EC2 instances
   2. Works with EC2 instances in multiple AZ
   3. Highly available, scalable (no capacity planning needed), pay per use, expensive
   4. Encrytion at rest using KMS
   5. **Scale**
      1. 1000s of NFS client
      2. 10 GB/s throughput
      3. Can grow to petabyte scale
   6. **Performance mode**
      1. General purpose (Less latency)
      2. Max I/O (More latency, but high throughput, highly parallel)
         1. Useful for big data app
   7. **Throughput mode**
      1. Bursting
         1. 1TB = 50MB/S, Burst = 100 MB/s
      2. Enhanced
         1. Provisioned
            1. Set your throughput regardless of storage size (ex: 1 GB/S for 1TB storage)
         2. Elastic
            1. Automatically scale throughput up/down based on workload
            2. Use case: Unpredictable workloads
   8. **Storage classes**
      1. Storage tiers (Data can be moved to tiers using life cycle policies)
         1. Standard
         2. Infrequent access (EFS-IA)
         3. Archive
   9. **Availability & Durability**
      1. Standard: Multi-AZ, great for prod
      2. One Zone: One AZ. Great for dev
## Terms
   1. Mount target
      1. Provides end point to mount EFS
      2. Recommended: One mount target per AZ
      
## Use case
   1. Content management
   2. Web serving
## Uses 
   1. NFS Protocol
## Limitation
   1. Only compatible with Linux AMI

----

# EBS vs EFS

1. EBS Volume
   1. Attached to one instance (except multi attach io1 / io2)
   2. Locked to an AZ
   3. gp2: IO increase with disk size
   4. gp3 and io1: IO can increase independently
   5. 