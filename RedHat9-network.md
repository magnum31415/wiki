
# Netowrk

## Hostname
```
 vim /etc/hostname
 or
 hostnamectl set-hostname <hostname>
or
 nmcli general hostname node1.lab.example.com
```

## setting ipv4/GW/DNS

```
nmcli connection modify enp0s3 autoconnect yes ipv4.method manual ipv4.addresses 172.25.250.100 ipv4.gateway 172.25.250.254 ipv4.dns 172.25.250.254

nmcli con add con-name "text1" ifname eth1 type ethernet ipv4.method manual ipv4.addresses  172.25.250.11/24 ipv4.gateway 172.25.250.254 ipv4.dns 172.25.250.254
nmcli con up text1

nmcli connection add con-name "test01" ifname tst01 type ethernet ipv4.method manual ipv4.addresses 10.0.0.10/24 ipv4.gateway 10.0.0.1 ipv4.dns 8.8.8.8

ip addr
```
Set connection enabled by default
````
sudo nmcli con mod test01 autoconnect yes
````

## nmcli
El archivo de configuración de la conexión en /etc/NetworkManager/system-connections/.

### Pasos para configurar una interfaz de red con nmcli

1. Verificar el estado del servicio NetworkManager ````sudo systemctl status NetworkManager; nmcli general status````
2. Listar las interfaces de red disponibles ````nmcli device status````
3. Configurar la interfaz de red con dirección IP estática ````sudo nmcli connection add type ethernet con-name <nombre-conexion> ifname <nombre-interfaz> ip4 <direccion-ip>/<mascara> ipv4.dns <ip-dns> gw4 <puerta-enlace>````
   1. -  ````nmcli con add con-name con1 type ethernet ifname eth1 ipv4.addresses 172.25.250.11/24 ipv4.gateway 172.25.250.254 ipv4.dns 172.25.250.254 ipv4.method manual````
5. Configurar la interfaz de red usando DHCP ````sudo nmcli connection add type ethernet con-name dhcp-conn ifname <nombre-interfaz> ipv4.method auto````
6. Activar la conexión ````sudo nmcli connection up <nombre-conexion> ````
7. Verificar la configuración ````nmcli connection show; ip addr show <nombre-interfaz>````
8. Configurar la conexión para que se active automáticamente al iniciar ````sudo nmcli connection modify <nombre-conexion> connection.autoconnect yes````


###  Checking the overall status 
```
nmcli general status
```

### nmcli connection 
Viewing only currently active connections
```
nmcli connection show --active
```
**connection.type**
A connection type. Allowed values are: adsl, bond, bond-slave, bridge, bridge-slave, bluetooth, cdma, ethernet, gsm, infiniband, olpc-mesh, team, team-slave, vlan, wifi, wimax.

**connection.interface-name**
````
nmcli con add connection.interface-name enp1s0 type ethernet
````

**connection.id**
A name used for the connection profile. If you do not specify a connection name, one will be generated as follows:
````
connection.type -connection.interface-name
````
This is particularly useful for mobile devices or when switching a network cable back and forth between different devices. Rather than edit the configuration, create different profiles and apply them to the interface as needed. 


###  Viewing only devices recognized by NetworkManager and their state
```
nmcli device status
```

###  Starting and Stopping a Network Interface Using nmcli
```
nmcli con up id bond0
nmcli dev disconnect bond0
```
The **nmcli connection down** command, deactivates a connection from a device without preventing the device from further auto-activation. 
The **nmcli device disconnect** command, disconnects a device and prevent the device from automatically activating further connections without manual intervention.

###  Checking the overall status 
```
nmcli general status
```

###  Checking the overall status 
```
nmcli general status
```



nmcli is used to create, display, edit, delete, activate, and deactivate network connections, as well as control and display network device status.
```
nmcli [OPTIONS] OBJECT { COMMAND | help }
```
where OBJECT can be one of the following options: general, networking, radio, connection, device, agent, and monitor.

