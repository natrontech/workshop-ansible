
## Show the inventory
Run the following command to show the inventory:
```bash
ansible-inventory --list --yaml
```

!!! note
    Notice how the variables are inherited from the parent groups. The variable `web_color` was defined in the group `webserver` and the host `webserver2` inherits the variable from the this group.
```yaml
all:
  children:
    ungrouped:
      hosts:
        webserver1:
          web_color: green
        webserver3:
          web_color: orange
    webserver:
      hosts:
        webserver2:
          web_color: blue
```
