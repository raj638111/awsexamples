
Is called `Auto Scaling Group`

# Goal

1. `Scale out` to match increased load
2. `Scale in` to match decreased load

# Features

1. Can be set parameter to have min & max no of instances running
2. Any EC2 instances part of ASG is automatically registered to load balancer
3. Can recreate new EC2 instance if another instance is deemed unhealthy
4. ASG is free
5. ASG can also terminated EC2 instace if it is deemed unhealthy by LB
6. Also integrates with `Cloud watch alarm` which can be used for scaling out/in (Based on average CPU, custom metric etc...)

# Attributes

1. Use `Launch Template`
   1. AMI + Instance type
   2. EC2 user data
   3. EBS Volumes
   4. Load balancer information
   5. etc...
2. Min, Max, Initial size
3. Scaling policies

# Debug

1. Instance being in unhealthy without getting created
   1. Can be either SG issue or EC2 user data script issue

# Scaling Policies

1. Available policies
   1. `Dynamic Scaling`
      1. `Target tracking scaling`
         1. Example
            1. I want average ASG CPU to stay around 40%
            2. I want average no of EC2 connections to be around 1000
      2. `Simple / Step scaling`
         1. When cloud watch alarm is trigger (ex CPU > 70): Add 2 units
         2. With step scaling policy, we can make bigger or smaller change to the size of the group based on step adjustment policy we specify
   2. `Scheduled scaling`
      1. Anticipate a scaling based on a known usage patterns
      2. Is machine learning driven
   3. `Predictive Scaling`
      1. Continously forecast load and schedule scaling ahead (Great for patterns that repeat)
2. Scaling cool down
   1. After a scaling activity, provides cooldown period (default: `300 seconds`)
   2. ASG will not launch or terminate instances during this time

# Good metrics to scale

1. CPU Utilization
2. Request count per target (Makes sure no of requests to EC2 is stable)
3. Average network in / out (Useful if our application is n/w bound)
4. Any custom metric in cloudwatch

