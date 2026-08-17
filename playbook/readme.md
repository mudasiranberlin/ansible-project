# Ansible Playbook Examples

This README contains basic Ansible YAML playbook examples covering:

1. First YAML Playbook
2. Installing HTTPD
3. Using Variables
4. Using Loops
5. Using Handlers
6. Using Conditions

---

# 1. My First YAML Playbook

```yaml
--- # My First Ymal
- hosts: demo
  user: ansibble
  become: yes
  connection: ssh
  gather_facts: yes
```

### Explanation

* `hosts: demo` → Runs the playbook on the `demo` group.
* `user: ansibble` → Specifies the remote user.
* `become: yes` → Gives elevated/root privileges.
* `connection: ssh` → Uses SSH to connect to the managed nodes.
* `gather_facts: yes` → Collects information about the managed nodes.

> **Note:** If your actual username is `ansible`, replace `ansibble` with `ansible`.

---

# 2. My Second YAML File

This playbook installs HTTPD on CentOS 7.

```yaml
--- # My Second Yml file 
- hosts: demo
  remote_user: ansible
  become: yes
  connection: ssh

  tasks:
    - name: Install HTTPD on CentOS 7
      yum:
        name: httpd
        state: present
```

### Explanation

* `tasks:` → Contains the actions Ansible needs to perform.
* `yum:` → Uses the YUM package manager.
* `name: httpd` → Specifies the HTTPD package.
* `state: present` → Makes sure HTTPD is installed.

---

# 3. My Third Tasks — Using Variables

Variables allow us to store a value and use it later in the playbook.

```yaml
---
- name: My Third Tasks
  hosts: demo
  remote_user: ansible
  become: yes
  connection: ssh

  vars:
    pkgname: httpd

  tasks:
    - name: Install HTTPD server on CentOS 7
      yum:
        name: "{{ pkgname }}"
        state: present
```

### Explanation

Here we create a variable:

```yaml
vars:
  pkgname: httpd
```

The variable is then used here:

```yaml
name: "{{ pkgname }}"
```

So Ansible reads:

```text
pkgname = httpd
```

and installs:

```text
httpd
```

### Why Use Variables?

Instead of writing `httpd` everywhere, we can use:

```yaml
"{{ pkgname }}"
```

If we later change:

```yaml
pkgname: nginx
```

the playbook will install `nginx` instead.

---

# 4. My Loops Playbook

A loop allows us to run the same task multiple times with different values.

```yaml
---
- name: My Loops Playbook
  hosts: demo
  remote_user: ansible
  become: yes
  connection: ssh

  tasks:
    - name: Add list of users in my nodes
      user:
        name: "{{ item }}"
        state: present
      loop:
        - Bhupinder
        - Sachin
        - einstein
        - vascodgama
```

### How It Works

The loop contains four users:

```text
Bhupinder
Sachin
einstein
vascodgama
```

`{{ item }}` represents the current user.

Ansible goes through the list one by one:

```text
Bhupinder
    ↓
Sachin
    ↓
einstein
    ↓
vascodgama
```

and creates each user on the managed nodes.

---

# 5. My Handler Project

Handlers are used when we want to perform an action after a task makes a change.

```yaml
---
- name: My handler project
  hosts: demo
  remote_user: ansible
  become: yes
  connection: ssh

  tasks:
    - name: Install HTTPD server on CentOS
      yum:
        name: httpd
        state: present
      notify: restart httpd

  handlers:
    - name: restart httpd
      service:
        name: httpd
        state: restarted
```

### How It Works

First, Ansible installs HTTPD:

```yaml
yum:
  name: httpd
  state: present
```

If the task makes a change, it calls:

```yaml
notify: restart httpd
```

This notification triggers the handler:

```yaml
handlers:
  - name: restart httpd
    service:
      name: httpd
      state: restarted
```

### Simple Flow

```text
Install HTTPD
      ↓
Did something change?
      ↓
     Yes
      ↓
Restart HTTPD
```

---

# 6. Condition Playbook

Conditions allow Ansible to run different tasks depending on the operating system.

```yaml
---
- name: Condition Playbook
  hosts: demo
  remote_user: ansible
  become: yes

  tasks:
    - name: Install Apache on Debian
      apt:
        name: apache2
        state: present
        update_cache: yes
      when: ansible_os_family == "Debian"

    - name: Install Apache on RedHat
      yum:
        name: httpd
        state: present
      when: ansible_os_family == "RedHat"
```

### How It Works

Ansible checks the operating system using:

```yaml
ansible_os_family
```

If the operating system is Debian:

```yaml
when: ansible_os_family == "Debian"
```

Ansible installs Apache using `apt`:

```text
apache2
```

If the operating system is RedHat:

```yaml
when: ansible_os_family == "RedHat"
```

Ansible installs HTTPD using `yum`:

```text
httpd
```

### Simple Flow

```text
             Check Operating System
                       |
              ┌────────┴────────┐
              ↓                 ↓
           Debian             RedHat
              ↓                 ↓
             apt               yum
              ↓                 ↓
          apache2             httpd
```

---

# Important Ansible Concepts

| Concept                | Meaning                                      |
| ---------------------- | -------------------------------------------- |
| `hosts`                | Specifies which hosts will run the playbook  |
| `user` / `remote_user` | Specifies the remote SSH user                |
| `become`               | Runs tasks with elevated/root privileges     |
| `connection`           | Specifies the connection method              |
| `gather_facts`         | Collects information about managed nodes     |
| `vars`                 | Defines variables                            |
| `tasks`                | Contains actions Ansible will perform        |
| `yum`                  | Manages packages on RedHat/CentOS            |
| `apt`                  | Manages packages on Debian/Ubuntu            |
| `user`                 | Creates and manages users                    |
| `loop`                 | Repeats a task for multiple values           |
| `notify`               | Calls a handler                              |
| `handlers`             | Performs actions such as restarting services |
| `when`                 | Runs a task only when a condition is true    |

---

# Quick Learning Summary

### Variables

```text
Store a value → Use the value later
```

Example:

```yaml
pkgname: httpd
```

### Loops

```text
One task → Multiple values
```

Example:

```text
Create 4 users
```

### Handlers

```text
Task changes something → Handler runs
```

Example:

```text
Install HTTPD → Restart HTTPD
```

### Conditions

```text
Check condition → Run the correct task
```

Example:

```text
Debian → apt
RedHat → yum
```

---

## Author

**Mudasir**
