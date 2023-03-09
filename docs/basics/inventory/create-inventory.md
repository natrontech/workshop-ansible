# Creating an ansible inventory

## Creating the inventory
This is an example inventory file. It contains 3 hosts and 1 group. The group `webserver` contains only server2.
```yaml title="inventory.yml" hl_lines="2 4 8"
all:
  vars:
    my_variable: "Hello World"
  hosts:
    server1:
    server2:
    server3:
  children:
    webserver:
      hosts:
        server2:
```

Ansible always has a global group called `all` and there are 3 sub *groups*

- `hosts` a list of hosts
- `vars` a list of variables
- `children` a list of child groups with their members.

## Configure ansible to use the inventory
A file called `ansible.cfg` in your working directory is required to reference the inventory file. The file should look like this:
```ini title="ansible.cfg"
[defaults]
inventory = ./inventory.yml
```
