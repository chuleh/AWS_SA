# Transferring large amounts of data in AWS

* proposed solutions for transfering large data into aws
  * ex1: 200TB into AWS over a 100Mbps connection
    * over internet with S2S VPN
      * immediate setup
      * will take 200TB \* 1000GB \* 1000MB \* 8Mb / 100Mbps == 16M secs == 185 days
    * over DirectConnect 1Gbps
      * long for the one time setup (over a month)
      * will take 200TB \* 1000GB \* 8Gb / 1Gbps == 1,6M secs == 18,5 days
    * over snowball
      * will take 2 to 3 snowballs in parallel
      * about 1 week for the end-to-end trasnfer
      * can be combined with DMS
  * for on-going replications / transfers: s2s vpn or DX with DMS or DataSync
