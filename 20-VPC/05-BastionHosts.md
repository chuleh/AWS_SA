# Bastion Hosts

* way to ssh into private EC2 instances
* bastion is connected to public instance => then connects to all private subnets
* bastion host must allow inbound traffic to SSH. No 0.0.0.0/0 - better to restrict CIDR
* private instances must allow SSH from bastion host
