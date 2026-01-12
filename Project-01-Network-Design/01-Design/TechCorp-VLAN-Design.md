# TechCorp VLAN Design

**Document Version:** 1.0  
**Date:** January 12, 2026  
**Author:** Keith R. Brown Jr.

---

## Overview

This document outlines the VLAN segmentation strategy for TechCorp's enterprise network. VLANs provide logical network segmentation for security, traffic management, and scalability.

---

## VLAN Strategy

### Design Principles
1. **Department-Based Segmentation:** Each department gets dedicated VLAN
2. **Security Isolation:** VLANs provide security boundaries
3. **Traffic Management:** Easier to manage and monitor traffic
4. **Scalability:** Easy to add new VLANs as needed
5. **Guest Isolation:** Guest network completely isolated

---

## VLAN Table

| VLAN ID | VLAN Name | Subnet | Department | Purpose | Users |
|---------|-----------|--------|------------|---------|-------|
| 1 | Native | 192.168.1.0/24 | Default | Default VLAN | - |
| 10 | Management | 192.168.10.0/24 | IT | Network devices | 15 |
| 20 | Servers | 192.168.20.0/24 | IT | Server infrastructure | 20 |
| 30 | Sales | 192.168.30.0/24 | Sales | Sales department | 50 |
| 40 | Engineering | 192.168.40.0/24 | Engineering | Engineering department | 80 |
| 50 | Operations | 192.168.50.0/24 | Operations | Operations department | 35 |
| 99 | Guest | 192.168.99.0/24 | Guest | Guest access | Variable |

**Total Users:** 200 (plus variable guest users)

---

## VLAN Details

### VLAN 1: Native (Default)
- **Purpose:** Default VLAN for unassigned ports
- **Subnet:** 192.168.1.0/24
- **Users:** None (should be empty in production)
- **Security:** Low (should migrate all devices)
- **Notes:** Keep minimal, migrate all devices to specific VLANs

### VLAN 10: Management
- **Purpose:** Network device management
- **Subnet:** 192.168.10.0/24
- **Users:** IT department (15 users)
- **Devices:** Routers, switches, firewalls, monitoring
- **Security:** High (restricted access)
- **Access:** IT department only, no inter-VLAN routing from other VLANs

### VLAN 20: Servers
- **Purpose:** Server infrastructure
- **Subnet:** 192.168.20.0/24
- **Users:** Server administrators
- **Devices:** All servers (DHCP, DNS, file, web, database)
- **Security:** High (restricted access)
- **Access:** IT and authorized users only

### VLAN 30: Sales
- **Purpose:** Sales department network
- **Subnet:** 192.168.30.0/24
- **Users:** Sales department (50 users)
- **Devices:** Sales team computers, printers
- **Security:** Medium
- **Access:** Sales department, access to servers (VLAN 20)

### VLAN 40: Engineering
- **Purpose:** Engineering department network
- **Subnet:** 192.168.40.0/24
- **Users:** Engineering department (80 users)
- **Devices:** Engineering team computers, development servers
- **Security:** Medium
- **Access:** Engineering department, access to servers (VLAN 20)

### VLAN 50: Operations
- **Purpose:** Operations department network
- **Subnet:** 192.168.50.0/24
- **Users:** Operations department (35 users)
- **Devices:** Operations team computers, printers
- **Security:** Medium
- **Access:** Operations department, access to servers (VLAN 20)

### VLAN 99: Guest
- **Purpose:** Guest and visitor access
- **Subnet:** 192.168.99.0/24
- **Users:** Guests, visitors (variable)
- **Devices:** Guest devices (BYOD)
- **Security:** Low (internet only)
- **Access:** Internet only, no internal network access
- **Isolation:** Complete isolation from internal networks

---

## Inter-VLAN Routing

### Routing Requirements

**Allowed Communication:**
- All departments → Servers (VLAN 20)
- All departments → Internet
- IT (VLAN 10) → All VLANs (management access)
- No department-to-department direct communication (via firewall)

**Blocked Communication:**
- Guest (VLAN 99) → All internal VLANs
- Departments → Management VLAN (VLAN 10)
- Departments → Other departments (direct)

### Routing Implementation
- **Router-on-a-Stick:** Router with sub-interfaces for each VLAN
- **Layer 3 Switch:** Core switch handles inter-VLAN routing
- **Firewall:** Controls inter-VLAN traffic and policies

---

## VLAN Naming Conventions

### Standard Format
- **Department VLANs:** Department name (e.g., "Sales", "Engineering")
- **Infrastructure VLANs:** Purpose name (e.g., "Management", "Servers")
- **Special VLANs:** Descriptive name (e.g., "Guest", "Native")

### Naming Rules
- Use descriptive names
- Avoid abbreviations
- Consistent capitalization
- Match department names exactly

---

## Security Considerations

### VLAN Security
1. **Isolation:** Guest VLAN completely isolated
2. **Access Control:** Firewall rules between VLANs
3. **Management VLAN:** Restricted access, no inter-VLAN routing
4. **Server VLAN:** Restricted access, authorized users only

### Best Practices
- Never use VLAN 1 for production traffic
- Assign all ports to specific VLANs
- Use VLAN access lists (VACLs) if needed
- Monitor VLAN traffic for anomalies
- Regular VLAN audits

---

## Scalability Planning

### Future VLANs
- **VLAN 60:** Future department expansion
- **VLAN 70:** IoT devices (planned)
- **VLAN 80:** VoIP phones (planned)
- **VLAN 90:** Wireless management (planned)

### Growth Considerations
- Each VLAN supports 254 hosts (adequate for growth)
- Easy to add new VLANs
- IP addressing scheme allows expansion
- No re-addressing required for 3+ years

---

## Implementation Notes

### Switch Configuration
- Configure VLANs on all switches
- Assign access ports to appropriate VLANs
- Configure trunk ports between switches
- Use VLAN Trunking Protocol (VTP) or manual configuration

### Router Configuration
- Configure sub-interfaces for each VLAN
- Assign IP addresses (gateway IPs)
- Enable routing between VLANs
- Configure access control lists (ACLs)

### Testing
- Verify VLAN isolation
- Test inter-VLAN routing
- Verify guest isolation
- Test firewall rules

---

## Documentation Requirements

### VLAN Documentation
- ✅ VLAN table (this document)
- ✅ IP addressing (IPAM)
- ✅ Switch port assignments
- ✅ Inter-VLAN routing configuration
- ✅ Security policies

### Maintenance
- Regular VLAN audits
- Port assignment tracking
- Access control review
- Performance monitoring

---

## Next Steps

1. **Implementation:** Configure VLANs on switches
2. **Routing:** Configure inter-VLAN routing
3. **Security:** Implement firewall rules
4. **Testing:** Verify VLAN functionality
5. **Documentation:** Update with actual configurations

---

**Document Status:** ✅ Complete - VLAN design finalized

