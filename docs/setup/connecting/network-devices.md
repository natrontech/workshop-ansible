# Connecting to network devices
The connection to network devices is vendor specific. Most ansible modules use ssh, but more modern solutions connect using HTTPS to REST APIs.

This is an example for connecting to a Cisco IOS device using ssh:

```ini title="ansible.cfg"
[defaults]
host_key_checking = false # Disable host key checking, only for testing
```

```yaml title="variables per network device"
ansible_host: myserver1 # IP or hostname
ansible_user: ansible-user
ansible_password: mypass # Password authentication
ansible_connection: network_cli
ansible_network_os: cisco.ios.ios
```