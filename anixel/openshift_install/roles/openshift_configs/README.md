# openshift_configs

Renders OpenShift **`install-config.yaml`**, runs **`openshift-install`** to produce ignition configs and CoreOS image metadata on the bastion. Pair with **`anixel.openshift_install.setup_clis`** in the same play.

## Requirements

- Collection **`community.general`** (`community.general.json_query` for CoreOS release stream parsing).
- **`openshift-install`** on the bastion (from **`anixel.openshift_install.setup_clis`**; path in `vars/main.yml`).
- Inventory: cluster name, base domain, pull secret, SSH public key, and assets workdir.

## Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `openshift_configs_workdir` | `""` | **Required.** Directory for `install-config.yaml`, manifests, and ignition files. |
| `openshift_configs_base_domain` | `""` | **Required.** `baseDomain` in install-config. |
| `openshift_configs_cluster_name` | `""` | **Required.** `metadata.name` in install-config. |
| `openshift_configs_compute_replicas` | `0` | Worker pool `replicas`. |
| `openshift_configs_control_plane_replicas` | `3` | Control plane `replicas`. |
| `openshift_configs_pull_secret` | `""` | **Required.** JSON string for `pullSecret`. |
| `openshift_configs_ssh_public_key` | `""` | **Required.** SSH public key for the `core` user. |

## Description

- **`tasks/setup_configs.yml`** — render install-config, create ignition configs, and resolve the RHCOS image URL for libvirt.
- **`tasks/wait_for_installation.yml`** — wait until the cluster install completes.
- **`tasks/asserts.yml`** — validate required inventory variables (included by the task files above).

Fixed install-config defaults (networking, pool names, FIPS, binary path) live in **`vars/main.yml`**.
