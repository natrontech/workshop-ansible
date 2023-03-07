# Running your first ansible task

## Creating a file
Create a file called `create-file.yml` with the following content:
```yaml title="create-file.yml"
- name: "First task"
  hosts: localhost
  tasks:
    - name: "First task"
      command: touch /tmp/note.txt
```

Run the playbook with the following command:
```bash
ansible-playbook create-file.yml
```

This will create a file called `note.txt` in the `/tmp` directory on your local host.
This task uses the ```command``` module to run the ```touch``` command on the local host.

## Copying a file to a remote system
Create a file called `copy-file.yml` with the following content:
```yaml title="copy-file.yml"
- name: "Second task"
  hosts: localhost
  tasks:
    - name: "Second task"
      copy: 
        src: /tmp/note.txt
        dest: /tmp/note2.txt
```

Run the playbook with the following command:
```bash
ansible-playbook copy-file.yml
```

This will copy the `/tmp/note.txt` file to `/tmp/note2.txt` on the local host.

## Analysing the output
The output of the second playbook run will look something like this:
``` hl_lines="7"
PLAY [Second task] **************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************
ok: [localhost]

TASK [Second task] **************************************************************************************************
changed: [localhost]

PLAY RECAP **********************************************************************************************************
localhost                  : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

If you run the second playbook again, the output will look like this:
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