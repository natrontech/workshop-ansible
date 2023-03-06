# What is Ansible?
Ansible is a configuration management tool. It allows you to configure remote systems in a declarative way. It is agentless and uses SSH, WinRM or HTTPS to connect to remote systems. Ansible is written in Python and runs on Linux, MacOS and Windows(WSL). Windows native is not supported.

Ansible is open-source and maintained by RedHat. Therefore it is completely **free** to use. It is also available as a commercial product called Ansible Tower directly from RedHat.

# Why not use scripts to configure remote systems?
Ansible offeres a lot of advantages over scripts. It is agentless, declarative and idempotent. It is also very easy to use and extend. Ansible has a big community and a lot of modules and plugins. Ansible has built-in parallelism. So you can update multiple systems at the same time. Because Ansible allows you to configure not only servers but also network devices, it is a good tool for multiple aspects.