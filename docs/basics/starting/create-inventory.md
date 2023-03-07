# Creating an ansible inventory

## Creating the inventory
Create a file called `inventory.yml` with the following content:
```yaml title="inventory.yml" hl_lines="2 4 6"
all:
  hosts:
    localhost:
  vars:
    my_var_1: "value"
  children:
    webserver:
      hosts:
        localhost:
```

Ansible always has a global group called `all` and there are 3 sub *groups*

- `hosts` a list of hosts
- `vars` a list of variables
- `children` a list of child groups with their members.

## Configure ansible to use the inventory
Create a file called `ansible.cfg` with the following content:
```ini title="ansible.cfg"
[defaults]
inventory = ./inventory.yml
```

## Show the inventory
Run the following command to show the inventory:
```bash
ansible-inventory --graph
```

Now you should see that localhost belongs to the `all` group and the `webserver` group:
```bash
@all:
  |--@ungrouped:
  |--@webserver:
  |  |--localhost
```
