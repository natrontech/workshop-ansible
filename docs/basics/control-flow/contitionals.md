# Using when in ansible tasks
The `when` allows you to run ansible task only when a condition is met.

## Based on variables
Run a certain task only when a certain variable is set.

```yaml
tasks:
  - name: Configure SELinux to start mysql on any port
    ansible.posix.seboolean:
      name: mysql_connect_any
      state: true
      persistent: true
    when: ansible_selinux.status == "enabled"
```

## Based on magic variables
Run a certain task only when the hosts meets a certain criteria.

```yaml
tasks:
  - name: my task
    ...
    when: 
    
    # Host is in the group 'web'
    - "'web' in group_names"

    # Host is the first member of the group 'db'
    - inventory_hostname == groups['db'][0]

    # Host is the last member of the group 'dc'
    - inventory_hostname == hostvars[groups['dc']|last]['inventory_hostname']
```

## Based on ansible facts
Run a certain task only when a certain fact is set.

```yaml
tasks:
  - name: Shut down Debian systems
    ansible.builtin.command: /sbin/shutdown -t now
    when: ansible_facts['os_family'] == "Debian"
```

## Combine conditions
You can combine conditions using `and` and `or`.

```yaml
tasks:
  - name: my task
    ...
    when: 
    
    # Host is in the group 'web' and in the group 'db'
    - "'web' in group_names"
    - "'db' in group_names"

    # Host is in the group 'web' or debian based
    - "'web' in group_names or ansible_facts['os_family'] == 'Debian'"
```

## Reference
[Ansible docs - conditionals](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_conditionals.html)