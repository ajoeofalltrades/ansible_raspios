# raspios\_network Role

## Synopsis

The `raspios_network` role configures network settings on Raspberry Pi OS hosts. It provides flexible support for both DHCP and static IP configurations across Ethernet and Wi-Fi interfaces, and integrates advanced networking features such as bridge setups and VLAN tagging.

## Requirements

* Target hosts running Raspberry Pi OS (Buster, Bullseye, or later)
* Python 3 installed on target
* SSH access with `sudo` privileges

## Role Variables

| Variable                            | Default | Description                                                                                             |
| ----------------------------------- | ------- | ------------------------------------------------------------------------------------------------------- |
| `raspios_network_eth_enabled`       | `true`  | Whether to configure Ethernet (`eth0`).                                                                 |
| `raspios_network_eth_dhcp`          | `true`  | Use DHCP for `eth0` if enabled.                                                                         |
| `raspios_network_eth_static`        | `{}`    | Static IP settings for `eth0` when DHCP is disabled (contains `address`, `netmask`, `gateway`, `dns`).  |
| `raspios_network_wifi_enabled`      | `false` | Whether to configure Wi-Fi (`wlan0`).                                                                   |
| `raspios_network_wifi_dhcp`         | `true`  | Use DHCP for `wlan0` if enabled.                                                                        |
| `raspios_network_wifi_static`       | `{}`    | Static IP settings for `wlan0` when DHCP is disabled (contains `address`, `netmask`, `gateway`, `dns`). |
| `raspios_network_wifi_ssid`         | `""`    | SSID for Wi-Fi network if Wi-Fi is enabled.                                                             |
| `raspios_network_wifi_psk`          | `""`    | Pre-shared key for Wi-Fi network.                                                                       |
| `raspios_network_bridge_enabled`    | `false` | Whether to create a network bridge (e.g., `br0`).                                                       |
| `raspios_network_bridge_interfaces` | `[]`    | List of interfaces (e.g., `['eth0', 'wlan0']`) to include in the bridge when enabled.                   |
| `raspios_network_vlans`             | `[]`    | List of VLAN definitions (each containing `id`, `interface`, and optional `name`).                      |

## Dependencies

* `community.general` collection for `nmcli` and `network_connections` modules

## Example Playbook

```yaml
- hosts: pi
  become: yes
  collections:
    - ajoeofalltrades.raspios
    - community.general
  roles:
    - role: ajoeofalltrades.raspios.raspios_network
      raspios_network_eth_enabled: true
      raspios_network_eth_dhcp: false
      raspios_network_eth_static:
        address: "192.168.1.100/24"
        gateway: "192.168.1.1"
        dns:
          - "8.8.8.8"
          - "8.8.4.4"
      raspios_network_wifi_enabled: true
      raspios_network_wifi_ssid: "HomeNetwork"
      raspios_network_wifi_psk: "SuperSecretPass"
      raspios_network_bridge_enabled: true
      raspios_network_bridge_interfaces:
        - eth0
        - wlan0
      raspios_network_vlans:
        - id: 10
          interface: eth0
          name: "guest"
```

## Tags

* `network` for all networking tasks
* `dhcp` for DHCP configuration
* `static` for static IP setup
* `wifi` for wireless configuration
* `bridge` for bridge interface setup
* `vlan` for VLAN tagging

## License

MIT

## Author Information

* Joe Clark (@ajoeofalltrades)
