# Magic Variables
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