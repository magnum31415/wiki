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
#to add new ACL for specific user
setfacl -m u:<user>:rwx /file
 
#add new ACL recursively to specific group
setfacl -R -m g:<group>:rwx /directory 

#add default ACL for specific user
setfacl -m d:u:<user>:rwx /directory 

#add new ACL for specific user with no permissions
$setfacl -m u:<user>:- /file

#denega permisos a un usuario especifico a un fichero
setfacl -m u:<user>:0 /file

#Elimina una acl 
setfctl -x u:<user> /file


setfacl -m u:user1:rw -m u:user2:r -m u:user3:rwx -m g:demo1:rw -m g:demo2:rwx -m o:r sample

#View ACL
getfacl sample

#Delete ACL for specific user
setfacl -x u:<user> /file
````

## Default ACLs

*¿Qué es una Default ACL?*
Se configura para que cualquier archivo o subdirectorio que se cree dentro del directorio herede automáticamente los permisos definidos por la Default ACL.
Las Default ACL permiten definir permisos adicionales (más allá de los permisos básicos de propietario, grupo y otros) que se aplican de manera predeterminada a cualquier nuevo archivo o subdirectorio.

- **Control Fino de Permisos:** Permiten gestionar permisos de manera más granular que los permisos tradicionales de UNIX.
- **Facilidad de Administración:** Si necesitas asegurar que ciertos usuarios o grupos siempre tengan permisos específicos en un conjunto de archivos o directorios, las Default ACLs son una herramienta poderosa.
- **Seguridad:** Aseguran que los nuevos archivos y directorios creados dentro de un directorio específico hereden automáticamente permisos que se alinean con las políticas de seguridad.

