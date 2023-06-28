# ElastiCache

## Summary

* managed Redis / Memcached (similar offerings as RDS but for cache)
* in-memory data store, sub-millisecond latency
* select an ElastiCache instance type and Multi AZ, Read Replicas (sharding)
* security through IAM, Security Groups, KMS, Redis Auth
* backup / snapshot / PITR
* managed and scheduled maintenance
* requires application code changes to be leveraged

* use cases:
  * key/value store
  * frequent reads
  * less writes
  * cache db results for queries
  * store session data for websites
  * cannot use sql on ElastiCache
