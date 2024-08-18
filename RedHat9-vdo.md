# VDO 
Una VDO partition (Virtual Data Optimizer) en RHEL 9 es una tecnología de Red Hat que permite optimizar el uso del almacenamiento mediante compresión, deduplicación y aprovisionamiento delgado (thin provisioning). VDO se utiliza para reducir el espacio de almacenamiento físico necesario para los datos, lo que puede ser especialmente útil en entornos con grandes volúmenes de datos redundantes.

## Características principales de VDO:
Deduplicación: Elimina bloques de datos duplicados en tiempo real, reduciendo el almacenamiento necesario.
Compresión: Comprime los datos para optimizar aún más el espacio en disco.
Aprovisionamiento delgado: Permite asignar más espacio lógico que físico, proveyendo solo el espacio físico necesario a medida que se requieren datos adicionales.
## Ventajas de usar VDO:
Ahorro de espacio: Al combinar deduplicación y compresión, VDO puede reducir significativamente el uso de almacenamiento.
Eficiencia: Ideal para entornos con almacenamiento limitado o costoso, donde es importante maximizar el uso del espacio disponible.
Transparencia: Funciona a nivel de bloque, lo que significa que es transparente para las aplicaciones y sistemas de archivos que utilizan el almacenamiento subyacente.

1. Instalar el paquete VDO
````
sudo dnf install vdo kmod-kvdo -y
````
3. Crear el dispositivo VDO
````
sudo vdo create --name=vdo1 --device=/dev/sdb --vdoLogicalSize=100G
````
5. Formatear el dispositivo VDO con ext3
````
sudo mkfs.ext3 /dev/mapper/vdo1

````
6. Montar el dispositivo VDO
````
sudo mkdir /mnt/vdo1
sudo mount /dev/mapper/vdo1 /mnt/vdo1

````
7. Verificar y configurar el montaje automático
````
/dev/mapper/vdo1 /mnt/vdo1 ext3 defaults 0 0

````
8. Verificación de deduplicación y compresión
````
sudo vdo status --name=vdo1

````
## LVM now incorporates VDO
LVM now incorporates VDO deduplication and compression as a configurable feature of regular logical volumes. 

````
dnf install vdo kmod-kvdo
lvcreate --type vdo --name vdo-lv01 --size 5G vg01
mkfs -t xfs /dev/vg01/lv01
mkdir /mnt/data
#/etc/fstab file.
/dev/vg01/lv01 /mnt/data xfs defaults 0 0

````
