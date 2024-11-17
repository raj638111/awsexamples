

1. Used to control EC2 placement strategy
2. Three options are available
   1. **Cluster**
      1. Cluster instances into low latency group in a single AZ
      2. Great n/w ing (Low latency)
      3. Bad: Single AZ (Usage: Big data jobs)
   2. **Spread**
      1. Used to minimize failure
      2.Spread across different hardware (max 7 instance / AZ) and AZ (For critical applications)
   3. **Partition**
      1. Similar to Spread
      2. Spread instances across partitions
      3. Partition: Represents a rack in AZ
      4. Upto 7 partitions per AZ
      5. Two partitions do not share the same rack
      6. Scales to 100s of instances per group (Hadoop, Cassandra)
      7. EC2 instance: Get access to partition metadata
      8. Partition failure do not affect instances in other partition