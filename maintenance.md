## Home Lab OSI Layer 2 (Data Link) Maintenance Checklist

### Daily/Weekly: Frame & Link Monitoring

* [ ] **Port Status Audit:** Check the UniFi Network console to ensure all 10G and 1G switch ports are negotiating at their expected speeds (no unexpected fallbacks from 10G to 1G).
* [ ] **Error Counters:** Monitor the USW-Pro-Aggregation and UCG-Fiber switch ports for dropped frames, RX/TX errors, or CRC errors (which often indicate a failing DAC cable or dirty SFP+ optic).
* [ ] **MAC/ARP Anomalies:** Briefly review the MAC address tables for unknown devices or MAC flapping (a device rapidly switching between ports).

### Monthly: Topology & Link Health

* [ ] **VLAN Tagging Audit:** Verify that switchport profiles are strictly enforced. Ensure the ports connecting to your gaming consoles, TVs, or IoT devices are untagged for their specific VLANs and isolated from the Proxmox/TrueNAS management VLAN.
* [ ] **LACP / Bond Status:** If you are using Link Aggregation (bonding multiple NICs together on TrueNAS or Proxmox for redundancy), verify both physical links are actively passing traffic and balancing the load.
* [ ] **Spanning Tree Protocol (STP) Check:** Check the switch logs for unexpected STP topology changes or blocked ports, which indicate network loops or misconfigured virtual switches inside Proxmox.

### Quarterly: Optimization & Firmware

* [ ] **Switch Firmware Updates:** Apply updates specifically to your Layer 2 switching hardware (the USW-Pro-Aggregation and the switching plane of the UCG-Fiber) during a low-traffic window.
* [ ] **Jumbo Frames (MTU) Verification:** If you are utilizing Jumbo Frames (MTU 9000) for your 10G storage traffic between TrueNAS and Proxmox, verify that MTU sizes match perfectly across the server NICs, virtual bridges, and the UniFi switch to prevent frame fragmentation.
* [ ] **Switchport Security Review:** Audit any MAC-based filtering, 802.1X authentication, or port isolation (Private VLAN) settings to ensure they are functioning as intended.

### Bi-Annually: Resilience Drills

* [ ] **Link Aggregation Failover Test:** Physically unplug one DAC cable from a bonded LACP group (e.g., on your TrueNAS box) while a file transfer is active. Verify that Layer 2 traffic seamlessly fails over to the surviving cable without dropping the connection.
* [ ] **Broadcast Storm Protection Test:** Verify that broadcast storm control is enabled on the UniFi switch to prevent a misconfigured network device from flooding the Layer 2 domain and locking up the switch.

### As Needed: Layer 2 Provisioning

* [ ] **Virtual Switch Maintenance:** Create or modify Linux Bridges (vmbr) or Open vSwitch configurations inside Proxmox to map new VMs to the correct physical NICs and VLAN tags.
* [ ] **MAC Address Spoofing/Cloning:** Update MAC addresses on your UCG-Fiber WAN port if your ISP requires a specific hardware MAC for authentication, or assign static MAC addresses to newly spun-up virtual machines.
