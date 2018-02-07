## Slow ansible-playbook commands with dynamic inventory

reference: https://github.com/ansible/ansible/issues/22633#issuecomment-293956861

> if your inventory does not return a `_meta` key 
> Ansible will call `--list` once, and then call `--host` for every host in the inventory. This can often cause confusion as to why when `--list` is run, why running via ansible takes longer.


It would need to at least have `'_meta': {'hostvars': {}}`