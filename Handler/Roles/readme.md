# Ansible Roles

Ansible **Roles** help organize a large Ansible project into separate folders and files.

Instead of putting everything inside one large playbook, we can create a role such as `webserver` and keep its tasks inside the role.

---

# 1. Install Tree

First, install the `tree` package:

```bash
sudo yum install tree
```

`tree` helps us view the Ansible project folder structure easily.

---

# 2. Create the Role Directory

Create the required directories:

```bash
mkdir -p playbook/roles/webserver/tasks
```

This creates the following structure:

```text
playbook/
└── roles/
    └── webserver/
        └── tasks/
```

---

# 3. Go Inside the Playbook Directory

```bash
cd playbook/
```

---

# 4. Create the Role Task File

Create `main.yaml`:

```bash
touch roles/webserver/tasks/main.yaml
```

This file will contain the tasks for the `webserver` role.

---

# 5. Create the Master Playbook

Create the main playbook:

```bash
touch master.yaml
```

The `master.yaml` file will call the `webserver` role.

---

# 6. Add the Webserver Task

Open the role's `main.yaml` file:

```bash
vi roles/webserver/tasks/main.yaml
```

Add:

```yaml
- name: install apache on Redhat
  yum: pkg=httpd state=latest
```

This task installs or updates the `httpd` package to the latest version.

---

# 7. Create the Master Playbook

Open `master.yaml`:

```bash
vi master.yaml
```

Add:

```yaml
--- #master playbook 
- hosts: demo
  user: ansible
  become: yes
  connection: ssh
  roles:
      - webserver
```

### Explanation

```yaml
roles:
    - webserver
```

This tells Ansible:

> "Use the `webserver` role for the `demo` hosts."

Ansible automatically looks for the role in:

```text
roles/webserver/
```

and then loads:

```text
roles/webserver/tasks/main.yaml
```

---

# 8. Project Structure

After creating the files, your project should look like this:

```text
playbook/
├── master.yaml
└── roles/
    └── webserver/
        └── tasks/
            └── main.yaml
```

You can check the structure using:

```bash
tree
```

---

# 9. Run the Master Playbook

From inside the `playbook` directory, run:

```bash
ansible-playbook master.yaml
```

Ansible will read `master.yaml`, find the `webserver` role, and execute the task inside `main.yaml`.

---

# How Ansible Roles Work

The basic flow is:

```text
master.yaml
     ↓
roles:
  - webserver
     ↓
roles/webserver/
     ↓
tasks/main.yaml
     ↓
Install HTTPD
```

### In Simple Words

**Master Playbook** → Calls the **Role** → Role runs the **Tasks**

This makes large Ansible projects easier to organize and maintain.

---

# Important Role Structure

A typical Ansible role can contain:

```text
webserver/
├── tasks/
│   └── main.yaml
├── handlers/
│   └── main.yaml
├── templates/
├── files/
├── vars/
│   └── main.yaml
├── defaults/
│   └── main.yaml
└── meta/
    └── main.yaml
```

For this example, we only need:

```text
roles/
└── webserver/
    └── tasks/
        └── main.yaml
```

## Author

**Mudasir**
