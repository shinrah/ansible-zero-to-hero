# Introduction to Ansible

## What is Ansible ?

Ansible is an open source IT automation engine that automates 
- provisioning 
- configuration management
- application deployment
- orchestration

and many other IT processes. It is free to use, and the project benefits from the experience and intelligence of its thousands of contributors.

## How Ansible works ?

Ansible is agentless in nature, which means you don't need install any software on the manage nodes.

For automating Linux and Windows, Ansible connects to managed nodes and pushes out small programs—called Ansible modules—to them. These programs are written to be resource models of the desired state of the system. Ansible then executes these modules (over SSH by default), and removes them when finished. These modules are designed to be idempotent when possible, so that they only make changes to a system when necessary.

For automating network devices and other IT appliances where modules cannot be executed, Ansible runs on the control node. Since Ansible is agentless, it can still communicate with devices without requiring an application or service to be installed on the managed node.

Ansiable Software (Open Source Software)
what is the ansible
ansible is open source and configuration management tools, which automate deployment on manage node. it is agentless, it is communicate to client by ssh.

Founder - Michael Dehaan.
GNU - General Public License - it will provide license to open source software.
Ansiable is agentless
it is communicte to the client by ssh.
rpm -qa |grep openssh -

python ruby or bash - to written module. 
to install ansiable
yum  install  https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm -y
yum install ansible -y
ansible --version

insert the worker node ip in below file
cat /etc/ansible/hosts
workernodeIP

create web group in web servers section 
vi /etc/ansible/hosts
web
192.168.0.11
192.168.0.12
ansible web --list-hosts

[costome inventory]
if you have inventory in diff location then give the below path to get access of that inventory
ansible web --list-hosts -i /tmp/ansiable/hosts

[seprate inventory for the user]

go to the user account, copy ansible.cfg inventory on user home account
cp /etc/ansible/ansible.cfg /home/monu/ansible.cfg
set the veriable
export ANSIBLE_CONFIG /home/monu/ansible.cfg
modify the /home/monu/ansible.cfg
set the path - /home/monu/inventory

command modules (only one command you can run from the ansible server to remote system)
ansible web -m command -a date
ansible web -m command -a time
ansible web -m command -a ls
ansible web -m command -a "df -h"
ansible web -m command -a last

password -
ansible web -m command -a date -k

raw module will run only two commands at the same time.

to check modules
ansible-doc -l
okansible-doc -l --> it will show you all the modules with discription. 

copy module to file copy from source to destination.
ansible web -m copy -a 'src=/etc/passwd dest=/tmp'

if you copy same file after modification then destination file will be overright.
if you put backup=yes then it will take copy of the file.

ansible web -m copy -a 'src=/etc/group dest=/tmp backup=yes'

copy file from remote system to pest file in remote system.
ansible web -m copy -a 'src=/etc/passwd remote_src=yes dest=/tmp'

fetch module copy content from remote system to source system
ansible web -m fetch -a 'src=/var/log/yum.log dest=/tmp/log' flat=yes

file module
to create directory on remote node
ansible web -m file -a 'path=/tmp/india state=directory mode=777
to create file on remote node
ansible web -m file -a 'path=/tmp/soft state=touch'x
to remove the file
ansible web -m file -a 'path=/tmp/soft state=absent'
to create a soft link 
ansible web -m file -a 'remote_src=yes src=/etc/passwd dest=/tmp/passwdlink state=link'

shell module - use for exucute the script
ansible web -m shell -a 'sh /tmp/my.sh'
ansible web -m package -a 'name=httpd state=present'
ansible web -m service -a 'name=httpd state=latest'
ansible web -m user -a 'name=rahul comment=system-admin uid=1099 group=sales'
ansible web -m group -a 'name=sales state=groupadd'
ansible web -m lineinfile -a 'path=/etc/sudoers line="rahul ALL=(ALL) ALL"'


to add the repository to the client system 
ansible web -m yum_repository -a 'name=raj-repository description="local-repo" baseurl=ftp://192.168.0.22/pub/localrepo gpgcheck=0 enabled=1'






