

Is a hybrid cloud for storage
1. Portion of infra can be kept in cloud & the other in on-premise
2. Is a bridge to expose cloud data (S3, Glacier etc...) as on-premise data

# Use cases

1. Disaster recovery
   1. Backup on-premise data into cloud
2. Backup & restore
3. Tiered storage (Cloud has cold data & on-premise warm data)
4. On premise cache & low-latency file access

# Available types

1. **S3 File Gateway**
   1. Can be used to connect S3 bucket (Not Glacier) to on-premise applicaiton server using standard n/w file system
   2. Uses `NFS` or `SMB` protocol
      1. `SMB`: Has integration with `Active Directory` for authentication
   3. Uses `HTTPS` request
   4. Most recently used data is cached for rapid access
   5. Files can be archived into Glacier using life cycle policy
   6. Steps
      1. Bucket access using IAM roles for each gateway
2. **FSx File Gateway**
   1. Provides native access to `Amazon FSx for Windows File Server`
   2. Although files can be accessed from on-premise without gateway, we get some additional features
      1. Local Cache for frequently accessed data
      2. Windows native compatibility (SMB, NTFS, Active Directory)
      
3. **Volume Gateway**
   1. Provide block storage using `iSCSI protocol` backed by S3
      1. Backed by EBS snapshots to help restore on-premise volumes
   2. Two types are available
      1. **Cached volumes**: Provides Cached Volumes for low latency access
      2. **Stored volumes**: Dataset is on-premise with scheduled backups on S3
4. **Tape Gateway**
   1. Used to store tape based backup in on-premise into AWS in S3 and Glacier
   2. Uses `Virtual tape library(VTL)` backed by S3 & Glacier
   3. Flow
      1. Backup server -iSCSI-> Tape -> Gateway -HTTPS-> S3 -> Glacier (Archives)

# Storage Gateway - Hardware Appliance

1. Is a hardware appliance provided by AWS
2. Can be used when,
   1. We do not have on-premise virtualization (ie VMWare ESXi, Amazon EC2, Linux KVM, ...)
   