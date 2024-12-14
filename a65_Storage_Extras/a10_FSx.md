

Is used to launch 3rd party high-performance filesystem as fully managed service




# FSx for Lustre

1. Is a parallel distributed fs for large scale computing (`HPC`)
2. Use case
   1. Video processing
   2. Financial modelling
   3. Scale: 100s GB/s, Millions of IOPS, sub-ms latencies
3. Other features
   1. Can read & write s3 as a fileystem
   2. Can be used from on-premise servers (**VPN** or **Direct Connect**)
4. Available deployment options
   1. **Scratch file system**
      1. Used for temp storage
      2. Data not replicated
      3. High burst (6x faster, 200MBps per TiB)
      4. Optional s3 repository
   2. **Persistent file system**
       1. Long term storage
       2. Data replicated within same AZ
       3. Optional s3 repository


# FSx for windows file server

1. Can be mounted on Linux EC2 instances also
2. Possible to 
   1. Group on premise windows file server with FSs windows file server
   2. Access from on premise infrastructure
   3. Can be configured with MultiAZ for high availability
3. Scale
   1. 10s of GB/s, Millions of IOPS, 100s PB of data
4. Backed up to S3 for disaster recover
5. Microsoft active directory integration
   
# FSx for NetApp ONTAP

1. Can be used to move workloads running on ONTAP or NAS to AWS
2. Is compatible with NFS, SMB & iSCSI protocol
3. More compatibility with OS & AWS services
   1. Linux, windows, MacOS, VMware cloud on AWS, ...
4. Storage shrinks or grows automatically
5. Provides replication, snapshots, low-cost, compression & data de-duplication
6. Point-in-time instantaneous cloning (Helpful for testing new workloads)


# FSx for OpenZFS (OpenZFS is a file system)

1. Compatible only with NFS protocol
2. Can be use to move workloads running on ZFS to AWS
3. Works with Many OS & AWS Services
4. Scale: Upto 1 million IOPS with < 0.5ms latency
5. Provides
   1. Snapshots, compression & low-cost
   2. Point-in-time instantaneous cloning

