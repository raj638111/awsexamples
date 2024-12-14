

# EKS: Features

1. Supports two launch modes
   1. Worker nodes
   2. Fargate mode
      1. To deploy serverless containers
2. Supported Node types
   1. **Managed Node Groups**
      1. AWS manages nodes (EC2 instances) for us
      2. Nodes are part of ASG managed by EKS
      3. Support for on-demand and spot instances
   2. **Self Managed Nodes**
      1. Nodes are created by us & registered to EKS (and managed by ASG)
      2. Can use `Amazon EKS Optimized AMI` or `Custom AMI`
      3. Supports on-demand & sport instances
   3. **AWS Fargate**
      1. No maintenance required & No nodes managed

# Architecture

1. AWS Cloud
   1. VPC
      1. Multiple AZ
         1. Public subnet
            1. Can contain Public ELB + NGW (NAT Gateway)
         2. Private subnet
            1. EKS Nodes [1...N]
               1. EKS Pods [1...N]
               2. Can be managed by ASG
            2. Can also contain Private ELB

# Data Volume support

1. Steps
   1. Specify `StorageClass` manifest for EKS cluster
   2. Leverage `Container Storage Interface` (CSI) compliant driver
2. Supports
   1. Amazon EBS
   2. Amazon EFS (Works with Fargate)
   3. Amazon FSx for Lustre
   4. Amazon FSx for NetApp ONTAP