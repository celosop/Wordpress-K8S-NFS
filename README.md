# WordPress on Kubernetes with NFS

In this project, we are deploying a WordPress application along with a MySQL database in Kubernetes using static volumes on an NFS storage server.

All the data of both the applications is saved on the NFS volumes.

## Prerequisites

[Docker](https://docs.docker.com/engine/install/)

[Kubernetes](https://kubernetes.io/docs/tasks/tools/)

[Node setup as an NFS Server](/NFS Storage Setup.md)

## Defaults

### Wordpress

```php
DB_HOST - mysql-svc
DB_USERNAME - wordpress
DB_PASSWORD - pilgrim12345
DB_NAME - wpdb
```

#### Mount Path on NFS

```bash
/mnt/nfs-shared/wp
```

### MySQL

```mysql
MySQL Root Password - pilgrim12345
```

#### Mount Path on NFS

```bash
/mnt/nfs-shared/db
```

## How To Use

This project makes use of Kustomization. The combined manifests for MySQL, WordPress and MySQL Secret are deployed using `kustomization.yaml`

### Clone the repository

```bash
git clone https://github.com/nyukeit/Wordpress-K8S-NFS.git
```

### Apply the Kustomization

```bash
kubectl apply -k ./
```

## Known Issues

These need to be done post deployment.

### WordPress

The credentials for `wp-config.php` are not copied into the final file used by WordPress. This will need to be done manually. 

### MySQL

Connection to the database is not happening post deployment. A MySQL user will need to be created manually by logging into the MySQL Pod.

[MySQL User Setup](/MySQL User Setup.md)

