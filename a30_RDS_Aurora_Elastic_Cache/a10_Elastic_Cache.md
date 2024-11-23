
# What is Elasticache?

1. Is managed `Redis` or `Memcached`
2. Are in-memory db with high performance & low latency
3. Helps reduce load of databases for read intensive workloads
4. Helps make our app stateless by putting state of the app into Elasticache

# Use case 1 : DB Cache

app > cache > RDS

1. Terms
   1. Cache hit / miss
   2. Cache invalidation strategy: To make sure only the current data is available in the cache

# Use case 2: User session store

1. Steps
   1. User logs in through app in EC2 instance & session info stored in cache
   2. User connects to app in another EC2 instance & session info is retrieved from cache and he does not need to login again


# Redis vs Memcached

1. **Redis**
   1. Multi AZ with auto failover
   2. Read replicas to read and have high availability
   3. Data durability using AOF persistence
   4. Backup and restore features
   5. Supports set & sorted sets
2. **Memcached**
   1. Multi-node for partitioning of data (Sharding)
   2. No high availability (replication)
   3. Non persistent
   4. Backup & restore available only for serverless
   5. Multithreaded architecture

# Cache Security

1. Supports IAM authentication for Redis
2. IAM Policies on EC are only used for AWS API level security
3. **Redis Auth**
   1. Can set pwd/token to create a Redis cluster
   2. Is extra level of security on top of Security groups
   3. Supports SSL in-flight encryption
4. **Memcached**
   1. Supports SASL-based authentication (advanced)


# Patterns for EC

1. `Lazy loading`
   1. All read data is cached. Data can become stale
2. `Write through`
   1. Adds / updates data in cache when written to DB (No stale data)
3. `Session store`
   1. Store temp session info in cache (using TTL features)

# Use cases for Redis

1. Gaming leaderboard
   2. Redis Sorted sets provide both uniqueness and element ordering
   3. Each time an element is added, its ranked real time then added in correct order
   4. (This basically helps us to avoid this stuff in application side)

