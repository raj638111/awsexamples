
# ENI

1. Logical component in a VPC
2. Represent a **virtual network card**
3. Provide EC2 instances access to n/w (Is also used outside of EC2)
4. Attributes of ENI
   1. Primary private IPV4
   2. One or more secondary IPV4
   3. One Elastic IP per private IPV4
   4. One Public IPV4
   5. One or more Security group
   6. A MAC address
5. Can be created independently & attached to EC2 instance for failover
6. Bound to an AZ
7. ENI created manually will stay permanent, whereas those created with EC2 will terminate

More reading: https://aws.amazon.com/blogs/aws/new-elastic-network-interfaces-in-the-virtual-private-cloud/