
Principles of network layer addressing and routing.

[[James Kurose, Keith Ross - Computer Networking_ A Top-Down Approach, 7th Edition.pdf#page=348]]

##  Network layer:

The **network layer** is the third layer in the **OSI model**. Its main function is to:

- **Deliver packets from the source to the destination** across multiple networks (also called internetworking).
    
- **Handle logical addressing**, typically using **IP addresses**.
    
- **Route data** by determining the best path through the network (routing).
    
- **Fragment and reassemble packets** if necessary.

![[network_layer.png]]
						(host, router network layer functions)
## Key Responsibilities:

- **Logical addressing** (e.g., IP addressing)
    
- **Routing** (finding the best path to the destination)
    
- **Packet forwarding** (move packets from router’s input to appropriate output)
    
- **Packet fragmentation and reassembly** (the process of breaking down large packets into smaller ones for transmission and then rebuilding them at the destination)
    
## Protocols:

- **IP (Internet Protocol)** – IPv4, IPv6
- **ICMP** (Internet Control Message Protocol)
- **Ethernet & WIFI**
- **Others**..
_________
##  Data vs Control Plane

![[Data-vs-Control-Plane.png]]

| Feature                   | Data Plane                                                                                              | Control Plane                                                                                           |
| ------------------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Function**              | - Forwards data packets<br>- Determines how datagram arriving on input port is forwarded to output port | - Determines routing decisions<br>- Routes datagrams from source to destination across multiple routers |
| **Role**                  | Execution (actual packet forwarding)                                                                    | Decision-making (path selection)                                                                        |
| **Operation**             | Local, per-router function                                                                              | Network-wide logic                                                                                      |
| **How it works**          | Uses forwarding table + values in arriving packet header                                                | Computes forwarding tables and updates them                                                             |
| **Location**              | Implemented in router hardware (fast path)                                                              | Implemented in router CPU or remote servers (SDN)                                                       |
| **Speed**                 | High-speed                                                                                              | Slower (complex computation)                                                                            |
| **Protocols involved**    | IP, Ethernet, MPLS                                                                                      | OSPF, RIP, BGP, SDN protocols                                                                           |
| **Control Approaches**    | N/A                                                                                                     | Traditional (in-router) or SDN (external server)                                                        |
| **Awareness of topology** | No                                                                                                      | Yes                                                                                                     |
| **Customization**         | Limited                                                                                                 | Highly customizable                                                                                     |

> [!Note] The **data plane** handles forwarding at each router locally, while the **control plane** decides the global path.
> The **data plane** moves packets; the **control plane** decides how to move them.


## IP Addressing

| Feature                            | IPv4                                             | IPv6                                                      |
| ---------------------------------- | ------------------------------------------------ | --------------------------------------------------------- |
| **Address Length**                 | **32 bits**                                      | **128 bits**                                              |
| **Address Format**                 | Decimal, dotted format (e.g., 192.168.1.1)       | Hexadecimal, colon-separated (e.g., 2001:0db8::1)         |
| **Address Space**                  | ~4.3 billion addresses                           | 3.4 × 10³⁸ addresses (vastly larger)                      |
| **Header Size**                    | 20 bytes (without options)                       | 40 bytes (fixed size)                                     |
| **Header Complexity**              | **Variable length (with options), more complex** | **Simplified, fixed-length header**                       |
| **Security**                       | **Optional (IPSec is optional)**                 | **Mandatory support for IPSec**                           |
| **How does a host get IP address** | Manual or From a server (use DHCP plug and play) | Auto-configuration (Stateless Address Auto Configuration) |
| **Fragmentation**                  | Routers and sender can fragment                  | Only sender can fragment                                  |
| **Checksum**                       | **Present**                                      | **Removed to speed up processing**                        |
| **NAT Usage**                      | Widely used due to limited address space         | Not needed (huge address space)                           |
| **Compatibility**                  | **Legacy system, still widely used**             | **Designed to replace IPv4 **                             |

### Fragmentation 
When a router receives a packet that's larger than the allowed transmission of the outgoing link, it splits the datagram into smaller pieces (fragments). These fragments are sent separately and reassembled at the final destination.

### NAT
![[NAT.png]]
Allows multiple devices on a private network to share one public IP address when accessing the Internet, uses NAT translation table to keep track of IPs.


### DHCP 
![[DHCP.png]]

 Dynamic Host Configuration Protocol, allows host to dynamically obtain its IP address from network server when it joins network.



### Why IPv6 and not IPv4?

![[IP_datagram.png]]
- **Address exhaustion**: IPv4 has a limited number of IP addresses (about 4.3 billion), and with the growing number of internet-connected devices, we are running out of IPv4 addresses.


- **Larger address space**: IPv6 provides a vastly larger address space (approximately 340 undecillion addresses), allowing for an almost unlimited number of unique addresses for devices.
    
- **Improved routing efficiency**: IPv6 simplifies network routing, making it more efficient and scalable.
    
- **Better security**: IPv6 was designed with security in mind, including mandatory support for IPsec, which helps secure communications (IPSec is a must!).
    
- **Better performance**: IPv6 has features like auto-configuration and simplified header structure, leading to potentially better performance compared to IPv4.
    
- **Support for modern technologies**: IPv6 supports emerging technologies, such as the Internet of Things (IoT), that require a large number of IP addresses and constant connectivity.



## IPv4 Addressing & Subnets
###  **Understanding IP Addressing:**

- **IP Address**: An IP address is made up of 4 octets (32 bits for IPv4, 128 bits for IPv6). Each octet has 8 bits, which means there are 256 possible values (0–255) per octet.
    
- **Subnet Mask**: A subnet mask divides the IP address into the network and host portions. For IPv4, it's typically written in dotted decimal notation (e.g., `255.255.255.0`).


### Subnet types

- **Class A**: `0.0.0.0` to `127.255.255.255` (default subnet mask `255.0.0.0`)
    
- **Class B**: `128.0.0.0` to `191.255.255.255` (default subnet mask `255.255.0.0`)
    
- **Class C**: `192.0.0.0` to `223.255.255.255` (default subnet mask `255.255.255.0`)
    
- **Class D (Multicast)**: `224.0.0.0` to `239.255.255.255`
    
- **Class E (Reserved)**: `240.0.0.0` to `255.255.255.255`



**CIDR (Classless Inter-Domain Routing) notation** tells you **how many bits are used for the network portion** of the address.

- `/24` means **the first 24 bits** of the IP address are used for the **network**, and the remaining **8 bits** are used for the **host** part.
    
- This is equivalent to the subnet mask `255.255.255.0`.
    

|CIDR|Subnet Mask|Hosts per Subnet|Notes|
|---|---|---|---|
|/8|255.0.0.0|16,777,214|Very large networks (Class A)|
|/16|255.255.0.0|65,534|Medium-sized networks (Class B)|
|/24|255.255.255.0|254|Common for home/small networks|
|/30|255.255.255.252|2|Point-to-point links|

🧠 **Quick trick**:

- The smaller the CIDR number, the more host addresses are available.
    
- The larger the CIDR number (closer to `/32`), the fewer hosts you can have in that subnet.
    

> Example:  
> `192.168.1.0/24` means:
> 
> - Network portion: first 24 bits (`192.168.1`)
>     
> - Host portion: last 8 bits (can range from `1` to `254`)
>     
> - Total usable hosts: `2⁸ - 2 = 254`
>     

### **Subnet Calculation Steps:**


**a.** **Determine the Network Address:**

To find the network address, perform a **bitwise AND** operation between the IP address and the subnet mask. For example:

IP Address: `192.168.1.10` Subnet Mask: `255.255.255.0`

Convert them to binary:

- `192.168.1.10` = `11000000.10101000.00000001.00001010`
- `255.255.255.0` = `11111111.11111111.11111111.00000000`

Perform the **AND** operation:

```
11000000.10101000.00000001.00001010
AND
11111111.11111111.11111111.00000000
-----------------------------------
11000000.10101000.00000001.00000000
```

The result is `192.168.1.0`, which is the **network address**.
> [!Important] TRICK: the network address is always the FIRST available address. Usually xxx.xxx.xxx.0

 **b.** **Calculate the Broadcast Address:**

To find the broadcast address, invert the subnet mask and **OR** it with the network address.

Inverting the subnet mask `255.255.255.0` gives `0.0.0.255`. Now OR it with the network address `192.168.1.0`:
```
11000000.10101000.00000001.00000000
OR
00000000.00000000.00000000.11111111
-----------------------------------
11000000.10101000.00000001.11111111
```
The result is `192.168.1.255`, which is the **broadcast address**.

> [!Important] TRICK: the broadcast address is always the LAST available address. Usually xxx.xxx.xxx.255

**c. Determine the Number of Hosts:**

The number of hosts per subnet is determined by the number of bits left in the subnet mask.

For `255.255.255.0` (or `/24`):

- For every 255 -> 8 bits are used.
- 32 bits (total bits) - 24 bits (used bits) = 8 bits for the host part.
- Number of hosts available = `2^8 - 2 = 254` (we subtract 2 for the network and broadcast addresses).

###  **Subnetting:**

If you want to create smaller subnets, you can "borrow" bits from the host portion and adjust the subnet mask accordingly.

For example, if you take a `/24` network (`255.255.255.0`) and borrow 2 bits for the subnet:

New subnet mask: `255.255.255.192` (or `/26`).

- The new subnet mask allows for `2^2 = 4` subnets, and each subnet will have `2^6 - 2 = 62` hosts.

### **IP Address Range:**

For each subnet, the usable IP range is from (network address + 1) to (broadcast address - 1).

For the subnet `192.168.1.0/24`:
`(Remember: /24 means that our subnet is 255.255.255.0 which leaves us with 254 hosts)`

- Network address: `192.168.1.0`
- Usable range: `192.168.1.1` to `192.168.1.254`
- Broadcast address: `192.168.1.255`


---
> [!Important] Author: Asmaa Alazmi
> Note: feel free to change the diagrams into prettier ones 
> `(I'm not getting paid enough to care about aesthetics)`.
> ANOTHER NOTE: this review is meant as a mind refresher, written with the assumption that the reader knows the basics (e.g. mac address, switches.. servers.. etc).

> [!Question] Reviewer Note..
> you're getting PAID?!?!