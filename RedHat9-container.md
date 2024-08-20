# Container 

# Examples


Run apache container with port and directory mappings
````
podman login registry.redhat.io
podman search httpd
podman pull docker.io/library/httpd
podman images
podman run -d -name web1 -p 8080:80  <IMAGE_ID>
podamn exec -it web1 /bin/bash
podman ps
podman stop web1
podman ps -a
podman rm web1
podman run -d -name web1 -p 8080:80 -v /web:/var/www/htdocs:Z <IMAGE_ID>
podman stop web1
podman rm web1

#Arranque como root
podman generate systemd web1 > /etc/systemd/system/web1-container.service
systemctl daemon-reload
systemctl enable web1-container.service
systemctl start web1-container.service

#Arranque como rootless
ssh user@server
podman pull docker.io/library/httpd
podman run -d -name web1 -p 8080:80 -v /web:/var/www/htdocs:Z <IMAGE_ID>
mkdir -p ~/.config/systemd/user/
cd ~/.config/systemd/user
podman generate systemd --name web1 --files --new
systemctl --user daemon-reload
systemctl --user enable --now container-web1
systemctl --user stop  container-web1
systemctl --user start  container-web1
#servico arrancado aunque el usuario este descontado
loginctl enable-linger

#Creacion Container a partir de Contarinerfile
echo " 
FROM alpine:latest
RUN apk add --no-cache nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
" > Containerfile
podman build -t nginx-web1 .
podman run -d  --name "nginx-web1" -p 8081:80 <IMAGE_ID>
````

## Run the python38 container in detached mode from an image with the python 3.8 package and based on the ubi8 image. The image is hosted on a remote registry.

Install the container-tools meta-package
````
sudo dnf install @Container-Management
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

## Network create

podman network create command to create a DNS-enabled network.

````
podman network create --gateway 10.87.0.1  --subnet 10.87.0.0/16 db_net
 podman network inspect db_net
 
 podman run -d --name db01 \
-e MYSQL_USER=student \
-e MYSQL_PASSWORD=student \
-e MYSQL_DATABASE=dev_data \
-e MYSQL_ROOT_PASSWORD=redhat \
-v /home/user/db_data:/var/lib/mysql:Z \
-p 13306:3306 \
--network db_net \
registry.lab.example.com/rhel8/mariadb-105

 podman run -d --name client01 \
--network db_net \
registry.lab.example.com/ubi8/ubi:latest \
sleep infinity

podman exec -it db01 dnf install -y iputils iproute
podman exec -it client01 dnf install -y iputils iproute

podman exec -it db01 ping -c3 client01
podman exec -it client01 ping -c3 db01


````
## Manage Containers as System Services
Configure a container as a systemd service

### Requirements for systemd User Services
The service starts when you open a session (graphical interface, text console, or SSH), and it stops when you close the last session. This behavior differs from a system service, which starts when the system boots and stops when the system shuts down.
when you create a user account with the useradd command, the system uses the next available ID from the regular user ID range. The system also reserves a range of IDs for the user's containers in the /etc/subuid file. If you create a user account with the useradd command --system option, then the system does not reserve a range for the user containers. As a consequence, you cannot start rootless containers with system accounts.
Create a dedicated user account to manage containers
````
sudo useradd appdev-adm
````
Podman is a stateless utility and requires a full login session
````
ssh appdev-adm@host
````
You then configure the container registry and authenticate with your credentials. You run the http container with the following command.

````
podman run -d --name webserver1 -p 8080:8080 -v ~/app-artifacts:/var/www/html:Z registry.access.redhat.com/ubi8/httpd-24
````

### Create systemd User Files for Containers
You can manually define systemd services in the ~/.config/systemd/user/ directory. 
Use the podman generate systemd command to generate systemd service files for an existing container. 
````
 podman generate systemd --name webserver1
````

The podman generate systemd command --new option instructs the podman utility to configure the systemd service to create the container when the service starts, and to delete the container when the service stops.
````
 podman generate systemd --name webserver1 --new
````

### Manage systemd User Files for Containers
Now that you created the systemd user file, you can use the systemctl command --user option to manage the webserver1 container.
````
systemctl --user daemon-reload
systemctl --user start container-webserver1.service
systemctl --user status container-webserver1.service
````
When you configure a container with the systemd daemon, the daemon monitors the container status and restarts the container if it fails. Do not use the podman command to start or stop these containers. Doing so might interfere with the systemd daemon monitoring.


|Type|System/User|Command|
|-------|------|------|
|Storing custom unit files| System services|/etc/systemd/system/unit.service|
|-|User services|~/.config/systemd/user/unit.service|
|Reloading unit files|System services|# systemctl daemon-reload|
||User services|$ systemctl --user daemon-reload|
|Starting and stopping a service|System services|# systemctl start UNIT # systemctl stop UNIT|
||User services| $ systemctl --user start UNIT $ systemctl --user stop UNIT|
|Starting a service when the machine starts|System services| # systemctl enable UNIT|
||User services| $ loginctl enable-linger $ systemctl --user enable UNIT|



### Configure Containers to Start at System Boot

The systemd service stops the container after a certain time if the user logs out from the system. This behavior occurs because the systemd service unit was created with the --user option, which starts a service at user login and stops it at user logout.
You can change this default behavior, and force your enabled services to start with the server and stop during the shutdown, by running the loginctl enable-linger.
You use the loginctl command to configure the systemd user service to persist after the last user session of the configured service closes.
````
loginctl show-user appdev-adm
loginctl enable-linger
loginctl show-user appdev-adm
````

````
sudo useradd contsvc
sudo passwd contsvc
ssh contsvc@servera

mkdir -p ~/.config/containers/
cp /tmp/containers-services/registries.conf ~/.config/containers/

mkdir -p ~/webcontent/html/
echo "Hello World" > ~/webcontent/html/index.html

podman login registry.lab.example.com
podman search registry.lab.example.com/
podman run -d --name webapp -p 8080:8080 -v ~/webcontent:/var/www:Z registry.lab.example.com/rhel8/httpd-24:1-163
curl http://localhost:8080

mkdir -p ~/.config/systemd/user/
cd ~/.config/systemd/user
podman generate systemd --name webapp --files --new
podman stop webapp
podman rm webapp
podman ps -a

systemctl --user daemon-reload
systemctl --user enable --now container-webapp
curl http://localhost:8080
podman ps
systemctl --user stop container-webapp
podman ps --all
systemctl --user start container-webapp

loginctl enable-linger
loginctl show-user contsvc

````

### Manage Containers as Root with systemd
1. Do not create a dedicated user for container management.
2. The service file must be in the /etc/systemd/system directory instead of in the ~/.config/systemd/user directory.
3. You manage the containers with the systemctl command without the --user option.
4. Do not run the loginctl enable-linger command as the root user.




````
````

````
````

