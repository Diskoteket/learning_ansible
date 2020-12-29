## Check if ansible can reach all hosts in inventory
```shell
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

