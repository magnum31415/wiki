# multipass

## Instalación

````
#Instalar Multipass
brew install --cask multipass

#Verifica
brew list --cask multipass
multipass version
````

## Versiones Ubuntu disponibles

Para ver qué versiones de Ubuntu tienes disponibles (por si quieres una LTS específica):
````
multipass find
````

## Ver recursos en mi mac

````
#CPU
sysctl -n hw.physicalcpu
sysctl -n hw.logicalcpu

#Memoria
sysctl -n hw.memsize

#Filesystem
diskutil apfs list
````

## Crear una VM Ubuntu (sin interfaz gráfica)

Multipass, por diseño, siempre instala Ubuntu Server (sin escritorio gráfico)
Para lanzar una VM básica con nombre, CPU, RAM y disco personalizados:
````
multipass launch 24.04 --name dev-vm --cpus 2 --memory 4G --disk 20G
````

### Disco
El ``--disk 20G`` que especificas es un límite máximo, no una reserva inmediata. Multipass en Mac usa QEMU por debajo, con discos virtuales en formato qcow2, que crecen dinámicamente:

Al crear la VM, el archivo de disco virtual empieza siendo pequeño (unos pocos cientos de MB, solo lo que ocupa el sistema base de Ubuntu)
A medida que instalas paquetes, guardas archivos, etc., el archivo va creciendo en tu Mac
Nunca superará los 20 GB que le asignaste como tope


## Montar tu carpeta de proyecto del Mac dentro de la VM
````
multipass mount /Users/TU_USUARIO/proyectos dev-vm:/home/ubuntu/proyectos
````

## Entrar a la VM
````
multipass shell dev-vm
````
# Instalar Claude code
Una vez dentro, ya estás en un Ubuntu completo por terminal. Ahí dentro instalarías Claude Code con:

````bash
curl -fsSL https://claude.ai/install.sh | bash
````

## Comandos útiles para el día a día

````bash
multipass list                  # ver todas tus VMs y su estado
multipass stop dev-vm           # apagarla
multipass start dev-vm          # encenderla
multipass info dev-vm           # ver IP, montajes, recursos
multipass delete dev-vm         # borrarla
multipass purge                 # limpiar VMs borradas definitivamente
````
