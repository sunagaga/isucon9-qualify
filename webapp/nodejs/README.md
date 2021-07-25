# isucon9-qualify nodejs 実装

## サポートする MySQL のバージョンについて

nodejsでアプケーションを起動する場合、MySQL 8.0の認証方式によっては動作しないことがあります。
詳しくは、 https://github.com/isucon/isucon9-qualify/pull/316 を参考にしてください

## To run nodejs app using Mysql 8.0
Change default auth method to 'mysql_native_password'

```sql
mysql > SELECT user, host, plugin FROM mysql.user;

#  +------------------+-----------+-----------------------+
#  | user             | host      | plugin                |
#  +------------------+-----------+-----------------------+
#  | isucari          | localhost | caching_sha2_password |
#  | ...              | ...       | ...                   |
#  +------------------+-----------+-----------------------+
```

```sql
mysql > ALTER USER 'isucari'@'localhost' IDENTIFIED WITH mysql_native_password BY 'isucari';

# +------------------+-----------+-----------------------+
# | user             | host      | plugin                |
# +------------------+-----------+-----------------------+
# | isucari          | localhost | mysql_native_password |
# | ...              | ...       | ...                   |
# +------------------+-----------+-----------------------+
```

If the password does not satisfy the requirements:
```sql
mysql > SHOW VARIABLES LIKE 'validate_password%';

# +--------------------------------------+--------+
# | Variable_name                        | Value  |
# +--------------------------------------+--------+
# | validate_password.check_user_name    | ON     |
# | validate_password.dictionary_file    |        |
# | validate_password.length             | 6      |
# | validate_password.mixed_case_count   | 1      |
# | validate_password.number_count       | 0      |
# | validate_password.policy             | MEDIUM |
# | validate_password.special_char_count | 1      |
# +--------------------------------------+--------+

mysql > SET GLOBAL validate_password.policy = LOW;
mysql > SET GLOBAL validate_password.length = 6;
```
