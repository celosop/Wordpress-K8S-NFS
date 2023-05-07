# NFS Server Setup

## On the NFS Server VM

#### Install NFS Kernel Server

This is done on the VM that you wish to designate as the NFS storage

```bash
sudo apt install nfs-kernel-server
```

#### Create Export Directory

This is the shared directory on the NFS VM that will be accessible to the client VMs for use.

```bash
sudo mkdir -p /path/to/directory
```

Remove restrictive permissions from this folder to enable client access.

```bash
sudo chown nobody:nogroup /path/to/directory
sudo chmod 777 /path/to/directory
```

#### NFS Export File

The export file details out which IP/subnets can access the NFS server and what actions can they perform on it. 

```bash
sudo nano /etc/exports
```

Add your Client IP / Subnet with required permissions.

```ini
/path/to/directory x.x.x.x/x(rw,sync,no_subtree_check)
```

Change the permissions of the folder which is supposed to be mounted by the MySQL pod.

```bash
sudo chown 999:999 -R /path/to/directory/db
```

By doing this, you will avoid the need to use `no_root_squash` in the export file permissions, as this allows root access and is extremely dangerous and absolutely [not recommended](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/4/html/security_guide/s2-server-nfs-noroot).

> Here is what these permissions mean
>
> ```
> rw: read and write operations
> sync: write any change to the disc before applying it
> no_subtree_check: prevent subtree checking
> ```

#### 2.2.4 Export the Shared Folder

This makes the settings take effect

```bash
sudo exportfs -a
```

Restart the NFS Kernel Server service

```bash
sudo systemctl restart nfs-kernel-server
```

#### 2.2.5 Firewall Rules

If your VM has an active firewall, it will be necessary to allow traffic to your NFS server. If you are not sure about the firewall status, you can check using this in Ubuntu.

```bash
sudo ufw status
```

Add the following rule to allow traffic

```bash
sudo ufw allow from x.x.x.x/x to any port nfs
# Replace with your own Subnet
```

#### 2.2.6 Client App

This app is installed on the other VMs which require access to the NFS Storage server.

```bash
sudo apt install nfs-common
```

