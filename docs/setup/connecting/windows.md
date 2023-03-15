# Connecting to windows hosts
To connect to windows hosts, the remote host needs to have WinRM enabled and a valid user login.
Ansible provides a Powershell script to enable WinRM on a windows host. The script can be found [here](https://github.com/ansible/ansible/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1).

In the Ansible variables, you can define the connection parameters:


```yaml title="variables per host"
ansible_host: myserver1 # IP or hostname
ansible_user: ansible-user
ansible_password: mypass # Password
ansible_connection: winrm # set winrm as connection type
ansible_winrm_server_cert_validation: ignore # ignore certificate validation, only for testing
```
