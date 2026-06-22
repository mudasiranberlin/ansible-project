# Ansible Ad-hoc Commands Guide

#  1. Create a File on All Hosts

Use the following command to create a file named `file1` on all managed nodes:

ansible all -a "touch file1"
----

# 2. Install Tree Package on All Hosts

Install the `tree` package on all hosts using:


```bash

ansible all -a "yum install tree -y"

```

# 3. Install HTTPD (Apache Web Server)

To install Apache HTTP server on all hosts:

```bash

ansible all -a "yum install httpd -y"
```

# 4. Remove Tree Package from All Hosts


To uninstall the `tree` package:

```bash

ansible all -a "yum remove tree -y"

```


# 5. Install Package on a Specific Group

To install `tree` only on hosts in the `demo` group:

```bash

ansible demo -a "yum install tree -y"

```

== Notes

* These are ad-hoc Ansible commands (no playbooks used).
* Replace `all` with specific groups or hosts as needed.
* Ensure proper privileges (you may need `become: yes` or `-b` flag in real environments).

Author Mudasir 
