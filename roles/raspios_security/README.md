# raspios\_security Role

## Synopsis

The `raspios_security` role applies essential security hardening to Raspberry Pi OS hosts. It focuses on SSH hardening, firewall configuration, user and sudo restrictions, and installation of intrusion prevention tools.

## Requirements

* Target hosts running Raspberry Pi OS (Buster, Bullseye, or later)
* Python 3 installed on target
* SSH access with a user having `sudo` privileges

## Role Variables

| Variable                                  | Default  | Description                                                                |
| ----------------------------------------- | -------- | -------------------------------------------------------------------------- |
| `raspios_security_ssh_harden`             | `true`   | Enable SSH hardening (port change, disable root login, disable passwords). |
| `raspios_security_ssh_port`               | `22`     | SSH port to set when hardening is enabled.                                 |
| `raspios_security_ssh_disable_root`       | `true`   | Disable root login over SSH.                                               |
| `raspios_security_ssh_disable_password`   | `false`  | Disable password authentication, requiring keys only.                      |
| `raspios_security_firewall`               | `true`   | Whether to configure UFW firewall.                                         |
| `raspios_security_firewall_default`       | `deny`   | Default policy for firewall (`allow` or `deny`).                           |
| `raspios_security_firewall_allowed_ports` | `[22]`   | List of ports to allow through the firewall.                               |
| `raspios_security_fail2ban`               | `true`   | Install and configure Fail2Ban to protect SSH.                             |
| `raspios_security_fail2ban_jail`          | `"sshd"` | The Fail2Ban jail to configure (e.g., `sshd`).                             |
| `raspios_security_users`                  | `[]`     | List of additional users to create with restricted sudo access.            |
| `raspios_security_sudo_nopasswd`          | `false`  | Grant passwordless sudo to `raspios_security_users` when creating.         |

## Dependencies

* `geerlingguy.firewall` role for UFW management (optional)
* `community.general` for `ufw` and `fail2ban` modules

## Example Playbook

```yaml
- hosts: pi
  become: yes
  collections:
    - ajoeofalltrades.raspios
    - community.general
  roles:
    - role: ajoeofalltrades.raspios.raspios_security
      raspios_security_ssh_port: 2222
      raspios_security_ssh_disable_password: true
      raspios_security_firewall_allowed_ports:
        - 2222
        - 80
        - 443
      raspios_security_users:
        - alice
        - bob
      raspios_security_sudo_nopasswd: true
```

## Tags

* `ssh` for SSH hardening
* `firewall` for UFW configuration
* `fail2ban` for intrusion prevention
* `users` for user and sudo management

## License

MIT

## Author Information

* Joe Clark (@ajoeofalltrades)
