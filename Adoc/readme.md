# hello 

create file using Adoc Command 
ansible all -a "touch file1"

install tree using Adoc 

ansible all -a "sudo yum install tree -y"

install http 

ansible all -a "sudo yum https tree -y"

delete 

ansible all -a "yum remove tree -y"

install for paticular group 

ansible demo -a "yum install tree -y"

# install for the group 
