# Ansible Lint
Ansible lint is a tool that checks playbooks for practices and behaviour that could potentially be improved.

## Installing ansible-lint
Ansible lint is a separate tool and can be installed with pip. It is not included in the Ansible package.
You can use the following command to install ansible-lint:
```bash
pip3 install ansible-lint
```

## Configuring ansible-lint
Ansible lint can be configured with a configuration file. The default location for the configuration file is `~/.ansible-lint`.
Additionally you can configure exceptions in the file `~/.ansible-lint-ignore`.

```bash title="~/.ansible-lint-ignore"
path/myplaybook1.yml no-handler
path/myplaybook2.yml command-instead-of-module
path/myplaybook3.yml yaml[line-length]
```

## Running ansible-lint
You can run ansible-lint on a playbook with the following command:
```bash
ansible-lint myplaybook.yml
```
You can also create a git pre-commit hook to run ansible-lint on your playbooks before you commit them.

## Ansible Lint in VSCode
If you install the [Ansible extension for VSCode](https://marketplace.visualstudio.com/items?itemName=redhat.ansible), you can use the ansible-lint to lint your playbooks.
This only works if you have ansible-lint installed.

The you need to make sure, that your yaml files are recognized as ansible files.
![Ansible VSCode](/assets/images/ansible_vscode.png){ width="800" }

The ansible-lint will show you validation errors in real time:
![Ansible VSCode](/assets/images/ansible_vscode2.png){ width="800" }

## Common ansible-lint errors
| Name      | Description                          |
| :---------------------------- | ------------- |
| `command-instead-of-module`       | the command module is used even though a possible ansible module exists |
| `name`       | the name for a task or role is incorrect (missing, wrong casing, ...)  |
| `no_handler`        | when: result.changed is used instead of a handler |
| `yaml`       | yaml is not valid (line lenght, trailing spaces, indentation, ...) |


## Reference
[Ansible Lint](https://ansible-lint.readthedocs.io)
[Ansible Lint rules](https://ansible-lint.readthedocs.io/rules/)