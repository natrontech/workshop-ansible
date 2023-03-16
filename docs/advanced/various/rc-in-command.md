# Making the command module indempotent
The ansible `command` and `shell` modules are special in that they are not idempotent. This means that if you run the same command twice, it will run the command twice. This is not always desirable. For example, if you are running a command that creates a file, you may not want to run it again if the file already exists. This is where the `creates` and `removes` options come in. If you are not working with files you will need to make the command idempotent itself or create your own Ansible module.

## Making the command indempotent itself
You can write your command so, that is returns a specific exit code if the final state is already met. This is a good option if no Ansible
module is available and you don't want to write one on your own.

This code will create an OU in Active Directory if it does not exist. If the OU already exists, it will return exit code 1001. Which results in the task showing status `ok` instead of `changed`.
```yaml
- name: Shell | create OU if it does not exist
  win_shell: |
    if (Get-ADOrganizationalUnit -Filter 'distinguishedName -eq "ou={{ my_ou_name }},dc=domain,dc=com"') 
    { Exit 1001 } else { 
    New-ADOrganizationalUnit -Name '{{ my_ou_name }}' -Path 'dc=domain,dc=com'
    }
  register: ou
  changed_when: ou.rc == 0
  failed_when: ou.rc == 1
```

## Creates / removes
Creates will only run a command if a certan file does not exist. If the file exists, the command will not run.
```yaml
- name: Run command if /path/to/database does not exist (with 'args' keyword)
  ansible.builtin.command: /usr/bin/make_database.sh db_user db_name
  args:
    creates: /path/to/database
```

Removes will only run a command if a certan file does exist. If the file doesn't exist, the command will not run.
```yaml
- name: Run command if /path/to/database does exist (with 'args' keyword)
  ansible.builtin.command: rm /path/to/database
  args:
    removes: /path/to/database
```


## Reference
[Ansible docs - command module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html)