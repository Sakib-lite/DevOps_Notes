
# Network Setup Overview

![Screenshot (36)](https://github.com/Sakib-lite/DevOps_Notes/assets/77607002/ec7af980-ac3f-43cf-9f22-cafc02ac4563)

## Host A

- **IP**: 10.10.10.10
- **MAC**: 1111.2222.3333
- **Default Gateway (DG)**: 10.10.10.1
- **DNS**: 10.10.100.1

## Switch 1

- **IP**: 10.10.10.1
- **MAC**: 4444.5555.6666

## Router A

- **IP**: 10.10.11.1
- **MAC**: 5555.6666.7777

## Router B

- **IP**: 10.10.11.2
- **MAC**: 6666.7777.8888

## Switch 2

- **IP**: 10.10.12.1
- **MAC**: 7777.8888.9999
- **Connects**: Router B to the Web Server

## Web Server

- **IP**: 10.10.12.10
- **MAC**: 2222.3333.4444
- **DG**: 10.10.12.1

## Switch 3

- **IP**: 10.10.100.1
- **MAC**: 8888.9999.AAAA

## DNS Server

- **IP**: 10.10.100.10
- **MAC**: 3333.4444.5555
- **DG**: 10.10.100.1

## DNS Query from Host A to DNS Server

### Host A to Switch 1

1. Host A creates a DNS query packet with the destination IP of 10.10.100.10 (DNS Server).
2. Host A looks up its ARP table for the MAC address of the default gateway (10.10.10.1). If it doesn't have it, it sends an ARP request.
3. Host A sends the packet to Switch 1 with the destination MAC address of the switch interface (MAC 4444.5555.6666).

### Switch 1 to Router A

4. Switch 1 forwards the packet to Router A (10.10.11.1) using its routing table. The packet is sent out on the interface connected to Router A (MAC 5555.6666.7777).

### Router A to Switch 3

5. Router A receives the packet, checks its routing table, and forwards the packet to Switch 3 using the interface connected to the 10.10.100.0/24 network (MAC 8888.9999.AAAA).

### Switch 3 to DNS Server

6. Switch 3 forwards the packet to the DNS Server based on the destination IP.

### DNS Server Response

7. The DNS Server responds with the IP address of <www.flackbox.com> (10.10.12.10).
8. The response packet travels back through Switch 3, Router A, and Switch 1 to Host A.

## HTTP Request from Host A to Web Server

### Host A Creates HTTP GET Request

1. Host A creates an HTTP GET request for <www.flackbox.com> (IP 10.10.12.10).
2. Host A looks up the ARP table for the MAC address of the default gateway (Switch 1, MAC 4444.5555.6666). If it doesn’t have it, it sends an ARP request.
3. Host A sends the packet to Switch 1 with the destination MAC address of the switch interface (MAC 4444.5555.6666).

### Switch 1 to Router A

4. Switch 1 forwards the packet to Router A based on the routing table.

### Router A to Router B

5. Router A receives the packet, checks its routing table, and forwards the packet to Router B's interface (IP 10.10.11.2, MAC 6666.7777.8888).

### Router B to Switch 2

6. Router B forwards the packet to Switch 2 (IP 10.10.12.1, MAC 7777.8888.9999).

### Switch 2 to Web Server

7. Switch 2 forwards the packet to the Web Server based on the MAC address (2222.3333.4444).

### Web Server Response

8. The Web Server processes the HTTP request and sends back the response.
9. The response packet travels back through Switch 2, Router B, Router A, Switch 1 to Host A.

## Summary of Hops

### DNS Query

- **Host A** → **Switch 1** (10.10.10.1, MAC 4444.5555.6666)
- **Switch 1** → **Router A** (10.10.11.1, MAC 5555.6666.7777)
- **Router A** → **Switch 3** (10.10.100.1, MAC 8888.9999.AAAA)
- **Switch 3** → **DNS Server** (10.10.100.10, MAC 3333.4444.5555)

### HTTP Request

- **Host A** → **Switch 1** (10.10.10.1, MAC 4444.5555.6666)
- **Switch 1** → **Router A** (10.10.11.1, MAC 5555.6666.7777)
- **Router A** → **Router B** (10.10.11.2, MAC 6666.7777.8888)
- **Router B** → **Switch 2** (10.10.12.1, MAC 7777.8888.9999)
- **Switch 2** → **Web Server** (10.10.12.10, MAC 2222.3333.4444)

`Each time a packet is forwarded by a router or switch, it is considered to have made one hop. The number of hops a packet takes is a measure of how many intermediary devices (routers or switches) the packet has to traverse to reach its destination.`