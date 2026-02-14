[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
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
│     │       │       ├── WAF: Sí
│     │       │       └── Uso típico:
│     │               Apps web multi-región
│     │               SaaS global
│     │               Alta disponibilidad global
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
      │       │      │               Aplicaciones web
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
