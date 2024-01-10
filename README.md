# ansible_basics

This repository contains the basic steps for the integration of a Debian machine setup with the Ansible automation tool.

## Prerequisites:
- Installed Ansible package on the control host
- Installed sudo on the target host
- Installed and configured SSH server on the target host
- SSH access to the target machine using a key file within the context of a user with sudo permissions

## Inventory and Connection test
## Inventory and Connection Test

To declare the target systems to Ansible, a text file is created containing the addresses of the target hosts and named as the inventory. The format of the file requires one address per line. The addresses can be either IP addresses or DNS names.

Example:
```
host1.mydomain.org
host2.mydomain.org
host3.mydomain.org
```
Connection test is performed using the ping module on all hosts listed in the inventory file.
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
