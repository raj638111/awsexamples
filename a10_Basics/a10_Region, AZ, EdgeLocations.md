# Region

Choose region based on, 
1. compliance
2. Proximity (To reduce latency)
3. Available services
4. Pricing

Region scoped services (Example)
- EC2, Elastic Beanstalk, Lambda, Rekognition, ...

Global services
- IAM, DNS, CloudFront, WAF (Web application firewall)

# Availability zones (AZ)

1. Each region has min 3 / max 6 availability zones
2. Each AZ is one or more data discrete centers with redundant power, networking & connectivity
3. Is separated from each other to isolate from disasters
4. Connected through high bandwidth, ultra low latency networking

# AWS Points of Presence (Edge Locations)

1. AWS has
   1. 400+ Edge Locations 
   2. 10+ Regional caches
   3. In 90+ cities across 40+ countries
2. Content is delivered to end users with low latency