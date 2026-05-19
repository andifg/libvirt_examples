# opnsense_vm

Thin wrapper around **`libvirt_vm`**: sets **`libvirt_vm_disk_type: img`** and **`libvirt_vm_download_url`** from **`opnsense_vm_url`** (OPNsense **nano**, typically **`.img.bz2`**). The image is staged and converted to **`{{ opnsense_vm_images_dir }}/vm-{{ opnsense_vm_name }}.qcow2`**, then the domain is created (**`import: true`**, single disk).

## Requirements

- **community.libvirt** (via **`libvirt_vm`**).

## Variables

| Variable | Default | Purpose |
|----------|---------|---------|
| **`opnsense_vm_networks`** | **`[]`** (required): list of **`{ network: <libvirt net name> }`** for **`libvirt_vm_networks`**. |
| **`opnsense_vm_name`**, **`opnsense_vm_url`**, **`opnsense_vm_os_name`** | see **`defaults/main.yml`** | Passed to **`libvirt_vm`** with **`libvirt_vm_disk_type: img`** and **`libvirt_vm_download_url`**. |
| **`opnsense_vm_image_cache_dir`**, **`opnsense_vm_images_dir`**, **`opnsense_vm_qcow2_download_timeout`** | | Passed through to **`libvirt_vm`**. |
| **`opnsense_vm_cloud_init_enabled`** | **`false`** — sets **`libvirt_vm_cloud_init_enabled`**. |
