# <p style="text-align: center;">Ansible Play Books</p>

- Playbooks are essentially set of instructions(plays) that you send to run on a single target or groups of targets(hosts).

# Create Ansible Playbook

- A playbook is a text file written in yaml format.
- The playbook begins with a line consisting a three dashes (---) as a stsrt of the document marker.
- An item in the yaml list starts with a single dash followed by a single space.
- hosts and tasks are mandatory items in a playbook.
- The playbook primarily use indentation with space characters to indicate the structure of its data. 
- modules are used to perfoem tasks
- comment starts with hash (#)

- Convert the following command to playbook `ansible all -m user -a "name=rdbharti" -b ` 
```yaml
# Create a yaml file vi create_user_ansible.yaml
---
- hosts: all # take all the hosts ip from defaults /etc/ansible/hosts
  become: true # become root
  tasks:
    - user: name=rdbharti

```

- To execute playbook: `ansible-playbook <playbook-name>`
- ` ansibl-playbook create_user_ansible.yaml `