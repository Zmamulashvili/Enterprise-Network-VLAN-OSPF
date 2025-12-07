# Enterprise Network VLAN OSPF Lab

## **Objective**
This project demonstrates routing skills using OSPF in a multi-branch enterprise network. The lab showcases:

- VLAN configuration and inter-VLAN routing on a Layer 3 switch.
- OSPF dynamic routing across multiple routers.
- Backup routing implementation and route prioritization.
- Centralized network management via SSH.

The primary focus of this project is routing, while VLANs, backup routes, and SSH add realism to the network topology.

---

## **Topology Overview**

The network consists of a main branch and two remote branches, all connected to a central router (R0). An Internet-simulating router (R5) is connected to the network to demonstrate external connectivity.

### **Main Branch**
- Layer 3 switch with **3 VLANs**:
  - Management VLAN
  - Staff VLAN
  - Guest VLAN
- Connected to **R0** for inter-VLAN routing.

### **Branch 1**
- Switch with **2 VLANs**:
  - Administration
  - Employees
- Connected to **R0** via trunk.

### **Branch 2**
- Switch with **2 VLANs**:
  - Administration
  - Employees
- Connected to **R0** via trunk.

### **Routing and Backup**
- **R0** connects to **R1**.
- **R1 → R2 → R3 → R5** serves as the **main route** (OSPF cost 10).
- **R1 → R4 → R3 → R5** serves as the **backup route** (OSPF cost 100).
- All routers participate in OSPF; only **R0 has SSH enabled** for centralized management.

*Topology diagram is provided in `Topology-Diagram.png`.*

---

## **Subnet and VLAN Table**

| VLAN Name      | Branch      | Subnet           | Default Gateway |
|----------------|------------|-----------------|----------------|
| Management     | Main       | 192.168.10.0/24 | 192.168.10.1   |
| Staff          | Main       | 192.168.20.0/24 | 192.168.20.1   |
| Guest          | Main       | 192.168.30.0/24 | 192.168.30.1   |
| Administration | Branch 1   | 192.168.40.0/24 | 192.168.40.1   |
| Employees      | Branch 1   | 192.168.50.0/24 | 192.168.50.1   |
| Administration | Branch 2   | 192.168.60.0/24 | 192.168.60.1   |
| Employees      | Branch 2   | 192.168.70.0/24 | 192.168.70.1   |

---
## **Interface Assignments (All /24)**

### **MLSW (Main L3 Switch)**
- **G0/1 — 1.1.1.1** (connection to R0)

### **R0 (Core Router)**
- **G0/0/0 — 10.10.10.2** (to R1)
- **G0/1**  
  - 192.168.40.1  (VLAN 40 — Branch 1 Admin)  
  - 192.168.50.1  (VLAN 50 — Branch 1 Employees)
- **G0/0 — 1.1.1.2** (to MLSW)
- **G0/2**  
  - 192.168.60.1  (VLAN 60 — Branch 2 Admin)  
  - 192.168.70.1  (VLAN 70 — Branch 2 Employees)

SSH is enabled on R0.

### **R1**
- **G0/0/0 — 10.10.10.1** (to R0)
- **G0/0 — 10.10.40.1** (to R4 — Backup route)
- **G0/1 — 10.10.20.1** (to R2 — Main route)

### **R2**
- **G0/1 — 10.10.20.2** (to R1)
- **G0/0 — 10.10.30.1** (to R3)

### **R3**
- **G0/0 — 10.10.30.2** (to R2)
- **G0/2 — 10.10.50.2** (to R4)
- **G0/1 — 11.22.33.1** (to R5 — Internet simulation)

### **R4**
- **G0/0 — 10.10.40.2** (to R1)
- **G0/2 — 10.10.50.1** (to R3)

### **R5 (Internet Router)**
- **G0/1 — 11.22.33.2** (to R3)

  
## **Configuration Files**

The `Configurations/` folder contains all router and switch configuration files:

- `R0.txt` – Central router, OSPF, SSH setup.  
- `R1.txt` – Connected to R0, main and backup route configuration.  
- `R2.txt`, `R3.txt`, `R4.txt` – Branch routers with OSPF.  
- `R5.txt` – Internet-simulating router.  
- `L3Switch.txt` – VLAN and inter-VLAN routing configuration.  

These files contain **all commands** needed to replicate the lab.

---

## **Testing and Validation**

Testing was performed to verify connectivity between VLANs, branches, and the Internet router:

- **Ping Results:** All VLANs can reach other VLANs across branches via OSPF.  
- **Traceroute Results:** Confirms that traffic follows the main route by default and switches to backup if main route fails.  
- **SSH Test:** Verified remote login to R0 from a connected PC.

Screenshots and logs are provided in the `Testing/` folder.

---

## **Usage Instructions**

1. Open `Project_OSPF.pkt` in Packet Tracer.    
2. Verify connectivity:
   - Use `ping` between PCs in different VLANs.
   - Use `traceroute` to see OSPF routing paths.  
3. Test backup route by shutting down the main route link and observing traffic failover.  
4. Optional: SSH into **R0** to manage the network.

---
