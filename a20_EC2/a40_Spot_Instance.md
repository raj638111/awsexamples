

# Spot instance

https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/learn/lecture/18756818#overview

## `Spot request`
   1. One time 
      1. Just a one time request for specific set of spot instances
   2. Persistent
      1. Has valid from - valid until time
      2. If instances get stopped / interrupted based on spot price, the spot request can relaunch the instances back based on the price again
      3. Termination process
         1. Delete stop request (So that no relaunch happens)
         2. Terminate the instance
      
## Spot fleet
   1. Spot instances + Optional On demand instances
   2. Can have launch pools
   3. Stop launching instances on budget (or) capacity
   4. Available strategies
      1. Lowest price (From the pool with lowest price)
      2. Diversified (Distributed across all pools)
      3. Price / Capacity Optimized
         1. Capacity: Pools with highest capacity available
         2. Price: Pools with highest capacity available, then select pool with lowest price)
         

## How to request spot fleet

Ec2 > Spot Requests > Request spot instances