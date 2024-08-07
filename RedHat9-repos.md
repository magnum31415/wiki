# Repositories
Puede agregar un repositorio de terceros de dos maneras. Puede crear un archivo .repo en el directorio /﻿etc/yum.repos.d/ o puede agregar una sección [repository] al archivo /﻿etc/﻿dnf/﻿dnf.conf. 
Red Hat recomienda usar archivos .repo y reservar el archivo dnf.conf para configuraciones de repositorio adicionales. 

El comando dnf config-manager puede habilitar o deshabilitar los repositorios.

```
dnf config-manager --enable rhel-9-server-debug-rpms
```

## List
```
dnf repolist all
```

## Add repos

```
dnf config-manager --add-repo "http://www.server.com/rhel9.0/x86_64/rhcsa"
```
