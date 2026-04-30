# Ansible Collection — `anixel.openshift_install`

Scaffold collection for OpenShift install–related Ansible content (playbooks, roles, plugins).

## Layout

- `galaxy.yml` — collection metadata
- `meta/runtime.yml` — Ansible runtime hints
- `playbooks/` — example or production playbooks (add as needed)
- `roles/` — roles for install prep, day-2, or wrappers around `openshift-install`
  - **`setup_clis`** — install `openshift-install` and `oc` from mirror (`anixel.openshift_install.setup_clis`)

## Install from path (development)

```bash
ansible-galaxy collection install anixel-openshift_install-0.1.0.tar.gz
# or
ansible-galaxy collection install git+https://example.com/your/repo.git#/anixel/openshift_install
```

Reference in playbooks:

```yaml
collections:
  - anixel.openshift_install
```
