# Aurora  

* propietary technology from AWS (not open source)
* Postgres and MySQL are both supported as AuroraDB
* AWS Cloud optimized - claims 5x performance improvement over MySQL on RDS and 3x of Postgres
* Storage automatically grows in increments of 10GB up to 64TB
* Can have 15 replicas while mysql has 5; replication process is faster (sub 10ms replica lag)
* Failover is instantaneous. It's HA native
* Costs approx 20% more than RDS
* automatic fail-over
* backup and recovery
* isolation and security
* industry compliance
* push-button scaling
* automated patching with *zero* downtime
* Backtrack
  * restore data at any point of time without using backups

## Aurora HA and Read Scaling

* 6 copies of your data across 3 AZ
  * 4 copies out of 6 needed for WRITES
  * 3 copies out of 6 needed for READS
  * self-healling with peer-to-peer replication
  * storage is striped across 100s of volumes
* One Aurora instance takes WRITES (master)
* Automated failover for master in less than 30 seconds
* Master + up to 15 Aurora Read Replicas *serve reads*
* Support for Cross Region Replication

## Aurora DB Cluster

* Writer endpoint => pointing to master
* Reader Endpoint => connection load balacing between Read Replicas
* Shared storage volume (auto expanding from 10G to 64TB)
* Master writes to the storage
* Aurora provides a *Writer Endpoint* pointing to the Master (a DNS Name)
* Client always talking to the writer endpoint, in failover, automatically redirected
* Read Replicas can have AutoScaling via *Reader Endpoint*
* Reader Endpoint is a Connection Load Balancing
* Reader Endpoint connects automatically to Read Replicas