# 📄 README: Install Node Exporter and SmartCTL Exporter on Linux Servers

> Automated deployment of Prometheus exporters across multiple Linux servers using Ansible.

## 🎯 Purpose

Deploy the following exporters on all Linux servers:
- **Node Exporter** — system metrics (CPU, RAM, disks, network, etc.)
- **SmartCTL Exporter** — S.M.A.R.T. disk health metrics (**only on physical servers**)

These metrics are used in **Prometheus + Grafana** for infrastructure monitoring.

---

## 🧩 Components

| Exporter | Version | Port | Purpose |
|--------|--------|------|--------|
| `node_exporter` | `1.9.1` | `9100` | OS-level metrics: CPU, memory, load, filesystems, etc. |
| `smartctl_exporter` | `0.14.0` | `9633` | Disk health metrics via S.M.A.R.T. |

> ⚠️ **SmartCTL Exporter is installed only on physical servers** (hosts in the `physical_servers` group).

---
### Prerequisites

- Ansible 2.9+
- Target servers:
  - Debian/Ubuntu (systemd-based)
  - SSH access with sudo/root privileges
  - Internet access for downloads
  - Ansible installed
  - Inventory with groups defined
---
## 🚀 Quick Start
ansible-playbook -i hosts do_monitor.yml

---
## 📦 Node Exporter Installation
Updates package cache (apt update)
Installs dependencies: wget, gnupg, systemd-sysv
Downloads from GitHub:
https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
Extracts to /usr/local/bin/
Renames binary to node_exporter
Creates systemd unit: /etc/systemd/system/node_exporter.service
Enables and starts the service

## 💾 SmartCTL Exporter Installation
Downloads archive:
https://github.com/prometheus-community/smartctl_exporter/releases/download/v0.14.0/smartctl_exporter-0.14.0.linux-amd64.tar.gz
Extracts to /usr/local/bin/
Creates systemd unit: /etc/systemd/system/smartctl_exporter.service
Reloads systemd and starts the service

## 🔧 Verification
# Check services
systemctl status node_exporter
systemctl status smartctl_exporter

# Test endpoints
curl http://localhost:9100/metrics       # Node Exporter
curl http://localhost:9633/metrics       # SmartCTL Exporter

## Grafana Integration
Recommended dashboards:

Node Exporter: Dashboard 1860

SmartCTL Exporter: Dashboard 12499

## ⚠️ Important Notes
Binaries are downloaded from GitHub - ensure network access

Services run as root - consider dedicated users for production

No TLS/authentication - expose only in trusted networks

ignore_errors: yes may mask real issues - use judiciously
