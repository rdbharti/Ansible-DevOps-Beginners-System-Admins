# Ansible Variable

- Ansible uses variables to manage differences between systems. 
- With Ansible, you can execute tasks and playbooks on multiple different systems with a single command. 
- To represent the variations among those different systems, you can create variables with standard YAML syntax, including lists and dictionaries. 
- You can define these variables in your playbooks, in your inventory, in re-usable files or roles, or at the command line.
-  You can also create variables during a playbook run by registering the return value or values of a task as a new variable.

- Variables can be used by passing
  - from external files
  - from hosts inventory
  - while running playbook
  - using group_vars, hosts_vars and so on.

- Example: We will add users through variable

```yaml

---
- hosts: all
  become: true
  vars:
    user: john # 'user' is a varibale-name
  tasks:
    - name: Create User
      user: 
        name: "{{ user }}" # referening variable: user
        shell: /bin/bash

```