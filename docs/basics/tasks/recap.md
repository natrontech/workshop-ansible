# Result of a playbook run
Every task in a playbook returns a state. Possible states are:

- ok
- changed
- unreachable
- failed
- skipped
- rescued
- ignored


??? example "Example result of a play"
    ```bash
    PLAY RECAP *********************************************************************
    server1     : ok=28   changed=1    unreachable=0    failed=0    skipped=10   rescued=0    ignored=0   
    server2     : ok=26   changed=1    unreachable=0    failed=0    skipped=10   rescued=0    ignored=0   
    server3     : ok=26   changed=1    unreachable=0    failed=0    skipped=8    rescued=0    ignored=0   
    server4     : ok=25   changed=1    unreachable=0    failed=0    skipped=8    rescued=0    ignored=0   
    server5     : ok=25   changed=1    unreachable=0    failed=0    skipped=7    rescued=0    ignored=0   
    ```

Based on the state you will know, if the playbook run was successful or not.

## Modifing the result of a task
You can override the result of a task by using the `changed_when` and `failed_when` keywords.
This can be usefull if you for example want a task to never be marked as changed.

```yaml title="never-change.yml"
- name: Hello World
  shell:
    command: echo "hello"
  changed_when: false
```

```yaml title="change-if.yml"
- name: Hello World
  shell:
    command: echo "hello"
  register: result
  changed_when: result.stdout == "hello"
```