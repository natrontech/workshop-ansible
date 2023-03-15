# Import und include
Import and include allow you to reuse files from other roles or playbooks.

- All **import** statements are pre-processed at the time playbooks are parsed.
- All **include** statements are processed as they are encountered during the execution of the playbook.

## Include a task
Include the task mytask.yml from the current folder.
```yaml title="main.yml"
- name: Include a task from this folder
  ansible.builtin.include_tasks: mytask.yml

- name: Include another task from this folder
  ansible.builtin.include_tasks: anothertask.yml
```

## Include a role
Include the role myrole in the current playbook.
```yaml title="playbook.yml"
- name: Include a role
  ansible.builtin.include_role:
    name: myrole
```

## Include a task from a role
Restart the webservice using a task from the role webserver.
```yaml title="playbook.yml"
- name: Role | webserver
  ansible.builtin.include_role:
    name: webserver
    tasks_from: restart-webservice
```

## Reference
[Ansible docs - Importing and Including](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse.html)