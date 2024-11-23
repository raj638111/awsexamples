
# About

1. Is fully managed `Authoritative DNS`
   1. `Authoritative`: Customer can manage DNS records
2. Is also a `Domain registrar`
   1. Ex: `example.com`
3. Translates human friendly host name into IP address
4. Provides ability to check health of our resources
5. Provides 100% availability SLA
6. `53`: Is the traditional DNS port

# Records

1. Records define how to route a traffic for a domain
2. Each record contains
   1. **Domain/ Subdomain name** 
      1. Ex: example.com
   2. **Record type** 
      1. Ex: A or AAAA
   3. **Value**
      1. Ex: 12.34.56.78
   4. **Routing Policy**
      1. How route53 responds to Queries
   5. **TTL**
      1. Amount of time records is cached in DNS resolvers
3. Route53 supports following DNS record types
   1. Must know (For exam)
      1. A record
         1. Maps a hostname to IPV4
      2. AAAA
         1. Maps a hostname to IPV6
      3. CNAME
         1. Maps a hostname to another hostname
         2. Target is a domain name which must have A or AAAA record
         3. Cant create CNAME record for top node of DNS namespace (`Zone Apex`)
         4. Ex: No CNAME for `example.com`, but can create for `www.example.com`
      4. NS
         1. Name servers of a hosted zone 
         2. Control how traffic is routed for a domain
   2. Advanced
      1. CAA / DS / MX / NAPTR / PTR / SOA / TXT / SPF / SRV

# Hosted Zone

1. Is a container for records that define how to route traffic to domain and its subdomain
2. Two types of hosted zone
   1. **Public hosted zone**
      1. Contains records that specify how to route traffic on the internet
      2. Example: public domain name: app1.mydomain.com
   2. **Private hosted zone**
      1. Contains records that specify how to route traffic within one or more VPCs (Private domain names)
      2. Ex: app1.company.internal
3. Cost: 0.50$ per month for a hosted zone


# How to register a domain

1. **Register Domain**
   1. Route53 > Domains > Registered domains > Register domains >  
   2. **Hosted Zones**
      1. Route53 > Hosted Zones > #Select Domain# > Create Record > ...

# Command line
```shell
sudo yum install -y bind-utils

nslookup test.rj.com

(or)

dig test.rj.com
```    

# DNS terms

1. `Domain registrar`
   1. Amazon Route53, GoDaddy
2. `DNS records`
   1. A, AAA, CNAME, etc...
3. `Zone file`
   1. Contains DNS records
4. `Name Server`
   1. Resolves DNS Queries
5. `Top level Domain`
   1. Ex: .com, .us, .in, .gov, etc...
6. `Second level domain`
   1. Ex: amazon.com, google.com
7. `FQN (Fully Qualified Domain Name`) 
   1. Example: `http://api.www.example.com.`<- root
                                      ^ Top level domain
                              ^ Second level domain (example.com)
                           ^Sub domain (www.example.com) 
                        ^ FQDN (api.www.example.com)
   

# Route53 TTL

1. TTL response from Route53 tells the client to cache the `A Rrecord` for specific seconds, so that subsequent requests are not sent to Route53 until those specific seconds
2. High TTL
   1. Less traffic on Route53
   2. Possible outdated records
3. Low TTL
   1. More traffic on Route53 (So more need to pay for Route53)
   2. Less outdated records
   3. Easy to change records
4. Except for `Alias records`, TTL is mandatory for each DNS record
5. Caching can be checked using `dig` command (where TTL will go down on subsequent requests)


# CNAME vs Alias

1. **CNAME**
   1. Points a host name to any other host name
      1. Ex: AWS resources (Load balancer, CloudFront) expose AWS hostname
         1. lb1-1234.us-east-2-elb.amazonaws.com 
         2. ^ This kind of host name can be mapped to DNS name (like myapp.mydomain.com) using CNAME
   2. Only works for non-root domain name (ex: something.mydomain.com & not for mydomain.com)
2. **Alias Records**
   1. Points host name to AWS resource only (app.mydomain.com => some.amazonaws.com)
   2. Works for both ROOT DOMAIN (`Zone Apex`: example: mydomain.com) & NON ROOT DOMAIN 
   3. Free of charge
   4. Native health check
   5. Is an extension to DNS functionality
   6. Automatically recognize changes in the resource IP address
   7. Is always of type A or AAAA for AWS resources
   8. Cannot set TTL. Is set automatically by Route53
   9. Alias record targets
      1. Elastic load balancers
      2. CloudFront distributions
      3. API Gateway
      4. Elastic Beanstalk environments
      5. S3 websites
      6. VPC interface endpoints
      7. Global Accelerator accelerator
      8. Route53 record in same hosted zone
   10. Note
       1. Cannot set Alias record for EC2 DNS name
    

# Routing Policies

1. Defines how R53 responds to DNS queries
2. Supports following policies
   1. **Simple**
      1. Route traffic to single resource
      2. Can specify multiple records
         1. In this case, client gets all records and choose one by random
      3. With alias, can only specify AWS resource
      4. No health check
   2. **Weighted**
      1. Controls the % (Total 100%) of requests that go to each specific resource
      2. We assign each record a relative weight (traffic% = weight of a record / sum of weights of all records)
      3. DNS records must have the same name & type
      4. Provides health checks
      5. Use case:
         1. Load balancing b/w regions
         2. Testing new application versions
      6. Weight 0 for a record: Stops sending traffic
      7. Weight 0 for all records: Traffic is send equally
   3. **Failover**
      1. Step (Example)
         1. Associate a Primary EC2 instance with health check
         2. If the primary is healthy, RG3 will use primary record
         3. If the primary is unhealthy, then R53 is failover to Disaster recovery instance (Secondary)
   4. **Latency based**
      1. Redirect to the resource that has the least latency close to us
      2. Latency is based on users & AWS regions
      3. Can be associated with health checks
   5. **Geolocation based**
      1. Routing is based on user location
      2. Location based on Continent, country or by US state
      3. Use case
         1. Website localization
         2. Restrict content distribution
         3. Load balancing
      4. Can be associated with health check
      5. Default record is used when no mapping is found
   6. **Multivalue based**
      1. Used to route traffic to multiple resources
      2. Here, R53 return multiple values/resources
      3. Can also add health checks (will return only values of healthy resources)
      4. Upto 8 healthy records (Clients choose one of the record from this) are returned for each Multi-value query
      5. 
   7. **Geoproximity based**
      1. Route traffic to resources based on geographic location of users & resources
      2. (Need to use Route53 traffic flow to use this feature)
      3. Bias value
         1. Used to shift more traffic to resource
         2. Specify bias value to change the size of geographic region
         3. To expand: 1 to 99 (ie more traffic to the resource)
         4. To Shrink: -1 to -99 
      4. Resources can
         1. AWS resource (Specify AWS region)
         2. Non-AWS resources (Specify latitude & longitude)
   8. **IP Based**
      1. Routing is based on client's IP address
      2. We provide a list of CIDR's for the clients and the corresponding endpoints
      3. Use case
         1. Optimize performance
         2. Reduce n/w costs
      4. Example: Route user from a specific ISP to a specific endpoint
      

# Health checks

1. HTTP health checks are only for public resources
2. Provides automated DNS failovers
   1. Say for example we have latency based record to two load balancers in different region
   2. If one region is down (based on health check), then we will be redirected to another regions
3. **Three types of healt checks are supported**
   1. **Monitor an endpoint**
      1. Ex: application, server, other AWS resource
      2. Health check is done from all around the world (Total of 15 health checker)
      3. 2XX, 3XX response from resource is considered as healthy
      4. Health check parameters
         1. Healthy / unhealthy threshold - 3 (Default)
         2. Interval - 30 sec (Lower value is higher cost)
         3. Supported protocol: HTTP, HTTPS & TCP
         4. If > 18% of health checkers report endpoint as healthy, R53 considers that endpoint as healthy
         5. Health check can also be set to fail based on the text in first 5120 bytes of response
         6. Note: Configure router/firewall to allow incoming request from health checkers
   2. **Calculated Health checks**
      1. These are health checks that monitor health checks
      2. Combines the result from multiple health checks into one single health check
      3. Can use AND, OR or NOT
      4. Can monitor upto 256 child health checks
      5. Can specify how many child health check need to pass to pass the parent
      6. Use case
         1. Perform maintenance to the website without causing all health checks to fail
   3. **State of CW Alarm**
      1. Health check that monitor cloud watch alarm
      2. Is helpful to monitor private resources
      3. Ex: Custom metrics, Throttles of Dynamo DB, Alarms on RDS
4. Health checks are also integrated with CW metrics
5. How to health check Private Hosted zones?
   1. R53 health checkers are outside the VPC (So can't access private endpoints)
   2. We use Cloud watch metric to create alarm which the health checker monitors


# Domain registrar vs DNS Service


1. We buy or register a domain name with a Domain registrar (GoDaddy, `Amazon Registrar Inc`)
2. Domain registrar usually provides DNS service (For Amazon, this is `Route53`) to manage our DNS records
3. It is possible to use another DNS service to manage our DNS records instead of R53 (or)
4. It is possible register with 3rd party registrar & use R53 and DNS service
   1. Buy domain from 3rd party registrar
   2. Create a hosted zone in R53
   3. Update `NS records` of 3rd party website to use `R53 Name servers`