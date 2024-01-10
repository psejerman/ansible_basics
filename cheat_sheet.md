### Connection and Information
| Operation          | Directive                    |
|--------------------|------------------------------|
| Connection test    | `ansible all -m ping`        |
| Get system details | `ansible all -m gater_facts` |

### APT
Docs: https://docs.ansible.com/ansible/2.9/modules/apt_module.html#apt-module 

| Operation                        | Directive                                                            |
|----------------------------------|----------------------------------------------------------------------|
| Update APT-Packages              | `ansible all -m apt -a update_cache=true --become --ask-become-pass` |
| Install apache2 package via APT: | `ansible all -m apt -a name=apache2 --become --ask-become-pass`      |
| Safe upgrade of all packages     | `ansible all -m apt -a upgrade=yes --become --ask-become-pass`       |

### Executing Playbooks
`ansible-playbook --ask-become-pass filename.yaml`

