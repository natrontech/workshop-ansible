# Ansible Installation
Install Ansible on your local machine in the user space.
It is not recommended to install ansible as root user.

## pip
Make sure your system has python3 and pip installed. To install these packages on Ubuntu you can use the apt package manager:
```bash
sudo apt install python3 python3-pip
```

Install ansible with pip:
```bash
python3 -m pip install --user ansible
```

## PATH
In some linux distros you need to add the ansible binary to your PATH.
So you can run ansible from the command line directly. Run this command in your terminal:
```bash
export PATH="/home/myuser/.local/bin:$PATH"
```


# Ansible Galaxy Content
If you need to have additional features or content installed from Ansible galaxy, use the following commands:
```bash
ansible-galaxy collection install collection.name
```