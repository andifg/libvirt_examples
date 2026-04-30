# setup_clis

Downloads **`openshift-install`** and the **`oc`** client (linux **x86_64** tarballs) from the public OCP clients mirror for a **caller-supplied release**, then installs both under a configurable directory (default `/usr/local/bin`).

Mirror layout: `https://mirror.openshift.com/pub/openshift-v4/clients/ocp/<version>/` (see [example 4.20.18 directory](https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.20.18/)).

## Requirements

- **Linux** target (the role always fetches **x86_64** archives `openshift-install-linux-<version>.tar.gz` and `openshift-client-linux-<version>.tar.gz`, regardless of host CPU).
- **`become: true`** if `openshift_install_binary_dir` is not writable by the remote user (typical for `/usr/local/bin`).

## Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `openshift_install_version` | `""` | **Required.** Release segment in the mirror path, e.g. `4.20.18`. |
| `openshift_install_binary_dir` | `/usr/local/bin` | Directory for the `openshift-install` executable. |
| `openshift_install_download_root` | `/var/tmp/openshift-install-cache` | Cache root; unpacked to `.../<version>/`. |
| `openshift_install_binary_path` | `{{ openshift_install_binary_dir }}/openshift-install` | Resulting `openshift-install` path. |
| `openshift_install_oc_binary_path` | `{{ openshift_install_binary_dir }}/oc` | Resulting `oc` path. |

Archive names follow the mirror (linux x86_64): `openshift-install-linux-<version>.tar.gz` and `openshift-client-linux-<version>.tar.gz`.

## Example

```yaml
---
- hosts: bastion
  become: true
  roles:
    - role: anixel.openshift_install.setup_clis
      vars:
        openshift_install_version: "4.20.18"
```

Or:

```yaml
ansible.builtin.include_role:
  name: anixel.openshift_install.setup_clis
vars:
  openshift_install_version: "4.20.18"
```
