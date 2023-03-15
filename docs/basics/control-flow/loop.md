# Looping 
Ansible allows you to loop over a list of items. You can reference the current item using the `item` variable.
Looping can get very complex, so we will only cover the basics here.
If you are interested in more advanced looping, check out reference at the end of this page.

## Loop over a list of items
You can add a list directly in the task and loop over it.
```yaml
tasks:
  - name: create users
    user:
      name: "{{ item }}"
    loop:
      - alice
      - bob
      - charlie
```

## Loop over a multivalued list
If your list has keys and values, you can reference them using the `item.key` variable.
```yaml
- name: Add several users
  ansible.builtin.user:
    name: "{{ item.name }}"
    state: present
    groups: "{{ item.groups }}"
  loop:
    - { name: 'testuser1', groups: 'wheel' }
    - { name: 'testuser2', groups: 'root' }
```

## Loop over a variable
You can also loop over a variable that contains a list.
```yaml
- name: Add multiple packages
  ansible.builtin.apt:
    name: "{{ item }}"
    state: present
  loop:
    - "{{ packages }}"
```

## Loop over magic variables
You can also loop over magic variables like `groups` or `hostvars`.
```yaml
- name: Show all the hosts in the inventory
  ansible.builtin.debug:
    msg: "{{ item }}"
  loop: "{{ groups['all'] }}"
```

## Reference
[Ansible docs - Loops](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_loops.html)