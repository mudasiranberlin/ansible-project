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



