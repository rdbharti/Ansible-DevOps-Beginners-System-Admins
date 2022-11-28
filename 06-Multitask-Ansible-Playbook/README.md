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