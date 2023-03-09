# Running your first ansible tasks

## Creating a file
This task will create a file called `note.txt` in the `/tmp` directory on your localhost.
```yaml title="create-file.yml"
- name: "First Play"
  hosts: localhost
  tasks:
    - name: "First task"
      command: touch /tmp/note.txt
```

The playbook will be run with the following command:
```bash
ansible-playbook create-file.yml
```

This task uses the ```command``` module to run the ```touch``` command on the local host.

## Copying a file to a remote system
This task will copy the file from before to `/tmp/note2.txt`.
```yaml title="copy-file.yml"
- name: "Second Play"
  hosts: localhost
  tasks:
    - name: "Second task"
      copy: 
        src: /tmp/note.txt
        dest: /tmp/note2.txt
```

The playbook will be run with the following command:
```bash
ansible-playbook copy-file.yml
```

This task uses the builtin ```copy``` module.

## Analysing the output
The output of the second playbook will look something like this:
``` hl_lines="7"
PLAY [Second task] **************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************
ok: [localhost]

TASK [Second task] **************************************************************************************************
changed: [localhost]

PLAY RECAP **********************************************************************************************************
localhost                  : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

If we run the second playbook again, the output will look like this:
``` hl_lines="7"
PLAY [Second task] **************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************
ok: [localhost]

TASK [Second task] **************************************************************************************************
ok: [localhost]

PLAY RECAP **********************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

The second run shows status ok, because the copy module is idempotent. 
Because the file was already copied, the module did not change anything.
More details about the playbook recap will follow in the next chapters.