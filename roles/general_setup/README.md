Role Name
=========

This role will fetch the inventory hosts and get the versions of the corresponding package list defined in the ```inventory/group_vars```. It will create an text file `all_node_facts.txt` in the ```playbooks``` directory with all the info gathered.

Requirements
------------

Ansible version ```2.13.13```

The key for the Ansible Vault to decrypt ```global_vars/secrets.yaml```

How to run
--------------

`ansible-playbook -i inventory/hosts.ini playbooks/main.yaml --ask-vault-pass`

