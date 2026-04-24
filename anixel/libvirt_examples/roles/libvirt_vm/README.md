# libvirt_vm

Provisions **one** libvirt guest using flat **`libvirt_vm_*`** variables. **`tasks/main.yml`** checks Red Hat family and **`libvirt_vm_disk_type`**, then includes **`tasks/disk_type_block.yml`** (**`qcow2`** / **`raw`** / **`img`**) or **`tasks/disk_type_iso_install.yml`** (**`iso`**). **`libvirt_vm_networks`** is passed verbatim to **`virt_install`** **`networks`**.

## Requirements

- Target host: Red Hat family (**`ansible_os_family == "RedHat"`**).
- Collections: **`community.libvirt`**, **`ansible.builtin`**.
- Hypervisor: **`virt-install`** (required by the module), **`qemu-img`**, and space under **`libvirt_vm_image_cache_dir`**, **`libvirt_vm_images_dir`** (import), and **`libvirt_vm_disk_dir`** (ISO install).

## Disk modes (`libvirt_vm_disk_type`)

| Value | Meaning |
|-------|---------|
| **`qcow2`** or **`raw`** | **Import**: **`get_url`** → cache → optional **bunzip2** → optional **raw/img → qcow2** (when URL yields **`.raw`/`.img`**) → copy to **`{{ libvirt_vm_images_dir }}/vm-{{ libvirt_vm_name }}.qcow2`**, then **`virt_install`** (**`import: true`**, **`qcow2`**). |
| **`img`** | Same **`disk_type_block.yml`** pipeline as **`qcow2`**, but **`img`** forces conversion to **`qcow2`** whenever the cached artifact is not already **`.qcow2`** (use for OPNsense **nano** **`.img`** / **`.img.bz2`**). |
| **`iso`** | **Install from ISO**: fetch installer ISO to **`libvirt_vm_iso_installer_path`**, then **`virt_install`** (**`libvirt_vm_iso_install_disk_size_gib`**, **`libvirt_vm_iso_install_extra_args`**, **`libvirt_vm_iso_install_console`** — see **`defaults/main.yml`** and **`disk_type_iso_install.yml`**). |

## Variables (summary)

| Variable | Required | Default / notes |
|----------|----------|-----------------|
| **`libvirt_vm_name`** | yes | Libvirt domain name |
| **`libvirt_vm_os_name`** | yes | **`virt_install`** **`osinfo`** name |
| **`libvirt_vm_disk_type`** | no | **`qcow2`**, **`raw`**, **`img`**, or **`iso`** |
| **`libvirt_vm_download_url`** | yes (per pipeline file) | Image or installer ISO URL (**`get_url`**) |
| **`libvirt_vm_image_cache_dir`** | no | **`/var/lib/libvirt/image-cache`** — download and staging cache (import + ISO) |
| **`libvirt_vm_images_dir`** | no | **`/var/lib/libvirt/images`** — import disk is **`vm-{{ libvirt_vm_name }}.qcow2`** here |
| **`libvirt_vm_disk_dir`** | no | **`{{ libvirt_vm_images_dir }}/vm-{{ libvirt_vm_name }}`** — used for **ISO** install artifacts only |
| **`libvirt_vm_iso_filename`**, **`libvirt_vm_iso_installer_path`** | no | ISO install: installer basename and full path (default **`{{ libvirt_vm_disk_dir }}/{{ libvirt_vm_iso_filename }}`**) |
| **`libvirt_vm_iso_install_disk_size_gib`** | no | **ISO** install: new disk size in GiB (**virt_install** **`disks`**); default **20**. |
| **`libvirt_vm_iso_install_extra_args`** | no | **`virt_install`** **`extra_args`** for **ISO** installs (default Anaconda text-on-serial). |
| **`libvirt_vm_iso_install_console`** | no | **`virt_install`** **`console`** dict for **ISO** installs (default PTY + **`target_type: serial`**). |
| **`libvirt_vm_memory`**, **`libvirt_vm_vcpus`**, **`libvirt_vm_graphics_type`**, **`libvirt_vm_autostart`** | no | See **`defaults/main.yml`** (**ISO** install uses **`libvirt_vm_memory`** / **`libvirt_vm_vcpus`**) |
| **`libvirt_vm_serial_console_enabled`** | no | **Import** only: if **`true`** (default), **`virt_install`** adds **`--serial pty`**. |
| **`libvirt_vm_networks`** | yes | List passed as-is to **`virt_install`** **`networks`** (see **`community.libvirt.virt_install`** and commented examples in **`defaults/main.yml`**). Use **`mac: { address: "52:54:…" }`** for a fixed MAC (e.g. to match **`libvirt_network_dhcp_hosts`**). |
| **`libvirt_vm_cloud_init_enabled`**, **`libvirt_vm_cloud_init_user_data`**, **`libvirt_vm_cloud_init_disable`** | no | OPNsense and similar guests usually set **`libvirt_vm_cloud_init_enabled: false`**. |

Paths **`libvirt_vm_image_cache_dir`** and **`libvirt_vm_images_dir`** align with **`libvirt_install`** when both roles target the same host.

## Example

```yaml
- ansible.builtin.include_role:
    name: libvirt_vm
  vars:
    libvirt_vm_name: my-guest
    libvirt_vm_os_name: centos-stream10
    libvirt_vm_disk_type: qcow2
    libvirt_vm_download_url: https://example.com/image.qcow2
    libvirt_vm_networks:
      - network: my-lan
      # mac:
      #   address: "52:54:00:12:34:56"
```

## Plugins

The collection ships **`libvirt_vm_disk_option_string`** (see **`plugins/filter/libvirt_vm.py`**) for custom **`virt-install`** scripting if needed.
