# tar

1. gzip (-z or --gzip) -  tar czvf name.tar.gz /usr/local
2. bzip2 (-j or --bzip2) - tar cjvf name.tar.bzip2 /usr/local
3. xz (-J or --xz) - tar cJvf name.tar.xz /usr/local



# passwd
````
echo "password" | passwd --stdin sarah
````

# Journal
````
#View the recorded log events in the previous 30 minutes on the serverb machine.
journalctl --since 06:49:00 --until 07:19:00

````
# Cron

cron also uses a daemon (crond) that reads different configuration files. There's a cron file for each user in the /etc/cron.d/ directory, and the /etc/crontab file is system-wide. 
````
systemctl status crond
````
To manipulate scheduled cron jobs, you can edit the crontab file (for system-wide tasks) or create files inside the user's cron.d directory (for specific tasks) with the necessary parameters inside them. Below are the most common crontab parameters:

-l displays the current crontab (jobs from the current user) 
-r removes the current crontab (jobs from the current user)
-e edits the current crontab

````
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
00 23 * * *	localuser	rsync -av /home/localuser/source /home/localuser/destination
00 12 * * 6	localuser	cp -R /home/localuser/destination/* /home/localuser/full/
````
# At
El comando at es útil para tareas puntuales que necesitas ejecutar en el futuro, sin tener que configurar cron jobs recurrentes.

````
$ at now + 5 minutes
at> touch /tmp/archivo_de_prueba.txt
at> <Ctrl+D>  # Presiona Ctrl+D para guardar y salir.
````
- atq: Muestra las tareas programadas.
- atrm <job_number>: Cancela una tarea programada.

# Manage Temporary Files
systemd-tmpfiles tool, which provides a structured and configurable method to manage temporary directories and files.
At system boot, one of the first systemd service units to launch is the systemd-tmpfiles-setup service. This service runs the systemd-tmpfiles command --create --remove option, which reads instructions from the /usr/lib/tmpfiles.d/*.conf, /run/tmpfiles.d/*.conf, and /etc/tmpfiles.d/*.conf configuration files.
Create the /etc/tmpfiles.d/volatile.conf file with the following content:
````
d /run/volatile 0700 root root 30s
````
````
systemd-tmpfiles --create /etc/tmpfiles.d/volatile.conf
````
##Clean Temporary Files Manually
The systemd-tmpfiles --clean command parses the same configuration files as the systemd-tmpfiles --create command, but instead of creating files and directories, it purges all files that were not accessed, changed, or modified more recently than the maximum age that is defined in the configuration file.

# Man
## Búsqueda en MAN

-  busca una palabra clave en los títulos y descripciones de las páginas man. ````man -k STRING```` equivalente a ````apropos````
-  busca la palabra clave en la página de texto completo, no solo en los títulos y descripcion ````man -K STRING````

Nota: si nos devuelve "Nothing Appropiate" ejecutar ````sudo mandb```` para actualizar mandb

# Streams
- **0** stdin
- **1** stdout
- **2** stderr
- **3** filename

## Redirects

- **>file** : redirigir STDOUT para sobreescribir un archivo
- **>>file** : redirigir STDOUT para agrregar a un archivo
- **2>file** : redirigir STDERR para sobreescribir un archivo
- **>file 2>&1**  or **&>file** : redirigir STDOUT y STDERR para sobreescribir el mismo archivo
- **>>file 2>&1**  or **&>>file** : redirigir STDOUT y STDERR para agregar al mismo archivo

# Netstat
El comando ss reemplaza la herramienta anterior netstat, que es parte del paquete net-tools.


# Firewalld


# Proxy

## wget
wget "https://download.maxmind.com/app/geoip_download?edition_id=GeoLite2-City&license_key=RzRnWREc7D2PG9tm&suffix=tar.gz.sha256" -e use_proxy=yes -e https_proxy=http://usr_useproxy:passwdd@bbkproxybc.server.es:8080 --no-check-certificate --content-disposition || { error 'Descarga de sha256 fallida'; }

 

````
firewall-cmd --add-service={nfs,mountd,rpc-bind} --permanent
firewall-cmd --reload
````

