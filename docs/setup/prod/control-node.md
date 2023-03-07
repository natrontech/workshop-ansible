# Ansible Installation
Install Ansible on your control node in the user space.
It is not recommended to install ansible as root user.

Your control node doesn't need to be a permanent server.
You can also create a docker image and use CI/CD to run ansible playbooks.

## pip
Make sure your system has python3 and pip installed. Use a requirements.txt file for keeping track of the version.
```bash
# requirements.txt
ansible==7.2.0

python3 -m pip install --user -r requirements.txt
```

## PATH
In some linux distros you need to add the ansible binary to your PATH.
```bash
export PATH="/home/myuser/.local/bin:$PATH"
```


# Ansible Galaxy Content
If you need to have additional features installed from Ansible galaxy, use the following approach.
Create a requirements.yml file for keeping track of the version:
```bash
# requirements.yml
collections:
- name: community.vmware
  version: 3.4.0

ansible-galaxy install -r requirements.yml
```