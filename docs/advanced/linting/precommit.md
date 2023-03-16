# Pre-commmit hook for ansible lint
You can use a pre-commit hook to automatically lint your ansible files before you commit them. This is a great way to ensure that your ansible files are always in a working state.

## Setup pre-commit hook
Setup your `.pre-commit-config.yaml` file to use the `ansible-lint` hook. You can find more information about the hook [here](https://github.com/ansible-community/ansible-lint).

```yaml title=".pre-commit-config.yaml"
repos:
  - repo: https://github.com/ansible-community/ansible-lint
    rev: v6.13.1
    hooks:
      - id: ansible-lint
```

Resulting output on a successful pre-commit hook:

```bash
user@host:~$ pre-commit   
Ansible-lint..............................................Passed
```

Resulting output on a failed pre-commit hook:

```bash
Ansible-lint.............................................Failed
- hook id: ansible-lint
- exit code: 2

INFO     Set ANSIBLE_LIBRARY=/home/myuser/.cache/ansible-compat/ds2asg/modules:/home/myuser/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/home/myuser/.cache/ansible-compat/ds2asg/collections:/home/myuser/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/home/myuser/.cache/ansible-compat/ds2asg/roles:/home/myuser/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Executing syntax check on playbook ansible/pb-cluster-setup.yml (0.61s)
INFO     Executing syntax check on playbook ansible/pb-setup.yml (0.62s)
WARNING  Listing 1 violation(s) that are fatal
ansible/roles/myrole/tasks/first.yml:42: name[casing]: All names should start with an uppercase letter.

Read documentation for instructions on how to ignore specific rule violations.

              Rule Violation Summary              
 count tag          profile  rule associated tags 
     1 name[casing] moderate idiom  
```