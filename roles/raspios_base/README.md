# raspbian\_base Role

## Synopsis

The `raspbian_base` role provides foundational configuration for Raspberry Pi OS (formerly Raspbian), ensuring a consistent and secure starting point for further customization. It handles package updates, essential package installation, system locale and timezone configuration, and basic system hardening.

## Requirements

* Target hosts running Raspberry Pi OS (Buster, Bullseye, or later)
* Python 3 installed on target
* SSH access to target with a user having `sudo` privileges

## Role Variables

| Variable                         | Default       | Description                                                |
| -------------------------------- | ------------- | ---------------------------------------------------------- |
| `raspbian_base_update_cache`     | `true`        | Whether to update the apt cache before package operations. |
| `raspbian_base_upgrade_system`   | `false`       | Whether to run `apt upgrade -y` to upgrade all packages.   |
| `raspbian_base_packages`         | `[]`          | List of additional apt packages to install.                |
| `raspbian_base_locale`           | `en_US.UTF-8` | System locale to configure.                                |
| `raspbian_base_timezone`         | `UTC`         | System timezone to configure.                              |
| `raspbian_base_harden_ssh`       | `true`        | Whether to apply basic SSH hardening settings.             |
| `raspbian_base_ssh_port`         | `22`          | SSH port to configure if `harden_ssh` is enabled.          |
| `raspbian_base_ssh_disable_root` | `true`        | Whether to disable root login over SSH.                    |

## Dependencies

None. This role is designed to operate independently, though other roles may depend on its baseline configuration.

## Example Playbook

```yaml
- hosts: pi
  become: yes
  collections:
    - ajoeofalltrades.raspbian
  roles:
    - role: ajoeofalltrades.raspbian.raspbian_base
      raspbian_base_timezone: "America/Los_Angeles"
      raspbian_base_packages:
        - htop
        - git
        - vim
```

## Tags

* `always` for tasks that must run on every execution (e.g., apt update)
* `packages` for package installation operations
* `locale` for locale configuration
* `timezone` for timezone configuration
* `ssh` for SSH hardening tasks

## License

MIT

## Author Information

* Joe Clark (@ajoeofalltrades)
