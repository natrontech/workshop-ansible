# Registering task results
You can register the return of a task to use it later in the playbook.

```yaml title="register.yml"
- name: Register result
  shell:
    command: echo "hello"
  register: result

- name: Show result
  debug:
    var: result
```

There are some common variables that you will find in the `result` variable.
For example:

- changed
- failed
- msg
- rc
- results
- skipped
- stderr
- stderr_lines
- stdout
- stdout_lines

```bash title="output"
TASK [Register result] ******************************************
changed: [localhost]

TASK [Show result] **********************************************
ok: [localhost] => {
    "result": {
        "changed": true,
        "cmd": "echo \"hello\"",
        "delta": "0:00:00.002058",
        "end": "2023-03-10 11:10:17.866992",
        "failed": false,
        "msg": "",
        "rc": 0, # (1)
        "start": "2023-03-10 11:10:17.864934",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "hello",
        "stdout_lines": [
            "hello"
        ]
    }
}
```

1. `rc` stands for return code. It is the exit code of the command. 
   This is very usefull because you can run code based upon the exit code of a command.