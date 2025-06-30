# raspios\_hardware Role

## Synopsis

The `raspios_hardware` role uses the `raspi-config` CLI (and direct edits to config files) to manage Raspberry Pi-specific hardware settings on Raspberry Pi OS hosts. It covers enabling/disabling interfaces (SPI, I²C, UART, camera), adjusting GPU memory split, configuring audio, and other platform-specific tweaks.

## Requirements

* Target hosts running Raspberry Pi OS (Buster, Bullseye, or later)
* Python 3 installed on target
* SSH access with `sudo` privileges
* `raspi-config` utility available on target (included by default on Raspberry Pi OS)

## Role Variables

| Variable                            | Default  | Description                                                                                        |
| ----------------------------------- | -------- | -------------------------------------------------------------------------------------------------- |
| `raspios_hardware_spi`              | `false`  | Enable (`true`) or disable (`false`) SPI interface via `raspi-config`.                             |
| `raspios_hardware_i2c`              | `false`  | Enable or disable I²C interface.                                                                   |
| `raspios_hardware_uart`             | `false`  | Enable or disable serial (UART) interface.                                                         |
| `raspios_hardware_camera`           | `false`  | Enable or disable the camera interface.                                                            |
| `raspios_hardware_ssh`              | `true`   | Enable (`true`) or disable (`false`) SSH server via `raspi-config`.                                |
| `raspios_hardware_gpu_mem`          | `16`     | GPU memory split in MB.                                                                            |
| `raspios_hardware_audio`            | `"auto"` | Audio output (`"auto"`, `"hdmi"`, or `"analog"`).                                                  |
| `raspios_hardware_boot_delay`       | `0`      | Boot delay in seconds (affects `/boot/config.txt` setting `boot_delay`).                           |
| `raspios_hardware_overlayfs`        | `false`  | Enable or disable the overlay filesystem via environment variables and `cmdline.txt` adjustments.  |
| `raspios_hardware_config_file_vars` | `{}`     | Dictionary of additional `/boot/config.txt` key/value pairs to manage directly (e.g., `gpu_freq`). |

## Dependencies

* None beyond standard Raspberry Pi OS packages; uses `command` or `shell` modules to invoke `raspi-config` and `lineinfile` to edit `/boot/config.txt` and `/boot/cmdline.txt`.

## Example Playbook

```yaml
- hosts: pi
  become: yes
  collections:
    - ajoeofalltrades.raspios
  roles:
    - role: ajoeofalltrades.raspios.raspios_hardware
      raspios_hardware_spi: true
      raspios_hardware_i2c: true
      raspios_hardware_camera: false
      raspios_hardware_gpu_mem: 64
      raspios_hardware_audio: "hdmi"
      raspios_hardware_config_file_vars:
        hdmi_safe: 1
        disable_overscan: 0
```

## Tags

* `hardware` for all platform-specific settings
* `interfaces` for enabling/disabling hardware interfaces
* `gpu` for GPU memory and frequency
* `audio` for audio output configuration
* `config` for direct config file management

## License

MIT

## Author Information

* Joe Clark (@ajoeofalltrades)