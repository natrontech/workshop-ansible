# Making variables global
Before the playbook starts, Ansible will always render variables directly to the hosts.
With modules like `set_fact` and `register` you can set variables for the hosts you are iterating over.

In some cases you might want to set variables *globally*, so that they are available for all hosts.
This is not possible but there is a workaround for this.

Example use cases are:

- Create a single cluster join token for all hosts to use in a playbook
- Create a git commit if any network device has changed configuration

## Workaround
You can use your localhost as a variable storage and reference the variables from there.
Just run a `set_fact` task on your localhost and set `delegate_facts` set to `true`.
Reference the fact in any play like this `hostvars['localhost']['my_global_var']`. In case the variable is not set we need to specify a default value so the playbook doesn't fail.

Here are some examples for the use cases above:

```yaml title="Generate a token if cluster join is pending"
- name: Stat | does config exist?
  ansible.builtin.stat:
    path: /etc/mycluster.config
  register: _join_config

- name: Set fact | is any cluster join pending?
  ansible.builtin.set_fact:
    cluster_join_pending: true
  delegate_to: localhost
  delegate_facts: true
  when: not _join_config.stat.exists

- name: Command | create join token for all pending joins
  ansible.builtin.command:
    cmd: cluster token create
  delegate_to: "{{ groups.master[0] }}"
  register: token
  run_once: true
  when: hostvars['localhost']['cluster_join_pending'] is true | default(false)
```

```yaml title="Git commit if any config changed"
# Play for network devices
hosts: routers

router_config:
  backup: yes
  backup_options:
  filename: "{{ inventory_hostname }}.cfg"
  dir_path: /mybackup 
register: configbkp

set_fact:
 configchange: true
delegate_to: localhost
delegate_facts: true
when: configbkp.changed

#Next play for localhost
hosts: localhost
gather_facts: true # required to keep fact from play above

shell:
  chdir: /data/ansible/cisco/backup
  cmd: "git add ."
when: configchange | default(false)

shell:
  cmd: git commit -m "Ansible backup"
when: configchange | default(false)
```