# Networking

 | Escenario                                                        | Servicio recomendado        |
| ---------------------------------------------------------------- | --------------------------- |
| Conexión económica y rápida                                      | Virtual WAN + VPN           |
| Empresa mediana con tráfico regional                             | ExpressRoute Standard       |
| Empresa grande con tráfico global                                | ExpressRoute Premium        |
| Corporación multinacional con alto throughput y conexión directa | ExpressRoute Premium Direct |

| Servicio                                    | Úsalo cuando…                                                                                                                                     | Tipo de conexión                                 | Ventajas clave                                                                 | Alcance                            |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | ------------------------------------------------------------------------------ | ---------------------------------- |
| 🌐 **Azure Virtual WAN + Site-to-Site VPN** | - No necesitas conexión dedicada<br>- Buscas menor costo<br>- La latencia no es crítica<br>- Sucursales pequeñas o despliegues rápidos            | Conexión sobre Internet cifrada (VPN IPSec)      | - Más flexible<br>- Más económico<br>- Implementación rápida                   | Regional / Global (sobre Internet) |
| 🔵 **ExpressRoute Standard**                | - Necesitas conexión privada dedicada<br>- Usas proveedor de conectividad (carrier)<br>- Solo necesitas conectividad regional                     | Conexión privada dedicada (no pasa por Internet) | - Mejor latencia<br>- Mayor estabilidad<br>- SLA superior a VPN                | Regional (limitado al geo)         |
| 🟣 **ExpressRoute Premium Direct**          | - Necesitas conectividad global entre regiones<br>- Alto volumen de tráfico<br>- Conexión directa a Microsoft<br>- Alta resiliencia y 10/100 Gbps | Conexión física directa a Microsoft              | - Máximo rendimiento<br>- Control total<br>- Escenarios enterprise/global      | Global                             |
| 🟢 **ExpressRoute Standard Direct**         | - Conexión directa a Microsoft<br>- No necesitas conectividad global<br>- Alto tráfico regional                                                   | Conexión física directa a Microsoft              | - Alto rendimiento<br>- Más económico que Premium Direct<br>- Enfoque regional | Regional                           |


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
