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
  - execute: ` ansible db-server -m ping ` or ` ansible -i ~/hosts hr -m ping `

# Ansible Configuration File: ansible.cfg

- Default ansible-config file: /etc/ansible/ansible.cfg
- Custom ansible-config file:
  - Changes can be made and used in a configuration file which will be searched for in the following order:

  - ANSIBLE_CONFIG (environment variable if set)

  - ansible.cfg (in the current directory)

  - ~/.ansible.cfg (in the home directory)

  - /etc/ansible/ansible.cfg

  - Ansible will process the above list and use the first file found, all others are ignored.
- To Create custom ansible-configuration file, we will create ansible.cfg in current home directory

  - Generating a sample ansible.cfg file
  - You can generate a fully commented-out example ansible.cfg file, for example:
  ```
  $ ansible-config init --disabled > ansible.cfg
  ```

  - You can also have a more complete file that includes existing plugins:

  ```
  $ ansible-config init --disabled -t all > ansible.cfg
  ```

  - You can use these as starting points to create your own ansible.cfg file.

# Ansible Modules

- A module is a rusable, standalone **script** that ansible run on your behalf, either locally or remotely.
- Modules interact with your local machines, an api, or a remote system to perform specific tasks like
  - Creating Users
  - Installing Packagaes
  - Updating Configurations
  - Spinning up instances
  - etc
- Modules are the program that performs the actual work of the tasks of a play.
- To list modules : ` ansible-doc -l `