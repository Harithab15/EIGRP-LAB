#  Enhanced Interior Gateway Routing Protocol Lab- EVE-NG

This lab demonstrates **EIGRP (Enhanced Interior Gateway Routing Protocol)** configuration using **5 routers** and **2 PCs** in the EVE-NG environment. It simulates redundant paths and validates EIGRP convergence, route learning, and failover behavior.

---


##  Topology Overview


- **Routers:** R1 to R5 (with FastEthernet or GigabitEthernet interfaces)
- **PCs:** PC1 (connected to R1), PC2 (connected to R5)
- **Routing Protocol:** EIGRP
- **Autonomous System (AS):** 10

---
PC1 --- R1 --- R2 ---
| |
| | R5 --- PC2
| | /
R4 --- R3 --/

##  Lab Topology
![Lab Screenshot](./eigrp_lab_topology.jpeg)
##  Lab Requirements

- EVE-NG Community or Pro Edition
- Cisco IOS image (e.g., `c3725-adventerprisek9-mz.124-15.T14.bin`)
- Enable interface-level connectivity
- EIGRP must advertise all internal networks
- Full connectivity from PC1 to PC2 after convergence

---

##  IP Addressing Table

| Device | Interface | IP Address     | Connected To |
|--------|-----------|----------------|---------------|
| PC1    | eth0      | 192.168.1.10/24 | R1 (e0/0)     |
| R1     | e0/0      | 192.168.1.1/24  | PC1           |
| R1     | e0/1      | 10.0.12.1/30    | R2 (e0/0)     |
| R1     | e0/2      | 10.0.13.1/30    | R3 (e0/0)     |
| R1     | e1/0      | 10.0.14.1/30    | R4 (e0/0)     |
| R2     | e0/0      | 10.0.12.2/30    | R1            |
| R2     | e0/1      | 10.0.25.1/30    | R5 (e0/0)     |
| R3     | e0/0      | 10.0.13.2/30    | R1            |
| R3     | e0/1      | 10.0.35.1/30    | R5 (e0/1)     |
| R4     | e0/0      | 10.0.14.2/30    | R1            |
| R4     | e0/1      | 10.0.45.1/30    | R5 (e0/2)     |
| R5     | e0/0      | 10.0.25.2/30    | R2            |
| R5     | e0/1      | 10.0.35.2/30    | R3            |
| R5     | e0/2      | 10.0.45.2/30    | R4            |
| R5     | e0/3      | 192.168.2.1/24  | PC2           |
| PC2    | eth0      | 192.168.2.10/24 | R5 (e0/3)     |

---

## Step-by-Step Configuration

###  PC1 & PC2
```bash
# PC1
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1

# PC2
IP Address: 192.168.2.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.2.1

```

### Router R1
```bash
hostname R1
interface e0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
interface e0/1
 ip address 10.0.12.1 255.255.255.252
 no shutdown
interface e0/2
 ip address 10.0.13.1 255.255.255.252
 no shutdown
interface e1/0
 ip address 10.0.14.1 255.255.255.252
 no shutdown

router eigrp 10
 network 192.168.1.0
 network 10.0.0.0
 no auto-summary
 ```

 ### Router R2
```bash
hostname R2
interface e0/0
 ip address 10.0.12.2 255.255.255.252
 no shutdown
interface e0/1
 ip address 10.0.25.1 255.255.255.252
 no shutdown

router eigrp 10
 network 10.0.0.0
 no auto-summary
```

### Router R3
```bash
 hostname R3
interface e0/0
 ip address 10.0.13.2 255.255.255.252
 no shutdown
interface e0/1
 ip address 10.0.35.1 255.255.255.252
 no shutdown

router eigrp 10
 network 10.0.0.0
 no auto-summary

```
### Router R4
```bash
hostname R4
interface e0/0
 ip address 10.0.14.2 255.255.255.252
 no shutdown
interface e0/1
 ip address 10.0.45.1 255.255.255.252
 no shutdown

router eigrp 10
 network 10.0.0.0
 no auto-summary

```
### Router R5

```bash
hostname R5
interface e0/0
 ip address 10.0.25.2 255.255.255.252
 no shutdown
interface e0/1
 ip address 10.0.35.2 255.255.255.252
 no shutdown
interface e0/2
 ip address 10.0.45.2 255.255.255.252
 no shutdown
interface e0/3
 ip address 192.168.2.1 255.255.255.0
 no shutdown

router eigrp 10
 network 192.168.2.0
 network 10.0.0.0
 no auto-summary

```

### Verification Commands

On each router:
```bash
# View EIGRP neighbors
show ip eigrp neighbors

# Check EIGRP routes
show ip route eigrp
# Test connectivity (e.g., from PC1 to PC2)
ping 192.168.2.10
```
##  Learning Outcomes

Configure and verify EIGRP on Cisco routers

Understand multi-path EIGRP routing

Observe failover routing behavior

Enhance troubleshooting and verification skills




