# SELinux
All of the semanage commands that add or modify the targeted policy configuration store information in *local files under the /etc/selinux/targeted directory tree. 

## Check in which mode SELinux is running.
```
sestatus
getenforce
```


## Changing SELinux to permissive mode 
Open the /etc/selinux/config  and Configure the SELINUX=permissive and reboot
or ````setenforce 0````

## Changing SELinux to enforcing  mode 
Open the /etc/selinux/config  and Configure the SELINUX=enforcing  and reboot
or  ````setenforce 1````

To prevent incorrectly labeled and unlabeled files from causing problems, SELinux automatically relabels file systems when changing from the disabled state to permissive or enforcing mode. Use the fixfiles -F onboot command as root to create the /.autorelabel file containing the -F option to ensure that files are relabeled upon next reboot.

Before rebooting the system for relabeling, make sure the system will boot in permissive mode, for example by using the enforcing=0 kernel option. This prevents the system from failing to boot in case the system contains unlabeled files required by systemd before launching the selinux-autorelabel service.

## check/set/check/unset semanage boolean
Los SELinux Booleans son variables que controlan el comportamiento de las políticas SELinux de manera dinámica. Permiten activar o desactivar ciertas reglas de seguridad en la política SELinux sin necesidad de modificar la política completa ni reiniciar el sistema. Estas variables proporcionan una manera de ajustar el nivel de seguridad de manera flexible y adaptada a las necesidades específicas de un sistema o aplicación.
```
sudo semanage boolean -l | grep httpd
sudo getsebool -a | grep http

sudo semanage boolean -m --off httpd_ssi_exec
sudo setsebool httpd_ssi_exec off

sudo semanage boolean -l | grep httpd

sudo semanage boolean -m --on httpd_ssi_exec
```

## check/set/check/unset semanage fcontext 
```
sudo semanage fcontext -l | grep sshd

sudo semanage fcontext -a -t sshd_key_t '/etc/ssh/keys(/.*)?'
sudo restorecon -r /etc/ssh/keys

sudo semanage fcontext -l | grep sshd

sudo semanage fcontext -d -t sshd_key_t '/etc/ssh/keys(/.*)?'
sudo restorecon -r /etc/ssh/keys
```

view any locally-customized file contexts by adding the -C option:
```
 sudo semanage fcontext -l -C
```

**¿Cuándo usar restorecon?** 
Se usa restorecon cuando cambias o reasignas contextos SELinux en archivos o directorios, no en la configuración de puertos. Por ejemplo, si mueves un archivo a un nuevo directorio o cambias un contexto manualmente, necesitarías ejecutar restorecon para aplicar el contexto correcto basado en la política.

## check/set/check/unset semanage ports
```
sudo semanage port -l | grep http

sudo semanage port -a -t http_cache_port_t -p tcp 8010

sudo semanage port -l | grep http

sudo semanage port -d -t http_cache_port_t -p tcp 8010
```

## place a single domain into permissive mode

To list any domains currently in permissive mode use:

```
sudo semanage permissive -l 

sudo semanage permissive -a squid_t
```

## Troubleshooting
To view what actions SELinux denies as root:
```
ausearch -m AVC,USER_AVC,SELINUX_ERR,USER_SELINUX_ERR -ts today
```
with the setroubleshoot-server package installed, enter:
```
# grep "SELinux is preventing" /var/log/messages
```
If SELinux is active and the Audit daemon (auditd) is not running on your system, then search for certain SELinux messages in the output of the dmesg command:
```
# dmesg | grep -i -e type=1300 -e type=1400
```
