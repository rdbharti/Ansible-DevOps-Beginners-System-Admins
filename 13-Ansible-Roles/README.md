# Ansible Roles

Roles let you automatically load related vars, files, tasks, handlers, and other Ansible artifacts based on a known file structure. 

After you group your content in roles, you can easily reuse them and share them with other users.

- We will create a Role named ` setup_apache` for ansible-playbook `install_apache_httpd_ansible.yaml`
- command: `ansible-galaxy init setup-apache`
  - This will create a directory structure
  - setup-apache/
        ├── defaults
        │   └── main.yml
        ├── files
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── README.md
        ├── tasks
        │   └── main.yml
        ├── templates
        ├── tests
        │   ├── inventory
        │   └── test.yml
        └── vars
            └── main.yml
- `defaults` : it contains the default vaules of inputs; inputs which are not provided explicitly in playbook
- `files` : we can store files in this folder is to be copied. Eg index.html
- `handlers` : this contains tasks which are executed when called upon by another task.
- `meta` : for metadata
- `README` : Introduction of the Role
- `Tasks` (required) :  This is the tasks to be run
- `templates` : configuration files according to target hosts os
- `tests` : It tests whether the role is working fine or not
- `vars` : to store variables.