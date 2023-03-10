# Linux unix users example

This example adds users to linux hosts.
The password will not be changed, if the user already exists.

=== "playbook.yml"

    ```yaml
    - name: Configure Users
      hosts: linux
      gather_facts: false
      tasks: 

      - name: csv | read users file
        read_csv:
          path: /path/to/users.csv
          delimiter: ';'
        register: csv_users
        delegate_to: localhost

      - name: user | add users from csv 
        user:
          name: "{{ item.username }}"
          uid: "{{ item.uid }}"
          home: "{{ item.home }}"
          password: "{{ item.password | password_hash('sha512') }}"
          update_password: on_create
        with_items: "{{ csv_users.list }}"
        loop_control:
          label: "{{ item.uid }}"
    ```

=== "users.csv"

    ```csv
    username;uid;home;password
    user1;1001;/home/user1;password1
    user2;1002;/home/user2;password2
    user3;1003;/home/user3;password3
    ```
