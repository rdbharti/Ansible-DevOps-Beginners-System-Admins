# Ansible Roles

Roles let you automatically load related vars, files, tasks, handlers, and other Ansible artifacts based on a known file structure. 

After you group your content in roles, you can easily reuse them and share them with other users.

- We will create a Role named ` setup_apache` for ansible-playbook `install_apache_httpd_ansible.yaml`
- command: `ansible-galaxy init setup-apache`
  - This will create a directory structure
  - setup-apache/ \
        ├── defaults \
        │   └── main.yml \
        ├── files \
        ├── handlers \
        │   └── main.yml \
        ├── meta \
        │   └── main.yml \
        ├── README.md \
        ├── tasks \
        │   └── main.yml \
        ├── templates \
        ├── tests \
        │   ├── inventory \
        │   └── test.yml \
        └── vars \
            └── main.yml \

- `defaults` : it contains the default vaules of inputs; inputs which are not provided explicitly in playbook
- `files` : we can store files in this folder is to be copied. Eg index.html
- `handlers` : this contains tasks which are executed when called upon by another task.
- `meta` : for metadata
- `README` : Introduction of the Role
- `Tasks` (required) :  This is the tasks to be run
- `templates` : configuration files according to target hosts os
- `tests` : It tests whether the role is working fine or not
- `vars` : to store variables.

## Adding more tasks to playbook : setup-apache.yaml

1. Adding notify and handlers
   
```yaml

---
- name: playbook to install apache httpd
  hosts: all
  become: true
  tasks:
    - name: "Install httpd on Redhat"
      yum:
        name: httpd
        state: installed
      when: ansible_os_family == "RedHat"
      notify: "Start httpd service"
  
    - name: "install apache2 on debian"
      apt:
        name: apache2
        state: present
      when: ansible_os_family == "Debian"
      notify: "Start apache2 service"

    - name: "COPY index.html"
      copy:
        src: /opt/ansible/index.html
        dest: /var/www/html/
        mode: 0666

        
  handlers:
    - name: "Start httpd service"
      service:
        name: httpd
        state: started
      when: ansible_os_family == "RedHat"

    - name: "Start apache2 service"
      service:
        name: apache2
        state: started
      when: ansible_os_family == "Debian"

```

- Now we want to run apache on `port 8081` , we will edit 
  - In RHEL
    - `/etc/httpd/conf/httpd.conf` 
    - and change : Listen 80 to Listen 8081
  - In Ubuntu/Debian
    - `/etc/apache2/ports.conf` 
    - change : Listen 80 to Listen 8081
- Update: setup-apache.yaml (Search google: update a file in ansible => 'lineinfile module – Manage lines in text files')

```yaml

...
# Change RHEL Listen 80 to Listen 8081        
    - name: Ensure the default Apache port is 8081
      when: ansible_os_family == "RedHat"
      lineinfile:
        path: /etc/httpd/conf/httpd.conf
        regexp: '^Listen '
        insertafter: '^#Listen '
        line: Listen 8081
      notify: "restart apache"

# Change UBUNTU Listen 80 to Listen 8081        
    - name: Ensure the default Apache port is 8081
      when: ansible_os_family == "Debian"
      lineinfile:
        path: /etc/apache2/ports.conf
        regexp: '^Listen '
        line: Listen 8081
      notify: "restart apache2"

handlers:
    - name: "restart apache"
      service:
        name: httpd
        state: restarted
      when: ansible_os_family == "RedHat"

    - name: "restart apache2"
      service:
        name: apache2
        state: restarted
      when: ansible_os_family == "Debian"
...

```

- Adding a variable for port number

```yaml

- name: playbook to install apache httpd
  hosts: all
  become: true
  vars: 
    port: 8082
  tasks:
...

# Change RHEL Listen 80 to Listen {{ port }}        
    - name: Ensure the default Apache port is {{ port }}
      when: ansible_os_family == "RedHat"
      lineinfile:
        path: /etc/httpd/conf/httpd.conf
        regexp: '^Listen '
        insertafter: '^#Listen '
        line: Listen {{ port }}
      notify: "restart apache"

# Change UBUNTU Listen 80 to Listen {{ port }}        
    - name: Ensure the default Apache port is {{ port }}
      when: ansible_os_family == "Debian"
      lineinfile:
        path: /etc/apache2/ports.conf
        regexp: '^Listen '
        line: Listen {{ port }}
      notify: "restart apache2"
  ```


# Convert a playbook into a Role

- Execute command ` ansible-galaxy init setup-apache `
- Move to folder `cd setup-apache/ `
- copy from setup-apache.yaml to Roles folder `setup-apache/`
  - vars: \ port: 8081 \ to vars/main.yml
    - ` port: 8082 `
  - Tasks to tasks/main.yml
  - Copy /opt/ansible/index.html to files/
  - Copy taska under handlers: to handlers/main.yml
  - Update the defaults/main.yml (in-case we miss to add variable value is vars/main.yml)
    - port: 8080

- To execute the role, we need to create a playbook which will run the role

```yaml

---
- name: This Playbook will install apache-httpd
  hosts: all
  become: true
  roles:
    - setup-apache

```