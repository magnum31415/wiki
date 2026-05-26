[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Application Gateway

- [Cómo se configura la Public IP del Azure Application Gateway WAF_v2](#cómo-se-configura-la-public-ip-del-azure-application-gateway-waf_v2)
- [Azure WAF Services and SKUs — Comparison Table](#azure-waf-services-and-skus--comparison-table)

---

# Azure WAF Services and SKUs — Comparison Table

| Service | SKU / Tier | Scope | WAF Included | DDoS Protection | CDN | Global Routing | Private Backend Support | OWASP Support | TLS Termination | Typical Use Case | Approx Cost |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Azure Application Gateway | Standard_v2 | Regional | ❌ No | ❌ No | ❌ No | ❌ No | ✅ Yes | ❌ No | ✅ Yes | Reverse proxy / L7 load balancer | ~100–300 USD/month |
| Azure Application Gateway | WAF_v2 | Regional | ✅ Yes | ❌ No | ❌ No | ❌ No | ✅ Yes | ✅ Yes | ✅ Yes | Secure ingress for APIs/web apps | ~200–500+ USD/month |
| Azure Front Door | Standard | Global Edge | ✅ Yes | Partial Edge Protection | ✅ Yes | ✅ Yes | ❌ Limited | ✅ Yes | ✅ Yes | Global web apps/CDN | ~35–150+ USD/month |
| Azure Front Door | Premium | Global Edge | ✅ Yes | Partial Edge Protection | ✅ Yes | ✅ Yes | ✅ Yes (Private Link) | ✅ Yes | ✅ Yes | Enterprise multi-region apps | ~300–1000+ USD/month |
| Azure CDN Premium | Verizon / Akamai | Global Edge | ✅ Yes | Partial | ✅ Yes | ❌ No | ❌ No | ✅ Yes | ❌ Partial | CDN + WAF scenarios | Variable |
| Azure Firewall | Standard | Regional | ❌ No real WAF | ❌ No | ❌ No | ❌ No | ✅ Yes | ❌ No | ❌ Limited | Network filtering / egress | ~1000+ USD/month |
| Azure Firewall | Premium | Regional | ⚠️ Partial L7 inspection | ❌ No | ❌ No | ❌ No | ✅ Yes | ❌ Not full OWASP | ✅ TLS Inspection | Advanced network security | ~1500+ USD/month |

---

# Azure Application Gateway SKUs

| SKU | WAF | Autoscaling | Zone Redundant | Recommended |
|---|---|---|---|---|
| Standard | ❌ | ❌ | ❌ | ❌ Legacy |
| WAF | ✅ | ❌ | ❌ | ❌ Legacy |
| Standard_v2 | ❌ | ✅ | ✅ | ⚠️ Only if no WAF needed |
| WAF_v2 | ✅ | ✅ | ✅ | ✅ Recommended |

---

# Azure Front Door SKUs

| SKU | WAF | Private Link | Bot Protection | Rules Engine | Recommended |
|---|---|---|---|---|---|
| Classic | ⚠️ Legacy | ❌ | ❌ | Limited | ❌ Deprecated direction |
| Standard | ✅ | ❌ | ⚠️ Basic | ✅ | ✅ |
| Premium | ✅ | ✅ | ✅ | ✅ Advanced | ✅ Enterprise |

---

# DDoS Services

| Service | Scope | Protects | Typical Cost | Best For |
|---|---|---|---|---|
| Azure DDoS Basic | Included Azure-wide | Basic L3/L4 | Included | All Azure workloads |
| Azure DDoS IP Protection | Single Public IP | Advanced L3/L4 | ~199 USD/month per IP | Single critical API |
| Azure DDoS Network Protection Plan | Multiple VNets | Enterprise DDoS | ~2944 USD/month | Large enterprise environments |

---

# Recommended Architectures

## Cost Optimized

```text
Internet
   ↓
DDoS Basic
   ↓
Application Gateway WAF_v2
   ↓
Private Backend
```

---

## Reinforced Security

```text
Internet
   ↓
DDoS IP Protection
   ↓
Application Gateway WAF_v2
   ↓
Private Backend
```

---

## Enterprise Global

```text
Internet
   ↓
Azure Front Door Premium
   ↓
Application Gateway WAF_v2
   ↓
Private Backend
```

---

# Which SKU is normally recommended?

| Scenario | Recommended |
|---|---|
| Simple regional API | Application Gateway WAF_v2 |
| Enterprise regional API | DDoS IP Protection + WAF_v2 |
| Global application | Front Door Standard |
| Enterprise multi-region | Front Door Premium |
| Private backend exposure | Front Door Premium |
| Centralized egress control | Azure Firewall HUB |

---

# Important Concepts

| Concept | Explanation |
|---|---|
| WAF | Protects Layer 7 (HTTP/HTTPS) |
| DDoS Protection | Protects Layer 3/4 volumetric attacks |
| App Gateway | Reverse proxy + regional ingress |
| Front Door | Global edge ingress |
| Firewall | Network filtering and egress control |

---

# Typical Modern ALZ Model

## Ingress

```text
DDoS
+
WAF
```

## Egress

```text
Azure Firewall HUB
```

## Governance

```text
Policies
+
Terraform Modules
+
Pipelines
```

# Cómo se configura la Public IP del Azure Application Gateway WAF_v2

# 1. Concepto general

El Azure Application Gateway necesita un:

```text
Frontend IP Configuration
```

para poder recibir tráfico.

Ese frontend puede ser:

| Tipo | Uso |
|---|---|
| Public | Internet-facing |
| Private | Internal only |

---

# 2. Arquitectura típica Internet-facing

```text
Internet
   ↓
Public IP
(associated to App Gateway Frontend)
   ↓
Application Gateway WAF_v2
   ↓
Backend Pool
```

---

# 3. Relación entre Public IP y App Gateway

La Public IP:
- NO pertenece “dentro” del gateway
- es un recurso Azure independiente

```text
Microsoft.Network/publicIPAddresses
```

---

# Luego se asocia al Frontend del App Gateway

```text
Public IP
   ↓
Frontend IP Configuration
   ↓
Application Gateway
```

---

# 4. Componentes internos del Application Gateway

| Componente | Función | Descripción | Ejemplo |
|---|---|---|---|
| Public IP | Entrada Internet | Dirección IP pública que expone el Application Gateway a Internet | `52.168.x.x` |
| Frontend IP | IP frontend del gateway | Configuración IP que usa el gateway para recibir tráfico | Frontend público HTTPS en puerto 443 |
| Listener | Escucha HTTP/HTTPS | Componente que escucha peticiones HTTP/HTTPS en un puerto concreto | Listener HTTPS para `api.company.com:443` |
| Rule | Routing | Regla que decide a qué backend enviar el tráfico | `/api/* → Backend Pool API` |
| Backend Pool | APIs/VMs/App Services | Grupo de servidores o servicios backend que reciben tráfico | AKS, VMs privadas, App Services |
| Probe | Health checks | Verificación periódica del estado de los backends | GET `/health` cada 30 segundos |
| WAF Policy | Protección OWASP | Reglas WAF para detectar y bloquear ataques web | OWASP CRS 3.2 + Prevention Mode |

---

# 5. Flujo completo

```text
Internet
   ↓
Public IP
   ↓
Frontend Listener
   ↓
Application Gateway WAF_v2
   ↓
WAF Inspection
   ↓
Backend Pool
```

---

# 6. Dónde se despliega

## Public IP

Puede estar:
- en la misma subscription
- normalmente en el mismo resource group
- misma región.

---

## Application Gateway

Debe estar:
- dentro de una subnet dedicada
- en una VNet.

---

# Importante

La subnet del App Gateway:

```text
debe ser exclusiva
```

---

# Ejemplo típico

```text
Spoke VNet
│
├── Subnet-AppGateway
│     └── Application Gateway WAF_v2
│
└── Backend Subnet
      └── APIs / AKS / VMs
```

---

# 7. Configuración en Portal Azure

# Paso 1 — Crear Public IP

```text
Create Resource
→ Public IP Address
```

---

# Recomendado

| Configuración | Valor |
|---|---|
| SKU | Standard |
| Assignment | Static |
| Zone redundant | Sí recomendado |
| DDoS IP Protection | Opcional |

---

# Paso 2 — Crear Application Gateway

```text
Create Resource
→ Application Gateway
```

---

# Paso 3 — Seleccionar SKU

```text
WAF_v2
```

---

# Paso 4 — Configurar Frontend

Seleccionar:

```text
Frontend IP Type:
Public
```

y elegir:

```text
Existing Public IP
```

o:
```text
Create new
```

---

# Paso 5 — Configurar Listener

Ejemplo:

| Campo | Valor |
|---|---|
| Protocol | HTTPS |
| Port | 443 |
| Hostname | api.company.com |
| Certificate | TLS certificate |

---

# Paso 6 — Configurar Backend Pool

Ejemplo:

```text
10.1.2.4
10.1.2.5
aks-ingress
app-service
```

---

# Paso 7 — Configurar WAF

| Configuración | Recomendado |
|---|---|
| WAF Enabled | Yes |
| Mode | Prevention |
| OWASP CRS | 3.2 o superior |
| Diagnostics | Enabled |

---

# 8. Arquitectura recomendada ALZ

```text
Internet
   ↓
Azure Global Edge
(DDoS IP Protection optional)
   ↓
Public IP
(Standard SKU)
   ↓
Application Gateway WAF_v2
(Public Frontend)
   ↓
Private Backend
(No Public IP)
```

---

# 9. Public IP + DDoS IP Protection

La protección DDoS se asocia a:

```text
Public IP
```

NO directamente al App Gateway.

---

# Flujo

```text
Internet
   ↓
Azure Global Edge
(DDoS mitigation)
   ↓
Public IP
   ↓
Application Gateway WAF_v2
```

---

# 10. Buenas prácticas

| Recomendación | Motivo |
|---|---|
| Standard SKU Public IP | Más seguro |
| Static IP | DNS estable |
| WAF_v2 | Recomendado por Microsoft |
| Prevention mode | Bloquea ataques |
| Backend privado | Reduce superficie ataque |
| NSG en backend subnet | Segmentación |
| Diagnostics enabled | Auditoría |
| UDR egress → Firewall HUB | Control salida |

---

# 11. Qué NO hacer

| Antipatrón | Problema |
|---|---|
| Backend con Public IP | Superficie ataque |
| App Gateway Standard_v2 sin WAF | Sin protección L7 |
| WAF en Detection permanente | No bloquea |
| Basic Public IP SKU | Legacy |
| Compartir subnet App Gateway | No soportado/recomendado |

---

# 12. Terraform — Public IP + WAF_v2

## Public IP

```hcl
resource "azurerm_public_ip" "appgw" {
  name                = "pip-appgw-prod"
  location            = azurerm_resource_group.net.location
  resource_group_name = azurerm_resource_group.net.name

  allocation_method = "Static"
  sku               = "Standard"
}
```

---

## Application Gateway

```hcl
resource "azurerm_application_gateway" "appgw" {
  name                = "appgw-prod"
  location            = azurerm_resource_group.net.location
  resource_group_name = azurerm_resource_group.net.name

  sku {
    name = "WAF_v2"
    tier = "WAF_v2"
  }

  gateway_ip_configuration {
    name      = "gateway-ip-config"
    subnet_id = azurerm_subnet.appgw.id
  }

  frontend_ip_configuration {
    name                 = "public-frontend"
    public_ip_address_id = azurerm_public_ip.appgw.id
  }
}
```

---

# 13. Resumen rápido

| Componente | Función |
|---|---|
| Public IP | Expone Internet |
| Frontend IP | Entrada App Gateway |
| WAF_v2 | Seguridad Layer 7 |
| Backend Pool | Aplicación privada |
| DDoS IP Protection | Protección volumétrica |
| Azure Firewall HUB | Egress control |
