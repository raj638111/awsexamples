

Used for stream processing

# Available services

1. **Kinesis Data Streams**
   1. Capture, process & store data streams
2. **Kinesis Data Firehose**
   1. Load streams into AWS or external stores
3. **Kinesis Data Analytics**
   1. Analyze data streams with `SQL` or `Flink`
4. **Kinesis video streams**
   1. Capture, process and store video streams

# Kinesis Data Streams (KDS)

### Features

   1. Producers
      1. Client, AWS SDK, KPL (Kinesis producer library), Kinesis agent
      2. Record
         1. Contains: partition key + Data Blob (upto 1MB) 
   2. Consumers
      1. Apps (KCL (Kinesis client library), SDK), Lambda, Kinesis Data Firehose, Kinesis Data Analytics
      2. Record
         1. Contains: Partition key + Sequence no + Data blob
   3. Realtime & Replay capability
      1. ~200 ms
   4. Data ordering
      1. Is maintained at shard level
   5. Sharding
      1. Need to provision during stream creation
      2. Numbered 1 to N
      3. Producer limit for a shard
         1. 1 MB /sec and 1000 msg / sec
      4. Consumer limit for a shard
         1. 2MB / sec shared for all consumers in a shard (`Shared mode`) (Is pull mode)
         2. 2MB / sec per shard per consumer (`Enhanced mode`) (Is push mode: Data is pushed to consumers)
   6. Retention
      1. 1 to 365 days
   7. Immutable
      1. Data cannot be deleted
   8. Capacity modes
      1. Provisioned mode
         1. In: 1MB/sec per shard
         2. Out: 2MB / sec per shard (Classic or enhanced fan out consumer)
         3. Cost: Pay per hour per shard
      2. On-demand mode
         1. No need to provision or manage capacity
         2. Default capacity
            1. In: 4MB/sec and 4000 records / sec
         3. Scaling
            1. Automatic scaling based on througput peak in the last 30 days
         4. Cost
            1. Pay per stream per hour & data in/out per GB
   9. Security
      1. IAM Policy based access/ authorization
      2. In flight encryption using HTTPS endpoints
      3. Encryption at rest using KMS
      4. VPC endpoints
         1. Available to access kinesis within VPC (Example: EC2 -> Kinesis in private subnet)
      5. Monitoring
         1. All API calls can be monitored using CloudTrail
      
### Commands

```
# get the AWS CLI version
aws --version

# PRODUCER

# CLI v2
aws kinesis put-record --stream-name test --partition-key user1 --data "user signup" --cli-binary-format raw-in-base64-out

# CONSUMER 

# describe the stream
aws kinesis describe-stream --stream-name test

# Consume some data
aws kinesis get-shard-iterator --stream-name test --shard-id shardId-000000000000 --shard-iterator-type TRIM_HORIZON

TRIM_HORIZON: Start reading from the beginning
```


# Kinesis Data Firehose

- Used to ingest data in batches
- Fully manager, serverless & automated scaling
- Near real time
   - Buffer interval of 0 to 900 seconds
      - Buffer size: 1 MB to 128MB (Write to destination happens after this criteria is met)
      
### Features

1. Source
   1. Application, Client, SDK, Kinesis Agent, KDS, Cloudwatch, AWS IoT
2. Destination
   1. AWS
      1. S3, Redshift (s3 -copy-> Redshift), OpenSearch
   2. Thirdparty
      1. Datdog, Splunk, New Relic, MongoDB, ...
   3. Custom Destination
      1. HTTP Endpoint
   4. Note:
      1. Also provides option to store failed data (or) all data to s3 bucket as backup
3. Record
   1. Upto 1MB
4. Data transformation integration
   1. Can be done using lambda
5. Cost
   1. Pay for data going through Firehose
6. Data
   1. Supports coversions, transformations & compression


   






