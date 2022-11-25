# ansible Role Helm Rancher Backup

[![Ansible Lint](https://github.com/Frantche/ansible_role_helm_rancher_backup/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/Frantche/ansible_role_helm_rancher_backup/actions/workflows/ansible-lint.yml)

## Description

Installs Rancher Backup helm chart on kubernetes cluster using helm.

[The official rancher documentation for Backup & Restore process](https://docs.ranchermanager.rancher.io/v2.6/pages-for-subheaders/backup-restore-configuration)

## Roles variables

| Name                                       | Description                                                                                                         | Value                     |
|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------|---------------------------|
| helm_rancher_backup_name                   | Release name to manage.                                                                                             | rancher-backup            |
| helm_rancher_backup_chart_repo_url         | Chart repository URL where to locate the requested chart.                                                           | https://charts.rancher.io |
| helm_rancher_backup_chart_version          | Chart version to install. If this is not specified, the latest version is installed.                                | latest                    |
| helm_rancher_backup_chart_ref              | chart_reference on chart repository.                                                                                | rancher-backup            |
| helm_rancher_backup_release_namespace      | Kubernetes namespace where the chart should be installed.                                                           | cattle-resources-system   |
| helm_rancher_backup_create_namespace       |                                                                                                                     | True                      |
| helm_rancher_backup_values                 | Value to pass to chart.                                                                                             | {}                        |
| helm_rancher_backup_values_files           | Value files to pass to chart. Paths will be read from the target host’s filesystem, not the host running ansible.   | []                        |
| helm_rancher_backup_wait                   | When release_state is set to 'present' wait for minimal readiness if 'absent' wait for all ressources to be deleted | True                      |
| helm_rancher_backup_disable_hook           | Helm option to disable hook on install/upgrade/delete.                                                              | "no"                       |
| helm_rancher_backup_force                  | Helm option to force reinstall, ignore on new install.                                                              | "no"                       |
| helm_rancher_backup_atomic                 | If set, the installation process deletes the installation on failure.                                               | "no"                       |
| helm_rancher_backup_skip_crds              | Skip custom resource definitions when installing or upgrading.                                                      | "no"                       |
| helm_rancher_backup_update_repo_cache      | Run helm repo update before the operation. Can be run as part of the package installation or as a separate step     | "no"                       |
| helm_rancher_backup_binary_path            | The path of a helm binary to use.                                                                                   | "/usr/local/bin"          |
| helm_rancher_backup_context                | Helm option to specify which kubeconfig context to use.                                                             | default                   |
| helm_rancher_backup_kubeconfig             | Helm option to specify kubeconfig path to use.                                                                      | ~/.kube/config            |
| helm_rancher_backup_validate_certs         | Whether or not to verify the API server’s SSL certificates.                                                         | "yes"                     |
| rancher_backup_filename                    | Backup file name, if define and not all ready present in cluster, it will restore rancher with this file            | ""                        |
| rancher_backup_access_key                  | S3 storage access key                                                                                               | ""                        |
| rancher_backup_secret_key                  | S3 storage secret key                                                                                               | ""                        |
| rancher_backup_bucket_name                 | S3 storage bucket name                                                                                              | ""                        |
| rancher_backup_region                      | S3 storage region                                                                                                   | ""                        |
| rancher_backup_folder                      | S3 storage folder                                                                                                   | ""                        |
| rancher_backup_endpoint                    | S3 storage endpoint                                                                                                 | ""                        |
| rancher_backup_endpoint_ca                 | S3 storage endpoint certificate authority                                                                           | ""                        |
| rancher_backup_encryption_config_secretbox | Backup encryption.                                                                                                  | ""                        |
| rancher_backup_schedule                    | Backup job schedule                                                                                                 | ""                        |
| rancher_backup_retentioncount              | Backup file count                                                                                                   | ""                        |



## Requirements

create a file name "requirements.yml"
```yaml
---
collections:
    - name: kubernetes.core
      version: 2.3.2
    - name: git+https://github.com/Frantche/ansible_collection_helm_wrapper.git,main
roles:
  - name: frantchenco.ansible_role_helm_rancher_backup
    type: git
    src: https://github.com/Frantche/ansible_role_helm_rancher_backup.git
    version: main
```

Install requirements with the Ansible Galaxy CLI:

```bash
ansible-galaxy install -r ./requirements.yml
```

## Playbook example


```yaml
---
- hosts: master[0]
  serial: 1
  roles:
  - role: frantchenco.ansible_role_helm_rancher_backup
```