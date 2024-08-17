

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
````
mkdir /mountpoint
mount -t nfs4 server:/ /mountpoint
ls /mountpoint
````

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

# Automounter Service
The master map file. The autofs service uses /etc/auto.master (master map) as its default primary configuration file.
The master map file lists mount points controlled by autofs

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
The autofs service automatically creates and removes the mount point. The autofs service creates and removes the /shares and /shares/work directories as needed.

#### Direct Map
To use directly mapped mount points, the master map file might appear as follows:
A direct map is used to map an NFS export to an absolute path mount point. Only one direct map file is necessary, and can contain any number of direct maps.
To use directly mapped mount points, the master map file might appear as follows:

````
/-  /etc/auto.direct
````
All direct map entries use /- as the base directory. In this case, the mapping file that contains the mount details is /etc/auto.direct.
The content for the /etc/auto.direct file might appear as follows:
````
/mnt/docs  -rw,sync  serverb:/shares/docs
````
### Examples

#### Ejemplo de Mapa Directo
En un mapa directo, cada punto de montaje se define con su ruta absoluta en el archivo del mapa, y autofs monta el sistema de archivos en esa ubicación exacta.

Configuración en /etc/auto.master o /etc/auto.master.d:
````
/-  /etc/auto.direct
````
Contenido de /etc/auto.direct:
````
/mnt/share1  -fstype=nfs,rw  server:/export/share1
/mnt/share2  -fstype=nfs,rw  server:/export/share2
/home/user1  -fstype=nfs,rw  server:/export/home/user1
````
Explicación:
/mnt/share1: Se monta desde server:/export/share1 cuando se accede a /mnt/share1.
/mnt/share2: Se monta desde server:/export/share2 cuando se accede a /mnt/share2.
/home/user1: Se monta desde server:/export/home/user1 cuando se accede a /home/user1.


#### Ejemplo de Mapa Indirecto
En un mapa indirecto, los sistemas de archivos se montan como subdirectorios bajo un directorio base (/mnt/indirect en este caso). Esto permite organizar los montajes de forma más estructurada, agrupando varios sistemas de archivos bajo un mismo directorio.

Configuración en /etc/auto.master:
````
/mnt/indirect  /etc/auto.indirect
Contenido de /etc/auto.indirect:
````
````
share1  -fstype=nfs,rw  server:/export/share1
share2  -fstype=nfs,rw  server:/export/share2
user1   -fstype=nfs,rw  server:/export/home/user1
````
Explicación:
share1: Se monta desde server:/export/share1 en /mnt/indirect/share1.
share2: Se monta desde server:/export/share2 en /mnt/indirect/share2.
user1: Se monta desde server:/export/home/user1 en /mnt/indirect/user1.


#### Start Automunter
````
sudo systemctl enable --now autofs
````

**NOTE**
With an automounter indirect map, you must access each exported subdirectory for them to mount. With an automounter direct map, after you access the mapped mount point, you can immediately view and access the subdirectories and content in the exported directory.


````
/etc/auto.master.d/direct.autofs
/-	/etc/auto.direct

/etc/auto.master.d/indirect.autofs
/internal	/etc/auto.indirect

/etc/auto.direct
/external	-rw,sync,fstype=nfs4	serverb.lab.example.com:/shares/direct/external

/etc/auto.indirect
*	-rw,sync,fstype=nfs4	serverb.lab.example.com:/shares/indirect/&
````

