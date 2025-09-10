# OSS/BSS Mediation & Provisioning System (Multi-vendor)

This project represents a **multi-vendor OSS platform** designed for telecom environments, enabling automated provisioning, monitoring, and customer support across Level 1 to Level 3 operations.  
It integrates heterogeneous access devices with a centralized OSS, providing agility, scalability, and reliability in network operations.

---

## 📌 My Role & Contributions
- **Mediation Layer (Perl):** Implemented the core mediation logic for device command normalization, enabling multi-vendor and multi-protocol support.  
- **Database (MySQL):** Designed and extended schemas for provisioning workflows, command repositories, and monitoring data.  
- **OSS Integrations:** Contributed to the data exchange between OSS, BSS, and reporting systems.  
- **Device Testing:** Validated CPEs and ONTs through **Telnet, SSH, SNMP, and TR-069** protocols.  

---

## 🚀 Key Features
- **Multi-vendor agnostic:** Supports DSLAM, OLT, ONT, CPE, and switches from different vendors.  
- **Dynamic automation:** Provisioning commands stored and managed from the database, enabling one-click provisioning and zero-touch workflows.  
- **Integrated monitoring:** Real-time operational visibility across access and core network elements.  
- **Scalability:** Designed to handle thousands of devices concurrently across multiple protocols.  
- **End-to-end operations:** Supports troubleshooting and service management from Level 1 helpdesk up to advanced Level 3 engineering.  

---

## 🗂️ Architecture Diagrams (C4 Model with PlantUML)

All diagrams are available both as **PlantUML source (`.puml`)** and as **rendered PNGs (`.png`)**.  
You can view the rendered images below or explore the source files inside the [`diagrams/`](./diagrams) folder.

---

### Level 1 – System Context
- [PUML Source](./diagrams/L1_Context.puml)  
- [Rendered PNG](./diagrams/L1_Context.png)  

![L1_Context](./diagrams/L1_Context.png)

---

### Level 2 – Container View
- [PUML Source](./diagrams/L2_Container.puml)  
- [Rendered PNG](./diagrams/L2_Container.png)  

![L2_Container](./diagrams/L2_Container.png)

---

### Level 3 – Component View (OSS Core Components)
- [PUML Source](./diagrams/L3_Components.puml)  
- [Rendered PNG](./diagrams/L3_Components.png)  

![L3_Components](./diagrams/L3_Components.png)

---

## 📄 License
This project is shared under the [MIT License](../LICENSE).
