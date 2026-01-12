# TechCorp Network Topology Documentation

**Document Version:** 1.0  
**Date:** January 12, 2026  
**Author:** Keith R. Brown Jr.

---

## Overview

This document describes the complete network topology for TechCorp's enterprise network. The design follows a hierarchical 3-layer architecture for scalability, manageability, and performance.

---

## Network Architecture

### Hierarchical Design

**Layer 1: Core Layer**
- High-speed backbone
- Redundant connections
- Fast switching
- Minimal features (routing only)

**Layer 2: Distribution Layer**
- Aggregation point
- Policy enforcement
- Inter-VLAN routing
- Redundancy

**Layer 3: Access Layer**
- User connectivity
- VLAN assignment
- Port security
- Power over Ethernet (PoE)

---

## Physical Topology

```
                    [Internet]
                         |
                    [Firewall]
                         |
                  [Core Router]
                         |
                  [Core Switch]
                    /        \
        [Dist Switch 1]  [Dist Switch 2]
            /    \          /    \
    [Access 1] [Access 2] [Access 3] [Access 4]
        |          |          |          |
    [Users]    [Users]    [Users]    [Users]
```

### Device Placement

**Core Layer:**
- 1x Core Router (inter-VLAN routing, internet gateway)
- 1x Core Switch (high-speed backbone)
- 1x Firewall (security, internet gateway)

**Distribution Layer:**
- 2x Distribution Switches (redundancy, aggregation)

**Access Layer:**
- 6x Access Switches (user connectivity)
  - Access Switch 1: Management + IT
  - Access Switch 2: Sales
  - Access Switch 3: Engineering
  - Access Switch 4: Operations
  - Access Switch 5: Servers
  - Access Switch 6: Guest (wireless)

---

## Logical Topology

### VLAN Segmentation

```
[Core Router]
    |
    ├── VLAN 10 (Management) → IT Department
    ├── VLAN 20 (Servers) → Server Infrastructure
    ├── VLAN 30 (Sales) → Sales Department
    ├── VLAN 40 (Engineering) → Engineering Department
    ├── VLAN 50 (Operations) → Operations Department
    └── VLAN 99 (Guest) → Guest Network (Isolated)
```

### Inter-VLAN Routing

- **Router-on-a-Stick:** Core router handles all inter-VLAN routing
- **Sub-interfaces:** One sub-interface per VLAN
- **Firewall:** Controls inter-VLAN traffic policies
- **Access Control:** Firewall rules enforce security policies

---

## Device Specifications

### Core Router
- **Model:** Cisco ISR 4331 (or equivalent)
- **Interfaces:** Gigabit Ethernet
- **Features:** Inter-VLAN routing, NAT, DHCP relay
- **Redundancy:** Single device (future: HSRP)

### Core Switch
- **Model:** Cisco Catalyst 9300 (or equivalent)
- **Ports:** 48x 1Gbps + 4x 10Gbps uplinks
- **Features:** Layer 3 switching, VLAN support
- **Redundancy:** Single device (future: stack)

### Distribution Switches
- **Model:** Cisco Catalyst 2960-X (or equivalent)
- **Ports:** 48x 1Gbps + 2x 10Gbps uplinks
- **Features:** Layer 2 switching, VLAN support
- **Redundancy:** Two switches for redundancy

### Access Switches
- **Model:** Cisco Catalyst 2960 (or equivalent)
- **Ports:** 48x 1Gbps + 2x 1Gbps uplinks
- **Features:** PoE support, VLAN assignment
- **Quantity:** 6 switches

### Firewall
- **Model:** Cisco ASA 5506-X (or equivalent)
- **Features:** Stateful inspection, VPN, IDS/IPS
- **Throughput:** 1 Gbps
- **Placement:** Between router and internet

---

## Connectivity

### Uplink Speeds
- **Core to Distribution:** 10 Gbps (fiber)
- **Distribution to Access:** 1 Gbps (copper)
- **Access to End Devices:** 1 Gbps (copper)
- **Internet Connection:** 1 Gbps (fiber)

### Redundancy
- **Distribution Layer:** Two switches (redundancy)
- **Uplinks:** Multiple uplinks per switch
- **Future:** Core switch redundancy (stacking)

---

## IP Addressing

### Network Addressing
- **Private Network:** 192.168.0.0/16
- **Subnet Size:** /24 (254 hosts per VLAN)
- **Gateway IPs:** .1 address in each subnet
- **Reserved IPs:** .1-.50 reserved for infrastructure

### Device Addressing
- **Routers:** 192.168.10.10-19
- **Switches:** 192.168.10.11-19
- **Firewall:** 192.168.10.20
- **Servers:** 192.168.20.10-50
- **DHCP Range:** .51-.254 in each subnet

---

## Scalability Design

### Current Capacity
- **Users:** 200 (current requirement)
- **VLANs:** 6 (current)
- **Switches:** 9 total
- **Bandwidth:** 1 Gbps internet, 10 Gbps core

### Growth Capacity
- **Users:** 500 (design capacity)
- **VLANs:** 10+ (easy expansion)
- **Switches:** Can add 10+ more access switches
- **Bandwidth:** Can upgrade to 10 Gbps internet

### Expansion Points
- **Access Layer:** Easy to add switches
- **Distribution Layer:** Can add third switch
- **Core Layer:** Can stack for redundancy
- **VLANs:** Unlimited VLAN capacity

---

## Security Architecture

### Network Segmentation
- **VLAN Isolation:** Logical separation
- **Firewall Rules:** Inter-VLAN policies
- **Guest Isolation:** Complete isolation
- **Management VLAN:** Restricted access

### Security Zones
- **Trusted Zone:** Internal departments
- **DMZ Zone:** Public-facing servers (future)
- **Untrusted Zone:** Internet, guest network
- **Management Zone:** Network devices only

---

## Implementation Phases

### Phase 1: Core Infrastructure ✅
- Core router and switch
- Basic connectivity
- **Status:** Designed

### Phase 2: Distribution Layer
- Distribution switches
- Redundancy configuration
- **Status:** Planned

### Phase 3: Access Layer
- Access switches
- VLAN assignment
- **Status:** Planned

### Phase 4: Security
- Firewall configuration
- Access control policies
- **Status:** Planned

---

## Packet Tracer Implementation

### Topology Created
- ✅ All devices placed
- ✅ Connections made
- ✅ VLANs configured
- ✅ Basic routing configured
- ✅ Connectivity tested

### Screenshots
- Topology diagram
- Device configurations
- VLAN assignments
- Routing tables

---

## Device Placement Rationale

### Core Router Location
- **Placement:** Network core
- **Rationale:** Central routing point
- **Benefit:** Single point for inter-VLAN routing

### Distribution Switches
- **Placement:** Between core and access
- **Rationale:** Aggregation and redundancy
- **Benefit:** Fault tolerance, traffic management

### Access Switches
- **Placement:** Near user work areas
- **Rationale:** User connectivity
- **Benefit:** Reduced cable runs, better performance

---

## Performance Considerations

### Latency
- **Core to Distribution:** < 1ms
- **Distribution to Access:** < 2ms
- **Access to End Device:** < 5ms
- **Total End-to-End:** < 10ms

### Throughput
- **Core Backbone:** 10 Gbps
- **Access Ports:** 1 Gbps per port
- **Internet:** 1 Gbps
- **Future:** Upgradeable to 10 Gbps

---

## Redundancy Strategy

### Current Redundancy
- **Distribution Layer:** Two switches
- **Uplinks:** Multiple uplinks per switch
- **Power:** UPS for critical equipment

### Future Redundancy
- **Core Switch:** Stacking for redundancy
- **Router:** HSRP for failover
- **Internet:** Dual internet connections
- **Power:** Generator backup

---

## Troubleshooting Considerations

### Network Monitoring
- **SNMP:** Monitor all devices
- **Logging:** Centralized logging
- **Alerting:** Automated alerts
- **Dashboards:** Real-time monitoring

### Common Issues
- **VLAN Misconfiguration:** Port assignment errors
- **Routing Issues:** Inter-VLAN routing problems
- **Performance:** Bandwidth bottlenecks
- **Security:** Unauthorized access

---

## Documentation Requirements

### Topology Diagrams
- ✅ Physical topology
- ✅ Logical topology
- ✅ VLAN diagram
- ✅ IP addressing diagram

### Configuration Documentation
- Switch configurations
- Router configurations
- Firewall rules
- VLAN assignments

---

## Next Steps

1. **Implementation:** Begin physical installation
2. **Configuration:** Configure all devices
3. **Testing:** Verify connectivity and routing
4. **Documentation:** Update with actual configurations
5. **Monitoring:** Set up network monitoring

---

**Document Status:** ✅ Complete - Topology finalized

