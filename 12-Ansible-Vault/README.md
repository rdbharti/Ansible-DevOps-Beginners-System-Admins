# Ansible Vault

## Introduction

- Ansible Vault is a feature of ansible that allows you to keep sensitive data such as passwords or keys in encrypted files, rather than as plaintext in playbooks or roles.
- create: To create ansible vault file in the encrypted format
- view : to see data of encrypted file
- edit : to edit the encrypted file 
- encrypt : to convert unencrypted to encrypted
- decrypt : to decrypt an encrypted file
- --ask-vault-pass : to provide password while running playbook
- --vault-password-file : to pass a vault password through a file

## Create a new file in encrypted format

` ansible-vault create vault-pass.yaml` 

- This will prompt to set a password
- Then a file will open to write in data and after saving the data to the file.
- If we 'cat filename' -> it will show encryppted data
  
```console

cat vault-pass.yaml
$ANSIBLE_VAULT;1.1;AES256
65383335393031343661336665656638656532663738383163313736626536333733623533356162
6230336132643766396433396266333562663130653937380a386362323363653462623739653462
66643031653664353531373433336236333037643363613161323533653363343630323138623861
3534376666366139320a343832633262653161346462663038636633326261633162356238653165
31636231633438346564633036616666346162316132343739343464316632396433
```

## Edit encrypted file

` ansible-vault edit filename`

- Above command will prompt to put password set for the filname
- Then file will open in clear-text to edit

## View : To view the file

`ansible-vault view filename`

- This command will prompt to put in the file-password
- Then display the unencrypted contents of the file

## Decrypt an encrypted file

` ansible-vault decrypt filename `

- This command will prompt to put in the file-password
- Then decrypt the encrypted file
- Now the file can be vied with cat command
```console

[ansadmin@ansible-control-node vault]$ ansible-vault decrypt vault-pass.yaml
Vault password:
Decryption successful

[ansadmin@ansible-control-node vault]$ cat vault-pass.yaml
This is an encrypted file
EDITED


```

## Encrypt: an un-encrypted file

` ansible-vault encrypt filename `

- Then set New Vault Password for the file

# Using Ansible Vault with Git

- Create a private repository (so it requires password for retrieving data)
- clone the repository on ansible-control-node
  - It will promt for username and password
  - we provide username and password in the url itself in git git url
    - https://'username':'Personal-access-tokens'@github.com/rdbharti/private-remo-name
- We will create an Ansible-playbook to test Ansible Vault

```yaml

---
- name: "Ansible playbook to test ansible vault"
  hosts: all
  become: true
  tasks: 
    - name: "clone a git repo" # search google: git module in ansible
      git:
        repo: https://rdbharti:TOKEN@github.com/rdbharti/gihub-basic-exercise.git
        dest: /opt/ansadmin/test-vault

```

- The above ansible-playbook will clone 'github.com/rdbharti/gihub-basic-exercise.git' repo to all hosts 

## Pass git token from encrypted file

1. Create an encrypted file
   1. `ansible-vault create git_tokens.yaml`
   2. set Vault Password for git_tokens.yaml file
   3. enter value
      1. rdbharti_token: ghc_jkehflkjhadxxxxx (Here 'rdbharti_token' is variable and its value is ghc_xxxxxx)
2. Edit ansible_vault.yaml
   1. replace ` https://rdbharti:TOKEN@github.com/rdbharti/gihub-basic-exercise.git ` -> ` https://rdbharti:{{ rdbharti_token }}@github.com/rdbharti/gihub-basic-exercise.git `
   2. Add varibale file
      1. ```yaml
            vars_files:
              - git_tokens.yaml
         ```
3. now execute the play-book with command:
   ` ansible-playbook ansible_vault.yaml --ask-vault-pass `
