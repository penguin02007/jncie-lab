- Configure an IPv4 firewall filter allowing protocol messages from AH, BFD, VRRP, RIP, OSPF, RSVP, LDP, PIM, IGMP, MSDP protocols.


```
set firewall family inet filter protect-re term ah from protocol ah
set firewall family inet filter protect-re term ah then accept
set firewall family inet filter protect-re term bfd from protocol udp
set firewall family inet filter protect-re term bfd from port 3784
set firewall family inet filter protect-re term bfd then accept
set firewall family inet filter protect-re term vrrp from protocol vrrp
set firewall family inet filter protect-re term vrrp then accept
set firewall family inet filter protect-re term rip from protocol udp
set firewall family inet filter protect-re term rip from port rip
set firewall family inet filter protect-re term rip then accept
set firewall family inet filter protect-re term ospf from protocol ospf
set firewall family inet filter protect-re term ospf then accept
set firewall family inet filter protect-re term rsvp from protocol rsvp
set firewall family inet filter protect-re term rsvp then accept
set firewall family inet filter protect-re term ldp from protocol tcp
set firewall family inet filter protect-re term ldp from port ldp
set firewall family inet filter protect-re term ldp then accept
set firewall family inet filter protect-re term pim from protocol pim
set firewall family inet filter protect-re term pim then accept
set firewall family inet filter protect-re term igmp from protocol igmp
set firewall family inet filter protect-re term igmp then accept
set firewall family inet filter protect-re term msdp from protocol tcp
set firewall family inet filter protect-re term msdp from port msdp
set firewall family inet filter protect-re term msdp then accept
```
- Configure the firewall filter so that BGP messages are accepted only from configured BGP neighbors. Make sure that a configured BGP neighbor is automatically allowed in the firewall filter.
```
set policy-options prefix-list bgp-peers apply-path "protocols bgp group <*> neighbor <*>"
set firewall family inet filter protect-re term bgp from source-prefix-list bgp-peers
set firewall family inet filter protect-re term bgp from protocol tcp
set firewall family inet filter protect-re term bgp from port bgp
set firewall family inet filter protect-re term bgp then accept   
```

- Configure the firewall filter to accept NTP, RADIUS, DNS, SNMP, SSH, Telnet, and FTP protocols only from the 10.10.1/24 management network.
```
set firewall family inet filter protect-re term snmp from source-address 10.10.1.0/24
set firewall family inet filter protect-re term snmp from protocol udp
set firewall family inet filter protect-re term snmp from port snmp
set firewall family inet filter protect-re term snmp then accept
set firewall family inet filter protect-re term radius from source-address 10.10.1.0/24
set firewall family inet filter protect-re term radius from protocol udp
set firewall family inet filter protect-re term radius from port radius
set firewall family inet filter protect-re term radius then accept
set firewall family inet filter protect-re term dns from source-address 10.10.1.0/24
set firewall family inet filter protect-re term dns from protocol udp
set firewall family inet filter protect-re term dns from port domain
set firewall family inet filter protect-re term dns then accept
set firewall family inet filter protect-re term ssh from source-address 10.10.1.0/24
set firewall family inet filter protect-re term ssh from protocol tcp
set firewall family inet filter protect-re term ssh from port ssh
set firewall family inet filter protect-re term ssh then accept
set firewall family inet filter protect-re term telnet from source-address 10.10.1.0/24
set firewall family inet filter protect-re term telnet from protocol tcp
set firewall family inet filter protect-re term telnet from port telnet
set firewall family inet filter protect-re term telnet then accept
set firewall family inet filter protect-re term ftp from source-address 10.10.1.0/24
set firewall family inet filter protect-re term ftp from protocol tcp
set firewall family inet filter protect-re term ftp from port ftp
set firewall family inet filter protect-re term ftp from port ftp-data
```
- Configure the firewall filter to accept ICMP and traceroute messages. Ensure that the flow of the messages is limited to 100 kbps with a burst size of 25 K. The excess traffic must be dropped.

```
set firewall policer re-policer if-exceeding bandwidth-limit 100k
set firewall policer re-policer if-exceeding burst-size-limit 25k
set firewall policer re-policer then discard
set firewall family inet filter protect-re term icmp from protocol icmp
set firewall family inet filter protect-re term icmp then policer re-policer
set firewall family inet filter protect-re term icmp then accept
set firewall family inet filter protect-re term traceroute from protocol udp
set firewall family inet filter protect-re term traceroute from port 33434-33534
set firewall family inet filter protect-re term traceroute then policer re-policer
set firewall family inet filter protect-re term traceroute then accept
```
- Configure the firewall filter to discard any other traffic, increment a named drop counter, and send a log message. Apply the firewall filter such as to ensure that it is used for RE protection

```
set firewall family inet filter protect-re term last then count dropped-packets
set firewall family inet filter protect-re term last then log
set firewall family inet filter protect-re term last then discard
set interfaces lo0 unit 0 family inet filter input protect-re
```