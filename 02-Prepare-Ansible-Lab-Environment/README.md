# PREPARE ANSIBLE LAB ENVIRONMENT

1. Setup EC2 Instance
2. Setup Hostname
3. Create user : ansadmin
4. Add user to sudoers
5. Generate SSH keys
6. Enable password based authentication 
7. Install Ansible

## Setup EC2 Instance
- generate key-pair
- Create and amazon-linux2 ec2-instance
- In Security groupp open ssh(22), http(80), 8080
- attach key-pair
- ssh from host local machine to ec2-instance with ssh pub key

## Setup Hostname
- edit /etc/hostname file

## Create user : ansadmin
- useradd -m ansadmin
- passwd

## Add user to sudoers

- `visudo` 
- `ansadmin ALL=(ALL) NOPASSWD:ALL`

## Generate SSH keys
- Switch User to ansadmin: `su - ansadmin`
- `ssh-keygen`

## Enable password based authentication 
- switch to root: ` sudo su - `
- edit /etc/ssh/sshd_config
- uncomment : ` PasswordAuthentication yes `
- comment out: ` #PasswordAuthentication no `

# Ansible Installation

Ansible is an open-source automation platform. It is very, very simple to set up and yet powerful. Ansible can help you with configuration management, application deployment, task automation.

### Pre-requisites

1. An AWS EC2 instance (on Control node)

### Installation steps:
#### on Amazon EC2 instance

1. Install python and python-pip
   ```sh
   yum install python
   yum install python-pip
   ```
1. Install ansible using pip check for version
    ```sh
    pip install ansible
   ansible --version
   ```
   
1. Create a user called ansadmin (on Control node and Managed host)  
   ```sh
   useradd ansadmin
   passwd ansadmin
   ```
1. Below command grant sudo access to ansadmin user. But we strongly recommended using "visudo" command if you are aware vi or nano editor.  (on Control node and Managed host)
   ```sh
   echo "ansadmin ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
   ```
   
1. Log in as a ansadmin user on master and generate ssh key (on Control node)
   ```sh 
   ssh-keygen
   ```
1. Copy keys onto all ansible managed hosts (on Control node)
   ```sh 
   ssh-copy-id ansadmin@<target-server>
   ```

1. Ansible server used to create images and store on docker registry. Hence install docker, start docker services and add ansadmin to the docker group. 
   ```sh
   yum install docker
   
   # start docker services 
   service docker start
   service docker start 
   
   # add user to docker group 
   usermod -aG docker ansadmin

   ```
1. Create a directory /etc/ansible and create an inventory file called "hosts" add control node and managed hosts IP addresses to it. 
 
### Validation test

   
1. Run ansible command as ansadmin user it should be successful (Master)
   ```sh 
   ansible all -m ping
   ```

# Setup Control Nodes (RHEL and UBUNTU)

1. Setup ec2 instance
2. Setup HostName
3. Create user: ansadmin
4. Add user ansadmin to sudoers
5. Enable password based login

- Copy Ansible-control-node SSH-key to rehel node and ubuntu node.
- ` ssh-copy-id <rhel-node-ip> `
- ` ssh-copy-id <ubuntu-node-ip> `