# Looping until a condition is met
Looping in ansible is very powerful. If you combine looping with the `until` conditional you can create usefull playbooks. In this section we will look at some examples of how to use looping with `until`.

`ignore_errors` is a very useful option to use with `until`. It will ignore any errors that occur during the loop. This is very useful if you want to wait for a service to come online. If the service is not yet online the loop will fail. If you use `ignore_errors` the loop will continue until the condition is met.

## Continue if website is online
Wait for a website to come online and continue only then.

```yaml title="Wait for a website to come online"
- shell:
    cmd: curl --connection-timeout 2 http://localhost
  register: result
  until: result is succeeded
  retries: 10
  delay: 30
  ignore_errors: true
```

## Configure a windows domain controller
If you configure a windows domain controller after the first reboot the connection might already be established, but the domain services are not yet ready. We want to only continue when the domain services are ready.

```yaml title="Configure a windows domain controller"
#Wait for DC to be back after promotion
- name: Wait for domain controller to be ready
  win_shell: |
    Get-ADDomain mydomain.com
  register: dc_ready
  until: dc_ready is not failed
  ignore_errors: yes
  retries: 180
  delay: 5
```