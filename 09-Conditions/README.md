# Conditions

- Ansible supports conditional evaluations before executing a specific task on the target hosts. 
- If the set condition is true, Ansible will go ahead and perform the task. 
- If the condition is not true (unmet), Ansible will skip the specified task.
- In a playbook, you may want to execute different tasks, or have different goals, depending on the value of a fact (data about the remote system), a variable, or the result of a previous task. 
- You may want the value of some variables to depend on the value of other variables. 
- Or you may want to create additional groups of hosts based on whether the hosts match other criteria. 
- You can do all of these things with conditionals.

## Conditional Install Apache
- We will create an ansible playbook to install apache on ubuntu and rhel remote servers.

```yaml

# vi install_apache_httpd_ansible.yaml

---
- name: playbook to install apache httpd
  hosts: all
  become: true
  tasks:
    - name: "Install httpd on Redhat"
      yum:
        name: httpd
        state: installed
      when: ansible_os_family == "RedHat" # 'ansible_os_family' got from ansible all -m status
    
    - name: "Start httpd service"
      service:
        name: httpd
        state: started
      when: ansible_os_family == "RedHat"

    - name: "install apache2 on debian"
      apt:
        name: apache2
        state: present
      when: ansible_os_family == "Debian"
    
    - name: "Start apache2 service"
      service:
        name: apache2
        state: started
      when: ansible_os_family == "Debian"

```

## Conditional UnInstall Apache

- We will remove apache from ubuntu and rhel from single playbook

```yaml

# vim uninstall_apache_httpd_ansible.yaml

---
- name: "Playbook to Uninstall apache"
  hosts: all
  become: true
  tasks:
    - name: "Stop apache service on RedHat"
      service: 
        name: httpd
        state: stopped
      when: ansible_os_family == 'RedHat'

    - name: "Uninstall apache from RedHat"
      yum:
        name: httpd
        state: removed
      when: ansible_os_family == 'RedHat'

    - name: "Stop apache2 on Ubuntu"
      service:
        name: apache2
        state: stopped
      when: ansible_os_family == 'Debian'

    - name: "Uninstall apache2 from Ubuntu"
      apt:
        name: apache2
        state: absent
      when: ansible_os_family == 'Debian' 

```

## Adding Copy Task to Apache Playbook

- In this we will copy index.html to /var/www/html

```yaml

# Edit in file: install_apache+httpd_ansible.yaml

- name: "COPY index.html"
      copy:
        src: /opt/ansible/index.html
        dest: /var/www/html/
        mode: 0666

```

## Lists

- How to install multiple packages with single task.
- Add package as a list

```yaml

---
- name: Packgaes from LIST
  hosts: rhel
  become: true
  tasks:
    - name: Install packages
      yum:
        name:
          - git
          - make
          - gcc
          - wget
          - telnet
          - gzip
        state: installed

```