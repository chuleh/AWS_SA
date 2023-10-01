# On-premise Strategy with AWS

* Ability to download Amazon Linux AMI as a VM (.iso)
  * works on: VMWare, KVM, VirtualBox (OracleVM), MS Hyper-V
* VM Import/Export
  * migrate existing apps into EC2
  * create a DR repo strategy for on-prem VMs
  * can export the VMs back to on-prem from EC2
* AWS App Discovery Service
  * service to gather data about your on-prem servers to plan a migration
  * server utilization and dependency mappings
  * track migration with AWS *Migration Hub*
* AWS DMS
  * dbs from: on-prem => AWS | AWS => AWS | AWS => on-prem
  * various db technologies
* AWS SMS (Server Migration Service)
  * incremental replication of on-prem live servers to AWS
