# Roles

| Role | FQCN | Purpose |
|------|------|--------|
| `setup_clis` | `anixel.openshift_install.setup_clis` | Download and install `openshift-install` and `oc` (linux x86_64) from mirror.openshift.com |
| `openshift_configs` | `anixel.openshift_install.openshift_configs` | `install-config.yaml` from `templates/install-config.yaml.j2` when `openshift_configs_workdir` is set |
