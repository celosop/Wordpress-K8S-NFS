# MySQL User Setup

This is done post deployment of apps.

Exec into the MySQL Pod

```bash
kubectl exec -it <podname> -- /bin/bash
```

Login as the `root` user. Use the password that was passed in the secret. If unchanged, it is `pilgrim12345`

```mysql
mysql -u root -p
```

Create a user named `wordpress`.

```mysql
CREATE USER 'wordpress'@'%' IDENTIFIED BY 'pilgrim12345';
```

Grant privileges to the user

```mysql
grant all privileges on *.* to 'wordpress'@'%' with grant option;
```

Refresh the permissions

```mysql
FLUSH PRIVILEGES;
```

Log out of the pod

```bash
mysql> quit
exit
```

