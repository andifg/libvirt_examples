# libvirt_install

Installs packages (`qemu-kvm`, `libvirt`, `virt-install`, Cockpit, `bzip2`), starts **`libvirtd`** and **Cockpit**, ensures a **`libvirt_install_username`** account (Cockpit login), and creates **`libvirt_install_images_dir`** and **`libvirt_install_image_cache_dir`**.

NAT networks and guests are not handled here; compose **`libvirt_network`** and **`libvirt_vm`** in playbooks (see **`playbooks/opnsense_setup.yml`**).

Variables: **`libvirt_install_username`** (default `cockpit`), **`libvirt_install_images_dir`**, **`libvirt_install_image_cache_dir`**. **`libvirt_install_username_password`** must be supplied by the playbook run (inventory / **`-e`** / vault); it has no role default.
