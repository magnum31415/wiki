# Repositories
Puede agregar un repositorio de terceros de dos maneras. Puede crear un archivo .repo en el directorio /﻿etc/yum.repos.d/ o puede agregar una sección [repository] al archivo /﻿etc/﻿dnf/﻿dnf.conf. 
Red Hat recomienda usar archivos .repo y reservar el archivo dnf.conf para configuraciones de repositorio adicionales. 

El comando dnf config-manager puede habilitar o deshabilitar los repositorios.

```
dnf config-manager --enable rhel-9-server-debug-rpms
dnf config-manager --dissable rhel-9-server-debug-rpms
```

Deberían descargar la clave en un archivo local en lugar de permitir que el comando dnf la recupere de una fuente externa. 
El siguiente archivo .repo usa el parámetro gpgkey para hacer referencia a una clave local:
````
[EPEL]
name=EPEL 9
baseurl=https://dl.fedoraproject.org/pub/epel/9/Everything/x86_64/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-9
````

```
rpm --import  http://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-9 
dnf install  https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```

## List
```
dnf repolist all
```

## Add repos

```
dnf config-manager --add-repo "http://www.server.com/rhel9.0/x86_64/rhcsa"
```
