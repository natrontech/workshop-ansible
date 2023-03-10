
## Show the inventory
Run the following command to show the inventory:
```bash
ansible-inventory --list --yaml
```

!!! note
    Notice how the variables are inherited from the parent groups.

=== "output"
    ```yaml
    all:
      children:
        ungrouped:
          hosts:
            webserver1:
              web_color: green #(1)!
            webserver3:
              web_color: orange #(2)!
        webserver:
          hosts:
            webserver2:
              web_color: blue #(3)!
    ```

    1. This variable was defined for the host `webserver1` directly
    2. This variable was defined in the group `all` and inherited by the host `webserver3`
    3. This variable was defined in the group `webserver` and inherited by the host `webserver2`

=== "inventory.yml"
    ```yaml
    all:
      hosts:
        webserver1:
            web_color: "green"
        webserver2:
        webserver3:
      vars:
        web_color: "orange"
      children:
        webserver:
          vars:
            web_color: "blue"
          hosts:
            webserver2:
    ```


