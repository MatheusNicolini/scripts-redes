```
EN
CONF T
HOST RT-EIGRP2

INT F0/0
NO SH
IP AD 10.0.0.1 255.254.0.0
DESC LAN EIGRP2


INT F0/0
IP DHCP EXCLUDED-ADDRESS 10.0.0.1
IP DHCP POOL LAN-EIGRP2
NET 10.0.0.0 255.254.0.0
DEFAULT-ROUTER 10.0.0.1


INT S0/0
NO SH
IP AD 201.30.19.1 255.255.255.248
BAND 256
CLOCK R 128000
DESC WAN EIGRP2-REDISTIBUTE


ROUTER EIGRP 2
NO AUTO-SUMMARY
NET 10.0.0.0
NET 201.30.19.0

EXIT
EXIT
SH R
COPY R ST
```