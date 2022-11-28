# Gather Facts

- Gatrhering Facts retrieves the remote/hosts system information, the ansible executes the tasks.
- To delete or Create a file we don't need to gather system information.
- Gather information is useful when installing, uninstalling etc
- To disable gather facts
  - `gather_facts: no`

```yaml

---
- name: "Playbook"
  hosts: all
  become: true
  gather_facts: no # disable fact gathering
  tasks:

```
