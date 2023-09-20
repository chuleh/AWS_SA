# Transit Gateway

* for having transitive peering between thousands of VPC and on-prem, hub-and-spoke (star) connection
* regional resource - can work cross-region
* share cross-account using Resource Access Manager (RAM)
* you can peer transit gateways across regions
* *RouteTables* limit which VPC can talk to which VPC
* supports *IP Multicast* -not supported by any other AWS service-

## Site2Site VPN ECMP

* ECMP == equal cost multi path routing
* routing strategy to allow to forward a packet over multiple best path
  * create multiple s2s vpn connections to increase the bandwidth of your connection to AWS
* throughput with ECMP
  * VPN to VPG
    * 1 VPN => 1 VPC == 1.25 Gbps (VPN connection == 2 tunnels)
  * VPN to TGW
    * 1 VPN => X VPC (all connected virtually to the same TGW) == 2.5 Gbps (ECMP) == uses both tunnels
    * can add more s2s vpn gateways to increase bandwidth
      * x2 == 5 Gbps (ECMP)
      * x3 == 7.5 Gbps (ECMP)
* pay per GB of TGW

## Share DX between multiple accounts

* customer gw to => AWS DX Endpoint
* TGW (aws) to DX Gateway
* connect TGW to AWS DX Endpoint
* can use AWS Resource Access Manager to share TGW with other accounts
