
# 101

1. What it does?
   1. Global Accelerator uses `Anycast IP` & users AWS internal network to route request to our application
   2. `Two` Anycast IPs are created for our application
2. Why needed?
   1. To avoid latency
3. Request path
   1. User request -> Closes Edge location -> internal n/w -> Server
4. Works with
   1. Elastic IP, EC2 instances, ALB, NLB
   2. Can be public or private
5. Additional features
   1. Health checks
      1. Performs health checks on our application
         1. Failover is < 1 min
   2. Security
      1. Only two IP address that needs to be whitelisted by our client
      2. DDoS protection through AWS Shield
6. Difference with CloudFront
   1. Provide failover
   2. Proxying packets from edge to applications running in one or more AWS regions
   3. We get two static IP address
7. Steps
   1. Global Accelerator -> ...
8. Fee
   1. Fixed fee / hour +
   2. Rate / gigabyte fee
       

   
# Terms

1. `Unicast IP`
   1. One server holds one IP address
2. `Anycast IP`
   1. All servers hold the same IP address & the client is routed to the closest one