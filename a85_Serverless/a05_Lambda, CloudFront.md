

# Lambda 101

1. What is Lambda?
   1. Are virtual functions
2. Advantage
   1. Runs on-demand
   2. Scaling is automated
   3. Integrate to many AWS services
      1. Can be invoked by
         1. API Gateway
         2. Kinesis
         3. DynamoDB
            1. Creates trigger through Lambda
         4. S3
         5. CloudFront
         6. CloudWatch events (or) EventBridge
            1. Example: Code pipeline stage changes
         7. CloudWatch logs
            1. To stream the logs to where ever we want
         8. SNS
         9. SQS
         10. Cognito
             1. To react whenever a user logs into database
         11. ...
   4. Supports many programming languages
      1. Node.js
      2. Python
      3. Java
      4. C#
      5. Ruby
      6. Custom Runtime API (Community supported: Ex: Rust or Golang)
      7. Lambda Container Image
         1. Must implement Lambda runtime API
         2. Prefer to run container image in ECS / Fargate
   5. Monitoring through CloudWatch
   6. Easy to get more resource / function (Upto 10GB of RAM)
   7. Use case
      1. Serverless Cronjob
         1. Create CloudWatch Events (or) EventBridge rule that triggered every one hour and starts Lambda
            
3. Limitations
   1. Limits are per region
   2. Execution limit
      1. Memory: 128MB - 10GB 
      2. Max time: 900 seconds (15 min)
      3. Env variables: 4KB
      4. Disk capacity: /tmp: 512MB to 10GB
      5. Concurrent executions: 1000 (Can be increased)
   3. Deployment limit
      1. Max compressed size: 50MB
      2. Max Uncompressed size: 250MB
         1. Can use /tmp directory to laod other files during startup
      3. Environment variables: 4KB
4. Cost
   1. Pay per request and compute time
   2. Free tier
      1. 1,000,000 lambda requests
      2. 400,000 GBs of compute time
         1. ie
            1. 400,000 seconds if function has 1GB RAM
            2. 3,200,000 if function has 128MB RAM
   3. After free tier (Payment calculated in terms of milliseconds)
      1. 0.20$ per 1 million requests
      2. 1$ for 600,000 GB-seconds
         

# Lambda SnapStart

1. **Why needed?**
   1. Improves performance upto 10x at no extra cost for Java11 and above
2. **Typical Lambda phases**
   1. Init 
      1. With SnapStart, function is invoked from pre-initialized state (ie no function initialization from scratch)
      2. Note: Init phase can take long time in Java
   2. Invoke
   3. Shutdown
3. **Lambda SnapStart process**
   1. Lambda initializes the function
   2. Takes a snapshot of memory & disk state of the intialized function
   3. Snapshot is cached for low latency access

# CloudFront: Lambda@Edge and CloudFront functions

1. Use case
   1. Customize CDN content at the edge
   2. Website security & privacy
   3. Search Engine Optimization (SEO)
   4. Intelligently Route across origin and data centers
   5. Bot mitigation at the edge
   6. A/B testing
   7. User authentication & authorization
   8. User prioritization
   9. User tracking and analytics
2. How this is done?
   1. Code is attached to `CloudFront` distributions
   2. Code is written in `JavaScript`
3. Two types of functions are provided by CloudFront
   1. **CloudFront**
      1. Client -Viewer-Request-> CloudFront -Origin-Request-> Origin -Origin-Response-> CloudFront -Viewer-Response -> Client
      2. CloudFront functions modify `ViewerRequest` & `ViewerResponse`
      3. Features
         1. Sub-ms startup times
         2. Millions of requests/second
         3. Memory: 2MB
         4. Package size: 10KB
   2. **Lambda@Edge**
      1. Written in Nodejs or Python
      2. Thousands of requests/second
      3. 5-10 seconds runtime
      4. Memory: 128MB to 10GB
      5. Package size: 10 to 50MB
      6. Can change: ViewerRequest, OriginRequest, ViewerResponse, OriginResponse
      7. Region
         1. Authored in one region
            1. CloudFront then replicates to its locations
4. NOTE:
   1. More differenc b/w CloudFront & Lambda@Edge can be found in the tutorial
   

# Lambda in VPC

1. **Default**
   1. Lambda function is launched in AWS owned VPC
      1. So, cannot access resource in our VPC (ie RDC, Elasticache, internal ELB etc...)
2. **How to launch Lambda in our own VPC?**
   1. Specify VPC ID, Subnets & the Security Groups
   2. Lambda will create an ENI (Elastic Network Interface) in our subnets
   3. ^ This makes Lambda to access resource in our VPC
3. **Use cases**
   1. Access RDS database in private subnet using RDS Proxy (which pools the connections)
      1. VPC[ Lambda functions -> Private Subnet [RDS Proxy -> RDS DB Instance] ]
      2. Benefit
         1. Connection pooling
         2. Improves failover by 66% and preserving connections
         3. Authentication & storing credentials in Secrets Manager
      3. Note: RDS Proxy is never publicly accessible


# Invoke Lambda from RDS & Aurora

1. How does this help?
   1. Allows us to process data events within database by invoking Lambda
2. Use case
   1. Once new user is registered in RDS, lambda can be used to send email notification to the user

# RDS Event notifications

1. How does this differ from `RDS Lambda invocation`?
   1. RDS Event notification only provides info about database (and not data)
      1. Example: Created, Stopped, Start etc...
   2. Available RDS event notifications
      1. DB Instance
      2. DB Snapshot
      3. DB Parameter
      4. DB Security Group
      5. RDS Proxy
      6. Custom Engine Version
   3. Near real time events: Up to 5 minutes)
   4. Send notification to SNS( -> (SQS, Lambda) )
   5. Subscribe to events using EventBridge (-> Lambda)

