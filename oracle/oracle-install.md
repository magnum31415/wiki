# Postinstall - steps

## ** Scripts SQL necesarios después de la creación de la base de datos en Oracle**

| **Script**        | **Ubicación** | **Función** |
|------------------|-------------|------------|
| `catalog.sql`   | `$ORACLE_HOME/rdbms/admin/catalog.sql` | Crea el **diccionario de datos**, incluyendo vistas `DBA_*`, `ALL_*`, `USER_*`. |
| `catproc.sql`   | `$ORACLE_HOME/rdbms/admin/catproc.sql` | Instala y compila **paquetes PL/SQL estándar** como `DBMS_STATS`, `DBMS_LOB`, `UTL_FILE`, `DBMS_JOB`. |
| `utlrp.sql`     | `$ORACLE_HOME/rdbms/admin/utlrp.sql` | **Recompila todos los objetos inválidos** en la base de datos. |
| `catclust.sql`  | `$ORACLE_HOME/rdbms/admin/catclust.sql` | Configura metadatos necesarios para **Oracle RAC** (solo si se usa RAC). |
| `catoctk.sql`   | `$ORACLE_HOME/rdbms/admin/catoctk.sql` | Instala paquetes de **seguridad y cifrado**, incluyendo `DBMS_CRYPTO`. |
| `catxdb.sql`    | `$ORACLE_HOME/rdbms/admin/catxdb.sql` | Habilita **XML DB**, necesario para **Oracle Enterprise Manager** y almacenamiento XML. |

## Después de crear la base de datos, ejecuta estos scripts en SQL*Plus como SYSDBA:

````sql
sqlplus / as sysdba
@?/rdbms/admin/catalog.sql
@?/rdbms/admin/catproc.sql
@?/rdbms/admin/catoctk.sql
@?/rdbms/admin/catxdb.sql
@?/rdbms/admin/utlrp.sql
````

## 📌 ¿Qué hace cada script?
### 1️⃣ catalog.sql (Diccionario de datos)
- Crea las vistas y sinónimos necesarios para administrar la base de datos.
- Registra las vistas DBA_, ALL_ y USER_*.
### 2️⃣ catproc.sql (Paquetes PL/SQL)
- Compila y configura los paquetes PL/SQL estándar como:
- DBMS_STATS (manejo de estadísticas de optimización)
- DBMS_JOB (ejecución de tareas programadas)
- UTL_FILE (manejo de archivos en el sistema operativo)
- DBMS_LOB (manejo de objetos grandes LOB)
### 3️⃣ catoctk.sql (Paquetes de cifrado y seguridad)
- Instala DBMS_CRYPTO, que permite cifrado y hashing de datos en Oracle.
- Requerido para funcionalidades de seguridad avanzadas.
### 4️⃣ catxdb.sql (Habilitar XML DB)
- Habilita el repositorio XML DB, necesario para:
- Oracle Enterprise Manager (EM).
- Almacenamiento y procesamiento de XML dentro de la base de datos.
### 5️⃣ utlrp.sql (Recompilar objetos PL/SQL)
- Recompila todos los objetos inválidos en la base de datos después de ejecutar los scripts anteriores.
- Asegura que todos los procedimientos, funciones y paquetes estén en estado VALID.

### 📌 Verificación después de ejecutar los scripts
Una vez ejecutados los scripts, puedes verificar si los componentes de la base de datos están correctamente instalados con:

````sql
SELECT comp_name, status, version FROM dba_registry;
📌 Salida esperada si todo está correcto:

COMP_NAME                            STATUS    VERSION
-----------------------------------  --------  ------------
Oracle Database Catalog Views        VALID     19.3.0.0.0
Oracle Database Packages and Types   VALID     19.3.0.0.0
Oracle XML Database                  VALID     19.3.0.0.0
Oracle Database Cryptographic Toolkit VALID    19.3.0.0.0
````
✅ Si algún componente aparece como INVALID, ejecutar utlrp.sql nuevamente.

## 📌 Conclusión
- Después de crear una base de datos, es obligatorio ejecutar catalog.sql y catproc.sql para que la base de datos sea funcional.
- Otros scripts (catoctk.sql, catxdb.sql) son opcionales, pero necesarios para funcionalidades avanzadas.
- Finaliza siempre con utlrp.sql para garantizar que todos los objetos PL/SQL sean compilados correctamente.
🚀 Siguiendo este procedimiento, tendrás una base de datos completamente operativa con todos los paquetes y vistas necesarias.
