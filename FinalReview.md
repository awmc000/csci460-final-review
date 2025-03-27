
# CSCI 460 Final Exam Study Guide

## Table of Contents

1. [Data Link Layer](#data-link-layer)  
   - [Data Link Layer Protocols for Flow Control](#data-link-layer-protocols-for-flow-control)  
     - [Stop and Wait Protocol](#stop-and-wait-protocol)  
     - [Sliding Window Protocol](#sliding-window-protocol)  
     - [Sliding Window Protocol with Go Back N](#sliding-window-protocol-with-go-back-n)  
     - [Sliding Window Protocol with Selective Repeat](#sliding-window-protocol-with-selective-repeat)  
     - [1-bit Sliding Window Protocol](#1-bit-sliding-window-protocol)  
2. [Medium Access Control Sublayer](#medium-access-control-sublayer)  
   - [Channel Allocation Problem](#channel-allocation-problem)  
   - [Multiple Access Protocols](#multiple-access-protocols)  
     - [Pure ALOHA](#pure-aloha)
     - [Slotted ALOHA](#slotted-aloha)  
     - [Carrier Sense Multiple Access (CSMA)](#carrier-sense-multiple-access-csma)  
     - [CSMA with Collision Detection (CSMACD)](#csma-with-collision-detection-csmacd)  
     - [Binary Exponential Backoff Algorithm in CSMA/CD](#binary-exponential-backoff-algorithm-in-csmacd)  
   - [Classic Ethernet](#classic-ethernet)  
   - [Switched and Gigabit Ethernet](#switched-and-gigabit-ethernet)  
3. [Network Layer](#network-layer)  
   - [Store and Forward Packet Switching](#store-and-forward-packet-switching)  
   - [Datagrams](#datagrams)  
   - [Routers](#routers)  
   - [Routing Algorithms](#routing-algorithms)  
     - [Distance Vector Routing](#distance-vector-routing)  
     - [Shortest Path Computation](#shortest-path-computation)  
     - [Link State Routing](#link-state-routing)  
   - [Internet Protocol (IP)](#internet-protocol-ip)  
     - [IP Header](#ip-header)  
     - [Classful and CIDR IP Addresses](#classful-and-cidr-ip-addresses)  
     - [Sub-netting, ISP Assignment, Aggregation, and Longest Prefix Matching](#sub-netting-isp-assignment-aggregation-and-longest-prefix-matching)  
     - [Network Address Translation (NAT)](#network-address-translation-nat)  
     - [Address Resolution Protocol (ARP)](#address-resolution-protocol-arp)  
     - [Dynamic Host Configuration Protocol (DHCP)](#dynamic-host-configuration-protocol-dhcp)  
     - [Internet Control Message Protocol (ICMP)](#internet-control-message-protocol-icmp)  
4. [Transport Layer](#transport-layer)  
   - [User Datagram Protocol (UDP)](#user-datagram-protocol-udp)  
   - [Transport Control Protocol (TCP)](#transport-control-protocol-tcp)  
     - [TCP Segment Header](#tcp-segment-header)  
     - [TCP Connection](#tcp-connection)  
     - [TCP Flow Control](#tcp-flow-control)  
     - [TCP Congestion Control](#tcp-congestion-control)  
     - [TCP Retransmission Timer](#tcp-retransmission-timer)  
5. [Application Layer](#application-layer)  
   - [DNS](#dns)  
   - [HTTP](#http)  
   - [FTP](#ftp)  
6. [POSIX system calls and the library functions covered in Project2](#posix-system-calls-and-the-library-functions-covered-in-project2)  
7. [Exam Concepts Review](#exam-concepts-review)  
   - [Sliding Window Protocol Calculations](#sliding-window-protocol-calculations)  
   - [p-persistent CSMA/CD Protocol](#p-persistent-csmacd-protocol)  
   - [Token Ring Protocol](#token-ring-protocol)  
   - [Ethernet Padding](#ethernet-padding)  
   - [Distance Vector Initialization](#distance-vector-initialization)  
   - [Link State Packet Creation](#link-state-packet-creation)  
   - [Routing Table from Link State Packets](#routing-table-from-link-state-packets)  
   - [Subnetting Practice](#subnetting-practice)  
   - [CIDR Address Aggregation](#cidr-address-aggregation)  
   - [NAT Translation and Table Updates](#nat-translation-and-table-updates)  
   - [ICMP Message Usages](#icmp-message-usages)  
   - [ARP and Proxy ARP](#arp-and-proxy-arp)  
   - [DHCP Workflow](#dhcp-workflow)  
   - [TCP Congestion Window Growth](#tcp-congestion-window-growth)  
   - [UDP Checksum Calculation](#udp-checksum-calculation)  
   - [TCP Buffering and Segment Exchange](#tcp-buffering-and-segment-exchange)  
   - [Steps in Web Page Fetching](#steps-in-web-page-fetching)  
   - [Recursive vs Iterative DNS Queries](#recursive-vs-iterative-dns-queries)  
   - [System Call Usages](#system-call-usages)  

---

## Data Link Layer

### Data Link Layer Protocols for Flow Control

#### Stop and Wait Protocol

#### Sliding Window Protocol

#### Sliding Window Protocol with Go Back N

#### Sliding Window Protocol with Selective Repeat

#### 1-bit Sliding Window Protocol

---

## Medium Access Control Sublayer

### Channel Allocation Problem

- The **channel allocation problem** is, how do we allocate
a single broadcast channel between different users?
- The channel might be the electromagnetic spectrum in a
certain region, or an optical fibre connection.
- It is also called the **multiple access problem**, and
attempts to rectify it, **multiple access protocols**.
- The ideal Multiple Access Protocol would have the following
characteristics.
  - If there is only one sender, they would get the entire
  bandwidth, wasting none.
  - If there are many senders, they would get equal access to
  the bandwidth, privileging or throttling none.
  - It should be simple, and cheap to implement.
- Multiple access protocols fall into one of three families.
- The first is **channel partitioning**.
  - Time Division Multiplexing divides the passage of time into
  time frames within which there are time slots for individual
  nodes to broadcast during.
  - The limiting factor in TDM is *rate*.
  - Frequency Division Multiplexing divides the EM spectrum,
  like how FM radio stations are laid out.
  - The limiting factor in FDM is *bandwidth*.
- The second is **random access**.
  - Pure ALOHA.
  - Slotted ALOHA has $L$ bit-time frames.
  - Slots are $L/R$ s.
  - Nodes can send at the start of a slot.
  - Nodes are synchronized so they all know when
  slots and frames are.
  - All nodes can detect a collision.
- The third is **taking turns**.
  - In the polling protocol, the master node polls each node
  round-robin to tell it to send some number of frames.
  - A downside is that there is delay that comes with polling.
  - Benefits are that there are no empty slots or collisions.
  - In the token passing protocol, there is no master node,
  and a special token frame is exchanged between nodes in some
  fixed order.
  - When a node gets the token, it forwards it if it has no
  frames to send. If it has frames to send, it sends them,
  up to some maximum, then forwards the token.

### Multiple Access Protocols

#### Pure ALOHA
- In Pure ALOHA, nodes transmit whenever they have data to send.
- After each node sends its frame to the central computer, the
central compute rebroadcasts the frame to all of the stations.
- A sending node listens to the "echo" to see if its frame got
through.
- If the frame was destroyed, the sender waits a random time and
sends it again.
- The waiting time is random so that the same frames do not 
collide over and over.
- Throughput is maximized if frames are the same size.
- There is tremendous potential for collisions in Pure ALOHA.
- If the first bit of a new frame overlaps with the last bit of a
frame before it, both frames will be destroyed.

#### Slotted ALOHA
- Slotted ALOHA divides time into discrete **slots**, which are
each the length it takes to transmit one frame.
- The slot boundaries can be synchronized by having a special
station transmit a heartbeat at the start of each interval,
acting as a clock.
- A station is not permitted to send at any time. It is required
to wait for the beginning of the next slot.
- The period of vulnerability for frames is halved.
- At best, slotted ALOHA has a 37% success rate.
- 37% of slots are empty and 26% are collisions.


#### Carrier Sense Multiple Access (CSMA)
- 1-Persistent CSMA works as follows.
  - When a node has data to send, it listens to the channel to
  see if anyone else is transmitting.
  - If the channel is free, the node sends its frame.
  - If the channel is busy, the node waits until it is free.
  - Then it sends its frame.
  - If there is a collision, the node waits a random time
  and starts over.
  - This is called 1-persistent because, when the node finds
  the channel empty, it has a probability of 1 that it will send.
- Non-persistent CSMA works as follows.
  - A node senses the channel when it wants to send a frame.
  - If the channel is free, the node sends its frame.
  - If the channel is busy, the node waits a random time and
  starts over.
  - Fewer collisions, better utilization, and longer delays 
  than 1-persistent CSMA.
- P-persistent CSMA
  - A node senses the channel when it wants to send a frame.
  - If it is idle, then there is probability $p$ that it transmits.
  - There is probability $q = 1 - p$ that it defers to next slot.
  - This process repeats until either the frame is transmitted,
  or another node begins transmitting.
  - In the event another node began transmitting, the node waits
  a random time and starts again.
- Propagation delay affects collision rate.
- If one node has just started sending, another station may
sense the channel and not yet have realized the first node is
sending.


#### CSMA with Collision Detection (CSMA/CD)

#### Binary Exponential Backoff Algorithm in CSMA/CD

### Classic Ethernet

### Switched and Gigabit Ethernet

---

## Network Layer

### Store and Forward Packet Switching

### Datagrams

### Routers

### Routing Algorithms

#### Distance Vector Routing

#### Shortest Path Computation

#### Link State Routing

### Internet Protocol (IP)

#### IP Header

#### Classful and CIDR IP Addresses

#### Sub-netting, ISP Assignment, Aggregation, and Longest Prefix Matching

#### Network Address Translation (NAT)

#### Address Resolution Protocol (ARP)

#### Dynamic Host Configuration Protocol (DHCP)

#### Internet Control Message Protocol (ICMP)

---

## Transport Layer

### User Datagram Protocol (UDP)

### Transport Control Protocol (TCP)

#### TCP Segment Header

#### TCP Connection

#### TCP Flow Control

#### TCP Congestion Control

#### TCP Retransmission Timer

---

## Application Layer

### DNS

### HTTP

### FTP

---

## POSIX system calls and the library functions covered in Project2

---

## Exam Concepts Review

### Sliding Window Protocol Calculations

### p-persistent CSMA/CD Protocol

### Token Ring Protocol

### Ethernet Padding

### Distance Vector Initialization

### Link State Packet Creation

### Routing Table from Link State Packets

### Subnetting Practice

### CIDR Address Aggregation

### NAT Translation and Table Updates

### ICMP Message Usages

### ARP and Proxy ARP

### DHCP Workflow

### TCP Congestion Window Growth

### UDP Checksum Calculation

### TCP Buffering and Segment Exchange

### Steps in Web Page Fetching

### Recursive vs Iterative DNS Queries

### System Call Usages

---

