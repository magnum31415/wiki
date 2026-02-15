[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# Networking

## Virtual network peering 

![azure-vnet-peering](./img/azure/azure-vnet-peering.png)

Enables you to seamlessly connect two or more Virtual Networks in Azure.
The virtual networks appear as one for connectivity purposes. The traffic between virtual machines in peered virtual networks uses the Microsoft backbone infrastructure.
Like traffic between virtual machines in the same network, traffic is routed through Microsoft’s private network only.

To to connect the two VNets in different regions, you need to configure Azure Virtual Network Peering.

## Azure Virtual Network Gateway or VPN Gateway
Azure VPN Gateway (tipo de Virtual Network Gateway) es el servicio que permite conectar una VNet de Azure con tu red on-premises mediante un túnel cifrado sobre Internet.

Puntos clave:

- 🔐 Usa IPsec/IKE (IKEv1 o IKEv2) para cifrar el tráfico.
- 🌐 Permite conexión Site-to-Site (S2S) entre tu CPD y Azure.
- 🧩 Cada VNet solo puede tener un VPN Gateway.
- 🔗 Puedes crear varias conexiones hacia ese gateway.
- 📶 Todas las conexiones comparten el ancho de banda del gateway.

En resumen:
**Es la opción estándar cuando necesitas conectividad segura sobre Internet entre tu red local y Azure, sin usar un circuito dedicado como ExpressRoute.**

---

## Escenarios

| Escenario                                                        | Servicio recomendado        |
| ---------------------------------------------------------------- | --------------------------- |
| Conexión económica y rápida                                      | Virtual WAN + VPN           |
| Empresa mediana con tráfico regional                             | ExpressRoute Standard       |
| Empresa grande con tráfico global                                | ExpressRoute Premium        |
| Corporación multinacional con alto throughput y conexión directa | ExpressRoute Premium Direct |

---

| Servicio                                    | Úsalo cuando…                                                                                                                                     | Tipo de conexión                                 | Ventajas clave                                                                 | Alcance                            |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | ------------------------------------------------------------------------------ | ---------------------------------- |
| 🌐 **Azure Virtual WAN + Site-to-Site VPN** | - No necesitas conexión dedicada<br>- Buscas menor costo<br>- La latencia no es crítica<br>- Sucursales pequeñas o despliegues rápidos            | Conexión sobre Internet cifrada (VPN IPSec)      | - Más flexible<br>- Más económico<br>- Implementación rápida                   | Regional / Global (sobre Internet) |
| 🔵 **ExpressRoute Standard**                | - Necesitas conexión privada dedicada<br>- Usas proveedor de conectividad (carrier)<br>- Solo necesitas conectividad regional                     | Conexión privada dedicada (no pasa por Internet) | - Mejor latencia<br>- Mayor estabilidad<br>- SLA superior a VPN                | Regional (limitado al geo)         |
| 🟣 **ExpressRoute Premium Direct**          | - Necesitas conectividad global entre regiones<br>- Alto volumen de tráfico<br>- Conexión directa a Microsoft<br>- Alta resiliencia y 10/100 Gbps | Conexión física directa a Microsoft              | - Máximo rendimiento<br>- Control total<br>- Escenarios enterprise/global      | Global                             |
| 🟢 **ExpressRoute Standard Direct**         | - Conexión directa a Microsoft<br>- No necesitas conectividad global<br>- Alto tráfico regional                                                   | Conexión física directa a Microsoft              | - Alto rendimiento<br>- Más económico que Premium Direct<br>- Enfoque regional | Regional                           |

---
## Arbol de decisión  
````
¿Necesitas conexión privada dedicada a Azure?
│
├── ❌ NO → Usa Azure Virtual WAN con VPN Site-to-Site
│
└── ✅ SÍ → ExpressRoute
      │
      ├── ¿Quieres conectarte directamente a la infraestructura de Microsoft 
      │   (sin proveedor intermedio)?
      │
      │   ├── ✅ SÍ → ExpressRoute Direct
      │   │       │
      │   │       ├── ¿Necesitas conectividad global entre regiones?
      │   │       │       ├── ✅ Sí → ExpressRoute Premium Direct
      │   │       │       └── ❌ No → ExpressRoute Standard Direct
      │   │
      │   └── ❌ NO → ExpressRoute Standard
      │
      └── ¿Requieres acceso global entre regiones o más VNets?
              ├── ✅ Sí → ExpressRoute Premium
              └── ❌ No → ExpressRoute Standard

````
---

## Service Endpoints en Azure
En Azure, los Service Endpoints se configuran por servicio específico en una subnet, no existe un endpoint global para todos los servicios.

En Azure, un Service Endpoint:

- Se habilita a nivel de subnet
- Se configura por servicio específico
- Permite acceso privado desde la VNet al servicio PaaS
- Mantiene el tráfico dentro del backbone de Microsoft

En una subnet activas:
- VM acceda a Azure Storage → activas Microsoft.Storage
- VM acceda a Azure SQL → activas Microsoft.Sql
- VM acceda a Key Vault → activas Microsoft.KeyVault

- 👉 No activas “Azure completo”.
- 👉 Activar Storage NO activa SQL.

![azure-service-endpoint](./img/azure/azure-service-endpoint.png)


#### ✅ Procedimiento recomendado (Private Endpoint)
**1) Crear Private Endpoint para Azure SQL Database**
- Azure SQL Server → Networking → Private endpoint connections
- Crear Private Endpoint en:
  - **La VNet de la app**
  - **La subnet donde están las VMs** (o una subnet dedicada a Private Endpoints, si tu estándar lo exige)
- Seleccionar target:
  - Microsoft.Sql/servers (tu SQL logical server) y el database si aplica

**2) Configurar DNS privado (imprescindible)**
- Crear o usar una Private DNS Zone:
privatelink.database.windows.net
- Vincularla a la VNet (VNet link)
- Verificar que el registro A del SQL server apunta a la IP privada del Private Endpoint

✅ Resultado: desde las VMs, tu-servidor.database.windows.net resolverá a IP privada.

**3) Bloquear acceso público al SQL**
- En Azure SQL Server → **Networking**
- **Public network access: Disabled**
- (Opcional) Asegúrate que “Allow Azure services…” esté Off, si existe esa opción en tu blade

**4) Verificación**

- Desde una VM en esa subnet:
  - nslookup tu-servidor.database.windows.net → debe devolver IP privada
  - Conexión SQL OK
- Desde fuera (tu PC, otra red):
 - No resuelve a IP pública útil o directamente no conecta

**5) Cómo funciona con Service Endpoint**

Cuando habilitas:

``
VNet Subnet → Service Endpoint → Microsoft.Sql
``

Lo que ocurre ea que el tráfico sigue yendo al endpoint público de SQL: ``*.database.windows.net``

Pero Azure marca el tráfico como proveniente de esa VNet.

Entonces necesitas **configurar el firewall del SQL Server (PaaS Firewall)** para:

- ❌ Bloquear todo
- ✅ Permitir esa VNet/Subnet específica

- **Porque con Service Endpoint el firewall es obligatorio para restringir acceso.**

# Azure Network Watcher

Azure Network Watcher es un servicio de diagnóstico y monitorización para redes en Azure. Proporciona herramientas para analizar conectividad, seguridad y tráfico de red.

---

## 🔵 VPN Troubleshoot

Herramienta específica para diagnosticar problemas en **Azure VPN Gateway**.

### ¿Qué hace?

- Verifica el estado de la conexión VPN.
- Comprueba que el gateway esté correctamente aprovisionado.
- Valida que el túnel IPsec/IKE esté operativo.
- Detecta problemas de configuración o conectividad.

### ¿Cuándo usarlo?

- La VPN no conecta.
- El túnel está caído o inestable.
- Necesitas validar el estado del gateway.

> Es la herramienta adecuada para troubleshooting de VPN.

---

## 🔵 NSG Diagnostics

Permite analizar cómo las reglas de un **Network Security Group (NSG)** afectan al tráfico.

### ¿Qué hace?

- Indica si el tráfico es **Allow** o **Deny**.
- Muestra qué regla NSG se está aplicando.
- Ayuda a validar la configuración de seguridad.

### ¿Cuándo usarlo?

- Una VM no puede comunicarse con otra.
- Un puerto parece estar bloqueado.

> No sirve para diagnosticar problemas de VPN.

---

## 🔵 Packet Capture

Permite capturar tráfico de red en:

- Máquinas virtuales (VMs)
- Virtual Machine Scale Sets

### ¿Qué hace?

- Captura paquetes para análisis detallado.
- Ayuda a detectar anomalías de red.
- Permite depurar comunicaciones cliente-servidor.
- Apoya investigaciones de seguridad.

### ¿Cuándo usarlo?

- Problemas de red complejos.
- Análisis de tráfico en profundidad.
- Investigación de incidentes.

> No está diseñado específicamente para troubleshooting de VPN.

---

## 🎯 Resumen para examen

| Herramienta        | Uso principal                         | No usar para              |
|-------------------|----------------------------------------|---------------------------|
| VPN Troubleshoot  | Diagnóstico de Azure VPN Gateway       | Análisis de NSG           |
| NSG Diagnostics   | Ver reglas Allow/Deny de un NSG        | Problemas de túnel VPN    |
| Packet Capture    | Analizar tráfico de red en detalle     | Diagnóstico específico VPN |
