# Cisco vManage Backup Playbook

This package contains an Ansible playbook that triggers a Cisco vManage configuration database backup, waits for the backup file to finish writing, retrieves it locally, and removes the remote artifact.

## Contents

- `playbooks/vmanage_backup.yml` — main playbook.
- `inventory/hosts.yml` — sample inventory targeting a `vmanage` group.
- `group_vars/vmanage/main.yml` — shared variables for the group.
- `group_vars/vmanage/vault.yml.example` — template for the vaulted credentials file (do **not** use it unencrypted).
- `backups/` — default local destination for downloaded backups.

## Prerequisites

- Ansible 2.15+.
- SSH reachability to the vManage host with an account permitted to run `request nms configuration-db backup`.
- Python installed on the control node.
- Access to `ansible-vault` to store sensitive values.

## Preparing authentication

1. Copy the credentials template and encrypt it with Ansible Vault:
   ```bash
   cp group_vars/vmanage/vault.yml.example group_vars/vmanage/vault.yml
   ansible-vault encrypt group_vars/vmanage/vault.yml
   ```
2. Edit the vaulted file to set the variable used by the playbook:
   ```yaml
   vault_vmanage_password: "<vManage SSH password>"
   ```
   To edit an encrypted file:
   ```bash
   ansible-vault edit group_vars/vmanage/vault.yml
   ```
3. Optionally adjust `inventory/hosts.yml` to match your environment (hostname, port, SSH options, user).

> **Note**: No plaintext passwords are stored in this repository. Authentication is expected to come from the vaulted file.

## Running the playbook

From the repository root, execute:

```bash
ansible-playbook -i inventory/hosts.yml playbooks/vmanage_backup.yml --ask-vault-pass
```

- The playbook runs the backup command `request nms configuration-db backup path /home/admin/backup` on the target.
- It waits until the backup file is present and its size is stable before fetching it via SCP.
- The file is downloaded to `./backups/` and the remote file is deleted afterward.

If you use a vault password file instead of prompting, add `--vault-password-file <path>` to the command.

## Verifying the playbook

Before targeting production, you can validate the syntax and dry-run against a lab system:

```bash
ansible-playbook -i inventory/hosts.yml playbooks/vmanage_backup.yml --ask-vault-pass --syntax-check
ansible-playbook -i inventory/hosts.yml playbooks/vmanage_backup.yml --ask-vault-pass --check
```

- `--syntax-check` ensures YAML and task structure are valid with your installed Ansible version.
- `--check` exercises the workflow without making changes to the remote host (backup generation still requires a real device).

## Customization

- Change `backup_remote_dir` in `group_vars/vmanage/main.yml` if the remote backup directory differs.
- Override `backup_local_dir` to change the local download path.
- All tasks are compatible with Ansible 2.15+ and rely solely on built-in modules.
