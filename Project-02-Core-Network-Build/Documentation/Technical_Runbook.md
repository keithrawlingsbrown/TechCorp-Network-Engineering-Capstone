# TECHNICAL RUNBOOK: TechCorp Network Operations

**Project:** TechCorp Network Engineering Capstone - Project 2  
**Version:** 1.0  
**Last Updated:** January 13, 2026  
**Purpose:** Operational procedures, troubleshooting, and common tasks  

---

## 📋 QUICK REFERENCE

### **Network Overview:**
- **Switches:** 4 (1 Core, 3 Access)
- **VLANs:** 6 production VLANs (10, 20, 30, 40, 99, 100)
- **Trunk Links:** 3 (CoreSwitch → Floor1/2/3)
- **Access Ports:** 12+ end devices
- **STP Mode:** PVST+
- **Root Bridge:** CoreSwitch-01

### **Device Passwords:**
| Password Type    | Value       | Notes                  |
|------------------|-------------|------------------------|
| Enable Secret    | Cisco123!   | Privileged EXEC mode   |
| Console Password | Console123! | Console line access    |
| VTY Password     | VTY123!     | Telnet/SSH access      |

### **Emergency Contacts:**
| Role              | Contact            |
|-------------------|-------------------|
| Network Admin     | Keith Brown       |
| Backup Contact    | [TBD]             |
| Vendor Support    | Cisco TAC         |

---

## 🚀 COMMON OPERATIONAL TASKS

### **Task 1: Verify Network Status**

**Objective:** Check overall network health

**Steps:**
```cisco
! Connect to CoreSwitch-01
enable
Cisco123!

! Check all trunk links
show interfaces trunk

! Verify all VLANs active
show vlan brief

! Check STP status
show spanning-tree summary

! Verify CDP neighbors
show cdp neighbors
```

**Expected Results:**
- All trunk ports showing "trunking"
- VLANs 10, 20, 30, 40, 99, 100 all "active"
- STP summary shows 0 blocking, 21 forwarding
- CDP shows all 3 access switches connected

**If Issues:** See Troubleshooting Section

---

### **Task 2: Add New User to VLAN**

**Objective:** Connect new PC to existing VLAN

**Scenario:** New Engineering PC needs to be added to VLAN 10

**Steps:**
1. **Identify available port:**
   ```cisco
   enable
   show interfaces status
   ```
   Look for "notconnect" ports

2. **Configure port:**
   ```cisco
   configure terminal
   interface fastEthernet 0/X
   switchport mode access
   switchport access vlan 10
   no shutdown
   description Engineering-PC-New
   exit
   ```

3. **Verify:**
   ```cisco
   show vlan id 10
   show interfaces fastEthernet 0/X switchport
   ```

4. **Save:**
   ```cisco
   copy running-config startup-config
   ```

5. **Test connectivity:**
   - PC should get DHCP address in 192.168.10.0/24 range
   - PC should ping other devices in VLAN 10
   - PC should NOT ping devices in other VLANs (unless router configured)

**Rollback:** If issues occur:
```cisco
configure terminal
interface fastEthernet 0/X
no switchport access vlan 10
shutdown
```

---

### **Task 3: Create New VLAN**

**Objective:** Add new VLAN for IOT devices (VLAN 50)

**Steps:**
1. **Create VLAN on all switches:**
   ```cisco
   ! On CoreSwitch-01
   enable
   configure terminal
   vlan 50
   name IOT
   exit
   end
   
   ! Repeat on Floor1, Floor2, Floor3
   ```

2. **Add to trunk allowed list:**
   ```cisco
   configure terminal
   interface range fastEthernet 0/2-4
   switchport trunk allowed vlan add 50
   exit
   end
   ```

3. **Verify:**
   ```cisco
   show vlan brief
   show interfaces trunk
   ```

4. **Assign ports to new VLAN:**
   ```cisco
   interface fastEthernet 0/X
   switchport mode access
   switchport access vlan 50
   ```

5. **Save on all switches:**
   ```cisco
   copy running-config startup-config
   ```

---

### **Task 4: Backup Switch Configuration**

**Objective:** Save current configuration before changes

**Steps:**
1. **View configuration:**
   ```cisco
   show running-config
   ```

2. **Save to startup-config:**
   ```cisco
   copy running-config startup-config
   ```

3. **Export to file (if TFTP available):**
   ```cisco
   copy running-config tftp:
   [Enter TFTP server IP: 192.168.99.10]
   [Enter filename: coreswitch-01-backup-2026-01-13.txt]
   ```

4. **Manual backup (copy/paste):**
   ```cisco
   show running-config
   ```
   Copy output to text file, save with date/time stamp

**Backup Schedule:**
- Before any configuration changes
- After successful configuration changes
- Weekly automated backups (if TFTP available)

---

### **Task 5: Verify Port Status**

**Objective:** Check if port is active and assigned correctly

**Steps:**
```cisco
enable

! Check physical status
show interfaces fastEthernet 0/X status

! Check switchport configuration
show interfaces fastEthernet 0/X switchport

! Check VLAN assignment
show vlan id [VLAN_ID]

! Check errors
show interfaces fastEthernet 0/X
```

**Interpreting Output:**
- **Status: connected** = Port is up, device connected
- **Status: notconnect** = Port is up, no device connected
- **Status: disabled** = Port is administratively shut down
- **VLAN: 1** = Port in default VLAN (may need assignment)

---

## 🔧 TROUBLESHOOTING GUIDE

### **Issue 1: User Can't Connect to Network**

**Symptoms:**
- PC shows "No network access"
- PC can't ping gateway
- PC not getting DHCP address

**Diagnostic Steps:**

**Step 1: Check Physical Connection**
```cisco
show interfaces fastEthernet 0/X status
```
- **If "notconnect":** Check cable, PC network port
- **If "disabled":** Port is shut down administratively

**Step 2: Check Port Configuration**
```cisco
show interfaces fastEthernet 0/X switchport
```
- **Check:** Administrative Mode: access (should be "static access")
- **Check:** Access Mode VLAN: [correct VLAN ID]
- **If wrong VLAN:** Reconfigure port

**Step 3: Verify VLAN Exists**
```cisco
show vlan id [VLAN_ID]
```
- **If VLAN doesn't exist:** Create VLAN first
- **If VLAN exists but inactive:** Check trunk configuration

**Step 4: Test Connectivity**
```cisco
! From switch
ping [PC IP address]

! From PC
ipconfig /all
ping [Gateway]
ping [Another PC in same VLAN]
```

**Common Fixes:**
```cisco
! Enable port
interface fastEthernet 0/X
no shutdown

! Correct VLAN assignment
switchport access vlan [correct_VLAN]

! Reset port
shutdown
no shutdown
```

---

### **Issue 2: Trunk Not Working Between Switches**

**Symptoms:**
- Native VLAN mismatch errors
- VLANs not passing between switches
- Port showing "not-trunking"

**Diagnostic Steps:**

**Step 1: Check Trunk Status**
```cisco
show interfaces trunk
```
- **If port not listed:** Trunk not configured
- **If Status: not-trunking:** Configuration issue

**Step 2: Check Both Ends**
```cisco
! On CoreSwitch-01
show interfaces fastEthernet 0/2 switchport

! On AccessSwitch-Floor1
show interfaces fastEthernet 0/1 switchport
```
- **Check:** Mode should be "trunk"
- **Check:** Native VLAN should match (99)
- **Check:** Allowed VLANs should match

**Step 3: Check CDP**
```cisco
show cdp neighbors detail
```
- Verify correct switch is connected
- Check duplex/speed mismatches

**Common Fixes:**
```cisco
! Reconfigure trunk
interface fastEthernet 0/X
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 10,20,30,40,99,100
no shutdown

! Clear interface errors
clear counters fastEthernet 0/X
```

**If Native VLAN Mismatch:**
```cisco
! Make native VLAN consistent (both ends)
interface fastEthernet 0/X
switchport trunk native vlan 99
```

---

### **Issue 3: Spanning Tree Blocking Port**

**Symptoms:**
- Port in "BLK" (Blocking) state
- Traffic not flowing through link
- Unexpected topology

**Diagnostic Steps:**

**Step 1: Check STP Status**
```cisco
show spanning-tree vlan [VLAN_ID]
```
- Identify which port is blocking
- Identify root bridge

**Step 2: Verify Root Bridge**
```cisco
show spanning-tree root
```
- Confirm expected root bridge
- Check if root bridge changed

**Step 3: Check Port Cost**
```cisco
show spanning-tree interface fastEthernet 0/X detail
```
- Verify port cost (19 for FastEthernet)
- Check if manual cost configured

**Common Fixes:**
```cisco
! Manually configure root bridge (if needed)
spanning-tree vlan [VLAN_ID] priority 24576

! Reset STP on interface
interface fastEthernet 0/X
no spanning-tree vlan [VLAN_ID]
spanning-tree vlan [VLAN_ID]
```

**Expected Behavior:**
- In current network: **NO blocked ports** (simple tree)
- If port blocked: Either misconfiguration or unexpected cable added

---

### **Issue 4: VLAN Not Passing Through Trunk**

**Symptoms:**
- Device in VLAN can't communicate with device in same VLAN on different switch
- VLAN shows in "show vlan brief" but not working

**Diagnostic Steps:**

**Step 1: Check VLAN Allowed on Trunk**
```cisco
show interfaces trunk
```
Look at "Vlans allowed on trunk" line
- **If VLAN missing:** Add to allowed list

**Step 2: Verify VLAN Created on Both Switches**
```cisco
! On source switch
show vlan id [VLAN_ID]

! On destination switch
show vlan id [VLAN_ID]
```

**Step 3: Check VLAN Active**
```cisco
show vlan brief
```
- VLAN Status should be "active"
- VLAN should show on both switches

**Common Fixes:**
```cisco
! Add VLAN to trunk allowed list
interface fastEthernet 0/X
switchport trunk allowed vlan add [VLAN_ID]

! Or reset allowed list
switchport trunk allowed vlan 10,20,30,40,99,100

! Create VLAN if missing
vlan [VLAN_ID]
name [VLAN_Name]
```

---

### **Issue 5: Can't Access Switch (Password Issues)**

**Symptoms:**
- "Password incorrect" errors
- Locked out of switch

**Solutions:**

**If Console Password Forgotten:**
1. **Password Recovery Mode (requires physical access):**
   - Power off switch
   - Hold MODE button while powering on
   - Release when LED stops flashing
   - Initialize flash filesystem
   - Boot without config
   - Reset passwords

**If Enable Password Forgotten:**
- Use password recovery procedure above
- **Prevention:** Document passwords securely!

**Current Passwords (Lab Environment):**
- Enable Secret: `Cisco123!`
- Console: `Console123!`
- VTY: `VTY123!`

---

### **Issue 6: Switch Performance Slow**

**Symptoms:**
- Slow network performance
- High latency
- Packet drops

**Diagnostic Steps:**

**Step 1: Check Interface Errors**
```cisco
show interfaces fastEthernet 0/X
```
Look for:
- Input errors, CRC errors
- Output errors, collisions
- Packet drops

**Step 2: Check CPU Usage**
```cisco
show processes cpu
```
- High CPU may indicate broadcast storm or loop

**Step 3: Check STP Topology**
```cisco
show spanning-tree inconsistentports
```
- Check for loops

**Common Fixes:**
```cisco
! Clear interface counters
clear counters fastEthernet 0/X

! Reset interface
interface fastEthernet 0/X
shutdown
no shutdown

! Check for broadcast storms
show interfaces | include broadcast
```

---

## 🛠️ MAINTENANCE PROCEDURES

### **Monthly Maintenance Checklist:**

**1. Configuration Backup**
- [ ] Backup all switch configs to TFTP server
- [ ] Verify backups are readable
- [ ] Store off-site copy

**2. Port Audit**
- [ ] Review all active ports
- [ ] Document any new devices
- [ ] Identify unused ports
- [ ] Shutdown unused ports for security

**3. VLAN Review**
- [ ] Verify all VLANs still needed
- [ ] Check VLAN memberships correct
- [ ] Remove unused VLANs

**4. STP Health Check**
- [ ] Verify root bridge is correct
- [ ] Check for any topology changes
- [ ] Verify no blocked ports (unless expected)
- [ ] Review STP logs for errors

**5. Security Review**
- [ ] Verify passwords not compromised
- [ ] Check for unauthorized devices
- [ ] Review port security violations
- [ ] Update access lists if needed

**6. Performance Review**
- [ ] Check interface errors/drops
- [ ] Review bandwidth utilization
- [ ] Identify bottlenecks
- [ ] Plan capacity upgrades

---

## 📊 MONITORING AND ALERTS

### **Key Metrics to Monitor:**

**1. Port Status**
```cisco
show interfaces status
```
- Monitor for unexpected "notconnect" (port failures)
- Monitor for "err-disabled" (security violations)

**2. Trunk Health**
```cisco
show interfaces trunk
```
- Verify all trunks "trunking"
- Check for errors or dropped packets

**3. VLAN Status**
```cisco
show vlan brief
```
- All VLANs should be "active"
- Monitor for unexpected VLAN changes

**4. STP Stability**
```cisco
show spanning-tree summary
```
- Monitor for topology changes
- Alert on unexpected root bridge changes

**5. Interface Errors**
```cisco
show interfaces | include error
```
- Monitor for CRC errors, collisions
- Alert on high error rates

---

## 🔐 SECURITY BEST PRACTICES

### **Current Security Configuration:**
- ✅ Enable secret password (MD5 encrypted)
- ✅ Console password protection
- ✅ VTY password protection
- ✅ Native VLAN changed from default
- ✅ Explicit VLAN allowed list on trunks

### **Recommended Additional Security:**

**1. Port Security (Future Implementation)**
```cisco
interface range fastEthernet 0/5-24
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
```

**2. DHCP Snooping (Future Implementation)**
```cisco
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,40
interface fastEthernet 0/5
 ip dhcp snooping trust
```

**3. Disable Unused Ports (Recommended Now)**
```cisco
interface range fastEthernet 0/8-24
 shutdown
 description UNUSED-PORT-SHUTDOWN
```

**4. Enable SSH Instead of Telnet (Future)**
```cisco
hostname CoreSwitch-01
ip domain-name techcorp.local
crypto key generate rsa modulus 2048
line vty 0 15
 transport input ssh
 login local
username admin privilege 15 secret AdminPass123!
```

---

## 📝 CHANGE MANAGEMENT

### **Change Request Process:**

**1. Plan Change**
- Document what will change
- Identify affected systems
- Plan rollback procedure
- Schedule maintenance window

**2. Backup Current Config**
```cisco
copy running-config startup-config
copy running-config tftp:
```

**3. Implement Change**
- Follow documented procedure
- Make one change at a time
- Verify after each step

**4. Verify Change**
- Test affected functionality
- Check for errors
- Verify no unintended impacts

**5. Document Change**
- Update configuration documentation
- Log in change management system
- Update network diagrams if needed

**6. Save Configuration**
```cisco
copy running-config startup-config
```

---

## 🆘 EMERGENCY PROCEDURES

### **Emergency Contact List:**
| Severity | Contact          | Phone       |
|----------|------------------|-------------|
| P1       | Network Admin    | [Phone]     |
| P2       | Backup Admin     | [Phone]     |
| P3       | Cisco TAC        | [TAC Number]|

### **Severity Definitions:**
- **P1 (Critical):** Network down, multiple users affected
- **P2 (High):** Partial outage, some users affected
- **P3 (Medium):** Single user issue, workaround available
- **P4 (Low):** Non-urgent, cosmetic issue

### **Emergency Rollback:**
```cisco
! If changes cause issues, revert to startup-config
reload
[Confirm]
! Switch will reboot with saved config
```

---

## ✅ RUNBOOK STATUS

**Version:** 1.0  
**Last Updated:** January 13, 2026  
**Next Review:** Monthly  
**Status:** Active ✅  

**This runbook covers:**
- ✅ Common operational tasks
- ✅ Troubleshooting procedures
- ✅ Maintenance checklists
- ✅ Security best practices
- ✅ Change management
- ✅ Emergency procedures

**Future Additions:**
- Router configuration procedures
- Inter-VLAN routing setup
- Advanced security features
- Automation scripts
- Performance tuning guides

---

**Document Owner:** Keith R. Brown Jr.  
**For Support:** Contact network administrator  
**Emergency:** Follow emergency procedures above
