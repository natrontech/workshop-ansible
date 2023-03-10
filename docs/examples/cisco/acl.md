# Cisco ACL example

This example configures an ACL on a Cisco IOS device.
It allows all ip adresses to access the device via SSH, HTTPS and ICMP on GigabitEthernet1.
For proper indempotence the ACL is preconfigured with a unique number for each line.

```yaml
ios_config: 
    lines: 
    - "{{ (item.0 + 1)*10 + 1 }}" permit tcp host {{ item.1 }} any eq 22"
    - "{{ (item.0 + 1)*10 + 2 }}" permit tcp host {{ item.1 }} any eq 443"
    - "{{ (item.0 + 1)*10 + 3 }}" permit icmp host {{ item.1 }} any"
    parents: ip access-list extended aclmgmt
    save_when: changed
with_indexed_items:
- 10.0.0.100
- 10.0.0.101
- 10.0.0.102

ios_config: 
    lines: ip access-group aclmgmt in
    parents: interface GigabitEthernet1
save_when: changed
```
 