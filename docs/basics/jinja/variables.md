# Jinja2 variables
Jinja2 is a templating lanugage that Ansible uses to create dynamic templates.
The main usecase for Jinja2 is the ansible `template` module. See the template module section for more information.

Templating can be usefull for generating configuration files, or for creating files with dynamic content. Good examples are webserver configuration files, systemd service files or even static HTML files.

All variables in Ansible can be accesses from within a Jinja2 template.

## Accessing variables
You can access variables from within a Jinja2 template by using the `{{ variable_name }}` syntax.

```jinja
The time on this host is {{ ansible_date_time.date }}
```

You can also access the ansible magic variables like `hostvars` and `groups`:

```jinja
The host {{ inventory_hostname }} is in these groups {{ group_names }}
```

## Reference
[Jinja 2 docs](https://jinja.palletsprojects.com/en/latest/templates/)