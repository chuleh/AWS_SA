# AWS DynamoDB

* fully managed
* highly available
* replication across multiple AZs
* NoSQL with transaction support
* scales to massive workloads, distributed db
* millions of requests per second, trillions of rows, 100s of TB of storage
* fast and consistent in performance (single-digit millisecond)
* integrated with IAM for security, auth and administration
* low cost - aut-scaling capabilities
* no maintenance or patching
* standard & infrequent access (IA) table class

## DynamoDB basics

* made of tables
* each table has a *Primary Key* (must be decided at creation time)
* each table can have an infinite number of items (==rows)
* each item has attributes (can be added over time and can be null)
* max item size 400KB
* data types supported:
  * Scalar Types: string, binary, number, boolean, null
  * Document Tyoes: List, Map
  * Set Types: String Set, Number Set, Binary Set

* DynamoDB can rapidly evolve Schemas

## R/W Capacity modes

* control how you manage your table's capacity (r/w thruput)
* provisioned mode (default):
  * specify the number of reads/writes per second
  * need to plan capacity beforehand
  * pay for provisioned Read Capacity Unit (RCU) && Write Capacity Unit (WCU)
  * possibility to add autoscaling for RCU && WCU
* on-demand mode:
  * read/writes automatically scale up/down with workloads
  * no capacity planning needed
  * pay for what you use => more expensive
  * great for *unpredictable* workloads and steep sudden spikes
