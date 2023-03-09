# Template module
Ansible Docs reference: [https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html)

The template module is one of the most powerful modules in Ansible. It allows you to create files from templates. The template module is a wrapper around [Jinja2](https://jinja.palletsprojects.com/en/3.1.x/), a templating language for Python. The template module is used to create configuration files, or any text files that require variables injected into them. Template files have the extension `.j2`.

The template file is also indempotent. 
If the file already exists, it will only be changed if the template file has changed.

Good examples for using templates are:

- Nginx Config file
- Firewall rule set
- Hosts file

## Creating a template
This is an example for a hosts file template. It will be used to create a hosts file on the remote system.

!!! example "Create hosts file with all webservers"
    === "hosts.j2"
        ```jinja
        # localhost
        127.0.0.1 localhost
        {{ ansible_host }} {{ inventory_hostname }}

        # webservers
        {% for host in groups['webservers'] %}
        {{ hostvars[host]['ansible_host'] }} {{ hostvars[host]['inventory_hostname'] }}
        {% endfor %}
        ```

    === "inventory.yml"
        ```yaml
        all:
        hosts:
        children:
            webserver:
            hosts:
                server1:
                server2:
                server3:
        ```

    === "playbook.yml"
        ```yaml
        - name: "DNS setup"
          hosts: webserver
          tasks:
          - name: "Create hosts file "
            template: 
              src: hosts.j2
              dest: /etc/hosts
        ```

    === "hosts file server1"
        ```bash
        # localhost
        127.0.0.1 localhost
        10.0.0.1 server1

        # webservers
        10.0.0.1 server1
        10.0.0.2 server2
        10.0.0.3 server3
        ```

    === "hosts file server2"
        ```bash
        # localhost
        127.0.0.1 localhost
        10.0.0.1 server2

        # webservers
        10.0.0.1 server1
        10.0.0.2 server2
        10.0.0.3 server3
        ```

    === "hosts file server3"
        ```bash
        # localhost
        127.0.0.1 localhost
        10.0.0.1 server3

        # webservers
        10.0.0.1 server1
        10.0.0.2 server2
        10.0.0.3 server3
        ```

## Advanced example
You can expand the template module with a lot of features. This is an example for a nftables firewall template:

!!! example "nftables-firewall.yml" 
    ```jinja
    table ip filter {
    set trusted {
    typeof ip saddr
    elements = {10.22.0.50, 10.22.0.51, 10.22.0.52}
    }

    chain input {
    type filter hook input priority filter; policy drop;
    ct state related,established counter accept
    
    #Trusted (icmp and ssh)
    ip saddr @trusted tcp dport 22 counter accept
    ip saddr @trusted ip protocol icmp counter accept

    #DNS Incomming all (all to dns udp port 53)
    {% if inventory_hostname in groups['dns'] %}
    udp dport 53 counter accept
    {% endif %}
    
    #DNS replication (dns to dns-master tcp 53 )
    {% if inventory_hostname in groups['dns'][0] %}
    {% for h in groups['dns'] %}
    {% if inventory_hostname not in hostvars[h]['inventory_hostname'] %}
    ip saddr {{hostvars[h]['ansible_host']}} tcp dport 53 counter accept
    {% endif %}
    {% endfor %}
    {% endif %}
    
    #DNS Management (localhost)
    iif lo counter accept

    #Web Traffic (all to port webport)
    {% if inventory_hostname in groups['web'] %}
    tcp dport {{ webport }} counter accept
    {% endif %}
    
    #Proxy to Web Traffic (ha to web port 8081)
    {% if inventory_hostname in groups['web'] %}
    {% for h in groups['ha'] %}
    ip saddr {{hostvars[h]['ansible_host']}} tcp dport 8081 counter accept
    {% endfor %}
    {% endif %}

    }
    ```