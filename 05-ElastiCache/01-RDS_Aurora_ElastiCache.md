# ElastiCache - port 6379

* Managed Redis or Memcached
* Caches are in-memory databases with really high performance, low latency
* Helps reduced load off of databases for read intensive workloads (reads from cache, not DB)
* Helps make your application stateless - puts state of app in ElastiCache
* AWS takes care of OS maintenance, optimizations, setup, configuration, monitoring, failure recovery and backups
* Write Scaling using sharding
* Read Scaling using Read Replicas
* Multi AZ with failover capability
* Application queries ElastiCache, if not available, get from RDS and store in ElastiCache
* Cache must have an invalidation strategy to make sure only the most current data is used in there
* User session store
  * user logs into any of the application
  * app writes the session data into ElastiCache
  * user hits another instance of our application
  * retrieves session from elasticache and the users is already logged in

⚠️ involves heavy application code changes - app must query cache before querying DB

## ElastiCache for Solutions Architect

* App queries ElastiCache => if not available get from RDS => store in ElastiCache
  * Patterns for ElastiCache
    * Lazy Loading - all the read data is cached, data can become stale in cache
    * Write Through - add or updates data in the cache when written to a DB (no stale data)
    * Session store - store temporary session data in a cache (using TTL features)
* Cache security
  * all caches support SSL in flight encryption
  * _do not support IAM authentication_
    * IAM Policies on ElastiCache are only used for AWS API-level security
* Redis AUTH
  * you can set a password/token when you create a redis cluster
  * extra level of security on top of SGs
  * supports SSL in flight encryption
  * Redis use case
    * Gaming Leaderboard
      * _Redis Sorted Sets_ guarantee both uniqueness and element ordering
      * each time a new element is added it's ranked in real time then added in correct order
* Memcached
  * supports SASL-based authentication

## Redis vs Memcached

* Redis
  * MultiAZ with auto-failover
  * Read Replicas to scale reads and have high availability
  * Data Durability using AOF persistence
  * Backup and restore features
* Memcached
  * multi-node for partitioning of data (sharding)
  * no replication (HA)
  * non-persistent cache
  * no backup and restore
  * multithreaded architecture
