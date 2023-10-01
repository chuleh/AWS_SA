# AWS MGN - Application Migration Service

* start with AWS App Discovery Service
  * plan migration projects by gathering information about on-prem
  * gather server utilization data and dependency mapping
* 2 types of migration
  * agentless discovery (AWS Agentless Discovery Connector)
    * VM inventory, configuration and performance history such as CPU, memory and disk usage
  * agent based discovery (AWS Application Discovery Agent)
    * system config, system performance, running processes and details of the network connections between systems
* all data can be seen in *AWS migration hub*
* moving to AWS with *AWS App Migration Service (MGN)*
  * lift-and-shift solution (rehost) which simplifies migrating to AWS
  * converts your physical, virtual and cloud-based servers to run natively on AWS
* supports wide range of platforms, OS' and DBs
* minimal downtime, reduced costs
* idea is to replicate data onto a staging env and once *cutover* is scheduled fully migrate to *prod*
