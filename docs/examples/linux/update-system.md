# Linux update system

This example shows how to update linux using the apt package manager.

```yaml title="update-system.yml"
- name: Update linux hosts
  hosts: linux
  gather_facts: false
  tasks: 

  - name: Apt | update all packages
    ansible.builtin.apt:
      upgrade: true
      update_cache: true

  - name: State | check if reboot is needed
    stat: 
      path: /var/run/reboot-required
    register: check_reboot

  - name: Reboot | reboot host
    ansible.builtin.reboot:
      post_reboot_delay: 30
    when: check_reboot.stat.exists
```
