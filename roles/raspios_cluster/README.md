# raspios\_cluster Role

## Synopsis

The `raspios_cluster` role orchestrates common configuration tasks for multi-node Raspberry Pi OS clusters. It handles synchronized user management and NFS share mounting to simplify the deployment and maintenance of Raspberry Pi clusters.

## Requirements

* Target hosts running Raspberry Pi OS (Buster, Bullseye, or later)
* Python 3 installed on all nodes
* SSH access with `sudo` privileges to each node
* Passwordless SSH or Ansible key-based connectivity between nodes for user sync

## Role Variables

| Variable                      | Default | Description                                                                               |
| ----------------------------- | ------- | ----------------------------------------------------------------------------------------- |
| `raspios_cluster_nodes`       | `[]`    | List of inventory hostnames or IPs forming the cluster.                                   |
| `raspios_cluster_user_sync`   | `true`  | Whether to synchronize user accounts across all cluster nodes.                            |
| `raspios_cluster_users`       | `[]`    | List of user definitions to create/sync (`name`, `uid`, `groups`, `ssh_authorized_keys`). |
| `raspios_cluster_nfs_enabled` | `true`  | Whether to mount NFS shares from a designated server.                                     |
| `raspios_cluster_nfs_server`  | `""`    | Hostname or IP of the NFS server.                                                         |
| `raspios_cluster_nfs_shares`  | `[]`    | List of NFS share definitions (`src`, `dest`, `opts`).                                    |

## Dependencies

* `community.general` for NFS and user modules

## Example Playbook

```yaml
- hosts: pi_cluster
  become: yes
  collections:
    - ajoeofalltrades.raspios
    - community.general
  roles:
    - role: ajoeofalltrades.raspios.raspios_cluster
      raspios_cluster_nodes:
        - pi01.local
        - pi02.local
        - pi03.local
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
```

## Tags

* `cluster` for multi-node orchestration tasks
* `users` for user synchronization
* `nfs` for NFS share management

## License

MIT

## Author Information

* Joe Clark (@ajoeofalltrades)
