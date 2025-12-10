📡 **Network Topology Architecture – Detailed Documentation**
=============================================================

This document explains the network architecture represented in the diagram. It covers the purpose of each device, addressing scheme, traffic flow, and the logical segmentation of the network.

🏗️ **1\. Overview of the Network Topology**
------------------------------------------------

The topology represents a **small enterprise network** consisting of:

*   Multiple **servers** (HTTP/HTTPS, DNS, FTP, DHCP)
    
*   A **core firewall**
    
*   A **router** connected to the **internet/cloud**
    
*   Layer 2 switches distributing networks to different VLAN subnets
    
*   Three separate **PC subnets**
    
*   Proper segmentation of server network and user network
    

The design uses a **hierarchical model**:

*   **Core Layer:** Router + Firewall
    
*   **Distribution Layer:** Switch₀ and central switch
    
*   **Access Layer:** End devices (PCs & Servers)
    

🌐 **2\. Addressing Overview**
----------------------------------
<img width="959" height="288" alt="image" src="https://github.com/user-attachments/assets/ae7404fa-3b4b-4722-ab89-69267b0ef1fc" />

Servers appear to be in a separate unmanaged subnet, routed through Switch0.

🖥️ **3\. Server Network (Left Side)**
------------------------------------------

### **Connected via Switch0**

1.  **HTTP/HTTPS Server**
    
    *   Provides web services (port 80/443).
        
2.  **DNS Server**
    
    *   Name resolution for internal networks.
        
3.  **FTP Server**
    
    *   Provides file sharing (port 21).
        

Each server is connected using **Fa0** interfaces into Switch0, which uplinks to the firewall through **Fa3/1 → Gi1/2**.

🔐 **4\. Firewall Segment (Cisco ASA)**
-------------------------------------------

The central red-colored device is a **Cisco ASA Firewall**.

### **Key Links**

*   **Gi1/1 → Core Switch (Fa0/10)**
    
*   **Gi1/3 → Router (Gi0/1)**
    
*   **Gi1/2 → Server Switch (Switch0)**
    

### **Firewall Purpose**

*   Performs **traffic filtering** between:
    
    *   Internal LAN (PC networks)
        
    *   DMZ (server network)
        
    *   WAN (router/internet)
        
*   Ensures secure access:
    
    *   PCs → Servers
        
    *   Internet → DMZ services (if allowed)
        

🚦 **5\. Router (R1 – 1841 Series)**
----------------------------------------

Connected to:

*   **Firewall** via **Gi0/1**
    
*   **Cloud/Internet** via **Fa0/1**
    

Configured with **public network 20.0.0.0/24**.

### **Router Functions**

*   Acts as a **gateway** to the external cloud/internet.
    
*   Performs **NAT** for outbound traffic.
    
*   Routes WAN ↔ Firewall traffic.
    

🏢 **6\. Core Switch (Center Switch)**
------------------------------------------

All PC networks connect here.

### **Ports Used**

*   **Fa0/1 – Fa0/9**
    
*   **Uplink to firewall on Fa0/10**
    
*   **Uplink to DHCP server on Gi0/2**
    

This is likely a **multi-VLAN switch** separating user networks.

🧑‍💻 **7\. User PC Networks**
----------------------------------

### **📍 Subnet 1: 192.168.10.0/24**

DeviceInterfacePC0Fa0PC1Fa0PC2Fa0

### **📍 Subnet 2: 192.168.20.0/24**

DeviceInterfacePC3Fa0PC4Fa0PC5Fa0

### **📍 Subnet 3: 192.168.30.0/24**

DeviceInterfacePC6Fa0PC7Fa0PC8Fa0

Each subnet is isolated through VLANs and routed via the firewall.

📦 **8\. DHCP Server (Right Side)**
---------------------------------------

Connected to the core switch via **Gig0/2**.

### **Purpose**

*   Dynamically assigns IP addresses to all PCs.
    
*   Likely configured with 3 DHCP pools:
    
    *   192.168.10.0/24
        
    *   192.168.20.0/24
        
    *   192.168.30.0/24
        

Servers on the left likely have **static IPs**.

🔄 **9\. Traffic Flow Explanation**
---------------------------------------

### 🔹 **Internal Communication**

PC Networks → Firewall → Server Network

*   The firewall allows DNS, HTTP, FTP access based on rules.
    

### 🔹 **External Communication**

PC/Server → Firewall → Router → Cloud

*   Router performs NAT for outbound traffic.
    

### 🔹 **DHCP Traffic**

PC → Broadcast → Core Switch → DHCP Server

*   DHCP replies routed back via the switch.
    

🧱 **10\. Logical Architecture Diagram (ASCII)**
----------------------------------------------------
<img width="853" height="694" alt="image" src="https://github.com/user-attachments/assets/7e425d35-ff64-4a8e-980b-db10c5a9b0c6" />


✅ **11\. Summary**
----------------------

This network implements segmentation, security, and organized routing:

*   **Firewall** protects between DMZ, LAN, and WAN.
    
*   **Router** connects to the internet and handles NAT.
    
*   **Servers** sit behind Switch0, reachable via firewall rules.
    
*   **Users** operate on 3 isolated subnets via VLANs.
    
*   **DHCP Server** automatically assigns IPs to client devices.
    
*   **Scalable & secure design** suitable for enterprise labs or production simulation.
