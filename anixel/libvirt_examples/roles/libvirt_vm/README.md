# libvirt_vm

Provisions **one** libvirt guest: download → optional bz2/raw handling → copy disk → `virt_install` with cloud-init.

## Variables

All settings exposed by this role use the **`libvirt_vm_`** prefix.

### Guest (required / optional)

| Variable | Required | Default / notes |
|----------|----------|-----------------|
| `libvirt_vm_name` | yes | Libvirt domain name; disk `vm-{{ libvirt_vm_name }}.qcow2` |
| `libvirt_vm_url` | yes | Image URL |
| `libvirt_vm_os_name` | yes | `virt_install` osinfo name |
| `libvirt_vm_memory` | no | `2048` |
| `libvirt_vm_vcpus` | no | `2` |
| `libvirt_vm_graphics_type` | no | `none` |
| `libvirt_vm_autostart` | no | `true` |
| `libvirt_vm_networks` | no | If set (non-empty list of `{ network: <libvirt net name> }`), used as **`virt_install`** NICs; see **`playbooks/opnsense_setup.yml`** (WAN + LAN). |
| `libvirt_vm_network` | no | Single NIC when **`libvirt_vm_networks`** is unset; falls back to **`libvirt_vm_nat_network_name`** |
| `libvirt_vm_nat_network_name` | no | `openshift` — default libvirt network for the single-NIC path |
| `libvirt_vm_cloud_init_user_data` | no | `libvirt_vm_cloud_init_default` |
| `libvirt_vm_cloud_init_disable` | no | `true` |

### Paths and timeouts

| Variable | Default |
|----------|---------|
| `libvirt_vm_image_cache_dir` | `/var/lib/libvirt/image-cache` |
| `libvirt_vm_images_dir` | `/var/lib/libvirt/images` |
| `libvirt_vm_qcow2_download_timeout` | `3600` |

Playbooks set **`libvirt_vm_*`** on the play (see **`playbooks/opnsense_setup.yml`**) or loop **`include_role: libvirt_vm`** over a dict if you define several guests.
