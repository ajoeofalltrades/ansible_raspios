# ajoeofalltrades.raspios Collection

## Overview

The `ajoeofalltrades.raspios` Ansible Collection provides a set of modular roles to configure and manage Raspberry Pi OS (formerly Raspbian) systems, from single devices to multi-node clusters. Each role focuses on a specific domain—base system configuration, networking, security hardening, cluster orchestration, and hardware settings—allowing you to pick and compose only the functionality you need.

## Included Roles

* **`raspios_base`**:

  * Updates apt cache and optionally upgrades packages
  * Installs essential packages
  * Configures system locale and timezone

* **`raspios_network`**:

  * Manages Ethernet and Wi-Fi (DHCP or static)
  * Supports network bridges and VLAN tagging

* **`raspios_security`**:

  * SSH hardening (port changes, disable root/password auth)
  * UFW firewall configuration
  * Fail2Ban installation and setup
  * User and sudo restrictions

* **`raspios_cluster`**:

  * Synchronizes specified users across cluster nodes
  * Mounts NFS shares from a central server

* **`raspios_hardware`**:

  * Enables/disables hardware interfaces via `raspi-config` (SPI, I²C, UART, camera, SSH)
  * Configures GPU memory split and audio output
  * Manages `/boot/config.txt` and `/boot/cmdline.txt` settings

## Requirements

* Ansible 2.13 or later
* Python 3 on control and target machines
* SSH connectivity with `sudo` privileges
* `raspi-config` utility available on target (for `raspios_hardware`)
* For `raspios_network`: requires `community.general` collection
* For `raspios_security`: requires `community.general` collection
* For `raspios_cluster`: requires `community.general` collection

## Installation

Install the collection from Ansible Galaxy:

```bash
ansible-galaxy collection install ajoeofalltrades.raspios
```

Alternatively, include it directly from a Git repository in your playbook:

```yaml
collections:
  - name: ajoeofalltrades.raspios
    source: https://github.com/ajoeofalltrades/ansible-raspios
```

## Quick Start

```yaml
- hosts: all
  become: yes
  collections:
    - ajoeofalltrades.raspios
    - community.general

  roles:
    # Base system setup
    - role: ajoeofalltrades.raspios.raspios_base
      raspios_base_timezone: "America/Denver"
      raspios_base_packages:
        - htop
        - git
        - vim

    # Network configuration
    - role: ajoeofalltrades.raspios.raspios_network
      raspios_network_eth_enabled: true
      raspios_network_eth_dhcp: false
      raspios_network_eth_static:
        address: "192.168.1.50/24"
        gateway: "192.168.1.1"
        dns:
          - "8.8.8.8"
          - "8.8.4.4"

    # Security hardening
    - role: ajoeofalltrades.raspios.raspios_security
      raspios_security_ssh_port: 2222
      raspios_security_ssh_disable_password: true
      raspios_security_firewall_allowed_ports:
        - 2222
        - 80
        - 443

    # Cluster orchestration (if managing multiple nodes)
    - role: ajoeofalltrades.raspios.raspios_cluster
      raspios_cluster_nodes:
        - pi01.local
        - pi02.local
      raspios_cluster_users:
        - name: deploy
          uid: 2000
          groups:
            - sudo
          ssh_authorized_keys:
            - "ssh-rsa AAAA... user@example.com"
      raspios_cluster_nfs_server: "nfs-master.local"
      raspios_cluster_nfs_shares:
        - src: "/export/data"
          dest: "/mnt/data"
          opts: "rw,sync"

    # Hardware configuration
    - role: ajoeofalltrades.raspios.raspios_hardware
      raspios_hardware_spi: true
      raspios_hardware_i2c: true
      raspios_hardware_gpu_mem: 64
      raspios_hardware_audio: "hdmi"
```

## Documentation & Support

* Each role contains its own `README.md` with detailed variables and examples.
* Issues and contributions: [https://github.com/ajoeofalltrades/ansible-raspios](https://github.com/ajoeofalltrades/ansible-raspios)
* License: MIT

Enjoy a streamlined, modular approach to managing Raspberry Pi OS systems with Ansible!

## Author Information

* Joe Clark (@ajoeofalltrades)