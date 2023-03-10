# Gathering ansible facts
Gathering facts means, that ansible connects to all hosts of the play and collects information about the host. This information is stored in the variable `ansible_facts`. This variable is a dictionary and contains information about the host, like the operating system, the hostname, the IP address, the CPU architecture, and so on. 

Gathered facts can be used later in the playbook. For example you can make config changes depending on the operating system of the host.

## Showing gathered facts with the `debug` module

```yaml title="gather-facts.yml"
- name: "My play"
  hosts: localhost
  tasks:
    - name: "Show facts"
      debug: 
        var: ansible_facts
```

Running the playbook to gather facts:
```bash
ansible-playbook gather-facts.yml
```

Resulting output:
```bash
PLAY [My play] *****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [localhost]

TASK [Show facts] **************************************************************
ok: [localhost] => {
    "ansible_facts": {
        "all_ipv4_addresses": [
            "10.0.0.1"
        ],
        "all_ipv6_addresses": [
            "fe80::215:251:2131:531"
        ],
        "ansible_local": {},
        "apparmor": {
            "status": "disabled"
        },
        "architecture": "x86_64",
        "bios_date": "NA",
        "bios_vendor": "NA",
        "bios_version": "NA",
        "board_asset_tag": "NA",
        ...
```

It is recommended to disable facts gathering, if you don't need the facts.
This imporoves the performance of the playbook massively.

You can manually disable facts gathering with the `gather_facts` option:
```yaml hl_lines="3"
- name: "My play"
  hosts: localhost
  gather_facts: false
  tasks:
    - name: "Show facts"
      debug: 
        var: ansible_facts
```