# Debug module
Ansible Docs reference: [https://docs.ansible.com/ansible/latest/collections/ansible/builtin/debug_module.html](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/debug_module.html)


The ansible debug module allows you to print variables to the console. This is useful for debugging.

## Debugging variables
This is an example tasks, that will print the variable `my_var` to the console.
```yaml
- name: "Print variable"
  debug:
    var: my_var
```

## Debugging text
This is an example tasks, that will print the variable `my_var` to the console.
```yaml
- name: "Print variable"
  debug:
    msg: "This is a text ending with a variable {{ my_var }}"
```