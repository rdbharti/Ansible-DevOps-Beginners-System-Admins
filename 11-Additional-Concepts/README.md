# Convert Shell commands into Ansible-Playbook

## Download and Install Tomcat Server

1. Install Java
2. Download tomcat onto /opt
3. Unzip .tar.gz
4. Give executable permission to startup.sh shutdown.sh
5. Start the tomcat service on port 8080

```yaml
 # vim setup_tomcat_ansible.yaml

 ---
- name: "setup tomcat"
  hosts: all
  become: true
  tasks:
# Install Java
    - name: "Install Java on RedHat"
      when: ansible_os_family == 'RedHat'
      yum:
        name: java
        state: installed

    - name: "Install Java Ubuntu"
      when: ansible_os_family == 'Debian'
      apt:
        name: default-jdk
        state: present

# Download tomcat package
    - name: "Download Tomcat"
      get_url: # used for wget; search google: wget command in ansible
        url: https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.84/bin/apache-tomcat-8.5.84.tar.gz 
        dest: /opt

# Untar .tar.gz
    - name: "Extract tomcat"
      unarchive: # search google: unzip command in ansible
        src: /opt/apache-tomcat-8.5.84.tar.gz
        dest: /opt
        remote_src: yes # This tells ansible to perform the task on remote-machine and not on local machine

# Give Execution permission on startup.sh
    - name: "Give Execution Permission to startup.sh"
      file: # Search google: file permissions command in ansible
        path: /opt/apache-tomcat-8.5.84/bin/startup.sh
        mode: ugo+x

# Start the Services
    - name: "Start tomcat services"
      shell: nohup ./startup.sh
      args:
        chdir: /opt/apache-tomcat-8.5.84/bin


```

# Using Tahs in Playbook

- If you have a large playbook, it may be useful to run only specific parts of it instead of running the entire playbook. 
- You can do this with Ansible tags. 
- Using tags to execute or skip selected tasks is a two-step process:
  - Add tags to your tasks, either individually or with tag inheritance from a block, play, role, or import.
  - Select or skip tags when you run your playbook