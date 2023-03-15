# Exection strategy
By default ansible will run tasks in parallel and continue to the next task as soon as all hosts have finished the current task. This is called the `linear` strategy. This is the default strategy and is the best choice for most use cases.

## Change strategy
Another common strategy is `free`. This strategy will run tasks in parallel and **not** wait for all hosts to finish before moving on to the next task. This is useful when you want to run a playbook as fast as possible.

```yaml
- hosts: all
  strategy: free
  tasks:
  # ...
```

## Forks
By default ansible is configured with 5 forks. This means that ansible will run 5 tasks in parallel on each host. You can change this number by setting the `forks` variable. This requires more compute resources on the ansible control node.

```ini title="ansible.cfg"
[defaults]
forks = 30
```

## Serial
Serial will limit the amount of hosts that ansible will run tasks on in parallel.
Compared to forks, serial can be configured with a percentage of all hosts involved in the play.

```yaml
- name: run a play on max. 30% of all hosts at the same time
  hosts: webservers
  serial: "30%"
```

## Run once
If you want to run a task only on one host, you can use the `run_once` option.
All gathered facts will be applied to all hosts of the play. By default ansible will run the task on the first host in the inventory.
You can override this by setting the `delegate_to:`.

```yaml
- command: python3 ./upgrade_database.py
  run_once: true
  delegate_to: webserver1
```

## Order of execution
By default ansible will execute the taks in order of the hosts listed in the inventory.
You can change this by setting the `order` option.

```yaml
- hosts: all
  order: sorted
  tasks:
  # ...
```

Possible values are:

- `inventory`: Use the order of hosts in the inventory
- `reverse_inventory`: Use the order of hosts in the inventory in reverse order
- `sorted`: Sort hosts by name
- `reverse_sorted`: Sort hosts by name in reverse order
- `shuffle`: Randomize the order of hosts each play

## Reference
[Ansible docs - Execution strategy](https://docs.ansible.com/ansible/latest/user_guide/playbooks_strategies.html)