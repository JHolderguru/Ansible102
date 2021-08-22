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

cd my_ansible_dev

ansible --version
#the config file will bet he default but we can make one for dev

 touch ansible.cfg

ansible --version
#note that the config file is local

touch hosts
# create the same user on all the nodes and exchange the ssh keys with your hosts


  ```
  #### 9. Steps to make and exchange keys
  ```
  ssh-keygen

  cd ./ssh

#share key with the managed node
  ssh-copy-id 172.31.5.69

  #now ssh into agent

  ssh 172.31.5.69

  # do the same for your other nodes

  #Note that I have also created the ansadmin user on the nodes and permitted root admin and password.
  ```

  #### 9. Establish the nodes

  ```
  vi ansible
  #then add [defaults]
  inventory  =./hosts

  #make sure your ips are pasted in the hosts file

  vi hosts

  ```

  #### 10.
  Grouping your hosts

  ```
  vi hosts
  [databases]
  place ips
  #[servers]
  place ips

  ```
  #### 11. Adhoc cmd
  ##### ansible databases -m ping
  -------------------------------------
  ##### (shell module)
  ##### ansible 172.31.5.69 -m shell -a "uptime"
  ##### (checking uptime for more than 1 group)
  ##### ansible databases:servers -m shell -a "uptime"
  ##### ansible databases:servers -m shell -a "free -m"

---------------------------------------
  #### (different inventory file)
  ##### ansible -i prod_inv -m shell -a "uptime"
  ##### ansible -i prod_inv -m shell -a "free -m"
  -----------------------------------

  #### 12. Adhoc commands syntax :
  #### ansible [-i pro_inv] server_name:server2:databases -m module

  (module based on task ping module has no args but shell has args)
