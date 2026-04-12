# libvirt_emtpy_host

Tears down **all** defined libvirt domains on the host (`qemu:///system`):

1. Force power-off, then **undefine** each domain with **`delete_volumes`** so **disk images referenced by the domain XML** are deleted (typically `vm-*.qcow2` under **`libvirt_vm_images_dir`** from **`libvirt_vm`**). **Does not** delete arbitrary directories such as the golden-image **cache** unless a domain still points at those paths.
2. **Lists** networks with **`virt_net` `command: list_nets`**, then for **each** name sets **`state: absent`** (stops if active, then undefines) — including the default **`default`** network if present.

Requires collection **`community.libvirt`**. Used from **`anixel.libvirt_examples.run`** with **`--tags destroy`**.
