

# Use of ELB
    
1. Spread laod across multiple EC2 instances
2. Single point of access (DNS) to your application
3. Seamlessly handle downstream instance failures
4. Do regular health check of instances
5. Provide SSL (HTTPS) for your websites
6. Enforce stickiness with cookies
7. High availability across zones
8. Separate public traffic from private traffic

---

# How to?

`EC2 > Load Balancers`

---

# ELB features

1. **Is a managed load balancer**
2. **Integrate with many AWS offerings**
   1. EC2 instances, EC2 ASG, Amazon ECS (Elastic compute cloud)
   2. AWS Certificate manager, cloud watch
   3. Route 53, AWS WAF, AWS Global Accelerator
3. **Four types**
   
4. ELB can be set as private (internal) or public (external)

---

# Four types

## Classic (CLB) (Deprecated)

1. HTTP, HTTPS, TCP, SSL

## Application Load Balancer (ALB)

1. Supports HTTP/2, HTTPS, WebSocket
2. Supports redirects (From HTTP to HTTPS)
3. Operates on Layer 7
4. Load balancing to multiple http applications across machines
5. Load balancing to multiple applications in the same machine (ex: Containers)
6. Provides `Routing tables` (Accessible in `Rules` tab) for different `target groups`
    1. Routing based on Path in URL (Example: example.com/users & example.com/posts)
    2. Routing based on hostname in URL (Example: one.example.com & other.example.com)
    3. Routing based on query string, headers (example.com/users?`id=123&order=false`)
7. Great for micro services & container based application
8. Has port mapping feature to redirect to dynamic port in ECS
9. Can route to multiple target groups
10. Health checks are done at target group level
11. An ALB gets fixed host name
12. Application servers do not see IP of clients
    1. True IP & Port & Protocol is inserted in header `X-Forwarded-For`, `X_Forwarded-Port` & `X-Forwarded-Proto`
13. Can load balance across multiple AZ

## Network Load Balancer (NLB)
        
1. Features
   1. Is Layer 4 load balancer
   2. Can be used to forward TCP, TLS, UDP traffic to your instances 
   3. Handles millions of requests per second
   4. Ultra low latency
   5. Has one static IP per AZ & supports assigning elastic IP to each AZ
      1. Can be used to expose our application with set of static IPs (Elastic IPs)
   6. Health check supports target groups of: TCP, HTTP & HTTPS
2. Target Groups can be,
   1. EC2 instances
   2. IP Addresses (Must be private IPs)
   3. Application Load Balancer (ALB)


## Gateway Load Balancer (GWLB)

1. Features
   1. Provides single entry point for all n/w traffic
   2. Can be used to analyze n/w traffic on network layer level (Layer 3) - IP Protocol
   3. Can be used to forward traffic to 3rd party `N/W virtual appliances` in AWS
      1. Example n/w virtual appliance: Firewalls, Intrusion Detection & prevention system, Deep packet inspection systems, Payload manipulation
   4. Uses `GENEVE` protocol on port `6081`
   
2. Target Groups can be,
   1. EC2 instances
   2. IP Addresses (Private IPs)

3. How works
   1. User > Route table > GWLB > Target Group -(If approved)-> Destination (Ex: Application)


# Terms

1. **Health Check**
   1. Is done using protocol (Example: HTTP), port and route (/health is common)
   2. Response is not 200 (OK) denotes unhealthy instance
2. **Security Group** 
   1. ELB Security Group 
      1. Allows HTTP (80) & HTTPS (443) from any address (ie 0.0.0.0/0)
   2. EC2 Security Group
      1. Allows traffic only from Load balancer
      2. Done using Security group of LB as source for EC2 instance
3. **Target Groups**: Target Group can be,
   1. EC2 instances managed by ASG - HTTP
   2. ECS tasks (Managed by ECS itself) - HTTP
   3. Lambda Functions -  HTTP request is translated into a JSON event
   4. IP Addresses - Must be Private IPs

# Sticky Sessions (Session Affinity)

1. Is a way to make the traffic from the same client goes to the same instance behind a load balancer
2. Can be enabled for,
   1. Class LB
   2. ALB
   3. NLB
3. How works?
   1. Using `cookie` that is sent from client as part of request
      1. `Cookie` has expiration date which we can control
4. Disadvantage
   1. Can bring imbalance to the load
5. Two types of cookie can be used to get sticky sessions
   1. Application based cookie
      1. Custom Cookie
         1. Generated by the application 
         2. Can include custom attributes
         3. Cookie name needs to be specified individually for each target group
         4. Reserved cookie names for ALB (AWSALB, AWSALBAPP, AWSALBTG)
      2. Application Cookie
         1. Generated by the load balancer
         2. Cookie name: AWSALBAPP
   2. Duration based cookie
      1. Cookie generated by the load balancer
      2. Cookie name: AWSALB for ALB, AWSELB for CLB
6. How to make sticky session
   1. EC2 > Target groups > <target group> > Edit target group attributes > Target selection configuration > Stickiness
7. How to view cookie
   1. Chrome > Dev tools > Network > Cookies

---


# Cross-Zone laod balancing

1. How useful?
   1. Helps with distributing traffic
2. Example:
   1. AZ1: Has 8 instances in TG
   2. AZ2: Has 2 instances in TG
   3. Client alternates traffic b/w AZ1 & AZ2 (50%-50% traffic)
   4. Irrespecitve of the AZ, each ALB will distribute across 10 instances across both AZ
3. For ALB
   1. Cross zone load balancing is enabled by default
   2. Can be disabled at target group level
   3. No charges for inter AZ data
4. For NLB & GLB
   1. Cross zone load balancing is disabled by default
   2. Need to pay charge for inter AZ data if enabled
5. For CLB
   1. Disabled by default
   2. No inter AZ charge if enabled
6. How to view / set this?
   1. EC2 > LB > _load_balancer(NLB,GLB) > Attributes
   2. EC2 > LB > _load_balancer(ALB) 
      1. To view: > Attributes
      2. To edit: > Listeners > _Target_Group > Attributes > Edit


---

# Elastic load balancer - SSL Certificates

1. Why needed?
   1. Public SSL/TLS certificates are attached to LB to encrypt / decrypt data b/w client and the load balancer
2. How works?
   1. User -HTTPS-over-WWW-> LB -HTTP-over-PRIVATE-VPC-> EC2_Instance
3. LB Uses X.509 certificate
   1. Certificates can be managed using ACM (AWS Certificate manager)
4. How to set SSL certificate in LB
   1. When setting up HTTPS listener specify the default certificate
   2. We can add optional list of certs to support multiple domains
   3. Clients can use `SNI` (`Server Name Indication`) to specify the hostname they reach
      1. SNI: Solves problem of loading multiple SSL certificates into one Server (to serve multiple websites)
      2. Is a newer protocol and require the client to indicate the host name of the target server in the initial SSL handshake
      3. Only works for ALB, NLB (and CloudFront) 
5. SSL Certificate support
   1. CLB 
      1. Support only one SSL certificate
      2. Need to use multiple CLB for multiple host name (say multiple websites) with SSL certificate
   2. ALB (v2)
      1. Supports multiple listeners with multiple SSL certificates
      2. Uses SNI to make it work
   3. NLB (V2)
      1. Supports multiple listeners with multiple SSL certificates
      2. Uses SNI to make it work