# Network ACL && Security Groups

* first comes NACL then goes to SG
* NACL are stateless || SG are stateful
  * for incoming request
    * SG stateful == if inbound is accepted then the response from server/app/etc will go out too.
    * NACL stateless == outbound response can be blocked even coming out from SG
  * for outgoing request
    * same for both *HOWEVER* == since SG is stateless it makes no difference due to initiating the request
    * only NACL makes a difference by allow/deny traffic

## NACL basics

* like a firewall that controls traffic to and from subnets
* one NACL per subnet -new subnets are assigned the default NACL-
* you define NACL rules:
  * rules have numbers -1 to 32766- == higher precedence lower number
  * first rule match will drive the decision
    * eg: #100 allow 1.2.3.4 || #200 deny 1.2.3.4 => allow traffic
  * last rules in * and denies request in case of no match
  * aws recommends adding rules in increments of 100
* newly created NACLs will *deny* everything
* great for blocking a specific IP at the subnet level
* default NACL accepts everything inbound/outbound with the subnets it' associated with
* *DO NOT* modify default NACL -create custom ones-

## Ephemeral Ports

* for any two endpoints to establish a connection they must use ports
* clients connect to a *defined port* and expect a response on an *ephemeral port*
* ephemeral due to connection for that transcation
* diff OS use diff ports -usually over port 30K on nix-

* NACL with ephemeral ports
  * scenario => web server connecting to db / web == public subnet || db == private subnet
    * web nacl *must allow* outbound TCP on 3306 to DB Subnet CIDR
    * db nacl *must allow* inbound TCP on 3306 from Web Subnet CIDR
  * client making request has ephemeral port so where does it connect to?
    * db nacl *must allow* outbound TCP on on port 1024-65535 to Web Subnet CIDR
    * web nacl *must allow* inbound TCP on port 1024-65535 from DB subnet CIDR
* if adding subnets to nacls == must edit rules to allow traffic to and from those new subnets

### NACL vs SG

* SG
  * instance level
  * supports *ALLOW* only
  * stateful == traffic allowed in is allowed out
  * all rules are evaluated for allowing traffic in
  * applies to EC2 instance when specified
* NACL
  * operates at subnet level
  * support *ALLOW* and *DENY*
  * stateless == return traffic must be explicitly allowed -see ephemeral ports-
  * rules are evaluated in order | low to high | first match wins
  * automatically applies to all EC2 instances in the subnet associated with
