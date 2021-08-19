### Setting up Ansible on AWS RHEL 8 on AWS

#### 1. Open the EC2 Dashboad
#### 2. sudo su
#### 3. yum update -y
#### 4. We need to get the package to install Ansible on our machine as mention in the documentation https://fedoraproject.org/wiki/EPEL
#### python --version
(no python, we need to install python to get Ansible installed)
``` yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

yum install python2

python2 --version

pip2 --version

pip2 install ansible (ignore warning)

ansible --version
```
#### 5. Create an Ansible dir
```cd /etc
mkdir ansible

mkdir roles

touch ansible.cfg hosts
```
#### 6. Add an Admin user for the Ansible Engine

```useradd ansadmin
passwd ansadmin
(enter password and confirm)

#lets give our user sudo priviledges

visudo
#press i (for insert)
add ansadmin to the ##read drop in files...

ansadmin   ALL=(ALL)   NOPASSWD: ALL
(press escape and wq!)
```

#### 7.  vi /etc/ssh/sshd_config

```
#/password and press enter

#change password authentication from no to yes

PasswordAuthentication yes

```

#### 8. sudo su ansadmin

#### ansible --version config file is the default
```
#exit to user
sudo su -

sudo su ansadmin

#create a ansible config for non prod
mkdir
 my_ansible_dev
