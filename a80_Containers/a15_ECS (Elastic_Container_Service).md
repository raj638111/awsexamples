

# Launch types (Two types)

### ECS - EC2 

1. Launch
   1. Docker launch = ECS tasks on ECS Clusters
2. 101
   1. We must provision and maintain the infrastructure (ie EC2 instances)
   2. EC2 instance
      1. Contains
         1. ECS Agent
            1. Registers the EC2 instance into ECS Cluster
   3. Starting & Stopping containers
      1. AWS takes care of that
3. Auto Scaling EC2 instances
   1. Available Options
      1. **Auto Scaling Group Scaling**
         1. Scale ASG based on CPU utilization
         2. Add EC2 instances over time
      2. **ECS Cluster Capacity Provider**
         1. Used to auto provision & scale the infra for ECS tasks
         2. Is paired with ASG
         3. Adds EC2 instance when missing capacity

### ECS - Fargate

1. Advantage
   1. Serverless
      1. We do not provision the infrastructure
         1. ie: No EC2 instance to manage
2. How works?
   1. We just created task definitions
   2. ECS runs tasks based on our CPU / RAM need
3. Scaling
   1. Just increase the no of tasks


# IAM Roles for ECS

### EC2 launch type

1. EC2 instance profile
   1. Used by ECS agent in EC2 instance
      1. Make API calls to ECS Service
      2. Send container logs to cloudwatch
      3. Pull docker image from ECR
      4. Refer secrets in secret manager or SSM parameter store
2. ECS task role
   1. Used for both EC2 & Fargate launch type
   2. Allows each task to have specific role (Say for example: Write to S3, etc...)
   3. Defined in
      1. Task definition

# Load balancer integrations

1. Works for both EC2 & Fargate launch type
2. **ALB** (Application Load Balancer)
   1. Tasks need to be exposed as HTTP endpoints in order to add load balancer in front of the cluster
3. **NLB**
   1. Recommended for high throughput use case
   2. To use with `AWS Private link`


# Data persistence on ECS

Available Options
1. **EFS**
   1. Supports both EC2 and Fargate
   2. Can be mounted directly to ECS task
2. Note
   1. S3 **cannot** be mounted as a Filesystem on ECS task



# ECS Task & Service

1. 101
   1. Is the way through which we deploy apps in the cluster
   2. Task definition
      1. A service is created using task definition
2. Steps to launch a service
   1. Create task definition
   2. Launch task definition as a service
      1. Cluster > Services > Service > Desired tasks (1 to N) ... > Create


# ECS Service Auto Scaling

1. **101**
   1. Is a way to increase / decrease ECS tasks
2. Scaling can be done using,
   1. `AWS Application Auto Scaling`
      1. Autoscaling can be based on
         1. ECS Service average CPU utilization
         2. ECS Service average Memory utilization - Scale on RAM
         3. ALB request count per target
            1. Using metric from ALB
   2. **Target tracking**
      1. Scale based on target value of specific Cloudwatch metric
   3. **Step Scaling**
      1. Based on cloudwatch alarm
   4. **Scheduled Scaling**
      1. Based on specific date/time
3. Note
   1. ECS Service auto scaling (task level) != EC2 Auto scaling (EC2 instance level when in EC2 launch type)





# ECS Task invocation

Task can be created / scaled in multiple ways as described below,

1. **Invoked by `Event Bridge`**
   1. Invocation Steps
      1. User -Upload-> S3 bucket -all-events-> Event Bridge -> Executed rule to run ECS task -> ...
2. **Event Bridge Schedule**
   1. Event Bridge -event-one-hour-> Run ECS task -> ....
3. **Using ECS SQS Queue**
   1. -message-> 
      1. SQS Queue -> 
         1. Service (Also integrated with ECS service auto scaling) in ECS poll for message (more messages mean more tasks created)
4. **Intercept stopped tasks using EventBridge**
   1. task stopped/exited -event-> EventBridge -> SNS -email-> Administrator

# ECS Task cleanup

1. Steps to cleanup
   1. Delete/Stop the Service
      1. Make sure zero tasks are running
      2. Reflects in `CloudFormation`
   2. Delete the cluster
      1. Will again reflect in CloudFormation
2. Note
   1. Task definition does not cost






