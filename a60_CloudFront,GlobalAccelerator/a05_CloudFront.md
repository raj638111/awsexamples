
# 101

Is a Content delivery network (`CDN`)

1. How does it work?
   1. Caches content at the edge
   2. 216 edge locations are available globally
2. Other features
   1. DDOS Protection
   2. Integration with `Shield`
   3. AWS Web application firewall
3. Cloud Front can be placed in front of (Origin)
   1. **S3 Bucket**
      1. For distributing files & caching them at edge
      2. Enhanced security with Origin Access Control (`OAC`) (Replaces the older OAI (Origin Access identity))
      3. Can be used as an ingress (To upload to S3)
   2. **Custom Origin (HTTP)**
      1. This can be,
         1. Application Load Balancer
         2. EC2 instance
         3. S3 website
         4. Any HTTP backend
4. Typical Process
   1. Client -> CloudFront Edge Location -> Origin (`S3` or `HTTP`)
5. Terms
   1. Origin
      1. Source of data (S3, HTTP etc...)


# CloudFront: S3 as origin

For setting up CloudFront, we create `Distributions` in AWS
1. Steps
   1. Cloud Front -> `Distributions` -> Origin -> ...

# CloudFront: ALB or EC2 as origin

1. EC2: User -> Edge -> EC2 (Should be public)
2. ALB: User -> Edge -> ALB -> EC2 (Can be private)

# Geo Restriction

Is possible to restrict based on Geo who is able to access the distribution

1. Configuration
   1. `AllowList`
      1. Only allow from the specific countries
   2. `BlockList`
2. Use case
   1. Copyright laws to control the content


# Price classes

1. Cost of data out per edge location varies
2. Reduce the no of edge location to reduce cost
3. Three price classes
   1. `Price Class All`
      1. All regions, best performance
   2. `Price Class 200`
      1. Exclude most expensive regions
   3. `Price Class 100`
      1. Only the least expensive regions

# Cache Invalidations

1. CloudFront is not aware of updates in backend origin
2. `CloudFront Invalidation`
   1. Can be used to force an entire or partial cache refresh
   2. Examples
      1. `*`: Invalidate all files
      2. `/images/*`: Special path invalidation