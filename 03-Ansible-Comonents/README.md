# Ansible Components

# Ansible ad-hoc command

- Ad-hoc commans are for tasks that are repeated rarely.
- Eg. To poweroff all lab machines for Christmas vacation, 
- You execute a quick one liner in Ansible without writing a playbook.
` ansible [pattern] -m [module] -m "[module option]" `

- Modules:
  - Ping
  - Command
  - Stat
  - Yum
  - User
  - Setup

- Ping: ` ansible all -m ping `
- Command: `ansible all -m command -a "uptime" `
- stat: `ansible all -m stat -a "path=/etc/hosts"` 
- yum: Useful to install a package ` ansible all -m yum -a "name=git"`
  - The above command throws error, because to install package on other node we need to run install as root on those nodes.
  - So we use flag -b (become root)
  - ` ansible all -m yum -a "name=git" -b `
- User: To create a user : ` ansible all -m user -a "name=john" -b `
- Setup: Gives entire system-infromation of managed nodes : ` ansible all -m setup `

# Ansible Inventory

- Ansible works against multiple managed modules or "hosts" in your infrastructure at the same time, using a list or group of lists known as **Inventory**
- Inventory file is a collection of hosts(nodes) which are managged by ansible control node.
  - Default inventoru : /etc/ansible/hosts
  - use -i flag: ansible -i \<custom_hosts\>
  - Define in ansible.cfg file in /etc/ansible/ansible.cfg

```console
# Create hosts file in home directory
vi ~/hosts
192.168.1.11
:wq

# Execute command
ansible -i ~/hosts all -m ping
```
- Grouping Servers in hosts file
  - use: [Group-Name]
  - eg: 
  ```console
  [db-server]
  192.168...
  192.168...
  192.168...

  [hr]
  192.168...
  192.168...
  192.168...

  [developer]
  192.168...
  192.168...
  192.168...

  ```