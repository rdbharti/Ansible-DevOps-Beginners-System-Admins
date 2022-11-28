# Modules

- We will write ansible playbook using yum module.

- Remove git from both the remote hosts
```yaml
---
- name: This playbook will install packages
  hosts: all
  become: true
  tasks:
    - name: install package
      yum: # yum doesnot work with ubuntu server; it will work with RHEL server
        name: git
        state: installed
```

| State | Detailed |
| ------|----------|
<table style="width: 100%">
<tbody>
<tr class="row-even"><td><div class="ansible-option-cell">
<div class="ansibleOptionAnchor" id="parameter-state"></div><p class="ansible-option-title" id="ansible-collections-ansible-builtin-yum-module-parameter-state"><strong>state</strong></p>
<a class="ansibleOptionLink" href="#parameter-state" title="Permalink to this option"></a><p class="ansible-option-type-line"><span class="ansible-option-type">string</span></p>
</div></td>
<td><div class="ansible-option-cell"><p>Whether to install (<code class="docutils literal notranslate"><span class="pre">present</span></code> or <code class="docutils literal notranslate"><span class="pre">installed</span></code>, <code class="docutils literal notranslate"><span class="pre">latest</span></code>), or remove (<code class="docutils literal notranslate"><span class="pre">absent</span></code> or <code class="docutils literal notranslate"><span class="pre">removed</span></code>) a package.</p>
<p><code class="docutils literal notranslate"><span class="pre">present</span></code> and <code class="docutils literal notranslate"><span class="pre">installed</span></code> will simply ensure that a desired package is installed.</p>
<p><code class="docutils literal notranslate"><span class="pre">latest</span></code> will update the specified package if it’s not of the latest available version.</p>
<p><code class="docutils literal notranslate"><span class="pre">absent</span></code> and <code class="docutils literal notranslate"><span class="pre">removed</span></code> will remove the specified package.</p>
<p>Default is <code class="docutils literal notranslate"><span class="pre">None</span></code>, however in effect the default action is <code class="docutils literal notranslate"><span class="pre">present</span></code> unless the <code class="docutils literal notranslate"><span class="pre">autoremove</span></code> option is enabled for this module, then <code class="docutils literal notranslate"><span class="pre">absent</span></code> is inferred.</p>
<p class="ansible-option-line"><span class="ansible-option-choices">Choices:</span></p>
<ul class="simple">
<li><p><code class="ansible-option-choices-entry docutils literal notranslate"><span class="pre">"absent"</span></code></p></li>
<li><p><code class="ansible-option-choices-entry docutils literal notranslate"><span class="pre">"installed"</span></code></p></li>
<li><p><code class="ansible-option-choices-entry docutils literal notranslate"><span class="pre">"latest"</span></code></p></li>
<li><p><code class="ansible-option-choices-entry docutils literal notranslate"><span class="pre">"present"</span></code></p></li>
<li><p><code class="ansible-option-choices-entry docutils literal notranslate"><span class="pre">"removed"</span></code></p></li>
</ul>
</div></td>
</tr>
</tbody>
</table>

# File module: Create/Remove a file/directory

- Module Name: FILE

```yaml
---
- name: This playbook creates a file or directory
  hosts: all
  become: true
  tasks:
    - name: "Create file: "
      file:
        path: /home/ansadmin/demofile.txt
        state: touch # for directory use 'directory'
```

- Create wile without becoming root
```yaml
---
- name: This playbook creates a file or directory
  hosts: all
  # become: true
  tasks:
    - name: "Create file: "
      file:
        path: /home/ansadmin/demofile_no_root.txt
        state: touch # for directory use 'directory'
```
```console
# output

ansible all -a "ls -l /home/ansadmin"

172.31... | CHANGED | rc=0 >>
total 0
-rw-r--r-- 1 root     root     0 Nov 28 06:16 demofile.txt
-rw-rw-r-- 1 ansadmin ansadmin 0 Nov 28 06:21 demofile_no_root.txt

172.31... | CHANGED | rc=0 >>
total 0
-rw-rw-r--. 1 ansadmin ansadmin 0 Nov 28 06:21 demofile_no_root.txt
-rw-r--r--. 1 root     root     0 Nov 28 06:16 demofile.txt

```

- We can see if creating file without becoming root the file will be owned by ansadmin
- and with become: true; the owner of file is root


- Create a directory

```yaml

---
- name: Ansible Playbook to Create a Directory
  hosts: all
  tasks:
    - name: Create Directory
      file:
        path: /home/ansadmin/dir1
        state: directory

```