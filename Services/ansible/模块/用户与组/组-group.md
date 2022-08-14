```yaml
- name: create oracle user
  user: name=oracle home=/home/oracle password={{ oracle_user_password | password_hash('sha512') }} group=oinstall groups={{ oracle_group }} shell=/bin/bash state=present

```

