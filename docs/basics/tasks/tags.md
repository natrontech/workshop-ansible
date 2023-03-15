# Tags
Ansible tags allow you to run only specific tasks in a playbook. This is useful if you want to run only a specific part of a playbook.

## Apply tags to tasks
You can apply one or more tags to tasks in a playbook like this:

```yaml
tasks:
- name: Task with tag1 and tag2
  ansible.builtin.debug:
    msg: "This task is tagged with 'test'"
  tags:
    - tag1
    - tag2
```
You can also apply tags to roles, playbooks and includes.

## Running playbook with tags
By default ansible will run all tasks regardless of the tags. You can specify tags to run on the command line like this:

```bash
ansible-playbook playbook.yml --tag "tag1"
```
When you specify a tag on the command line, ansible will only run tasks that have that tag.
These are other options for specifying tags on the command line:

```bash
--tags all               #run all tasks, ignore tags (default behavior)
--tags [tag1, tag2]      #run only tasks with either the tag tag1 or the tag tag2
--skip-tags [tag3, tag4] #run all tasks except those with either the tag tag3 or the tag tag4
--tags tagged            #run only tasks with at least one tag
--tags untagged          #run only tasks with no tags
```

## Inheritance
Ansible does not inherit tags for the `include_role` and `include_tasks` modules. This means that you have to manually enforce inheritance like this:

```yaml
- name: Apply the db tag to the include and to all tasks in db.yml
  include_tasks:
    file: db.yml
    # adds 'db' tag to tasks within db.yml
    apply:
      tags: db
  # adds 'db' tag to this 'include_tasks' itself
  tags: db
```

or you can create a block to apply the tag to all tasks in the block:

```yaml
- block:
   - name: Include tasks from db.yml
     include_tasks: db.yml
  tags: db
```

## Special tags
`Always` and `Never` are special tags that can be used to run tasks regardless of the tags specified on the command line.

```yaml
tasks:
- name: Print a message
  ansible.builtin.debug:
    msg: "Always runs"
  tags:
  - always

- name: Print a message
  ansible.builtin.debug:
    msg: "Never runs"
  tags:
  - never
```

## Reference
[Tags - Ansible docs](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_tags.html)