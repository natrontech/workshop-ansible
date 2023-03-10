# Handlers
Handlers in ansible are tasks that are triggered by a notification. A notification is sent by a task, and the handler will be run after the task has finished. This is useful for restarting services, or other tasks that should only be run after a change has been made.

Good examples for handlers are:

- Restarting Nginx service after config change
- Reboot after installing a new kernel

## Defining handlers
Handlers can be in a separate file, if you use the production directory structure.
For small playbooks, you can also define them in the same file as the tasks.

```yaml title="nginx-setup.yml" hl_lines="6 7 8 9 10"
- name: setup nginx
  hosts: webserver
  tasks:
  ...
  
  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
```

## Running handlers
To run the handlers, you need to notify them in a task.
The notification will only occur, if the task has status **changed**. 
The task will be run, and after it has finished, the handler will be run.
If you have multiple tasks notifing the same handler, the handler will only be run once at the end of the play.

```yaml title="nginx-setup.yml" hl_lines="8 11"
- name: setup nginx
  hosts: webserver
  tasks:
    - name: copy nginx config
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
      notify: restart nginx

  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
```

## Running handlers during a play
If you want to run a handler during a play, you can use the `meta: flush_handlers` task.
This will run all handlers that have been notified until that point.

```yaml title="nginx-setup.yml" hl_lines="10 11"
- name: setup nginx
  hosts: webserver
  tasks:
    - name: copy nginx config
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
      notify: restart nginx

    - name: Flush all pending handlers
      meta: flush_handlers

      ...

  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
```

## Running multiple handlers
If you have multiple handlers, you can run them by using the `listen` keyword.
This will group the handlers in a single notification called `restart web services`.
```yaml hl_lines="4 11 17"
tasks:
  - name: Restart everything
    command: echo "this task will restart the web services"
    notify: "restart web services"

handlers:
  - name: Restart memcached
    service:
      name: memcached
      state: restarted
    listen: "restart web services"

  - name: Restart apache
    service:
      name: apache
      state: restarted
    listen: "restart web services"
```

You can also notify multiple handlers in a single task.
```yaml hl_lines="4 5 6"
tasks:
  - name: Restart everything
    command: echo "this task will restart the web services"
    notify: 
    - "Restart memcached"
    - "Restart apache"
```