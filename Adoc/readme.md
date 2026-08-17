# Ansible Ad-hoc Commands Guide

## Hello 👋

This guide contains some basic **Ansible Ad-hoc commands** for performing tasks on managed hosts.

## 1. Create a File Using Ad-hoc Command

Create a file named `file1` on all hosts:

```bash
ansible all -a "touch file1"
```

---

## 2. Install Tree Using Ad-hoc Command

Install the `tree` package on all hosts:

```bash
ansible all -a "sudo yum install tree -y"
```

---

## 3. Install HTTPD

Install the HTTPD (Apache web server) package on all hosts:

```bash
ansible all -a "sudo yum install httpd -y"
```

---

## 4. Delete / Remove Tree

Remove the `tree` package from all hosts:

```bash
ansible all -a "sudo yum remove tree -y"
```

---

## 5. Install Tree Without Using `sudo`

Instead of writing `sudo` inside the command, use the Ansible `-b` option (**become**):

```bash
ansible all -ba "yum install tree -y"
```

Here:

* `-b` → Runs the command with elevated privileges.
* `-a` → Specifies the command/action to execute.

---

## 6. Install Tree for a Particular Group

If you have a group named `demo` in your Ansible inventory, install `tree` only on that group:

```bash
ansible demo -a "yum install tree -y"
```

---

## 7. Install a Package for a Group

You can use the group name instead of `all` to run a command only on the hosts belonging to that group.

Example:

```bash
ansible demo -ba "yum install tree -y"
```

This installs `tree` on **only the hosts inside the `demo` group**.

---

## Notes

* **Ad-hoc commands** are used to perform quick tasks without creating a playbook.
* `all` means **all hosts** in the inventory.
* `demo` means **only hosts in the `demo` group**.
* `-a` means **ad-hoc command/action**.
* `-b` means **become**, which allows Ansible to run the command with elevated privileges.

### Author

**Mudasir**
