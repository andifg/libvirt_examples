# Ansible Collection — `anixel.openshift_install`

## Playbook Flow: `playbooks/run.yaml`

The main playbook orchestrates the OpenShift cluster bring-up workflow on Libvirt, and can be tailored with inventory and variable overrides.

### High-Level Steps:

1. **cloudflare_dns**
   - Integrates with Cloudflare DNS to create public records for cluster endpoints.
2. **setup_clis**
   - Installs the OpenShift Installer and CLI (`openshift-install`, `oc`) from the official mirrors.
3. **openshift_configs**
   - Renders `install-config.yaml`, ignition configs, and CoreOS image metadata using templates and inventory-provided variables.
4. **openshift_libvirt**
   - Provisions VMs for all cluster nodes (bootstrap, control-plane, workers) using Libvirt.
   - Sets up a CentOS Stream VM acting as a load balancer running HAProxy to front OpenShift API and routes.
   - Applies network/dhcp/firewalld configuration to enable cluster communication.

---

## Accessing your environment

### SSH into the HAProxy Loadbalancer VM

After the playbook finishes, the loadbalancer is provisioned as a CentOS Stream VM (see `openshift_libvirt_haproxy_vm_name` in your inventory), at the first DHCP address on the lab network (`openshift_libvirt_network_dhcp_range_start`, default `192.168.150.2`).

**Default credentials:**  
- User: `cockpit`  
- Password: *value of* `openshift_libvirt_haproxy_vm_password` (check your vault or inventory)

**Example:**
```bash
ssh cockpit@192.168.150.2
```
> Replace the IP with your configured value if it's different.

---

### SSH into the Bootstrap Node

Similarly, the bootstrap node is created as libvirt domain **`bootstrap`** (see `tasks/nodes.yml`). If using default settings, find its IP lease in the Libvirt network (or assign a static mapping in your DHCP config).

If you have SSH keys injected (see your RHCOS or coreos cloud-init setup), access the bootstrap node as follows:

**Example:**
```bash
ssh core@<bootstrap-node-ip>
```
- User: usually `core` for RHCOS/CentOS Stream CoreOS.
- IP: find using `virsh domifaddr <bootstrap-vm-name>` or inspect the DHCP lease.
