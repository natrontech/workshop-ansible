# Variables
Ansible variables can be defined directly in the inventory file. This is useful for defining variables that are common to all hosts in a group. For example, you can define a variable called `ansible_user` to specify the user to use when connecting to the hosts in a group.

If you have a lot of variables you can write them into a separate file and include it in the inventory file. This is useful for keeping the inventory file clean and readable.

!!! danger "Attention"
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

## Defining variables in a separate file
You can define variables in separate files. This is useful for keeping the inventory file clean and readable.
The folders `group_vars` and `host_vars` in your working directory can be used to assign variables per host or group.
=== "Host variable files"
    === "host_vars/server1.yml"
        ```yaml
        my_var_for_server1: "Hello World"
        ```
    === "host_vars/localhost.yml"
        ```yaml
        my_var_for_localhost: "Hello World"
        ```
=== "Group variable files"
    === "group_vars/webserver.yml"
        ```yaml
        my_var_for_all_webservers: "Hello World"
        ```
    === "group_vars/all.yml"
        ```yaml
        my_var_for_all_hosts: "Hello World"
        ```


## Magic Variables
Magic variables are variables that are automatically set by Ansible, they are not set in your inventory file.
These variables are very important because they are used a lot in ansible playbooks and roles.


Some good examples for magic variables are:

Variable               | Content |
------------           | ------------- | 
`hostvars`   | A dictionary/map with all the hosts in inventory and variables assigned to them
`group_names`           | List of groups the current host is part of  |
`groups`                | A dictionary/map with all the groups in inventory and each group has the list of hosts that belong to it | 
`inventory_hostname`       | The inventory name for the ‘current’ host being iterated over in the play   | 

Ansible Reference:
[https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html#magic-variables](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html#magic-variables)