


# Use case
   1. Once message -> Many receivers
   2. Is a pub-sub service


# Features

1. Subscription limit
   1. 12,500,000 / topic
2. Topic limit
   1. 100,000 topic / account
3. Subscribers can be
   1. Email
   2. Mobile notifications
   3. HTTPS endpoints
   4. SQS
   5. Lambda
   6. Kinesis Data Firehose
4. FIFO
   1. Similar to SQS FIFO (All messages in the same group are ordered)
5. Message filtering
   1. JSON policy to filter messages to subscriptions
   2. 
6. Producers can be
   1. Cloud watch alarms
   2. ASG
   3. Cloudformation state changes
   4. S3 Bucket
   5. AWS DMS
   6. RDS Events
7. Cross region Delivery
   1. Works with SQS queue in other regions
8. Two ways to publish
   1. Topic Publish (Using SDK)
   2. Direct publish (for mobile apps SDK)
      1. Create platform app
      2. Create platform endpoint
      3. Publish to platform end
      4. (Works with Google GCM, Apple APNS, Amazon ADM)

# Security

Same as SQS


# Policies

Similar to SQS
