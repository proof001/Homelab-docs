<p><a target="_blank" href="https://app.eraser.io/workspace/DpzInvpTQB3fzEzGdWyu" id="edit-in-eraser-github-link"><img alt="Edit in Eraser" src="https://firebasestorage.googleapis.com/v0/b/second-petal-295822.appspot.com/o/images%2Fgithub%2FOpen%20in%20Eraser.svg?alt=media&amp;token=968381c8-a7e7-472a-8ed6-4a6626da5501"></a></p>

Below is an outline for setting up and managing your homelab infrastructure, incorporating the components you've mentioned: a 3-node Proxmox cluster (Dell OptiPlex Micro PCs), a Dell Precision 5810 (Plex Media Server), a Ubiquity Gateway Router/Firewall (DHCP), a Buffalo NAS (4TB local storage), and a Dell PowerEdge (Windows Domain/File Server). This outline aligns with your prior conversations about virtualization, networking, storage, and documentation, and includes steps for implementation, maintenance, and scalability.

---

### Homelab Infrastructure Outline
#### 1. Define Goals and Objectives
- **Primary Goals**:
    - Virtualization using a 3-node Proxmox cluster.
    - Media streaming via Plex on Dell Precision 5810.
    - Centralized file storage and backups on Buffalo NAS (4TB).
    - Windows domain and file services on Dell PowerEdge.
    - Secure and isolated networking with Ubiquity Gateway Router/Firewall.
- **Secondary Goals**:
    - High availability (HA) for critical services.
    - Scalable storage and network performance.
    - Learning and experimentation (e.g., VLANs, clustering).
#### 2. Inventory and Hardware Setup
- **3x Dell OptiPlex Micro PCs (Proxmox Cluster)**:
    - Specs: Ensure 16–32GB RAM, quad-core CPU (e.g., Intel i5/i7), 512GB–1TB SSD per node.
    - Power: 19.5V, 90W adapters per node, connected to a 600VA UPS.
- **Dell Precision 5810 (Plex Media Server)**:
    - Specs: Upgrade to 64GB RAM, Xeon CPU, 1TB NVMe SSD, 4TB HDD for media.
    - Add 10GbE NIC for faster media transfers.
- **Ubiquity Gateway Router/Firewall**:
    - Model: Confirm UniFi Dream Machine Pro or similar.
    - Features: DHCP server, VLAN support, firewall rules.
- **Buffalo NAS (4TB)**:
    - Configure RAID 1 for redundancy (2TB usable).
    - Use Buffalo native software for file sharing (SMB/NFS).
- **Dell PowerEdge (Windows Domain/File Server)**:
    - Specs: Ensure 32–64GB RAM, 1TB SSD for OS, 4TB+ HDD for file storage.
    - Install Windows Server (e.g., 2022) for domain controller and file services.
- **Physical Setup**:
    - Use a 9U–12U rack for organization.
    - Ensure proper ventilation and cable management.
#### 3. Network Configuration
- **Topology**:
    - Ubiquity Gateway as central router: 192.168.1.1/24, DHCP range 192.168.1.2–254.
    - VLANs:
        - VLAN 10: Management (Proxmox, PowerEdge).
        - VLAN 20: Media (Plex, NAS).
        - VLAN 30: Guest/Testing.
- **Connections**:
    - Proxmox Cluster, Precision 5810, PowerEdge, and NAS connect to Ubiquity Gateway.
    - Direct 10GbE link between Precision 5810 and Buffalo NAS (10.0.0.1/24, MTU 9000).
- **Firewall Rules**:
    - Restrict VLAN 30 (Guest) from accessing VLAN 10 (Management).
    - Allow Plex traffic (port 32400) from VLAN 20 to external network.
#### 4. Storage Configuration
- **Proxmox Cluster**:
    - Deploy Ceph across 3 nodes (512GB SSD each, ~500GB usable with replication).
    - Use for VM storage with HA.
- **Buffalo NAS (4TB)**:
    - Configure as SMB/NFS share for Plex media and file server storage.
    - Backup target for Proxmox VMs and PowerEdge files.
- **Dell PowerEdge**:
    - Local 4TB HDD for file shares, mirrored to NAS for redundancy.
- **Backups**:
    - Proxmox Backup Server (PBS) on a Proxmox node, targeting NAS.
    - Offsite backup to Backblaze B2 for critical data (~2TB, ~$10/month).
#### 5. Virtualization and Services
- **Proxmox Cluster (3x OptiPlex Micro)**:
    - VMs/Containers:
        - File Server (Samba, 2 vCPUs, 4GB RAM).
        - VPN (WireGuard, 1 vCPU, 2GB RAM).
        - Test VMs (e.g., Windows 10, 2 vCPUs, 8GB RAM).
    - Enable HA for critical services.
- **Dell Precision 5810**:
    - Run Plex natively with GPU transcoding.
    - Optional: Add lightweight VMs (e.g., media management tools).
- **Dell PowerEdge**:
    - Windows Server 2022 as domain controller.
    - File server with shared folders, synced to NAS.
- **Resource Allocation**:
    - Balance VMs across Proxmox nodes (e.g., File Server on Node 1, VPN on Node 2, Test VM on Node 3).
#### 6. Backup and Disaster Recovery
- **Local Backups**:
    - Daily incremental VM backups to NAS via PBS.
    - Weekly file backups from PowerEdge to NAS.
- **Offsite Backups**:
    - Monthly sync to Backblaze B2 for critical data.
- **Recovery Plan**:
    - Test restores quarterly.
    - Document recovery steps (e.g., reinstall Proxmox, restore Ceph, recover Windows domain).
#### 7. Monitoring and Security
- **Monitoring**:
    - Use Proxmox GUI for cluster health.
    - Install Grafana/Prometheus on a VM for metrics (CPU, RAM, network).
- **Security**:
    - Ubiquity Gateway firewall rules to isolate VLANs.
    - Encrypt Backblaze backups.
    - Update all systems regularly (Proxmox, Windows Server, Ubiquity firmware).
#### 8. Documentation
- **Format**: Markdown file stored on NAS or Git repo.
- **Sections**:
    - Hardware specs and roles.
    - Network configuration (IP addresses, VLANs).
    - Storage setup (Ceph, NAS, backups).
    - Virtualization details (VMs, HA settings).
    - Maintenance schedules and procedures.
- **Example Procedure**:
    - Add new VM: Create in Proxmox GUI, assign VLAN, configure backup.
#### 9. Implementation Steps
- **Step 1**: Set up hardware (rack, power, cabling).
- **Step 2**: Configure Ubiquity Gateway (DHCP, VLANs, firewall).
- **Step 3**: Deploy Ceph on Proxmox cluster.
- **Step 4**: Configure Buffalo NAS (RAID, SMB/NFS).
- **Step 5**: Install Windows Server on PowerEdge, set up domain/file shares.
- **Step 6**: Install Plex on Precision 5810, connect to NAS.
- **Step 7**: Set up backups (PBS, Backblaze).
- **Step 8**: Document setup and test connectivity.
#### 10. Maintenance and Scalability
- **Weekly**:
    - Check Proxmox/Ceph health.
    - Verify backups.
- **Monthly**:
    - Update firmware/software.
    - Test restores.
- **Scalability**:
    - Add Proxmox nodes for more VMs.
    - Upgrade NAS storage or switch to TrueNAS on PowerEdge.
    - Expand to 10GbE networking for all devices.
#### 11. Budget and Resources
- **Estimated Costs**:
    - Hardware upgrades (RAM, SSDs): $500–$1000.
    - UPS and rack: $200–$500.
    - Backblaze B2 (2TB): $10/month.
- **Cost-Saving Tips**:
    - Use refurbished hardware.
    - Leverage open-source software (Proxmox, PBS).
---

This outline provides a structured approach to building, managing, and scaling your homelab. Let me know if you'd like to expand any section (e.g., detailed steps for a specific task) or adjust priorities (e.g., focus on security or cost-saving)!


<!-- eraser-additional-content -->
## Diagrams
<!-- eraser-additional-files -->
<a href="/diagrams/Homelab-Homelab Network Topology-1.eraserdiagram" data-element-id="yTZHfPaY2bOBr9RnIz3wo"><img src="/.eraser/DpzInvpTQB3fzEzGdWyu___HFXQVSu3pldcodNSLP8va2rBHFg1___---diagram----9ac967197578c5901e21d1bdbd95bf15-Homelab-Network-Topology.png" alt="" data-element-id="yTZHfPaY2bOBr9RnIz3wo" /></a>
<!-- end-eraser-additional-files -->
<!-- end-eraser-additional-content -->
<!--- Eraser file: https://app.eraser.io/workspace/DpzInvpTQB3fzEzGdWyu --->