# NFS
Red Hat Enterprise Linux 9 uses NFS version 4.2. RHEL fully supports both NFSv3 and NFSv4 protocols.
NFS servers export directories. NFS clients mount exported directories to an existing local mount point directory. NFS clients can mount exported directories in multiple ways:

1. Manually by using the mount command.
2. Persistently at boot by configuring entries in the /etc/fstab file.
3. On demand by configuring an automounter method.

The automounter methods, which include the autofs service and the systemd.automount facility.
You must install the nfs-utils package to obtain the client tools for manually mounting, or for automounting, to obtain exported NFS directories.
````
dnf install nfs-utils
````
## Query a Server's Exported NFS Directories
The method to query a server to view the available exports is different for each protocol version NFSv3 and NFSv4

### NFSv3 used the RPC protocol,
NFSv3 used the RPC protocol, which requires a file server that supports NFSv3 connections to run the rpcbind service. 
An NFSv3 client connects to the rpcbind service at port 111 on the server to request NFS service.
The server responds with the current port for the NFS service. 
Use the showmount command to query the available exports on an RPC-based NFSv3 server.
````
showmount --exports server
````
### NFSv4 protocol eliminated the use of the legacy RPC protocol for NFS transactions.
NFSv4 protocol eliminated the use of the legacy RPC protocol for NFS transactions.
NFSv4 introduced an export tree that contains all of the paths for the server's exported directories. 
To view all of the exported directories, mount the root (/) of the server's export tree. Mounting the export tree's 
root provides browseable paths for all exported directories, as children of the tree's root directory, but does not mount ("bind") any of the exported directories.
[root@host ~]# mkdir /mountpoint
[root@host ~]# mount server:/ /mountpoint
[root@host ~]# ls /mountpoint


## Mount

### Manually Mount Exported NFS Directories
````
mount -t nfs -o rw,sync server:/export /mountpoint
````

### Persistently Mount Exported NFS Directories
````
vim /etc/fstab
exserver:/export  /mountpoint  nfs  rw,sync  0 0
mount /mountpoint
````

### Automounter Service

####  Install packeges configure an automount 
````
 sudo dnf install autofs nfs-utils

````

#### Create a Master Map
Add a master map file to /etc/auto.master.d. This file identifies the base directory for mount points, and identifies the mapping file to create the automounts.
````
sudo vim /etc/auto.master.d/demo.autofs
/shares  /etc/auto.demo
````
This entry uses the /shares directory as the base for indirect automounts. The /etc/auto.demo file contains the mount details.

#### Create an Indirect Map
Each mapping file identifies the mount point, mount options, and source location to mount for a set of automounts.
The mapping file-naming convention is /etc/auto.name, where name reflects the content of the map.
````
sudo vim /etc/auto.demo
work  -rw,sync  serverb:/shares/work

#systemctl restart autofs
#systemctl enable --now autofs

````
