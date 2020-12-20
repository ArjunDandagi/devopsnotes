# week 1 

## OSI LAYER 

```
7. application (app)
6. presentation (.pdf , .csv , how to present data)
5. Session ( socks , connections, channel)
4. Transport  ( TCP , UDP packets)
3. Network ( IP packets/ segments)
2. Data link ( Data frames). ----> unicast , multicast , broadcast 
1. physical ( wire , cables , electricity flowing) - Twisted pair cables , optical fibres,
```

## introduction to TCP/IP stack and OSI Layers
Hubs - Layer 1 devices 
Switches - Layer 2 Devices ( understands MAC addresses) - used in LAN heavily 
Routers -  Layer 3 Devices ( IP address) 

# Week 2 

## Network layer 
IP Addresses , IP datagrams , ARP ( address resolution protocol) 
subnetting , subnetting mask , CIDR , 

Basic routing concepts ,how does router decide where to forward the packet 
they learn the best possible way to push 

1. interior Gateway protocols - link state,  distance-vector protocal   - OSPF , IS-IS , RIP ( routing information protocal) 
2. Exterior Gateway protocols - BGP , EGP 

# week 3 

TCP layer , 

Multiplexing - combining data from different port traffic and passing on data
Demultiplexing - extracting data and sending it to proper application port 

TCP segment dissect:
HEADER  - SRC port, DEST port  , seq no ,ack no , header length, control flags( 6 bits) , window(16),checksum,urgent(16),options, padding


TCP control flags 
URG, ACK  ,PSH ( push),RST ( reset),SYN,FIN

socket  -instantiatio of an end-point in a potential TCP connection


## TCP socket states
SYN-SENT ( A syn is sent - ususally the client)
SYN_RECEIVED  ( the syn is received and a SYN/ACK is sent , usually the server ) 
ESTABLISHED
FIN_WAIT ( connection closed but app haven't closed the port yet)
CLOSE_WAIT ( the socket hasn't been closed , though the connection is over)
CLOSED 











