# Ansible vault
Ansible vault allows you to encrypt sensitive data such as passwords or API keys. This is useful if you want to store your Ansible playbooks in a public repository.

Ansible vault is integrated in Ansible and can be used in combination.

## Setting up Ansible vault
To use Ansible vault, you need to create a vault password file. This file contains the password that is used to encrypt and decrypt the vault files. The vault password file should never be committed to a git repository. The vault password file can also be an executable script that returns the password on stdout.

```bash title=".vault-pass"
mysecretpassword
```

```ini title="ansible.cfg"
[defaults]
vault_password_file = .vault-pass
```

## Encrypting files
You can use the `ansible-vault encrypt/decrypt` command to encrypt/decrypt files.
```bash
ansible-vault encrypt mysecretfile
ansible-vault decrypt mysecretfile
```

When running an Ansible playbook, you can also use the `--ask-vault-pass` flag to enter the vault password.
Ansible will automatically decrypt the vault files when running the playbook.

An encrypted Ansible vault file will look like this:
```ini title="mysecretfile"
$ANSIBLE_VAULT;1.1;AES256
38643762643366326564356637313332613431636664353133323665393231323839343839653136
6464306164356432656537356263336362373166386639620a353132396535633062663533366430
65376635366539636262613964326265626536383138626664363134396433376266653931343135
3931323438393236350a633839623764623765393634646331366634326562373730643934333732
3133
```

## Reference
[Ansible vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html)