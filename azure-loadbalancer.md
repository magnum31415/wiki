 Documento principal: [Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Índice – Load Balancing en Azure

## 📌 Contenido

- [LoadBalancer](#loadbalancer)
  - [¿Qué Load Balancer usar en Azure?](#qué-load-balancer-usar-en-azure)
  - [Comparativa Load Balancing en Azure (Referencia AZ-305)](#-comparativa-load-balancing-en-azure-referencia-az-305)
  - [Árbol de decisión – ¿Qué servicio elegir?](#-árbol-de-decisión--qué-servicio-elegir)
  - [Azure Application Gateway](#azure-application-gateway)
  - [SSL Termination](#ssl-termination)
  - [Azure Front Door – Tiers y Características (AZ-305)](#azure-front-door--tiers-y-características-az-305)
    - [Tiers disponibles](#tiers-disponibles)
    - [Tabla comparativa](#tabla-comparativa)
    - [Diferencia clave para examen](#diferencia-clave-para-examen)
    - [Regla mental rápida](#regla-mental-rápida)



# LoadBalancer

## ¿Qué Load Balancer usar en Azure?

| Si necesitas…             | Usa…                      |
| ------------------------- | ------------------------- |
| Global + Web inteligente  | **Azure Front Door**      |
| Global + solo DNS routing | **Traffic Manager**       |
| Regional + Web + WAF      | **Application Gateway**   |
| Regional + TCP/UDP        | **Azure Load Balancer**   |
| Insertar Firewall/NVA     | **Gateway Load Balancer** |
| Gestionar APIs            | **API Management**        |

---

## 🔷 Comparativa Load Balancing en Azure (Referencia AZ-305)

> **Leyenda**
>
> - **Basic LB** = Azure Load Balancer (Basic)
> - **Std Public LB** = Azure Load Balancer (Standard - Public)
> - **Std ILB** = Azure Load Balancer (Standard - Internal Load Balancer)
> - **App GW** = Application Gateway
> - **Front Door** = Azure Front Door
> - **Traffic Mgr** = Traffic Manager
> - **GW LB** = Gateway Load Balancer



| Característica | **Basic LB** | **Std Public LB** | **Std ILB** | **App GW** | **Front Door** | **Traffic Mgr** | **GW LB** |
|----------------|:------------:|:-----------------:|:-----------:|:----------:|:--------------:|:---------------:|:---------:|
|---|---|---|---|---|---|---|---|
| **Capa OSI** | L4 (TCP/UDP) | L4 (TCP/UDP) | L4 (TCP/UDP) | L7 (HTTP/HTTPS) | L7 global (HTTP/HTTPS) | DNS | L3/L4 |
| **Ámbito** | Regional | Regional | Regional | Regional | Global | Global | Regional |
| **Público / Interno** | Público o interno | Público | Interno | Público o interno | Público | Público mediante DNS | Interno / transparente |
| **Port forwarding (Inbound NAT Rule)** | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| **HTTPS health probe** | ❌ (solo TCP/HTTP) | ✅ | ✅ | ✅ | ✅ | ✅ (HTTP/HTTPS/TCP según monitorización) | ❌ |
| **URL-based routing** | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ |
| **Path-based routing** | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ |
| **Host-based routing** | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ |
| **WAF** | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ |
| **TLS/SSL termination** | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ |
| **Global failover** | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ |
| **Multi-region routing** | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ |
| **Backend pool** | VMs del mismo **Availability Set** o del mismo **VMSS** | VMs / VMSS mediante NIC o IP, dentro de una misma VNet | VMs / VMSS mediante NIC o IP, dentro de una misma VNet | NICs, VMs, VMSS, IPs, FQDN y App Service | Origin Groups: App Service, Storage, Azure u orígenes personalizados | Endpoints DNS: Azure, externos o anidados | VMs o VMSS que ejecutan NVAs, dentro de una VNet |
| **Protección SQL Injection** | ❌ | ❌ | ❌ | ✅ (WAF) | ✅ (WAF) | ❌ | ❌ |
| **Escalado automático** | ❌ El LB no escala las VMs | ⚠️ Se integra con VMSS/autoscale | ⚠️ Se integra con VMSS/autoscale | ✅ En SKU v2 | ✅ Servicio global gestionado | ❌ No escala endpoints; solo enruta DNS | ⚠️ Se integra con VMSS/NVAs escalables |
| **CDN / Edge caching** | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| **Backend típico** | VMs heredadas en Availability Set o VMSS | VMs / VMSS publicados en Internet | Aplicaciones privadas y servicios internos | Web Apps / APIs HTTP(S) | Aplicaciones web multi-región | Endpoints regionales o externos | Firewalls, IDS/IPS y otras NVAs |
| **Accesible desde Internet** | ✅ si es público | ✅ | ❌ | ✅ si tiene frontend público | ✅ | ✅ mediante resolución DNS | ❌ directamente |
| **IP Frontend** | Pública o privada | Solo pública | Solo privada | Pública o privada | Pública global | Nombre DNS | IP privada |
| **Availability Zones** | ❌ | ✅ | ✅ | ✅ en SKU v2 | Servicio global distribuido | N/A | ✅ |
| **Secure by default** | ❌ | ✅; requiere NSG para permitir tráfico | ✅; requiere NSG para permitir tráfico | ✅ | ✅ | N/A | ✅ |
| **SLA** | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Limitaciones** | SKU retirado. Sin Availability Zones ni SLA. El backend debe pertenecer al mismo Availability Set o VMSS. | Solo L4; no interpreta URL, host o path. Sin WAF ni TLS termination. | Solo acceso privado. No aporta salida a Internet por sí mismo; requiere NAT Gateway, Public IP u otra salida explícita. Solo L4. | Solo HTTP/HTTPS. Regional. No balancea protocolos TCP/UDP arbitrarios. | Solo HTTP/HTTPS. Los orígenes deben ser accesibles desde Front Door; Private Link requiere Premium en escenarios compatibles. | Solo decide qué endpoint devolver mediante DNS. No inspecciona, proxifica ni transporta el tráfico. Depende del TTL y caché DNS. | Diseñado para insertar NVAs transparentemente. Solo reglas HA Ports y tráfico VXLAN; no publica aplicaciones directamente. |

## Inbound NAT Rule vs Load Balancing Rule

**Solo aplica al Azure Load Balancer (L4), tanto Public como Internal.**

| Característica                | Inbound NAT Rule                         | Load Balancing Rule         |
| ----------------------------- | ---------------------------------------- | --------------------------- |
| Destino                       | Una VM concreta                          | Varias VMs del backend pool |
| Objetivo                      | Acceso administrativo o conexión directa | Distribuir tráfico          |
| Ejemplo                       | `LB:3389 → VM3:3389`                     | `LB:443 → VM1/VM2/VM3:443`  |
| Reparte tráfico               | ❌ No                                     | ✅ Sí                        |
| Selecciona una VM determinada | ✅ Sí                                     | ❌ No                        |


| Servicio                            | Load Balancing Rule | Inbound NAT Rule | Comentario                                                                       |
| ----------------------------------- | :-----------------: | :--------------: | -------------------------------------------------------------------------------- |
| **L4 Azure Load Balancer (Public)** |          ✅          |         ✅        | Caso típico del AZ-104.                                                          |
| **Internal Load Balancer (ILB)**    |          ✅          |         ✅        | Exactamente igual que el público, pero con IP privada.                           |
| **Application Gateway (L7)**        |          ❌          |         ❌        | Utiliza **Listeners**, **Routing Rules** y **Backend Pools**.                    |
| **Azure Front Door**                |          ❌          |         ❌        | Utiliza **Routing Rules**, **Origin Groups** y **Origins**.                      |
| **Traffic Manager**                 |          ❌          |         ❌        | Solo resuelve DNS. No balancea puertos.                                          |
| **Gateway Load Balancer**           |          ❌          |         ❌        | Encadena appliances de red (Firewall, NVA...). No tiene reglas NAT de este tipo. |


---


![azure-load-balancing-options](./img/azure/azure-load-balancing-options.png)

````
¿Necesitas balanceo GLOBAL entre varias regiones?
│
├── Sí →
│     │
│     ├── ¿Necesitas routing HTTP/HTTPS inteligente (L7), WAF,
│     │   SSL offload, path-based routing?
│     │
│     │       ├── Sí → Azure Front Door
│     │       │       ├── Capa: L7 (HTTP/HTTPS)
│     │       │       ├── Alcance: Global (multi-región)
│     │       │       ├── Tipo: Anycast global
│     │       │       ├── Failover: Automático entre regiones
│     │       │
│     │       │       ├── SKUs:
│     │       │       │
│     │       │       │   ├── Basic
│     │       │       │   │       ├── Routing L7: Sí
│     │       │       │   │       ├── SSL offload: Sí
│     │       │       │   │       ├── WAF: ❌ No
│     │       │       │   │       ├── Private Link backend: ❌ No
│     │       │       │   │       ├── Rules Engine avanzado: Limitado
│     │       │       │   │       └── Uso típico:
│     │       │       │           Apps web globales simples
│     │       │       │           Balanceo multi-región sin requisitos avanzados
│     │       │       │
│     │       │       │   └── Premium
│     │       │       │           ├── Routing L7: Sí
│     │       │       │           ├── SSL offload: Sí
│     │       │       │           ├── WAF: ✅ Sí (integrado)
│     │       │       │           ├── Private Link backend: ✅ Sí
│     │       │       │           ├── Rules Engine avanzado: Sí
│     │       │       │           ├── Bot protection: Sí
│     │       │       │           └── Uso típico:
│     │       │       │               SaaS enterprise
│     │       │       │               Exposición segura de backends privados
│     │       │       │               Requisitos avanzados de seguridad
│     │       │
│     │       │       └── Resumen examen:
│     │       │               Seguridad avanzada / WAF / Private Link → Premium
│     │       │               Solo balanceo global HTTP básico → Basic
│     │
│     │       └── No →
│     │              ¿Solo necesitas decidir a qué región enviar tráfico?
│     │
│     │              └── Sí → Traffic Manager
│     │                      ├── Capa: DNS
│     │                      ├── Alcance: Global
│     │                      ├── Tipo: DNS routing
│     │                      ├── Failover: Basado en DNS
│     │                      ├── Modos:
│     │                      │       Priority
│     │                      │       Weighted
│     │                      │       Performance
│     │                      │       Geographic
│     │                      └── Uso típico:
│     │                              DR entre regiones
│     │                              Routing geográfico simple
│
└── No (solo una región) →
      │
      ├── ¿Es tráfico HTTP/HTTPS (web)?
      │
      │       ├── Sí →
      │       │      ¿Necesitas WAF o routing por path/host?
      │       │
      │       │      ├── Sí → Application Gateway
      │       │      │       ├── Capa: L7
      │       │      │       ├── Alcance: 1 región
      │       │      │       ├── WAF: Sí
      │       │      │       ├── SSL termination: Sí
      │       │      │       └── Uso típico:
      │       │      │               Aplicaciones web regionales
      │       │      │               Protección OWASP
      │       │
      │       │      └── No →
      │       │             Azure Load Balancer
      │       │             ├── Capa: L4 (TCP/UDP)
      │       │             ├── Alcance: 1 región
      │       │             ├── WAF: No
      │       │             └── Uso típico:
      │       │                     Web simple sin WAF
      │       │                     Balanceo básico TCP
      │
      └── ¿Es tráfico no HTTP? (TCP/UDP puro)
              │
              └── Azure Load Balancer
                      ├── Capa: L4
                      ├── Interno o Público
                      ├── Standard SKU recomendado
                      └── Uso típico:
                              VMs
                              SQL
                              RDP
                              Servicios backend

¿Necesitas insertar un firewall/NVA en medio del tráfico?
│
└── Sí → Gateway Load Balancer
        ├── Capa: L3/L4
        ├── Alcance: 1 región
        ├── Función: Inserta NVAs (firewalls, IDS)
        └── Uso típico:
                Arquitecturas con appliances de seguridad

¿Estás exponiendo APIs como producto?
│
└── Sí → API Management
        ├── Capa: L7
        ├── Alcance: 1 región (multi-región con despliegue adicional)
        ├── Función: Gestión de APIs
        ├── Incluye:
        │       Throttling
        │       Autenticación
        │       Versionado
        └── Uso típico:
                API gateway empresarial
                Monetización de APIs

````

---


**Azure Application Gatewa**y is a web traffic load balancer that enables you to manage traffic to your web applications. Traditional load balancers operate at the transport layer (OSI layer 4 – TCP and UDP) and route traffic based on source IP address and port, to a destination IP address and port. Application Gateway can make routing decisions based on additional attributes of an HTTP request, for example, URI path or host headers.

![Azure-AppGateway-Diagram](./img/azure/Azure-AppGateway-Diagram.png)

SSL termination refers to the process of decrypting encrypted traffic before passing it along to a web server. TLS is just an updated, more secure version of SSL. An SSL connection sends encrypted data between a user and a web server by using a certificate for authentication. SSL termination helps speed up the decryption process and reduces the processing burden on the servers.

Azure Application Gateway is specifically designed to offer advanced routing capabilities, SSL offloading (which alleviates the load on web servers), and autoscaling features to efficiently handle varying traffic loads.
**Azure Front Door** . Although it supports SSL offloading, this service is not a load balancer. Azure Front Door is a global, scalable entry-point that uses the Microsoft global edge network to create fast, secure, and widely scalable web applications.


# Azure Front Door – Tiers y Características (AZ-305)

## Tiers disponibles
- Standard
- Premium
(Front Door Classic está en retirada y no es foco actual de examen)

---

## Tabla comparativa

| Característica | Standard | Premium |
|---------------|----------|----------|
| Global HTTP/HTTPS Load Balancing | ✅ | ✅ |
| Anycast global | ✅ | ✅ |
| Health probes automáticos | ✅ | ✅ |
| Path-based routing | ✅ | ✅ |
| Host-based routing | ✅ | ✅ |
| Redirecciones / Rewrites | ✅ | ✅ |
| Rules Engine | ✅ | ✅ |
| CDN integrado (edge caching) | ✅ | ✅ |
| Compresión | ✅ | ✅ |
| TLS termination | ✅ | ✅ |
| Certificados gestionados | ✅ | ✅ |
| WAF | ✅ | ✅ |
| Private Link hacia backend | ❌ | ✅ |
| Soporte para backend privado (App Service privado, AKS privado, etc.) | ❌ | ✅ |

---

## Diferencia clave para examen

### Standard
- Aplicaciones públicas globales
- CDN + WAF + Global Load Balancer
- Backend público

### Premium
- Todo lo anterior
- Soporte Private Link
- Backend privado (no expuesto a Internet)
- Arquitectura Zero Trust

---

## Regla mental rápida

Standard = Web pública global + CDN + WAF  
Premium = Standard + Private Link (backend privado)

