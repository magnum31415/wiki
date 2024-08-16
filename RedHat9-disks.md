# Parted

1. Tabla de Particiones MSDOS (MBR - Master Boot Record)
- Compatibilidad: Es el tipo de tabla de particiones más antiguo y es compatible con la mayoría de los sistemas operativos, incluyendo versiones antiguas de Windows y Linux.
- Número de Particiones: Permite hasta 4 particiones primarias. Si necesitas más de 4 particiones, una de las particiones primarias puede ser una partición extendida que contenga particiones lógicas.
- Tamaño Máximo de Disco: Soporta discos de hasta 2 TB.
- Antigüedad: Es un esquema de particiones más antiguo y tiene ciertas limitaciones en comparación con GPT.
- Limitaciones: Tiene limitaciones en cuanto a la cantidad de particiones y el tamaño máximo del disco, lo que lo hace menos adecuado para discos modernos grandes.
2. Tabla de Particiones GPT (GUID Partition Table)
- Compatibilidad: Es el tipo de tabla de particiones más moderno y es necesario para discos que usan UEFI en lugar de BIOS tradicional. Es compatible con la mayoría de los sistemas operativos modernos, aunque algunos sistemas antiguos no lo soportan.
- Número de Particiones: Soporta hasta 128 particiones sin necesidad de particiones extendidas.
- Tamaño Máximo de Disco: Puede manejar discos de tamaños superiores a 2 TB, hasta 9.4 zettabytes (ZB).
- Resistencia a Errores: Incluye redundancia y corrección de errores mediante el almacenamiento de múltiples copias de la tabla de particiones en diferentes partes del disco.
- Ventajas: Es más robusto y adecuado para discos grandes y sistemas modernos.
3. Resumen de Uso
- Usar mklabel msdos: Si necesitas compatibilidad con sistemas más antiguos o estás trabajando con discos menores de 2 TB y no necesitas más de 4 particiones primarias.
  ````
  sudo parted /dev/sdX mklabel msdos
  ````
- Usar mklabel gpt: Para sistemas modernos, discos grandes (más de 2 TB), o cuando necesitas más particiones y mayor resiliencia.
  ````
  sudo parted /dev/sdX mklabel gpt
  ````


  ## Create a 2 GiB partition on the /dev/vdb disk
  ````
  parted /dev/vdb mklabel msdos
  parted /dev/vdb mkpart primary 1MiB 2GiB
  parted /dev/vdb set 1 lvm on 

  ````

  ## Create the extra_storage volume group with the /dev/vdb1
  ````
  pvcreate /dev/vdb1
  vgcreate extra_storage /dev/vdb1
  lvcreate -L 1GiB -n vol_home extra_storage
  mkfs -t xfs /dev/extra_storage/vol_home
  mkdir /user-homes
  lsblk -o UUID /dev/extra_storage/vol_home
  echo "UUID=988c...0ab6 /user-homes xfs defaults 0 0" >> /etc/fstab
  mount /user-homes
  ````
