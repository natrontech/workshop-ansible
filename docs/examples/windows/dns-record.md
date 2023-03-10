# Windows dns record

This example shows how to add DNS records based on a json customer list.


=== "playbook.yml"

    ```yaml
    
    #Import json
    shell: "cat /path/to/customers.json"
    delegate_to: localhost
    register: data

    # Set fact
    set_fact:
      customers: "{{ data.stdout | from_json }}"

    #DNS
    win_dns_record:
      zone: customers.com 
      type: A
      name: "{{ item.domain_prefix }}"
      value: "{{ item.ip }}"
    with_items: "{{ customers }}"

    ```

=== "customers.json"

    ```json
    [
        {
            "name": "Art Gallery",
            "domain_prefix": "art",
            "ip": "10.0.0.4"
        },
        {
            "name": "Retail Store",
            "domain_prefix": "store",
            "ip": "10.0.0.5"
        }
    ]
    ```