

1. **Features**
   1. Unlimited throughput
   2. Unlimited no of messages
   3. Low latency (< 10 ms)
   4. 
2. **Attributes**
   1. Default retention
      1. 4 days, max of 14 days
   2. Msg size limit
      1. 256KB per message
   3. Consumer side
      1. Max message at a time
         1. 10
   4. Message visibility timeout
      1. Default: 30 sec
         1. Can be modified using API: `ChangeMessageVisibility`
   5. Long polling
      1. Adv: Reduces latency & too many API calls
      2. Time out can be set from `1 to 20 sec`
      3. Can be enabled at queue or API level (`waitTimeSeconds`)
3. **Limitations**
   1. Duplicates are possible (ie atleast once delivery)
   2. Out of order messages possible
4. Use cases
   1. Can be used with ASG (Based on queue length cloudwatch metric)
      1. queue length -> Alarm -> ASG

# Security

1. In flight using HTTPS API
2. At rest using KMS keys

# Policies

1. IAM Policy to regulate API
2. SQS Access policies
   1. For cross account access
   2. To provide access for other services (Ex: SNS)


# FIFO Queue

1. Advantage
   1. Exactly once capability
   2. Also has content based deduplicaton
2. Features
   1. Parallelism is based on Group ID
      1. If all data has only one group ID (or not group ID), then we can have only one consumer
      2. Example: Two group IDs means we can use two consumer
3. Limitation
   1. Throughput
      1. 300 msg / sec (without batching)
      2. 3000 msg / sec (with batching)
