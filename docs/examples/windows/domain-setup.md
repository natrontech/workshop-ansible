# Windows domain setup

This example shows how to setup a Windows domain controller and join other domain controllers.
The join the rest of the hosts to the domain.

!!! danger "Warning"
    This example is only for demonstration purposes.
    Make sure to change flow according to your needs.

```yaml title="playbook.yml"

#ADDS installation on all DCs
win_feature:
    state: present
    name: AD-Domain-Services
    include_sub_features: yes

#PDC Setup
win_domain:
    dns_domain_name: customers.com
    safe_mode_password: "{{ ansible_password }}"
when: inventory_hostname in groups['dns'][0]
register: pdc

#Reboot when: pdc.changed
ansible.windows.win_reboot:
  reboot_timeout: 60
when: pdc.changed

#Wait for DC to be back (takes aprox. 8 minutes // up to 15 minutes)
name: Wait for domain controller to be ready
win_shell: |
    Get-ADDomain customers.com
register: dc_ready
until: dc_ready is not failed
ignore_errors: yes
retries: 180
delay: 5
when: pdc.changed

#Domain Join other DCs
win_domain_controller:
    state: domain_controller
    dns_domain_name: customers.com
    domain_admin_user: "{{ ansible_user }}@customers.com"
    domain_admin_password: "{{ ansible_password }}"
    safe_mode_password: "{{ ansible_password }}"
when: inventory_hostname is not groups['dns'][0]
register: dc

#Reboot when: dc.changed
ansible.windows.win_reboot:
  reboot_timeout: 60
when: dc.changed

#Domain Join Members
win_domain_membership:
    state: domain
    dns_domain_name: customers.com
    domain_admin_user: "{{ ansible_user }}@customers.com"
    domain_admin_password: "{{ ansible_password }}"
when: "'dc' not in group_names"
register: domainjoin

#Reboot when: domainjoin.changed
ansible.windows.win_reboot:
  reboot_timeout: 60
when: domainjoin.changed

```