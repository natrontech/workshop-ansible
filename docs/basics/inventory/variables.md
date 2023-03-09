# Variables
Ansible variables can be defined directly in the inventory file. This is useful for defining variables that are common to all hosts in a group. For example, you can define a variable called `ansible_user` to specify the user to use when connecting to the hosts in a group.

If you have a lot of variables you can write them into a separate file and include it in the inventory file. This is useful for keeping the inventory file clean and readable.

!!! danger
    Variables will always be rendered directly to the hosts before you run a playbook. This means that ansible itself has no **global** variables that can be modified for all hosts at runtime. In the advanced section you will learn how to make variables usable across different hosts.

## Defining variables in the inventory file
You can define variables *globally*, for a group or for a host. The variable defined closest to the host will always take precedence.
```yaml title="inventory.yml" hl_lines="4 8 12"
all:
  hosts:
    webserver1:
        web_color: "green"
    webserver2:
    webserver3:
  vars:
    web_color: "orange"
  children:
    webserver:
      vars:
        web_color: "blue"
      hosts:
        webserver2:
```