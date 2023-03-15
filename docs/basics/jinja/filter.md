# Jinja2 Filters
Jinja2 filters are functions that can be applied to content in a Jinja2 template. 
Filters can be chained together, just like functions in normal coding languages like python.

## Loop
The `loop` function can be used to loop over a list of items.

```jinja
{% for host in groups['web'] %}
Server Hostname: {{ hostvars[host]['inventory_hostname'] }}
Server IP: {{ hostvars[host]['ansible_host'] }}
{% endfor %}
```

## If else statement
Only add certain content to the template, if a condition is met.
```jinja
zone "mydomain.com" {

{% if inventory_hostname in groups['dns'][0] %}
type master;

{% else %}
type slave;

{% endif %}

};
```

## Reference
Jinja offers a lot of templating functions. You can find more information in their documentation:
[Jinja 2 docs](https://jinja.palletsprojects.com/en/3.1.x/templates/)