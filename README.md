# Ansible Setup and SSH Configuration on EC2

This guide explains how to set up **Ansible**, create an `ansible` user, give the user `sudo` permissions, configure SSH, and establish a trust relationship between the Ansible server and an EC2 node.

> **Important:** The commands below are kept exactly as provided. The explanations are added to make each step easier to understand.

---

## 1. Install Python and Ansible

First, install Python 3, Python 3 pip, and Ansible.

```bash
sudo yum install python3 -y

sudo yum -y install python3-pip

pip3 install ansible --user
```

### Explanation

* `python3` — installs Python 3.
* `python3-pip` — installs `pip`, which is used to install Python packages.
* `pip3 install ansible --user` — installs Ansible for the current user without requiring a system-wide installation.

---

## 2. Create the Ansible Directory

Create the standard Ansible configuration directory:

```bash
sudo mkdir /etc/ansible
```

### Explanation

`/etc/ansible` is commonly used to store Ansible configuration files, including the inventory file.

---

## 3. Create the List of Hosts

Open the Ansible hosts/inventory file:

```bash
sudo vi /etc/ansible/hosts
```

### Explanation

The `/etc/ansible/hosts` file contains the servers/nodes that Ansible will manage.

You can add the private IP addresses or host information of your EC2 nodes in this file.

---

## 4. Add the `ansible` User

Create a new user named `ansible`:

```bash
sudo useradd ansible

sudo passwd ansible
```

### Explanation

* `useradd ansible` — creates a user named `ansible`.
* `passwd ansible` — creates a password for the `ansible` user.

---

## 5. Switch to the Ansible User

Now switch from the current user to the `ansible` user:

```bash
su - ansible
```

### Explanation

`su - ansible` logs you into the `ansible` user's environment.

---

## 6. Try Installing Something Without `sudo`

Now try installing a package directly:

```bash
yum install tree
```

You will get an error similar to:

```text
This command has to be run with superuser privileges (under the root user on most systems).
```

### Explanation

The `ansible` user is a normal user at this point.

Normal users cannot install system packages using `yum` unless they have **root privileges** or are allowed to use `sudo`.

---

## 7. Try Using `sudo`

Now try:

```bash
sudo yum install tree
```

When you enter the password, you may see:

```text
ansible is not in the sudoers file.
```

### Explanation

This means the `ansible` user does not currently have permission to use `sudo`.

We need to add the `ansible` user to the sudoers configuration.

---

## 8. Exit from the Ansible User

Exit from the `ansible` user and return to the root user.

```bash
exit
```

### Explanation

We need root access because modifying the sudoers configuration requires administrator privileges.

---

## 9. Edit the Sudoers Configuration

Enter:

```bash
visudo
```

### Explanation

`visudo` safely opens the sudoers configuration file.

Add the following:

```text
## Allow root to run any commands anywhere

root    ALL=(ALL)     ALL

ansible ALL=(ALL) NOPASSWD: ALL
```

### Explanation

This line:

```text
ansible ALL=(ALL) NOPASSWD: ALL
```

allows the `ansible` user to run commands using `sudo` without being asked for a password.

### Save and Exit

In `vi`:

1. Press `Esc`
2. Type `:wq`
3. Press `Enter`

If you are using the editor mentioned in your original instructions, save with:

```text
Control + O
```

Press **Enter**, then:

```text
Control + X
```

### Check `visudo` Again

Run:

```bash
visudo
```

Check that the changes are present.

---

## 10. Switch to the Ansible User Again

Now go back to the `ansible` user:

```bash
su - ansible
```

---

## 11. Install `tree` Using `sudo`

Now try:

```bash
sudo yum install tree
```

This time, the package should install successfully.

### Explanation

The `ansible` user now has permission to use `sudo`.

This means the user can execute commands that require administrator/root privileges.

---

## 12. Configure SSH

Open the SSH server configuration:

```bash
sudo vi /etc/ssh/sshd_config
```

Find the relevant settings and make them like this:

```text
#PasswordAuthentication no

PermitEmptyPasswords yes
```

The configuration should be:

```text
#PasswordAuthentication no

PermitEmptyPasswords yes
```

### Explanation

This file controls the SSH server configuration.

The configuration is being changed so that SSH behavior can support the authentication setup used in this exercise.

> **Note:** In a production environment, SSH authentication should normally be configured securely using SSH keys, and password/empty-password authentication should be carefully reviewed before enabling it.

### Restart SSH

After making the changes, restart the SSH service:

```bash
sudo service sshd restart
```

### Explanation

Restarting `sshd` applies the new SSH configuration.

---

## 13. Switch to the Ansible User

Now switch to the `ansible` user:

```bash
su - ansible
```

---

## 14. Create an SSH Key Pair

Create an SSH key pair:

```bash
ssh-keygen
```

### Explanation

`ssh-keygen` creates an SSH public/private key pair.

The key pair allows the Ansible server to authenticate with another server using SSH keys.

You will normally see prompts for the file location and passphrase.

For this exercise, you can accept the default options by pressing **Enter** when appropriate.

---

## 15. Connect the Ansible Server to the Node

Now create a trust relationship between the Ansible server and the node.

Run this command **on the Ansible server**:

```bash
ssh-copy-id ansible@172.31.21.4
```

> **Important:** The IP address should be the **private IP address of the EC2 instance/node**.

You will be asked for the password of the `ansible` user on the node.

Enter the password.

You should see output similar to:

```text
Number of key(s) added: 1

Now try logging into the machine, with: "ssh 'ansible@172.31.21.4'"
```

### Explanation

`ssh-copy-id` copies the Ansible server's public SSH key to the node.

After this, the Ansible server can connect to the node using the SSH key instead of asking for the password every time.

This creates the SSH **key-based trust relationship** required for Ansible to communicate with managed nodes.

---

## 16. Connect to the Node

Now connect to the node:

```bash
ssh 172.31.21.4
```

### Explanation

This connects from the Ansible server to the node using SSH.

Once connected, you are working inside the node.

Now create files:

```bash
touch file file2
```

### Explanation

The `touch` command creates empty files.

In this example, two files are created:

```text
file
file2
```

---

## 17. If the Command Does Not Work

Sometimes you may need to move to the parent directory:

```bash
cd ..
```

### Explanation

`cd ..` moves you one directory up.

Depending on your current location and what command you are running, you may need to use:

```bash
cd ..
```

Sometimes you do not need it.

---

## 18. Now You Can Install Anything

At this point, the `ansible` user has `sudo` permission, and the Ansible server has SSH access to the node.

You can now install packages and manage the node using commands such as:

```bash
sudo yum install tree
```

You can also use Ansible to automate tasks across your nodes.

---

# Overall Flow

The complete process can be remembered like this:

```text
Install Python
      ↓
Install Ansible
      ↓
Create /etc/ansible
      ↓
Create Ansible inventory
      ↓
Create ansible user
      ↓
Give ansible sudo permission
      ↓
Configure SSH
      ↓
Create SSH key
      ↓
Copy SSH key to node
      ↓
SSH into node
      ↓
Ansible can manage the node
```

# Key Concepts to Remember

| Concept              | Purpose                                                         |
| -------------------- | --------------------------------------------------------------- |
| `/etc/ansible/hosts` | Contains the Ansible managed hosts/nodes                        |
| `ansible` user       | User used for Ansible administration                            |
| `sudo`               | Allows a user to execute commands with elevated privileges      |
| `visudo`             | Safely edits the sudoers configuration                          |
| `ssh-keygen`         | Creates an SSH key pair                                         |
| `ssh-copy-id`        | Copies the public key to the remote node                        |
| `sshd_config`        | SSH server configuration                                        |
| `ssh`                | Connects to the remote node                                     |
| Private IP           | Used for communication between EC2 instances inside the network |

## Final Result

After completing these steps:

* Ansible is installed.
* The `ansible` user exists.
* The `ansible` user can use `sudo`.
* Ansible has an inventory file.
* An SSH key pair has been created.
* The public key has been copied to the node.
* The Ansible server can SSH into the node.
* The node can be managed using Ansible.
========================================================================================
Paster This commands first 

sudo yum install python3 -y
sudo yum -y install python3-pip
pip3 install ansible --user


# create a directory 

sudo mkdir /etc/ansible

List of hosts

sudo vi /etc/ansible/hosts

Add user
sudo useradd ansible
sudo passwd ansible


Go to ansible user
su - ansible

Not try to install something like yum install tree
Error: This command has to be run with superuser privileges (under the root user on most systems).

Now i will try to use 
sudo yum install tree
When i enter the password it says   : ansible is not in the sudoers file.
Exit from ansible user
Enter to root user
Eneter the command 
Visudo
And write there 
## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
ansible ALL=(ALL) NOPASSWD: ALL

Now save  control + o  and eneter and then control + x

Check visudo // again changes are happen 

Now go to ansible user
su - ansible

Now i will try to use 
sudo yum install tree
Now its installing 

sudo vi /etc/ssh/sshd_config
# restarting sshd in the default instance launch configuration.
           make this like this : 
#PasswordAuthentication no
PermitEmptyPasswords yes

#PasswordAuthentication no
PermitEmptyPasswords yes


now restart : sudo service sshd restart

Now su - ansible

Create an SSH key pair:
ssh-keygen
Now connect node with ssh command his creates a trust relationship so Ansible can connect to nodes without asking for a password.
Ip address you have to enter private ip of ec2 instance

Enetr this command on asnible server
ssh-copy-id ansible@172.31.21.4

Now ask you to enter the password : enter your password 
Ouput : Number of key(s) added: 1
Now try logging into the machine, with: "ssh 'ansible@172.31.21.4'"


Now go into the Node : 
ssh 172.31.21.4
Create file in node touch file file2
If it will not work then use 
Now sometime u need cd ..    (important) and sometime not


Now you can install anything 




======================================================================================

ssh 'ansible@172.31.20.138'   //eneter the node eneter the private ip 

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




