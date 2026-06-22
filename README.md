# Ansible Installation and Setup Guide

## Prerequisites

Create **2 or 3 Linux machines** (servers).

* One machine will be the **Ansible Server (Control Node)**.
* The other machines will be the **Managed Nodes**.

---

# Step 1: Create the Ansible User

Run the following commands on **all machines**:

```bash
sudo useradd ansible
sudo passwd ansible
```

Set a password for the `ansible` user when prompted.

---

# Step 2: Grant Sudo Access

Open the sudoers file:

```bash
sudo visudo
```

Add the following line below the root user entry:

```bash
ansible ALL=(ALL) NOPASSWD: ALL
```

Save and exit.

---

# Step 3: Enable Password Authentication

Edit the SSH configuration file:

```bash
sudo vi /etc/ssh/sshd_config
```

make this like this : #PasswordAuthentication no
make this one also: PermitEmptyPasswords yes
now restart : sudo service sshd restart
```

**Repeat Steps 1–3 on all managed nodes.**

---

# Step 4: Install Python and Ansible

On the **Ansible Server** only:

```bash
sudo yum install python3 -y
sudo yum install python3-pip -y
pip3 install ansible --user
```

Verify installation:

```bash
ansible --version
```

---

# Step 5: Create Ansible Configuration Directory

```bash
sudo mkdir /etc/ansible
```

Switch to the ansible user:

```bash
sudo su - ansible
```

---

# Step 6: Generate SSH Key

Create an SSH key pair:

```bash
ssh-keygen
```

Press **Enter** for all prompts.

This creates a trust relationship so Ansible can connect to nodes without asking for a password.

---

# Step 6.1 Important Restart:

sudo service sshd restart

# Step 7: Copy SSH Key to Managed Nodes

Run the following command for each managed node:

```bash
ssh-copy-id ansible@172.31.22.200
```

Replace `172.31.22.200` with the private IP address of your node.

Example:

```bash
ssh-copy-id ansible@172.31.22.200
ssh-copy-id ansible@172.31.22.201
ssh-copy-id ansible@172.31.22.202
```

After this, Ansible can connect to the nodes without requiring a password.

---

# Step 8: Configure the Inventory File

Create and edit the hosts file:

```bash
sudo vi /etc/ansible/hosts
```

Add your managed nodes:

```ini
[demo]
172.31.22.200
172.31.22.201
172.31.22.202
```

Save and exit.

---

# Step 9: Test Connectivity

Verify that all nodes are reachable:

```bash
ansible all -m ping
```

Expected output:

```bash
SUCCESS
```

for all nodes.

---

# Step 10: Create Your First Playbook

Go to your Ansible project directory:

```bash
cd ~/ansible
```

Create a playbook:

```bash
vi first-playbook.yml
```

Add your playbook content and save.

Run the playbook:

```bash
ansible-playbook first-playbook.yml
```

---

# Useful Commands

## Login to a Managed Node

```bash
ssh ansible@172.31.22.209
```

Replace the IP with your node's private IP.

## Exit the Node

```bash
exit
```

---

# Quick Verification Checklist

✅ Ansible user created on all machines

✅ Passwordless sudo configured

✅ SSH service running

✅ Python installed

✅ Ansible installed on control node

✅ SSH keys copied to all nodes

✅ Hosts file configured

✅ `ansible all -m ping` successful

✅ First playbook executed successfully

Author Mudasir Ahmad 

# installation create 2 or 3 machines and name one as ansible server 
1. sudo useradd ansible.
2. sudo passwd ansible
3. sudo visudo   (enter this near the root user and save ctrl+x+y +y)
4. ansible ALL=(ALL) NOPASSWD: ALL
5. sudo vi /etc/ssh/sshd_config
6. # restarting sshd in the default instance launch configuration.
make this like this : #PasswordAuthentication no
make this one also: PermitEmptyPasswords yes
now restart : sudo service sshd restart
# Now do the same thing with oter machines as well 
# After that install
sudo yum install python3 -y
sudo yum -y install python3-pip
pip3 install ansible --user

# create a directory 
sudo mkdir /etc/ansible
sudo su ansible 

ssh-keygen    // create trust relationship and make sure u have the ansible user that time  // will not ask password after using this command 
ssh-copy-id ansible@172.31.22.200     [ip you have to paste the private ip of the nodes    // if u have 4 paste 4 times ] // now if u want to push code it will execute without asking password 
sudo vi /etc/ansible/hosts    //       [demo] 172.1.12.3  write private ip 


# check all nodes working 
ansible all -m ping|
# then go back
cd ..
then go to cd ansible
ls // check files 
# create ansible files playbook yml file 
vi first-playbook.yml
# run it 
ansible-playbook first-playbook.yml

# if you want to login node on your ansible server simple use 

ssh 172.31.22.209  //private ip you have to mention here 

to exit from node simple use exit




