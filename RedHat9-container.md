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
