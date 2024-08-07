# Unix Permisions

| Permision    | effect in files | effect in dirs  |
|--------------|----------|----------|
| u+s (suid)   | El archivo se ejecuta como el usuario propietario del archivo, no como el usuario que lo ejecutó. | None |
| g+s (sgid)   | El archivo se ejecuta como el grupo propietario. | Los archivos creados en el directorio han establecido al propietario del grupo para que coincida con el propietario del grupo del directorio.|
| o+t (sticky) | None | usuarios con acceso escritura en directorio solo pueden eliminar archivos de los que son propietarios, pero no pueden eliminar ni modificar cuyos propietarios sean otros usuarios.|



Los valores de **umask** predeterminados del sistema para usuarios de shell Bash se definen en los archivos /etc/profile y /﻿etc/﻿bashrc. 
Los usuarios pueden anular los valores predeterminados del sistema en sus archivos .bash_profile o .bashrc en sus directorios de inicio.


Uso con **chmod: X** es útil cuando quieres añadir permisos de ejecución a directorios, pero no a archivos regulares, a menos que estos ya tengan permiso de ejecución.

# ACLs
An access control list (ACL) lets you assign permissions for each unique user or group. 

You can use the setfacl and getfacl command utilities to assign and verify the ACL of a file or directory. 

````
#Set ACL
setfacl --modify user1:rw sample
setfacl --modify user2:r sample
setfacl -m u:user1:rw -m u:user2:r -m u:user3:rwx -m g:demo1:rw -m g:demo2:rwx -m o:r sample

#View SLC
getfacl sample

#Delete ACL
setfacl -x u:user1 sample
````
