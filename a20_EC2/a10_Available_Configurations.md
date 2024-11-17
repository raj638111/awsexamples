
# Available configurations

1. **Available OS**
   1. Linux, windows, Mac OS
2. **CPU**
3. **RAM**
4. **Storage**
   1. Network attached (EBS & EFS)
   2. Hardware (EC2 instance store)
5. **Network card**
   1. Card speed
   2. Public IP Address
6. **Firewall rules (`Security Group`)**
7. **Bootstrap script (`EC2 User data` script)**  
   1. Is used to configure EC2 instance during startup
   2. Runs with root user
8. **Instance types**
   1. Example: `t2.micro`

# Instance state
 
1. `Stop` instance 
   1. `Stopped` instance can be restarted
   2. Stopped instance also loses its public IP (Unless we assign a DNS name to the instance) upon restart
   3. `Private IP` remains the same for restarted instance
2. `Terminate` instance
   1. `Terminated` instance cannot be restarted and also loses the volume attached?
  