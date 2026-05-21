# openshift_configs

Install configuration for OpenShift: renders **`install-config.yaml`** from `templates/install-config.yaml.j2` into **`openshift_configs_workdir`**, using variables in `defaults/main.yml`.

Pair with **`anixel.openshift_install.setup_clis`** for `openshift-install` and `oc` on the bastion.

## Use in a playbook

```yaml
---
- hosts: bastion
  become: true
  roles:
    - role: anixel.openshift_install.openshift_configs
      vars:
        openshift_configs_workdir: /var/lib/openshift-install
        openshift_configs_pull_secret: "{{ lookup('file', '/path/to/pull-secret') }}"
        openshift_configs_ssh_public_key: "{{ lookup('file', '/path/to/id_ed25519.pub') }}"
```

## Variables (install-config)

| Variable | Default | Description |
|----------|---------|-------------|
| `openshift_configs_workdir` | `""` | **Required.** Directory where `install-config.yaml` is written. |
| `openshift_configs_base_domain` | `example.com` | `baseDomain` |
| `openshift_configs_cluster_name` | `test` | `metadata.name` |
| `openshift_configs_compute_*` | see `defaults/main.yml` | First compute pool |
| `openshift_configs_control_plane_*` | see `defaults/main.yml` | Control plane |
| `openshift_configs_cluster_network_cidr` | `10.128.0.0/14` | Cluster pod CIDR |
| `openshift_configs_cluster_network_host_prefix` | `23` | `hostPrefix` |
| `openshift_configs_network_type` | `OVNKubernetes` | CNI |
| `openshift_configs_service_network_cidr` | `172.30.0.0/16` | Service CIDR |
| `openshift_configs_fips` | `false` | FIPS mode |
| `openshift_configs_pull_secret` | `""` | **Required when rendering.** JSON string for `pullSecret`. |
| `openshift_configs_ssh_public_key` | `""` | **Required when rendering.** SSH public key for core user. |

`platform` is fixed to **`none: {}`** in the template (bare-metal / UPI-style). Extend the template if you need another platform block.

## Template

Source: `templates/install-config.yaml.j2` → `{{ openshift_configs_workdir }}/install-config.yaml`.
