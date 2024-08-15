# Container 

## Run the python38 container in detached mode from an image with the python 3.8 package and based on the ubi8 image. The image is hosted on a remote registry.

Install the container-tools meta-package
````
sudo dnf install container-tools
````
Configure the registry.lab.example.com  registry in your home directory. 
Log in to the container registry with admin as the user and xxxx
````
#/home/user/.config/containers/registries.conf 
unqualified-search-registries = ['registry.lab.example.com']
[[registry]]
location = "registry.lab.example.com"
insecure = true
blocked = false
````
Verify class registry added
````
podman info
````
Log in to the classroom registry.
````
podman login registry.lab.example.com
````

Search for a python-38 container in the registry.lab.example.com 
````
podman search registry.lab.example.com/
````
Inspect the image.
````
skopeo inspect docker://registry.lab.example.com/ubi8/python-38
````

Pull the python-38 container image.
````
podman pull registry.lab.example.com/ubi8/python-38
````


Verify that the container is downloaded to the local image repository.
````
podman images
````

Start the python38 container.
````
podman run -d --name python38 registry.lab.example.com/ubi8/python-38 sleep infinity
````

Verify that the container was created.
````
podman ps
````



## Build a container image called python39:1.0 from a container file, and use the image to create a container called python39.

Containerfile
````
FROM registry.lab.example.com/ubi9-beta/ubi:latest
RUN echo -e '[rhel-9.0-for-x86_64-baseos-rpms]\nbaseurl = http://content.example.com/rhel9.0/x86_64/dvd/BaseOS\nenabled = true\ngpgcheck = false\nname = Red Hat Enterprise Linux 9.0 BaseOS (dvd)\n[rhel-9.0-for-x86_64-appstream-rpms]\nbaseurl = http://content.example.com/rhel9.0/x86_64/dvd/AppStream\nenabled = true\ngpgcheck = false\nname = Red Hat Enterprise Linux 9.0 Appstream (dvd)'>/etc/yum.repos.d/rhel_dvd.repo
RUN yum install --disablerepo=* --enablerepo=rhel-9.0-for-x86_64-baseos-rpms --enablerepo=rhel-9.0-for-x86_64-appstream-rpms -y python3
````
Create the container image from the container file.
````
podman build -t python39:1.0 /home/student/python39/.
````

Verify that the container image exists in the local image repository.
````
podman images
````
Inspect the python39 container.
````
podman inspect localhost/python39:1.0
````
Create the python39 container.
````
podman create --name python39 localhost/python39:1.0
````
Start the python39 container.
````
podman start python39
````

Verify that the container is running.
````
podman ps
````

Copy the /home/student/script.py python script into the /tmp directory in both containers.
````
podman cp /home/student/script.py python39:/tmp/script.py
podman cp /home/student/script.py python38:/tmp/script.py
````
Run the Python script in both containers, and then run the Python script on the host.
````
podman exec -it python39 python3 /tmp/script.py
podman exec -it python38 python3 /tmp/script.py
````
Stop both containers.
````
podman stop python39 python38
````
Remove both containers.
````
podman rm python39 python38
````

Remove both container images.
````
podman rmi localhost/python39:1.0  registry.lab.example.com/ubi8/python-38:latest  registry.lab.example.com/ubi9-beta/ubi
````

## Environment variables to customize the container at creation time.
Run container
````
 podman run -d registry.lab.example.com/rhel8/mariadb-105 \
--name db01
````
Check startup
````
 podman ps -a
CONTAINER ID  IMAGE                                              COMMAND         CREATED         STATUS                     PORTS       NAMES
20751a03897f  registry.lab.example.com/rhel8/mariadb-105:latest  run-mysqld      29 seconds ago  Exited (1) 29 seconds ago              db01
You use the podman container logs command to investigate the reason of the container status.
````
Check Logs
````
 podman container logs db01
...output omitted...
You must either specify the following environment variables:
  MYSQL_USER (regex: '^[a-zA-Z0-9_]+$')
  MYSQL_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
  MYSQL_DATABASE (regex: '^[a-zA-Z0-9_]+$')
Or the following environment variable:
  MYSQL_ROOT_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
Or both.
````
Inspect the mariadb-105 container image to find more information about the environment variables to customize the container.
The usage label from the output provides an example of how to run the image. 

````
skopeo inspect docker://registry.lab.example.com/rhel8/mariadb-105
...output omitted...
        "name": "rhel8/mariadb-105",
        "release": "40.1647451927",
        "summary": "MariaDB 10.5 SQL database server",
        "url": "https://access.redhat.com/containers/#/registry.access.redhat.com/rhel8/mariadb-105/
images/1-40.1647451927",
        "usage": "podman run -d -e MYSQL_USER=user -e MYSQL_PASSWORD=pass -e MYSQL_DATABASE=db -p 3306:3306 rhel8/mariadb-105",
        "vcs-ref": "c04193b96a119e176ada62d779bd44a0e0edf7a6",
        "vcs-type": "git",
        "vendor": "Red Hat, Inc.",
...output omitted...
````
Use the podman run command -e option to pass environment variables to the container
````
podman run -d --name db01 \
-e MYSQL_USER=student \
-e MYSQL_PASSWORD=student \
-e MYSQL_DATABASE=dev_data \
-e MYSQL_ROOT_PASSWORD=redhat \
registry.lab.example.com/rhel8/mariadb-105
````

## Container Persistent Storage
By default  all of the new data that the user or the application writes is lost after removing the container.
To persist data, you can use host file-system content in the container with the --volume (-﻿v) option. 
````
````

 In a rootless container, the user has root access from within the container, because Podman launches a container inside the user namespace.
You can use the podman unshare command to run a command inside the user namespace. To obtain the UID mapping for your user namespace, use the podman unshare cat command.
In the container, the root user (UID and GID of 0) maps to your user (UID and GID of 1000) on the host machine.
In the container, the UID and GID of 1 maps to the UID and GID of 100000 on the host machine.
````
[user@host ~]$ podman unshare cat /proc/self/uid_map
         0       1000          1
         1     100000      65536
[user@host ~]$ podman unshare cat /proc/self/gid_map
         0       1000          1
         1     100000      65536
````
 use the podman exec command to view the mysql user UID and GID inside the container that is running with ephemeral storage.

````
podman exec -it db01 grep mysql /etc/passwd
mysql:x:27:27:MySQL Server:/var/lib/mysql:/sbin/nologin
````
The UID and GID of 27 in the container maps to the UID and GID of 100026 on the host machine. 
You can verify the mapping by viewing the ownership of the /home/user/db_data directory with the ls command.
````
ls -l /home/user/
drwxrwxr-x. 3  100026  100026 18 May  5 14:37 db_data
````

use the podman run -v /home/user/db_data:/var/lib/mysql:Z command to set the SELinux context for the /home/user/db_data directory when you mount it as persistent storage for the /var/lib/mysql directory.
````
podman run -d --name db01 \
-e MYSQL_USER=student \
-e MYSQL_PASSWORD=student \
-e MYSQL_DATABASE=dev_data \
-e MYSQL_ROOT_PASSWORD=redhat \
-v /home/user/db_data:/var/lib/mysql:Z \
registry.lab.example.com/rhel8/mariadb-105

````

## Port Mapping to Containers
se the podman run command -p option to set a port mapping from the 13306 port from the container host to the 3306 port on the db01 container.
````
podman run -d --name db01 \
-e MYSQL_USER=student \
-e MYSQL_PASSWORD=student \
-e MYSQL_DATABASE=dev_data \
-e MYSQL_ROOT_PASSWORD=redhat \
-v /home/user/db_data:/var/lib/mysql:Z \
-p 13306:3306 \
registry.lab.example.com/rhel8/mariadb-105
````
Use the podman port command -a option to show all container port mappings in use. You can also use the podman port db01 command to show the mapped ports for the db01 container.
````
 podman port -a
1c22fd905120	3306/tcp -> 0.0.0.0:13306
 podman port db01
3306/tcp -> 0.0.0.0:13306
````
You use the firewall-cmd command to allow port 13306 traffic into the container host machine to redirect to the container.
````
firewall-cmd --add-port=13306/tcp --permanent
firewall-cmd --reload
````
