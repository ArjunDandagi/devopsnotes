 
# week 1 

## OSI LAYER 

```
7. application (app)
6. presentation (.pdf , .csv , how to present data)
5. Session ( socks , connections, channel)
4. Transport  ( TCP , UDP packets)
3. Network ( IP packets/ segments)
2. Data link ( Data frames). ----> unicast , multicast , broadcast 
1. physical ( wire , cables , electricity flowing) - Twisted pair cables , optical fibers,
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


# Week 4 

Network services 


DNS: (network layer, layer 3 )
 reading binary is not good , not numbers also as humans find it hard to remember  - hence DNS - name system to ip 
 caching , recursive resolver , SOA ,TLD , ROOT etc 
 
 
DHCP: (app layer) -uses UDP heavily 
  client 0.0.0.0:68 ---> 255.255.255.255:67 ( give me ip )
  server  ----> 255.255.255.255:68 [data: 12.12.12.12 ] ( mac address is included. so the client know it was intended for it)  
  client 0.0.0.0:68 --> 255.255.255.255:67  ( i am ok to assign the ip 12.12.12.12 )
  server ---> 255.255.255.67. assigned
  client 12.12.12.12 - ok 
  
 IPV4 address allocation: 
 
 For some time now, the IANA has primarily been responsible for assigning address blocks to the five regional internet registries or RIRs. The five RIRs are:

AFRINIC, which serves the continent of Africa
ARIN, which serves the United States, Canada and parts of the Caribbean
APNIC, which is responsible for most of Asia, Australia and New Zealand and Pacific Island nations
LACNIC, which covers Central and South America and any parts of the Caribbean not covered by ARIN
RIPE, which serves Europe, Russia,the Middle East, and portions of Central Asia
  
 VPN: tunneling  ( network layer)
 
 proxy : in front of client - 
  

# Week 5 
  dial-up POTS (Plain Old Telephone System) , PSTN( Public Switched Telephone Network. ) , modems 
  baud rate (bits that can be sent across a telephone line every second.) . 
  
  Broadband 
  
 1. T-carrier => T1 (transmission system 1) -> data 1.544 mb/s , 
 2. DSL ( digital subscriber line) - dailup and internet in the same line (ADSL , assymetric DSL - upload and download speed is different) (SDSL -same down and upload )
3. cable broadband - shared b/w technology - cable modem 
4. Fiber connections - FTTX - FTTN(neighbourhood) , FTTB ( building, business) ,FTTH (home) ,FTTP( premesis)

WAN - wide area network -- Frame Relay:, HDLC (High-Level Data Link Control) , and ATM ( Asynchronous Transfer Mode)
p2p VPN  - 

 ## Wireless networking
 IEEE 802.11 - 802.11nc is the highest 
 frame control field , duration field,src , dest , recv , transmitter , sequence control, 
 5Ghz band are almost always faster, but have less of a range. Most of the 2.4Ghz networks are slightly slower and more susceptible to interference, but usually cover a larger area
 
 ad-hoc network 
 wireless LAN (wlan) 
 mesh network  ( many access points are interlinked ) - making edge nodes connected to all 
 
 channel - help alleviate collusion domain - 2.4gHz - some auto sense channel congestion
 
 wireless security -  using security header 
 cellular networking - 
 
 # Week 6 
 
 ping , traceroute , nslookup ( set type=MX, set debug are some options)
 public DNS - Level 3 communication , GOOGLe ( 8.8.8.8 , 8.8.4.4) 
 public cloud, storage , 
 
 
 IPV6- 
 FF00 - multicast 
 FE80 - link local unicast 
 
 header - version - class (8) - flow label(20) - payload length (16) - next header () - hop limit ( ) - src(128) - dest (128) - payload
 
 
 
 
 [Coursera ](https://www.coursera.org/learn/computer-networking)
 
 
 
 




