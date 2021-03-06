# CONFIGURAÇÃO ROTEADOR REDISTRIBUTE

## RIP
```
EN
CONF T
HOST REDISTRIBUTE

INT S0/0
NO SH
IP AD 1.0.0.2 255.0.0.0
BAND 512
CLOCK R 128000
DESC WAN REDISTRIBUTE-RIP

EXIT
EXIT
SH R
COPY R ST
```

## REDISTRIBUTE RIP
```
EN
CONF T

ROUTER RIP
NET 1.0.0.0
NET 2.0.0.0
NET 3.0.0.0
NET 4.0.0.0

REDISTRIBUTE STATIC METRIC 1
REDISTRIBUTE EIGRP 1 METRIC 1
REDISTRIBUTE EIGRP 100 METRIC 1
```
obs: a métrica 1 diz respeito a quantidade mínima de saltos

## EIGRP
```
EN
CONF T
HOST REDISTRIBUTE

INT S0/1
NO SH
IP AD 2.0.0.2 255.0.0.0
BAND 512
CLOCK R 128000
DESC WAN REDISTRIBUTE-EIGRP

EXIT
EXIT
SH R
COPY R ST
```

## REDISTRIBUTE EIGRP
```
EN
CONF T

ROUTER EIGRP 1
NO AUTO-SUMMARY
NET 1.0.0.0
NET 2.0.0.0
NET 3.0.0.0
NET 4.0.0.0

REDISTRIBUTE RIP METRIC 10000 100 255 1 1500
REDISTRIBUTE STATIC METRIC 10000 100 255 1 1500
REDISTRIBUTE OSPF 100 METRIC 10000 100 255 1 1500
```
obs: a métrica 10000 diz respeito quanto de banda máxima que será liberada para aquela redistruibuição, 
a métrica 100(cem microsegundos) diz respeito ao tempo máximo de timeout caso a conexão esteja lenta,
a métrica 255 diz respeito a porcentagem de confiabilidade que terá a redistribuição, lembrando que 255 é equivalente a 100% de confiabilidade
a métrica 1 diz respeito carregamento
a métrica 1500 diz respeito ao MTU, que é equivalente 

## OSPF
```
EN
CONF T
HOST REDISTRIBUTE

INT S0/2
NO SH
IP AD 3.0.0.2 255.0.0.0
BAND 512
CLOCK R 128000
DESC WAN REDISTRIBUTE-OSPF

EXIT
EXIT
SH R
COPY R ST
```

## REDISTRIBUTE OSPF
```
EN
CONF T

ROUTER OSPF 100
NO AUTO-SUMMARY
NET 1.0.0.0 0.255.255.255 AREA 0
NET 2.0.0.0 0.255.255.255 AREA 0
NET 3.0.0.0 0.255.255.255 AREA 0
NET 4.0.0.0 0.255.255.255 AREA 0

REDISTRIBUTE RIP METRIC 200 SUBNETS
REDISTRIBUTE STATIC METRIC 200 SUBNETS
REDISTRIBUTE EIGRP 1 METRIC 200 SUBNETS
```
obs: a métrica 200 SUBNETS diz respeito a quantidade máxima de redes que ele vai alcançar

## ESTÁTICO
```
EN
CONF T
HOST REDISTRIBUTE

INT S0/3
NO SH
IP AD 4.0.0.2 255.0.0.0
BAND 512
CLOCK R 128000
DESC WAN REDISTRIBUTE-ESTATICO

EXIT
EXIT
SH R
COPY R ST
```

## REDISTRIBUTE ESTÁTICO
```
EN
CONF T

IP ROUTE 172.10.0.0 255.255.0.0 SERIAL 0/0
IP ROUTE 172.20.0.0 255.255.0.0 SERIAL 0/1
IP ROUTE 172.30.0.0 255.255.0.0 SERIAL 0/2
IP ROUTE 172.40.0.0 255.255.0.0 SERIAL 0/3
```