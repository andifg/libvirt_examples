# libvirt_network

Defines one libvirt virtual network from **`templates/network.xml.j2`**:

- **`libvirt_network_type: nat`** (default) — `<forward>` plus optional host IPv4 (and DHCP) and IPv6.
- **`libvirt_network_type: isolated`** — L2-only bridge (**no** `<forward>`, **no** host `<ip>`); STP enabled on the bridge.

Variables use the **`libvirt_network_*`** prefix (see `defaults/main.yml`). Invoke the role once per network with different **`libvirt_network_name`** / **`libvirt_network_bridge`**.

Requires `libvirtd` running and collection `community.libvirt`. Run after a play that starts **`libvirtd`** (for example **`libvirt_install`** in **`playbooks/opnsense_setup.yml`**).

Guest **`virsh dumpxml`** only shows `<source network='…' bridge='…'/>` for a virtual NIC. **NAT, forward mode, and the libvirt host address** appear in **`virsh net-dumpxml <network-name>`**, not in the domain XML.

## Static DHCP reservations

When **`libvirt_network_dhcp_enabled`** is true and the network is not **`isolated`**, set **`libvirt_network_dhcp_hosts`** to a list of objects with any of **`mac`**, **`name`**, or **`ip`** (each optional; omit keys or use empty strings to skip an attribute). These become **`<host/>`** elements inside **`<dhcp>`** next to **`<range>`**. Entries with none of the three set are skipped. Match each **`mac`** to a guest NIC (**`libvirt_vm_networks`** with the same **`mac`** string on **`libvirt_vm`**). Optional **`name`** is the DNS hostname libvirt dnsmasq serves for that reservation.
