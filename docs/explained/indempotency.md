# Indempotency

Indempotency is a property of operations that guarantees that the operation can be executed multiple times without changing the result beyond the initial application.


<figure markdown>
  ![Indemportency](/assets/images/control.png){ width="300" }
  [^1]
</figure>


Almost every Ansible module is by default idempotent. This means that if you run the same task multiple times, the result will be the same. This is a very important property of Ansible. It allows you to run the same playbook multiple times without changing the result. This is very useful if you want to apply the same configuration to multiple systems. You can run the playbook multiple times without worrying about the result.

[^1]: <a href="https://www.flaticon.com/free-icons/process" title="process icons">Process icons created by Uniconlabs - Flaticon</a>