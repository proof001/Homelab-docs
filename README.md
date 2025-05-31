# Homelab Documentation Repository

## Overview
This repository serves as the central hub for documentation, diagrams, and configuration files for my homelab infrastructure. The homelab is designed for virtualization, media streaming, file storage, and network experimentation, with a focus on high availability and scalability.

### Homelab Components
- **Proxmox Cluster**: 3x Dell OptiPlex Micro PCs (Node 1, Node 2, Node 3) for virtualization.
- **Plex Media Server**: Dell Precision 5810 for media streaming.
- **Networking**: Ubiquity Gateway Router/Firewall handling DHCP and VLANs.
- **Storage**: Buffalo NAS with 4TB (RAID 1, 2TB usable) for local storage and backups.
- **Windows Domain/File Server**: Dell PowerEdge running Windows Server for domain control and file sharing.

## Repository Structure
- **/docs/**: Detailed documentation for setup, maintenance, and procedures.
  - `infrastructure.md`: Overview of hardware, network, and storage configurations.
  - `proxmox-setup.md`: Proxm |ox cluster configuration and VM management.
  - `networking.md`: VLANs, firewall rules, and inter-device connectivity.
- **/diagrams/**: Network diagrams and architecture visuals.
  - `network-diagram.md`: Instructions to generate a network diagram using a diagramming tool.
- **/configs/**: Configuration files for services (e.g., Proxmox, Ubiquity Gateway).
  - `ubiquity-vlan-config.txt`: VLAN settings for the Ubiquity Gateway.
- **/scripts/**: Automation scripts for backups, monitoring, etc.
  - `backup-to-nas.sh`: Script to back up Proxmox VMs to Buffalo NAS.
- **README.md**: This file, providing an overview and navigation guide.

## Setup and Usage
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/homelab-docs.git
   ```
2. **Navigate to Relevant Sections**:
   - Read `/docs/infrastructure.md` for a full overview of the homelab setup.
   - Check `/diagrams/` for visual representations of the network.
   - Use `/scripts/` for automation tasks (e.g., backup scripts).
3. **Generate Diagrams**:
   - Follow instructions in `/diagrams/network-diagram.md` to recreate the network diagram using a tool like Mermaid or PlantUML.

## Contributing
- **Add Documentation**:
  - Create a new branch (e.g., `git checkout -b update-docs`).
  - Edit or add files in the `/docs/` directory.
  - Commit and push: `git push origin update-docs`.
  - Open a pull request to merge into the main branch.
- **Update Diagrams**:
  - Modify diagram prompts in `/diagrams/` and include updated visuals.
- **Sensitive Information**:
  - Do not commit sensitive data (e.g., IPs, credentials). Use `.gitignore` for such files.

## Maintenance
- **Regular Updates**:
  - Update documentation as the homelab evolves (e.g., new hardware, services).
  - Commit changes weekly to keep the repo current.
- **Backups**:
  - A copy of this repo is mirrored on the Buffalo NAS for redundancy.

## Contact
For questions or collaboration, open an issue on GitHub or reach out directly via GitHub Discussions.
