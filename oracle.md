



## Multithenant CDB & PDB
* Verificar el contenedor actual: ``SHOW CON_NAME;``
* Cambiar la sesión al PDB "DB1": ``ALTER SESSION SET CONTAINER = DB1;``
* Cambiar del PDB actual a la base de datos contenedora (CDB): ``ALTER SESSION SET CONTAINER = CDB$ROOT;``
* Abrir el PDB "DB1": ``ALTER PLUGGABLE DATABASE DB1 OPEN;``
* Ver los PDBs disponibles: ``SHOW PDBS;`` o  ``SELECT name, open_mode FROM v$pdbs;``
* Si deseas cerrar solo un PDB y mantener el CDB activo, conéctate al CDB y ejecuta: ``ALTER PLUGGABLE DATABASE <nombre_del_PDB> CLOSE;``

## Listener

*Parar Listener: ``/u01/app/oracle/product/19.21.0.0/CDB1/bin/lsnrctl stop LISTENER_CDB1``
