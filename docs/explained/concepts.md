# Concepts

## :fontawesome-solid-list-check: Task
A task is the smallest unit of work in Ansible. It is a single command or action that is executed on a single host. A task can be as simple as a command to copy a file or as complex as a loop that iterates over a list of items.
```yaml
- name: task1
  command: echo "Hello World"
```

## :fontawesome-solid-book: Playbook
A playbook is a YAML file that contains a list of plays. A play is a collection of tasks that are executed on a single or multiple hosts.
```yaml title="playbook.yml"
- name: myplay1
  hosts: webserver
  tasks:
    - name: task1
      command: echo "Hello World"
```

## :fontawesome-solid-puzzle-piece: Module
A module is a self-contained script that implements a specific feature. Modules are the building blocks of Ansible. They can be executed directly on the command line or in a playbook. Modules can be written in any programming language. The most common languages are Python and Powershell. Examples for often used modules are command and template.
```yaml
name: Task using the command module
command: echo "Hello World"

name: Task using the template module
template:
  src: template.j2
  dest: /tmp/template.txt
```

## :fontawesome-solid-user-doctor: Role
A role is a collection of tasks and files that are executed on a single or multiple hosts. Roles are often used to configure a specific service or application. For example a webserver role could install nginx and configure host firewall settings.

## :fontawesome-solid-clipboard-list: Collection
A collection is a distribution format for Ansible content. It can be a role, a module or a plugin. A collection can be published on [Ansible Galaxy](https://galaxy.ansible.com/) and installed with the ansible-galaxy command. Most vendors that provide Ansible content publish their content as a collection.
The most popular collections are documented directly on the [Ansible documentation page](https://docs.ansible.com/ansible/latest/collections/index.html). 
