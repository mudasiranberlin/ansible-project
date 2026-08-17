# Ansible Vault

Ansible Vault is used to **encrypt sensitive files** such as passwords, secrets, variables, and other confidential information.

---

# 1. Create a New Encrypted File

Create a new encrypted file using Ansible Vault:

```bash
ansible-vault create vault.yaml
```

Ansible will ask you to enter a password.

The file will then be created in encrypted form.

---

# 2. Edit an Encrypted File

To open and edit the encrypted file:

```bash
ansible-vault edit vault.yaml
```

Ansible will ask for the Vault password before allowing you to edit the file.

---

# 3. Change the Vault Password

To change the password of an encrypted file:

```bash
ansible-vault rekey vault.yaml
```

Ansible will ask for:

1. The current password
2. The new password
3. Confirmation of the new password

---

# 4. Encrypt an Existing Playbook

If you already have a YAML file and want to encrypt it:

```bash
ansible-vault encrypt target.yaml
```

The existing `target.yaml` file will become encrypted.

---

# 5. Decrypt an Encrypted Playbook

To decrypt an encrypted file:

```bash
ansible-vault decrypt target.yml
```

After decryption, the file will become readable as normal YAML.

---

# Ansible Vault Command Summary

| Task                   | Command                             |
| ---------------------- | ----------------------------------- |
| Create encrypted file  | `ansible-vault create vault.yaml`   |
| Edit encrypted file    | `ansible-vault edit vault.yaml`     |
| Change password        | `ansible-vault rekey vault.yaml`    |
| Encrypt existing file  | `ansible-vault encrypt target.yaml` |
| Decrypt encrypted file | `ansible-vault decrypt target.yml`  |

---

# Simple Workflow

```text
Create Vault
     ↓
ansible-vault create vault.yaml
     ↓
Edit Vault
     ↓
ansible-vault edit vault.yaml
     ↓
Change Password if needed
     ↓
ansible-vault rekey vault.yaml
     ↓
Decrypt when required
     ↓
ansible-vault decrypt target.yml
```

## Important Note

The correct command is:

```bash
ansible-vault
```

Make sure to use **`ansible-vault`** consistently, rather than `anisble-valut` or `anible-vault`.

## Author

**Mudasir**
