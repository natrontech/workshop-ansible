# Connecting to linux hosts
To connect to linux hosts, the remote host needs to have ssh enabled and a valid user login (password or publickey).
In the Ansible configuration file, you can define the connection parameters:

```ini title="ansible.cfg"
[defaults]
host_key_checking = false # Disable host key checking, only for testing
```

```yaml title="variables per host"
ansible_host: myserver1 # IP or hostname
ansible_user: ansible-user
ansible_password: mypass # Password authentication
ansible_ssh_private_key_file: /home/ansible/.ssh/id_rsa # publickey authentication
```