# BAsic Networking

## Understanding CIDR - IPv4

* Classless Inter-Domain Router - a method for allocating IP addresses
* used in Security Groups and AWS Networking in general
* CIDR has 2 components:
  * Base IP
    * represents an IP contained in the range (X.X.X.X)
    * ex: 10.0.0, 192.160.0.0
  * Subnet Mask
    * defines how many bits can change in the IP
    * example /0, /24, /32
    * can take two forms:
      * /8 <=> 255.0.0.0
      * /16 <=> 255.255.0.0
      * /24 <=> 255.255.255.0
      * /32 <=> 255.255.255.255

## Understanding CIDR - Subnet Mask

* subnet mask basically allows part of the underlying IP to get additional next values from the base IP
* ex:
  * 192.168.0.0/32 => allows 1 IP (2**0) => 192.168.0.0
  * 192.168.0.0/31 => allows 2 IP (2**1) => 192.168.0.{0,1}
  * 192.168.0.0/30 => allows 4 IP (2**2) => 192.168.0.{0..3}
  * 192.168.0.0/29 => allows 8 IP (2**3) => 192.168.0.{0..7}
  * 192.168.0.0/28 => allows 16 IP (2**4) => 192.168.0.{0..15}
  * 192.168.0.0/27 => allows 32 IP (2**5) => 192.168.0.{0..31}
  * 192.168.0.0/24 => allows 256 IP (2**8) => 192.168.0.{0..255}
  * 192.168.0.0/16 => allows 65536 IP (2**16) => 192.168.{0..255}.{0.255}
  * 192.168.0.0/0 => allows all IP => {0..255}.{0..255}.{0..255}.{0..255}

* quick memo
  * CIDR made out of 4 octets == a.b.c.d
    * /32 => no octet change
    * /24 => last can octet change
    * /16 => last can 2 octets change
    * /8 => last can 3 octects change
    * /0 => all can octets change

## Private vs Public IPs

* private ranges can only allow certain values
  * 10.0.0.0/8 (10.0.0.0 - 10.255.255.255) => big networks
  * 172.16.0.0/12 (172.16.0.0 - 172.16.255..255) => AWS default vpc range
  * 192.168.0.0/24 (192.168.0.0 - 192.168.0.255) => home networks
* all of the rest *on the internet* == public IPs
