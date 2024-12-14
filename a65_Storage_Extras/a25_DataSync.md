

# DataSync

1. Can be used to move data
2. On premise / other cloud -> AWS (NFS, SMB, HDFS)
   1. Needs an agent on premise to create a connection
3. AWS -> AWS (Example: Different storage)
   1. No agent required
4. Can sync to
   1. S3, EFS, FSx
5. Sync tasks are scheduled
   1. Can be hourly, daily, weekly
6. File permissions and meta data are preserved 
   1. ie Compliant with NFS Posix, SMB file transfer protocols)
7. Speed
   1. One agent task can use 10Gbps (Limit can also be set to not overload n/w)
8. Overall flow
   1. On premise (NFS or SMB server) -> On Premise (AWS datasync agent) -TLS-> AWS (AWS Datasync service) -> AWS (S3 / Glacier / EFS / FSx / ...)
9. Sync works in both ways
10. What is there is no n/w bandwidth available?
    1. Use snow cone device in onpremise (Comes with datasync agent installed)
    2. (Note: snowcone device is shipped to AWS)



