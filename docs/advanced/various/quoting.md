# Quoting 
Quoting in Ansible can be a bit tricky. This page will give you a quick overview of the different types of quoting and how to use them.

## Referencing variables in a playbook
In playbooks you can use Jinja2 templating language to reference variables.
Playbooks need to be valid YAML, so you need to quote the variables in a way that YAML understands.
Most of the time you can use double quotes `"` and single quotes `'` on playbooks interchangeably.

Some keywords like `when` and `register` expect variables and therefore you don't need to quote there.

=== "Correct example"
    ```yaml
    debug:
      msg: '{{ mymessage }}/foo.cfg'
    
    debug:
      msg: "{{ mymessage }}/foo.cfg"

    # Here we don't need to quote the variable
    when: inventory_hostname in groups['webserver']
    register: myvar
    ```

=== "Incorrect example"
    ```yaml
    debug:
      msg: {{ mymessage }}/foo.cfg # (1)!
    ```

    1. This playbook will fail with an error like 
       ```bash
        The offending line appears to be:
        debug:
            msg: {{ mymessage }}
                ^ here

            We could be wrong, but this one looks like it might be an issue with
            missing quotes. Always quote template expression brackets when they
            start a value. For instance:

                with_items:
                - {{ foo }}

            Should be written as:

                with_items:
                - "{{ foo }}"
       ```

## Multiple types of quotes
If you need to to define quotes itself in a line, you need to use differents quotes arround the string.

The desired command to be run is:
```bash
echo 'Hello <name>'
              ^ Variable in ansible
```

Here we need to add multiple quotes so that ansible knows how to formulate the command.
```yaml
shell:
    cmd: "echo 'Hello {{ name }}'"
```

## Referencing variables in a Jinja2 template
Jinja templates(`.j2`) are are not parsed as YAML and therefore don't require quoting like ansible playbooks.

```jinja title="template.j2"
Hello {{ mymessage }}
```

## When to quote
Sometimes you don't need to quote variables.

Here is an example using the `when` keyword in YAML:
```yaml title="playbook.yml"
# Only run when the current host is the same as the host in the loop
when: inventory_hostname == hostvars[item]['inventory_hostname']
```
This can be confusing, so let's break it down:

| Name      | Description                          |
| ----------- | ------------------------------------ |
| `inventory_hostname`       | :material-variable: A variable referencing the current host  |
| `==`       | :material-select-compare: Equal operation  |
| `hostvars[]`       | :material-function: A function calling hostvariables of a certain host |
| `item`       | :material-variable: A variable referencing the current item in the loop |
| `'inventory_hostname'`    | :material-tag-text-outline: Is a sting referencing the variable name that we want to access from <br> the `hostvars[]` funtion |

This is similar in Jinja2 templates.
Here is an example that is even more tricky:
```jinja title="template.j2"
# Set the last host in the dns group as the master
{% if inventory_hostname in hostvars[groups['dns']|last]['inventory_hostname'] %}
  master = true
{% endif %}
```

Again let's break it down:

| Name      | Description                          |
| :---------------------------- | ------------- |
| `inventory_hostname`       | :material-variable: A variable referencing the current host  |
| `in`       | :material-select-compare: In operation (is this string in the compare value?)  |
| `hostvars[]`       | :material-function: A function calling hostvariables of a certain host |
| `groups[]`       | :material-function: A function calling groupmembers of a certain group |
| `'dns'`       | :material-tag-text-outline: A string referencing the name of the group we want to get |
| `|last`       | :material-filter: A Jinja2 filter extracting the last item in a list |
| `'inventory_hostname'`    | :material-tag-text-outline: Is a sting referencing the variable name that we want to access from <br> the `hostvars[]` funtion |

## Reference
[Ansble docs - quoting](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html#when-to-quote-variables-a-yaml-gotcha)