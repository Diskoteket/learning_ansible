## Check if ansible can reach all hosts in inventory
```bash
ansible all --key-file ~/.ssh/ansible -i inventory -m ping
```
If you have created an inventory file:
```shell
ansible all -m ping
```

## Gather facts about machines in inventory
```shell
ansible all -m gather_facts
```
If you want to limit your facts:
```shell
ansible all -m gather_facts --limit 192.168.122.101
```

## Elevated ad-hoc commands
Assuming you have the same password for all servers:
```shell
ansible all -m apt -a update_cache=true --become --ask-become-pass
```

