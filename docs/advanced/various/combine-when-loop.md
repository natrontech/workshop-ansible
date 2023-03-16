# Combine when and loop
`when` and `loop/with_items` can be combined to create a loop that only runs when a condition is met. This is useful when you want to loop over a list of items, but only run the task if the item meets a condition.

## Loop over a list of items
You can add a list directly in the task and loop over it.
This will loop over all names except for `bob`.
```yaml
tasks:
  - name: create users
    user:
      name: "{{ item }}"
    loop:
      - alice
      - bob
      - charlie
    when: item != 'bob'
```

## Loop over magic variables
You can also loop over magic variables like `groups` or `hostvars`.
This is for example if you want to loop over all hosts in a group except for the current host itself.

Here we loop over all hosts in the `dns` group except for the current host itself.
This will allow dns replicaton traffic(tcp 53) from all dns hosts to the master (first member of the `dns` group) host.

Based on your needs you can use an Ansible playbook or the rules.j2 template.
=== "playbook.yml"

    ```yaml
    - name: iptables | dns server traffic
    iptables:
        chain: INPUT
        source: "{{ hostvars[item]['ansible_host'] }}"
        protocol: tcp
        destination_port: "53"
        jump: ACCEPT
    when: 
        - inventory_hostname == hostvars[groups['dns'][0]]['inventory_hostname']
        - inventory_hostname != hostvars[item]['inventory_hostname']
    loop: "{{ groups.dns }}"
    notify:
        - iptables | save
    ```
=== "rules.j2"

    ```jinja
    {% if inventory_hostname in groups['dns'][0] %}
    {% for h in groups['dns'] %}
    {% if inventory_hostname not in hostvars[h]['inventory_hostname'] %}
    ip saddr {{hostvars[h]['ansible_host']}} tcp dport 53 counter accept
    {% endif %}
    {% endfor %}
    {% endif %}
    ```