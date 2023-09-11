# Internet Gateway

* allows resources in a VPC to connect to the internet
* scales horizontally and is HA and redundant
* must be created separately from the VPC
* 1 IGW per VPC and vice versa
* IGW on their own *do not allow* internet acces => must edit *route tables*
* route table:
  * route table must be associated with corresponding *subnet* -can be public or private-
  * route table needs to be edited and add 0.0.0./0 (all traffic) *target* the IGW
  * public instance on SG allows router => router to IGW => to internet
