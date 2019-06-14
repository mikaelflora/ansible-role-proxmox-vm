# ansible-role-proxmox-vm

## Overview

Ansible role - manage VM via Proxmox API (clone, configure, start, stop and remove).

Tags:  

  - **up:** clone, configure and start the VM
  - **halt:** stop the VM
  - **reload:** stop, reconfigure and start the VM
  - **destroy:** stop and remove the VM
  - **status:** get the current status of the VM

## Getting started with an example

### Configure

Edit `proxmox-vm/vars/main.yml`:  

```yaml
---
# vars file for proxmox-vm
proxmox_host: proxmox.lan
proxmox_user: root@pam
proxmox_password: password
node: node1
template_name: base
vm_name: test
vm_user: user
vm_password: password
vm_sshkeys:
  - 'ssh-rsa XXX...XXX user@localhost'
vm_ipconfig0: 'ip=192.168.0.10/24,gw=192.168.0.254'
vm_nameserver: '8.8.8.8 8.8.4.4'
```

### Run

`playbook.yml`:  

```yaml
- hosts: localhost
  connection: local
  gather_facts: false

  roles:
    - proxmox-vm
```

Running playbook:  

```bash
# create VM from template
ansible-playbook playbook.yml
# destroy VM
ansible-playbook playbook.yml --tags destroy
```

## Variables

### Required

| Name | Type | Description |
| --- | --- | --- |
| `proxmox_host` | string | Proxmox IP address or hostname (ex: `192.168.0.10`, `proxmox.lan`) |
| `proxmox_user` | string | Proxmox user (ex: `root@pam`, `admi@pve`) |
| `proxmox_password` | string | Proxmox user password |
| `node` | string | node name (ex: `node1`) |
| `template_name` | string | template name. If `template_vmid` is setted, it can take arbitrary value |
| `vm_name` | string | VM name. If `vm_vmid` is setted, it is not required |
| `vm_vmid` | int | VMID (`100-N`). If `vm_name` is setted, it is not required |

### Optional

| Name | Type | Description |
| --- | --- | --- |
| `vm_user` | string | user name (*cloud-init required*) |
| `vm_password` | string | user password (*cloud-init required*) |
| `vm_sshkeys` | list of string | list of public SSH keys (OpenSSH format - *cloud-init required*) |
| `vm_ipconfig0` | string | Ip address and gateway (ex: `ip=192.168.0.20/24,gw=192.168.0.254` - *cloud-init required*) |
| `vm_nameserver` | string | DNS server IP address (ex: `8.8.8.8 8.8.4.4` - *cloud-init required*) |
| `vm_searchdomain` | string | DNS search domains (ex: `google-public-dns-a.google.com` - *cloud-init required*) |
| `vm_sockets` | int | the number of CPU sockets (ex: `2`) |
| `vm_cores` | int | the number of cores per socket (ex: `2`) |
| `vm_memory` | int | amout of RAM for the VM in `MB` (ex: `512`, `1024`, `2048`) |
| `vm_diskname` | string | the disk to resize (ex: `scsi0`, `ide1`). Requires `vm_disksize` |
| `vm_disksize` | string | the new disk size (ex: `20G`, `1T`). Requires `vm_diskname` |
| `template_vmid` | int | template VMID (`100-N`) |

## Licence

GNU General Public License v3 (GPL-3). See `LICENSE` for further details.  
