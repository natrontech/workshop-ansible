# Ansible files
Ansible files used for **running** Ansible can generally be split into three categories: configuration, content and inventory files.

## Configuration
The configuration files define, how the Ansible binary behaves.
Here you can configure settings like the location of the inventory file, the location of the SSH key or the location of the Ansible modules.


!!! note "Ansible looks in the following locations for configuration files:"

    - ANSIBLE_CONFIG (environment variable if set)
    - ansible.cfg (**INI** file in the current directory)
    - ~/.ansible.cfg (**INI** file in the home directory)
    - /etc/ansible/ansible.cfg (**INI** file)

Reference for the configuration options:
[https://docs.ansible.com/ansible/latest/reference_appendices/config.html](https://docs.ansible.com/ansible/latest/reference_appendices/config.html)

??? example "Example configuration file"
    ```ini title="ansible.cfg"
    [defaults]
    inventory = ./inventory
    host_key_checking = False
    remote_user = ansible
    private_key_file = ~/.ssh/id_rsa
    roles_path = ./roles
    ```

## Content
Content files are the files that are used by Ansible to configure the remote systems. Content files are **YAML** files.
Content files can be playbooks that contain a list of plays or roles that contain a list of tasks.

??? example "Example content file"
    ```yaml title="playbook.yml"
    - name: myplay1
      hosts: webserver
      tasks:
        - name: task1
          command: echo "Hello World"
    ```

## Inventory
The inventory files are **YAML** or **INI** files.
The inventory files contain remote systems and variables defining the remote systems. .
Variables can be directly defined in the inventory file or in a separate variables file.

        
??? example "Example inventory file"
    === "YAML"
        ```yaml title="inventory.yml"
        all:
        hosts:
            mail.example.com:
        children:
            webservers:
            hosts:
                foo.example.com:
                bar.example.com:
            dbservers:
                hosts:
                    one.example.com:
                    two.example.com:
                    three.example.com:
        ```
    === "INI"
        ```ini title="inventory.ini"
        mail.example.com

        [webservers]
        foo.example.com
        bar.example.com

        [dbservers]
        one.example.com
        two.example.com
        three.example.com
        ```
