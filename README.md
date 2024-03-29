# ansible_basics

This repository contains the basic steps for the integration of a Debian machine setup with the Ansible automation tool.

## Prerequisites:
- Installed Ansible package on the control host
- Installed sudo on the target host
- Installed and configured SSH server on the target host
- SSH access to the target machine using a key file within the context of a user with sudo permissions

## Inventory and Connection Test

To declare the target systems to Ansible, a text file is created containing the addresses of the target hosts and named as the inventory. The format of the file requires one address per line. The addresses can be either IP addresses or DNS names.

Example:
```
host1.mydomain.org ansible_user=username
host2.mydomain.org ansible_user=username
host3.mydomain.org ansible_user=username
```
Connection test is performed using the ping module on all hosts listed in the inventory file.

Note: The ansible_user variable can be specified as a command-line argument using `ansible -u` or `ansible --user`, or in the `~/.ssh/config` file on the control host.
```bash
ansible all --key-file ~/.ssh/key_file -u username -i inventory -m ping
```
Response on success:
```
host1.mydomain.org | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
host2.mydomain.org | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
host3.mydomain.org | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

To simplify operations, parameters can be stored in a configuration file for Ansible. For this, a text file named ansible.cfg is created, where the parameter assignments are specified. The configuration file is automatically read when Ansible is invoked.
```
[defaults]
inventory = inventory
private_key_file = ~/.ssh/key_filename
```
This allows the previously used ping command to be shortened as follows.
```bash
anbsible all -m ping
```
## Performing Operations
### Privilege Elevation
To make changes to the system, the remote user needs administrative privileges. While for manual inputs, the sudo module is used for this purpose, with Ansible operations, the options --become and --ask-become-pass must be used to achieve privilege elevation. For example, updating package sources via the APT package manager is triggered with the command `ansible all -m apt -a "update_cache=true" --become --ask-become-pass`.

### Execution of Playbooks
Playbooks are instructions for Ansible, stored in text files in YAML format. The following file describes the instruction to install the Apache2 package on all servers in the inventory using the APT package manager.
Playbook-Example:
```yaml
---
- hosts: all
  become: true
  tasks:

  - name: install apache2 package
    apt:
      name: apache2

```
The execution of playbooks is triggered using the `ansible-playbook` tool.
```bash
ansible-playbook --ask-become-pass filename.yaml
```

Package removal in playbooks is controlled by the state. The value "absent" instructs the target host to remove the package.

```yaml
---
- hosts: all
  become: true
  tasks:

  - name: install apache2 package
    apt:
      name: apache2
      state: absent

```
Playbooks support variables, allowing for the replacement of distribution-specific package names during the execution of the playbook. Variable declarations can be made in the inventory file and assigned to the target hosts based on the installed distribution. The usage of variables is evident in the files playbooks/refactored.yml and /inventory.

Example:
```yaml
---
- hosts: all
  become: true # sudo elevation
  tasks:

  - name: install apache and php package
    package: # generic module for package managers
      name:
        # variables defined in the inventory
        - "{{ apache_package }}"
        - "{{ php_package }}"
      state: latest
      update_cache: yes
```

