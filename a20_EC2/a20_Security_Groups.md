
# Security Groups

1. Is used to control traffic in and out of EC2 instances
2. Is used to regulate
   1. Access to ports
   2. Authorized IP ranges
   3. Control Inbound network
   4. Control Outbound network
3. Can be attached to multiple instances
4. Locked down to a region + VPC combination
5. Contains only allow rules
6. EC2 instance can have many security groups attached
7. Rules can reference by 
   1. IP or 
   2. Security Group
      1. **Example**
         1. Ins.1 has SG1 attached whose Inbound rule refers SG2
         2. Ins.2 has SG2 attached
         3. So now, Ins.1 can take inbound request from Ins.2
      2. Is common to use SG reference in Load balancers       
8. Recommendation
   1. Maintain one security group for SSH access
9. Troubleshoot
   1. `Timeout`: Is a security group issue
   2. `Connection refused`: Might be the application (in EC2) issue
10. Defaults
    1. All inbound traffic is blocked by default
    2. All outbound traffic is authorised by default
11. Classic Ports
    1. 22 - SSH
    2. 21 - FTP
    3. 22 - SFT 
    4. 80 - HTTP
    5. 443 - HTTPS
    6. 3389 - RDP (Remote Desktop Protocol - log into windows instance)

# How to connect to EC2 instance

## SSH

```shell
ssh -i file.pem ec2-user@<ip>
```

## EC2 instance connect

1. Is browser based SSH
2. No need to manage SSH key
   1. A temp key will be created and used
3. 