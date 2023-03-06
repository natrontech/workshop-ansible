!!! danger "Note"
    This is a production setup guide for advanced users.
    
    Make sure you are familiar with the basic concepts of ansible.

# Ansible Setup
Ansible is developed in python and runs on Linux, MacOS and Windows(WSL).
Windows native is not supported.

## Control Node
Ansible is an binary that can be installed via pip or apt.
All configration is done via the control node. The control node can be your own local client or a remote management server.

<figure markdown>
  ![Ansible Control Node](/assets/images/ansible_control_node.png){ width="800" }
</figure>