# Cisco interface example

This example configures a lopback interface on a Cisco IOS device.
It assigns the IP address of the loopback interface based on the index of the host in the group.

```yaml
# Variable defined for all cisco devices
loop_net: 10.0.0.0/24

#task
ios_config:
 lines:
 - "ip address {{ loop_net | ipmath(groups.cisco.index(inventory_hostname))}} 255.255.255.255"
 - "description Ansible - Loopback Interface"
 parents: interface Loopback0
 save_when: changed
```
