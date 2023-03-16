# Ansible runner
Ansible runner allows you to abstract the execution of Ansible playbooks and roles. This is useful if you want to run Ansible playbooks in a CI/CD pipeline or if you want to run Ansible playbooks in a container.

It makes sure that the Ansible environment is set up correctly and that the correct Ansible version is used.
Ansible runner is also the tool that is used by Ansible tower and AWX to run Ansible playbooks.

You can run Ansible runner in three ways:

- A standalone command line tool (ansible-runner) that can be started in the foreground or run in the background asynchronously
- A reference container image that can be used as a base for your own images and will work as a standalone container or running in Openshift or Kubernetes
- A python module - library interface

## Reference
[Ansible runner](https://ansible-runner.readthedocs.io/en/stable/)