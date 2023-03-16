# Delegating tasks
By default all ansible tasks are run on the host specified in the inventory file. Sometimes you may want to run a task on a different host than the one you are currently iterating over. This is called *delegating* a task.

## Delegate to a specific host
This task reads a file on localhost and copies its contents to a remote host.

```yaml title="playbook-with-delegation.yml"
- name: myplay
  hosts: webserver1
  tasks:
  - name: read a file from localhost
    command:
      cmd: cat /etc/hosts
    delegate_to: localhost
    register: _file_localhost

  - name: create a file on the webserver
    shell:
      cmd: "echo '{{ _file_localhost.stdout }}' > /tmp/file.txt"
```

## Delegate facts
As you can see in the example above, all gathered facts are available on the current host, that we are iterating over.
If we want to attach the facts to the delegated host, we can use the `delegate_facts` option.

```yaml title="playbook-with-delegated-facts.yml" hl_lines="8"
- name: myplay
  hosts: webserver1
  tasks:
  - name: read a file from localhost
    command:
      cmd: cat /etc/hosts
    delegate_to: localhost
    delegate_facts: true
    register: _file_localhost

  - name: create a file on the webserver
    shell:
      cmd: "echo '{{ hostvars['localhost']['_file_localhost']['stdout'] }}' > /tmp/file.txt"
```

## Reference
[Ansible docs - Delegate tasks](https://docs.ansible.com/ansible/latest/user_guide/playbooks_delegation.html)