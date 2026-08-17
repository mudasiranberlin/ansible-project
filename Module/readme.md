# Ansible Ad-hoc Commands Using Modules

This guide explains how to use **Ansible modules** with ad-hoc commands to install, remove, update packages, manage services, create users, copy files, and check system information.

---

## 1. Install Package Using the `yum` Module

Install the `tree` package on the `demo` group:

```bash
ansible demo -b -m yum -a "pkg=tree state=present"
```

### Explanation

* `demo` → Target group.
* `-b` → Run with elevated privileges.
* `-m yum` → Use the `yum` module.
* `pkg=tree` → Package to install.
* `state=present` → Make sure the package is installed.

---

## 2. Uninstall Package Using the `yum` Module

Remove the `tree` package:

```bash
ansible demo -b -m yum -a "pkg=tree state=absent"
```

Here, `state=absent` means the package should be removed.

---

## 3. Update Package Using the `yum` Module

Update `httpd` to the latest available version:

```bash
ansible demo -b -m yum -a "pkg=httpd state=latest"
```

`state=latest` means Ansible will make sure the package is updated to the latest available version.

---

## 4. Start HTTPD Service

Start the Apache HTTPD service:

```bash
ansible demo -b -m service -a "name=httpd state=started"
```

### Explanation

* `-m service` → Use the `service` module.
* `name=httpd` → The service we want to manage.
* `state=started` → Start the service.

---

## 5. Check HTTPD Service Status

On the managed node, check the status of the HTTPD service:

```bash
status httpd.service
```

---

## 6. Create a New User

Create a user named `mudasir`:

```bash
ansible demo -b -m user -a "name=mudasir"
```

### Check the User

You can check the users on the node with:

```bash
cat /etc/passwd
```

---

## 7. Create a File and Copy It to the Node

First, create a file on the Ansible control machine:

```bash
touch file
```

Then copy the file to `/tmp` on the managed nodes:

```bash
ansible demo -b -m copy -a "src=file dest=/tmp"
```

### Check the File

On the managed node:

```bash
ls /tmp/
```

You should see the copied `file`.

---

## 8. Run the Command on a Specific Host

You can also target a specific host from the `demo` group.

For example:

```bash
ansible demo[0] -b -m copy -a "src=file dest=/tmp"
```

This runs the command on the first host in the `demo` group.

---

## 9. Use the `setup` Module

The `setup` module collects information (facts) about the managed nodes.

Run:

```bash
ansible demo -m setup
```

Ansible will display information about the managed hosts, such as:

* Operating system
* IP addresses
* CPU information
* Memory
* Hostname
* Network information
* Installed system information

---

## 10. Check IPv4 Information Using a Filter

To display IPv4-related information:

```bash
ansible demo -m setup -a "filter=*ipv4*"
```

The `filter` limits the output so you can find the IPv4 information more easily.

---

# Summary

| Task                   | Module / Command |
| ---------------------- | ---------------- |
| Install package        | `yum`            |
| Remove package         | `yum`            |
| Update package         | `yum`            |
| Start service          | `service`        |
| Create user            | `user`           |
| Copy file              | `copy`           |
| Get system information | `setup`          |

### Author

**Mudasir**
