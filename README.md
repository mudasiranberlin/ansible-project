# installation create 2 or 3 machines and name one as ansible server 
1. sudo useradd ansible.
2. sudo passwd ansible
3. sudo visudo   (enter this near the root user and save ctrl+x+y +y)
4. ansible ALL=(ALL) NOPASSWD: ALL
5. sudo vi /etc/ssh/sshd_config
6. # restarting sshd in the default instance launch configuration.
make this like this : #PasswordAuthentication no
make this one also: PermitEmptyPasswords yes
