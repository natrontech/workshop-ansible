# Blocks
With blocks you can group multiple statements into a single compound statement.
Blocks are useful when you want to run a set of tasks only if a certain condition is met.
If you are running a playbook in serial mode (one host at a time), you can also use blocks to run a set of tasks on a single host before moving on to the next host.

## Block of tasks
This task will run on CentOS only.
```yaml
tasks:
    - name: Install, configure, and start Apache
      when: ansible_facts['distribution'] == 'CentOS'
      block:
      - name: Install httpd and memcached
        ansible.builtin.yum:
          name:
          - httpd
          - memcached
          state: present

      - name: Apply the foo config template
        ansible.builtin.template:
          src: templates/src.j2
          dest: /etc/foo.conf

      - name: Start service bar and enable it
        ansible.builtin.service:
          name: bar
          state: started
          enabled: True
```

## Always and rescue
Always and rescue are similar to `try/catch/finally` in other languages like Powershell.
The rescue block will run if any of the tasks in the block fail.
The always block will always run, regardless of whether the block or rescue block ran.

```yaml
- name: Attempt and graceful roll back demo
  block:
    - name: Print a message
      ansible.builtin.debug:
        msg: 'I execute normally'

    - name: Force a failure
      ansible.builtin.command: /bin/nonexistent

  rescue:
    - name: Print when errors
      ansible.builtin.debug:
        msg: 'I caught an error'

    - name: Force a failure in middle of recovery!
      ansible.builtin.command: /bin/nonexistent

  always:
    - name: Always do this
      ansible.builtin.debug:
        msg: "This always executes"
```

## Reference
[Ansible docs - Blocks](https://docs.ansible.com/ansible/latest/user_guide/playbooks_blocks.html)
