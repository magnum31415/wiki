# Unix Permisions

| Permision | efect in files | efect in dirs  |
|----------|----------|----------|
| u+s (suid) | El archivo se ejecuta como el usuario propietario del archivo, no como el usuario que lo ejecutó. | None |
| g+s (sgid) | El archivo se ejecuta como el grupo propietario. | Los archivos creados en el directorio han establecido al propietario del grupo para que coincida con el propietario del grupo del directorio. |
| o+t (sticky) | None | Los usuarios con acceso de escritura en el directorio solo pueden eliminar los archivos de los que son propietarios, pero no pueden eliminar ni forzar el guardado de archivos cuyos propietarios sean otros usuarios.|
