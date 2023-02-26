# Firewall filter for RE protection

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
