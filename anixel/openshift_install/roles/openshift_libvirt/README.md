# openshift_libvirt

Provisions the OpenShift UPI libvirt lab on the hypervisor: OPEN virtual network with DNS DHCP reservations, firewalld NAT to the HAProxy guest, a CentOS Stream load-balancer VM, and RHCOS bootstrap. Pair with **`anixel.openshift_install.openshift_configs`** in the same play; control-plane and worker VMs are not created yet beyond DHCP/DNS names in the network.

## Requirements

- Collection **`anixel.libvirt`** (`libvirt_network`, `libvirt_vm`, `firewalld_mgt`).
- **`libvirtd`** on the target host.
- Run after **`setup_clis`** and **`openshift_configs`** (`tasks/setup_configs.yml`), or set all **`openshift_libvirt_*`** inputs yourself.
- Inventory: cluster name, base domain, HAProxy VM password, and ignition / CoreOS image vars (typically wired from **`openshift_configs_*`** facts).

## Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `openshift_libvirt_coreos_qcow2_gz_location` | `""` | **Required.** RHCOS qemu `qcow2.gz` URL. |
| `openshift_libvirt_coreos_qcow2_gz_sha256` | `""` | **Required.** sha256 for the qcow2.gz artifact. |
| `openshift_libvirt_bootstrap_ign_path` | `""` | **Required.** Bootstrap ignition file path on the bastion. |
| `openshift_libvirt_master_ign_path` | `""` | **Required.** Control-plane ignition file path. |
| `openshift_libvirt_worker_ign_path` | `""` | **Required.** Worker ignition file path. |
| `openshift_libvirt_haproxy_vm_password` | `""` | **Required.** HAProxy VM cloud-init password. |
| `openshift_libvirt_cluster_name` | `""` | **Required.** Cluster name segment (e.g. `openshift-prod`). |
| `openshift_libvirt_base_domain` | `""` | **Required.** Base domain (e.g. `example.com`). |
| `openshift_libvirt_master_nodes` | `3` | Control-plane node count; HAProxy backends and DHCP names `master-0` … `master-N`. |
| `openshift_libvirt_worker_nodes` | `0` | Worker node count; HAProxy ingress backends and DHCP names `compute-0` … `compute-N`. |
| `openshift_libvirt_bootstrap_vm` | `memory: 16384`, `vcpus: 4`, `os_name: rhel9.6` | Bootstrap RHCOS VM sizing for **`tasks/nodes.yml`**. |

## Description

- **`tasks/asserts.yml`** — validate required inventory variables (included from **`tasks/main.yml`**).
- **`tasks/network.yml`** — build libvirt DHCP host entries (load balancer IP, then sequential IPs and FQDN names for masters and workers), define the OPEN lab network via **`anixel.libvirt.libvirt_network`**.
- **`tasks/firewall.yml`** — source NAT on the lab bridge and DNAT TCP 80/443/6443/22623 on the hypervisor to the load-balancer guest.
- **`tasks/haproxy.yml`** — CentOS Stream HAProxy VM with cloud-init config from **`templates/haproxy.cfg.j2`**.
- **`tasks/nodes.yml`** — RHCOS bootstrap VM (libvirt domain name `bootstrap`).

Network layout, HAProxy image URL, bridge name, and load-balancer IP (first address in the DHCP range) are fixed in **`vars/main.yml`**. Align **`openshift_libvirt_master_nodes`** / **`openshift_libvirt_worker_nodes`** with install-config replica counts from **`openshift_configs`**.
