# NetPractice

Network configuration and troubleshooting exercises covering IP addressing, subnetting, routing, and TCP/IP protocols.

## Description

NetPractice is a practical networking project that teaches fundamental network concepts through hands-on exercises. You'll work with IP addressing, subnetting, routing tables, and network topology to solve various networking scenarios. This project builds essential knowledge for system administration and network engineering.

## Learning Objectives

- **IP Addressing**: Understanding IPv4 address structure and classes
- **Subnetting**: Network segmentation and subnet mask calculations
- **Routing**: Static routing and routing table configuration
- **Network Topology**: Understanding network design and connectivity
- **TCP/IP Stack**: Protocol layers and communication flow
- **Network Troubleshooting**: Identifying and resolving connectivity issues

## Topics Covered

### IP Addressing Fundamentals
- **IPv4 Address Structure**: 32-bit addressing system
- **Address Classes**: Class A, B, C network ranges
- **Private vs Public IP**: RFC 1918 private address spaces
- **Special Addresses**: Loopback, broadcast, network addresses
- **CIDR Notation**: Classless Inter-Domain Routing

### Subnetting Concepts
- **Subnet Masks**: Network and host portion identification
- **Variable Length Subnet Masking (VLSM)**: Efficient IP allocation
- **Subnet Calculations**: Network range, broadcast, and host addresses
- **Supernetting**: Route aggregation and summarization

### Routing Principles
- **Static Routing**: Manual route configuration
- **Default Routes**: Gateway of last resort
- **Route Tables**: Destination, gateway, and interface mapping
- **Longest Prefix Matching**: Route selection algorithm

## Network Scenarios

### Scenario 1: Basic Subnetting
```
Network: 192.168.1.0/24
Requirements: 4 subnets with at least 50 hosts each

Solution:
Subnet 1: 192.168.1.0/26   (hosts: .1 to .62)
Subnet 2: 192.168.1.64/26  (hosts: .65 to .126)
Subnet 3: 192.168.1.128/26 (hosts: .129 to .190)
Subnet 4: 192.168.1.192/26 (hosts: .193 to .254)
```

### Scenario 2: VLSM Implementation
```
Network: 10.0.0.0/16
Requirements: 
- 1 subnet for 1000 hosts
- 2 subnets for 500 hosts each
- 4 subnets for 100 hosts each

Solution:
Large subnet:  10.0.0.0/22    (1022 hosts)
Medium subnet: 10.0.4.0/23    (510 hosts)
Medium subnet: 10.0.6.0/23    (510 hosts)
Small subnet:  10.0.8.0/25    (126 hosts)
Small subnet:  10.0.8.128/25  (126 hosts)
Small subnet:  10.0.9.0/25    (126 hosts)
Small subnet:  10.0.9.128/25  (126 hosts)
```

### Scenario 3: Multi-Router Network
```
Router A Configuration:
Interface eth0: 192.168.1.1/24
Interface eth1: 10.0.1.1/30
Default route: 10.0.1.2

Router B Configuration:
Interface eth0: 10.0.1.2/30
Interface eth1: 172.16.1.1/24
Route to 192.168.1.0/24 via 10.0.1.1
```

## Practical Exercises

### Exercise 1: IP Configuration
Configure network interfaces with appropriate IP addresses:

```bash
# Host A
IP: 192.168.10.10
Netmask: 255.255.255.0
Gateway: 192.168.10.1

# Host B
IP: 192.168.10.20
Netmask: 255.255.255.0
Gateway: 192.168.10.1

# Router Interface
IP: 192.168.10.1
Netmask: 255.255.255.0
```

### Exercise 2: Routing Configuration
Set up routing between multiple networks:

```bash
# Router 1 Routing Table
Destination     Gateway         Interface
0.0.0.0/0       10.0.1.2        eth1
192.168.1.0/24  0.0.0.0         eth0
10.0.1.0/30     0.0.0.0         eth1

# Router 2 Routing Table
Destination     Gateway         Interface
192.168.1.0/24  10.0.1.1        eth0
172.16.1.0/24   0.0.0.0         eth1
10.0.1.0/30     0.0.0.0         eth0
```

### Exercise 3: Network Troubleshooting
Identify and fix connectivity issues:

**Problem**: Host A cannot ping Host B
**Diagnosis Steps**:
1. Check IP configuration
2. Verify subnet masks
3. Test local network connectivity
4. Check routing tables
5. Verify gateway configuration

## Subnetting Calculator

### Manual Calculation Method

```python
def calculate_subnet(network_ip, subnet_mask):
    """
    Calculate subnet information
    """
    # Convert IP and mask to binary
    ip_parts = [int(x) for x in network_ip.split('.')]
    mask_parts = [int(x) for x in subnet_mask.split('.')]
    
    # Calculate network address
    network = [ip_parts[i] & mask_parts[i] for i in range(4)]
    
    # Calculate broadcast address
    wildcard = [255 - mask_parts[i] for i in range(4)]
    broadcast = [network[i] | wildcard[i] for i in range(4)]
    
    # Calculate host range
    first_host = network.copy()
    first_host[3] += 1
    
    last_host = broadcast.copy()
    last_host[3] -= 1
    
    # Calculate number of hosts
    host_bits = sum(bin(x).count('1') for x in wildcard)
    num_hosts = (2 ** host_bits) - 2
    
    return {
        'network': '.'.join(map(str, network)),
        'broadcast': '.'.join(map(str, broadcast)),
        'first_host': '.'.join(map(str, first_host)),
        'last_host': '.'.join(map(str, last_host)),
        'num_hosts': num_hosts,
        'subnet_mask': subnet_mask
    }

# Example usage
result = calculate_subnet('192.168.1.0', '255.255.255.192')
print(f"Network: {result['network']}")
print(f"First Host: {result['first_host']}")
print(f"Last Host: {result['last_host']}")
print(f"Broadcast: {result['broadcast']}")
print(f"Number of Hosts: {result['num_hosts']}")
```

### CIDR Conversion Table

| CIDR | Subnet Mask     | Wildcard Mask  | Hosts |
|------|-----------------|----------------|-------|
| /24  | 255.255.255.0   | 0.0.0.255      | 254   |
| /25  | 255.255.255.128 | 0.0.0.127      | 126   |
| /26  | 255.255.255.192 | 0.0.0.63       | 62    |
| /27  | 255.255.255.224 | 0.0.0.31       | 30    |
| /28  | 255.255.255.240 | 0.0.0.15       | 14    |
| /29  | 255.255.255.248 | 0.0.0.7        | 6     |
| /30  | 255.255.255.252 | 0.0.0.3        | 2     |

## Network Design Principles

### Hierarchical Network Design
```
Core Layer (High-speed backbone)
    │
Distribution Layer (Routing and filtering)
    │
Access Layer (End device connectivity)
```

### IP Addressing Scheme
```
Core Network:     10.0.0.0/8
- Data Center:    10.1.0.0/16
- Branch Offices: 10.2.0.0/16
- Remote Sites:   10.3.0.0/16

Point-to-Point Links: 172.16.0.0/16
Management Network:   192.168.100.0/24
```

## Common Network Topologies

### Star Topology
```
    Host A
      │
   Switch ── Host B
      │
    Host C
```

### Mesh Topology
```
Router A ──── Router B
    │    \   /    │
    │     \ /     │
    │      X      │
    │     / \     │
    │    /   \    │
Router C ──── Router D
```

## Routing Protocols Overview

### Static Routing
- **Advantages**: Simple, predictable, secure
- **Disadvantages**: No automatic adaptation, manual configuration
- **Use Cases**: Small networks, stub networks

### Dynamic Routing
- **RIP**: Distance vector, hop count metric
- **OSPF**: Link state, cost-based metric
- **BGP**: Path vector, policy-based routing

## Network Security Concepts

### Access Control Lists (ACLs)
```bash
# Permit specific network
access-list 100 permit ip 192.168.1.0 0.0.0.255 any

# Deny specific host
access-list 100 deny ip host 192.168.1.100 any

# Permit all other traffic
access-list 100 permit ip any any
```

### Network Address Translation (NAT)
```bash
# Static NAT
ip nat inside source static 192.168.1.10 203.0.113.10

# Dynamic NAT
ip nat inside source list 1 pool PUBLIC_POOL

# PAT (Port Address Translation)
ip nat inside source list 1 interface FastEthernet0/0 overload
```

## Troubleshooting Tools

### Command Line Tools
```bash
# Ping - Test connectivity
ping 192.168.1.1

# Traceroute - Trace packet path
traceroute 8.8.8.8

# Netstat - Show network connections
netstat -rn

# ARP - Show/modify ARP table
arp -a

# Route - Show/modify routing table
route -n
```

### Network Analysis
```bash
# Check interface configuration
ifconfig eth0

# Monitor network traffic
tcpdump -i eth0

# Scan network
nmap -sn 192.168.1.0/24

# Check DNS resolution
nslookup google.com
dig google.com
```

## Practical Lab Scenarios

### Lab 1: Basic Network Setup
1. Configure two hosts on the same subnet
2. Test connectivity with ping
3. Add a router and configure routing
4. Test inter-subnet communication

### Lab 2: VLSM Implementation
1. Design addressing scheme for complex network
2. Configure multiple subnets with different sizes
3. Implement routing between subnets
4. Verify end-to-end connectivity

### Lab 3: Network Troubleshooting
1. Identify connectivity issues in pre-configured network
2. Use diagnostic tools to isolate problems
3. Implement fixes and verify solutions
4. Document troubleshooting process

## Performance Optimization

### Network Design Best Practices
- **Hierarchical Design**: Layer network functions
- **Load Balancing**: Distribute traffic across paths
- **QoS Implementation**: Prioritize critical traffic
- **Bandwidth Planning**: Adequate capacity for requirements

### Routing Optimization
- **Route Summarization**: Reduce routing table size
- **Metric Tuning**: Influence path selection
- **Load Sharing**: Equal-cost multi-path routing
- **Convergence Time**: Minimize routing updates

## Study Resources

### Reference Materials
- **RFC 791**: Internet Protocol specification
- **RFC 950**: Internet Standard Subnetting Procedure
- **RFC 1918**: Address Allocation for Private Internets
- **RFC 1519**: Classless Inter-Domain Routing (CIDR)

### Practice Tools
- **Packet Tracer**: Cisco network simulation
- **GNS3**: Network emulation platform
- **Wireshark**: Network protocol analyzer
- **Online Calculators**: IP subnet calculators

## Common Mistakes to Avoid

1. **Overlapping Subnets**: Ensure non-overlapping address ranges
2. **Incorrect Subnet Masks**: Match subnet masks on same network
3. **Missing Default Routes**: Configure default gateways
4. **Asymmetric Routing**: Ensure return path exists
5. **MTU Mismatches**: Check maximum transmission unit sizes

## Exam Preparation Tips

1. **Practice Subnetting**: Master binary calculations
2. **Understand Routing**: Know how packets are forwarded
3. **Memorize Common Values**: CIDR notation and subnet masks
4. **Draw Network Diagrams**: Visualize network topology
5. **Use Calculators Wisely**: Verify manual calculations

## Requirements

- **Basic Math Skills**: Binary and decimal conversion
- **Computer**: For simulation and calculation tools
- **Network Simulator**: Packet Tracer or equivalent
- **Practice Networks**: Lab environment for testing

## Author

Viacheslav Moroz - 42 Student
