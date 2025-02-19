

## Improve user exprience with sqlplus

rlwrap with sqlplus: ``rlwrap sqlplus scott/tiger@orcl``

**rlwrap** This command invokes the wrapper that adds readline functionalities, such as command history, line editing, and autocomplete.
sqlplus scott/tiger@orcl: This starts SQL*Plus, connecting with the username "scott", password "tiger" to the database identified as "orcl".
Using this command, SQL*Plus will now support:

Navigating through command history using the arrow keys.
* Interactive editing of the command line.
* An overall more comfortable experience similar to shells that have native readline support.
* This is especially useful when SQL*Plus doesn't natively include these features.

## Multithenant CDB & PDB
* Verificar el contenedor actual: ``SHOW CON_NAME;``
* Cambiar la sesión al PDB "DB1": ``ALTER SESSION SET CONTAINER = DB1;``
* Cambiar del PDB actual a la base de datos contenedora (CDB): ``ALTER SESSION SET CONTAINER = CDB$ROOT;``
* Abrir el PDB "DB1": ``ALTER PLUGGABLE DATABASE DB1 OPEN;``
* Ver los PDBs disponibles: ``SHOW PDBS;`` o  ``SELECT name, open_mode FROM v$pdbs;``
* Si deseas cerrar solo un PDB y mantener el CDB activo, conéctate al CDB y ejecuta: ``ALTER PLUGGABLE DATABASE <nombre_del_PDB> CLOSE;``

## Listener

*Parar Listener: ``/u01/app/oracle/product/19.21.0.0/CDB1/bin/lsnrctl stop LISTENER_CDB1``
