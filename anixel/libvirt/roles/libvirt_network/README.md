# libvirt_network

Defines one libvirt virtual network from **`templates/network.xml.j2`**:

- **`libvirt_network_type: nat`** (default) — `<forward>` plus optional host IPv4 (and DHCP) and IPv6.
- **`libvirt_network_type: isolated`** — L2-only bridge (**no** `<forward>`, **no** host `<ip>`); STP enabled on the bridge.

Variables use the **`libvirt_network_*`** prefix (see `defaults/main.yml`). Invoke the role once per network with different **`libvirt_network_name`** / **`libvirt_network_bridge`**.

Requires `libvirtd` running and collection `community.libvirt`. Run after a play that starts **`libvirtd`** (for example **`libvirt_install`** in **`playbooks/opnsense_setup.yml`**).

Guest **`virsh dumpxml`** only shows `<source network='…' bridge='…'/>` for a virtual NIC. **NAT, forward mode, and the libvirt host address** appear in **`virsh net-dumpxml <network-name>`**, not in the domain XML.

## Static DHCP and DNS

When the network is not **`isolated`**, set **`libvirt_network_dhcp_hosts`** to a list of objects with any of **`mac`**, **`ip`**, or **`hostname`**. **`mac`** + **`ip`** render a static lease in **`<dhcp>`**; **`hostname`** + **`ip`** on the same item also render a **`<dns>`** entry (see **`templates/network.xml.j2`**).

Libvirt uses two XML sections (do not put **`<hostname>`** inside **`<dhcp><host>`** — it is ignored):

```xml
<dns>
  <host ip="192.168.150.6">
    <hostname>bootstrap.example.com</hostname>
  </host>
</dns>
<ip address="192.168.150.1" netmask="255.255.255.0">
  <dhcp>
    <host mac="52:54:00:ab:cd:20" ip="192.168.150.6"/>
  </dhcp>
</ip>
```

Match each DHCP **`mac`** to a guest NIC (**`libvirt_vm_networks`** on **`libvirt_vm`**). Query the network DNS server at the libvirt host address (e.g. **`nslookup bootstrap.example.com 192.168.150.1`**).

The role stops an **active** network with **`virsh net-destroy`** before **`virt_net define`** so DNS/DHCP changes take effect.
