# Multitask Ansible Playbooks

- How run more than one tasks on a single playbook
- Will Install apache on managed nodes.

- create a yaml file: install_httpd_ansible.yaml
  - YUM module to install httpd package
  - Service Module to start the httpd service
    - Name: Name of Service (httpd)
    - State:
      - reloaded
      - restarted
      - started
      - stopped

```yaml

--- 
- name: This playbook will install httpd Package
  hosts: rhel # since yum doesn't work on ubuntu machines
  become: true
  tasks:
    - name: "Installing APACHE HTTPD"
      yum:
        name: httpd
        state: installed
    
    - name: "Start httpd service, if not already started"
      service:
        name: httpd
        state: started
```

- Uninstall HTTPD
  - First Stop the servive
    - state: stopped
  - Then Uninstall/remove the package
  
```yaml

--- 
- name: This playbook will uninstall httpd Package
  hosts: rhel # since yum doesn't work on ubuntu machines
  become: true
  tasks:
    - name: "Stopping httpd service"
      service:
        name: httpd
        state: stopped

    - name: "UnInstalling APACHE HTTPD"
      yum:
        name: httpd
        state: removed

```

## Installing Apache on Ubuntu System

- Module Name: apt 
- state: present
  - absent
  - build-dep
  - latest
  - present
  - fixed

```yaml

---
- name: Install apache2 on Ubuntu remote server
  hosts: ubuntu
  become: true
  tasks:
    - name: "Install on Ubuntu: Apache2"
      apt:
        name: apache2
        state: present

    - name: "Start service apache2"
      service:
        name: apache2
        state: started

```

- Uninstall apache2 from ubuntu remote server
  - First Stop the service
  - then remove the apache2 package
  
```yaml

---
- name: "Playbook to uninstall apache2 from ubuntu remote server"
  hosts: ubuntu
  become: true
  tasks:
    - name: "Stop apache2 service on ubuntu"
      service:
        name: apache2
        state: stopped

    - name: "Uninstall ubuntu package: Apache2"
      apt:
        name: apache2
        state: absent

```

