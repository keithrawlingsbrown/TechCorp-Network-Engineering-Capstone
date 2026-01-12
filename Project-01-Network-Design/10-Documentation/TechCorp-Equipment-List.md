# TechCorp Equipment List

**Document Version:** 1.0  
**Date:** January 12, 2026  
**Author:** Keith R. Brown Jr.

---

## Overview

This document lists all network equipment required for TechCorp's enterprise network implementation. Equipment is selected based on requirements, scalability, and budget considerations.

---

## Core Layer Equipment

### Core Router
- **Quantity:** 1
- **Model:** Cisco ISR 4331 (or equivalent)
- **Specifications:**
  - 3x Gigabit Ethernet ports
  - 2x 10 Gigabit Ethernet ports
  - Inter-VLAN routing capability
  - NAT support
  - VPN capability
- **Estimated Cost:** $3,000 - $5,000
- **Purpose:** Inter-VLAN routing, internet gateway

### Core Switch
- **Quantity:** 1
- **Model:** Cisco Catalyst 9300-48P (or equivalent)
- **Specifications:**
  - 48x 1 Gigabit Ethernet ports (PoE+)
  - 4x 10 Gigabit Ethernet uplink ports
  - Layer 3 switching capability
  - Stacking capability (future)
- **Estimated Cost:** $8,000 - $12,000
- **Purpose:** High-speed backbone, core switching

### Firewall
- **Quantity:** 1
- **Model:** Cisco ASA 5506-X (or equivalent)
- **Specifications:**
  - 8x Gigabit Ethernet ports
  - 1 Gbps throughput
  - VPN support
  - IDS/IPS capability
- **Estimated Cost:** $1,500 - $2,500
- **Purpose:** Security, internet gateway, VPN

---

## Distribution Layer Equipment

### Distribution Switches
- **Quantity:** 2
- **Model:** Cisco Catalyst 2960-X-48TS-L (or equivalent)
- **Specifications:**
  - 48x 1 Gigabit Ethernet ports
  - 2x 10 Gigabit Ethernet uplink ports
  - Layer 2 switching
  - Stacking capability
- **Estimated Cost:** $2,500 - $3,500 each
- **Total Cost:** $5,000 - $7,000
- **Purpose:** Aggregation, redundancy

---

## Access Layer Equipment

### Access Switches
- **Quantity:** 6
- **Model:** Cisco Catalyst 2960-48TC-L (or equivalent)
- **Specifications:**
  - 48x 1 Gigabit Ethernet ports
  - 2x 1 Gigabit Ethernet uplink ports
  - PoE support (where needed)
  - VLAN support
- **Estimated Cost:** $1,500 - $2,000 each
- **Total Cost:** $9,000 - $12,000
- **Purpose:** User connectivity, VLAN assignment

**Switch Assignments:**
- Access Switch 1: Management + IT (VLAN 10)
- Access Switch 2: Sales (VLAN 30)
- Access Switch 3: Engineering (VLAN 40)
- Access Switch 4: Operations (VLAN 50)
- Access Switch 5: Servers (VLAN 20)
- Access Switch 6: Guest/Wireless (VLAN 99)

---

## Server Infrastructure

### DHCP Server
- **Quantity:** 1
- **Type:** Virtual or Physical
- **Specifications:**
  - Windows Server or Linux
  - DHCP service
  - Redundancy (future)
- **Estimated Cost:** $500 - $1,000 (if physical)
- **Purpose:** IP address assignment

### DNS Server
- **Quantity:** 1
- **Type:** Virtual or Physical
- **Specifications:**
  - Windows Server or Linux
  - DNS service
  - Redundancy (future)
- **Estimated Cost:** $500 - $1,000 (if physical)
- **Purpose:** Domain name resolution

### File Server
- **Quantity:** 1
- **Type:** Physical or NAS
- **Specifications:**
  - Storage: 2-4 TB
  - RAID configuration
  - Backup capability
- **Estimated Cost:** $2,000 - $4,000
- **Purpose:** File storage and sharing

---

## Cabling and Infrastructure

### Network Cabling
- **Type:** Cat6 or Cat6a
- **Quantity:** ~5,000 feet (estimated)
- **Estimated Cost:** $3,000 - $5,000
- **Purpose:** Device connectivity

### Patch Panels
- **Quantity:** 2-3
- **Type:** 48-port Cat6 patch panels
- **Estimated Cost:** $200 - $300 each
- **Purpose:** Structured cabling termination

### Patch Cables
- **Quantity:** 100+ (various lengths)
- **Type:** Cat6 patch cables
- **Estimated Cost:** $500 - $1,000
- **Purpose:** Device connections

### Racks and Enclosures
- **Quantity:** 2-3
- **Type:** 19" server racks (42U)
- **Estimated Cost:** $1,000 - $2,000 each
- **Purpose:** Equipment mounting

---

## Power and Cooling

### UPS (Uninterruptible Power Supply)
- **Quantity:** 2-3
- **Type:** 1500VA or 2000VA
- **Estimated Cost:** $500 - $1,000 each
- **Purpose:** Power backup for network equipment

### Cooling
- **Type:** Adequate HVAC
- **Cost:** Included in facility
- **Purpose:** Equipment cooling

---

## Management and Monitoring

### Network Management Software
- **Type:** SNMP-based monitoring
- **Options:** PRTG, Nagios, LibreNMS (open source)
- **Estimated Cost:** $0 - $2,000 (depending on solution)
- **Purpose:** Network monitoring and management

### Configuration Management
- **Type:** Version control for configurations
- **Options:** Git, RANCID
- **Estimated Cost:** $0 (open source)
- **Purpose:** Configuration backup and versioning

---

## Cost Summary

| Category | Quantity | Unit Cost | Total Cost |
|----------|----------|-----------|------------|
| Core Router | 1 | $3,000 - $5,000 | $3,000 - $5,000 |
| Core Switch | 1 | $8,000 - $12,000 | $8,000 - $12,000 |
| Firewall | 1 | $1,500 - $2,500 | $1,500 - $2,500 |
| Distribution Switches | 2 | $2,500 - $3,500 | $5,000 - $7,000 |
| Access Switches | 6 | $1,500 - $2,000 | $9,000 - $12,000 |
| Servers | 3 | $500 - $2,000 | $1,500 - $6,000 |
| Cabling | - | - | $3,000 - $5,000 |
| Infrastructure | - | - | $2,000 - $4,000 |
| **Total Equipment** | - | - | **$33,000 - $53,500** |
| Installation | - | - | $5,000 - $10,000 |
| **Grand Total** | - | - | **$38,000 - $63,500** |

---

## Equipment Alternatives

### Budget Option
- Use refurbished equipment
- Open-source firewall (pfSense)
- Reduce switch count
- **Estimated Savings:** 30-40%

### Premium Option
- Latest generation equipment
- Redundant core switches
- Advanced monitoring
- **Estimated Increase:** 50-100%

---

## Procurement Considerations

### Vendor Selection
- **Cisco:** Industry standard, higher cost
- **HP/Aruba:** Good value, reliable
- **Ubiquiti:** Budget-friendly, good features
- **Refurbished:** Significant cost savings

### Support and Warranty
- **Warranty:** 1-3 years standard
- **Support:** 8x5 or 24x7 options
- **Cost:** 10-15% of equipment cost annually

---

## Implementation Timeline

### Week 1: Procurement
- Order equipment
- Lead time: 1-2 weeks
- **Status:** Planned

### Week 2-3: Installation
- Physical installation
- Cabling
- Rack mounting
- **Status:** Planned

### Week 4: Configuration
- Device configuration
- Testing
- Documentation
- **Status:** Planned

---

## Maintenance and Support

### Ongoing Costs
- **Support Contracts:** $5,000 - $10,000/year
- **Maintenance:** $2,000 - $5,000/year
- **Upgrades:** $3,000 - $5,000/year
- **Total Annual:** $10,000 - $20,000

### Lifecycle
- **Equipment Life:** 5-7 years
- **Refresh Cycle:** Every 5 years
- **Budget Planning:** $10,000 - $15,000/year

---

## Next Steps

1. **Finalize Equipment Selection:** Choose specific models
2. **Obtain Quotes:** Get pricing from vendors
3. **Procurement:** Place orders
4. **Installation Planning:** Schedule installation
5. **Configuration:** Prepare configurations

---

**Document Status:** ✅ Complete - Equipment list finalized

