# 用户-user

明文创建账户（前提是Successfully installed passlib-1.7.4）passlib-1.7.4-py2.py3-none-any.why

```bash
-m user -a "name=foo password={{ oracle_user_password | password_hash('sha512') }}"
```

密文创建用户

```bash
# 获取加密后的密码
python3 -c "from passlib.hash import sha512_crypt; import getpass; print(sha512_crypt.using(rounds=5000).hash(getpass.getpass()))"

# 具体创建方法
-m user -a "name=foo password='$6$6aTJ2Epp0iiyBnN5$ucTF1zJcvam4l2VVS25Oqwirn1lvxGYy1BcD.byzraF7nBBtetYkMj98S39abvhYjPtnK1D9QkrELSuGVBZMa.'"
```

删除用户

```bash
-m user -a "name=foo state=absent"
```
