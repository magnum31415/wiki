[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)


# WAF (Web Application Firewall) en Azure — Documento de estudio

# 1. Qué es un WAF

WAF significa:

```text
Web Application Firewall
```

Es un firewall especializado en proteger aplicaciones web y APIs HTTP/HTTPS frente a ataques Layer 7.

---

# 2. Qué protege un WAF

Un WAF protege frente a ataques web típicos como:

| Ataque | Protegido |
|---|---|
| SQL Injection | ✅ |
| Cross-Site Scripting (XSS) | ✅ |
| Remote Code Execution | ✅ |
| Command Injection | ✅ |
| Path Traversal | ✅ |
| HTTP anomalies | ✅ |
| Bots básicos | ✅ parcialmente |
| OWASP Top 10 | ✅ |

---

# 3. Qué NO protege un WAF

Un WAF NO protege correctamente frente a:

| Ataque | Protegido |
|---|---|
| SYN Flood | ❌ |
| UDP Flood | ❌ |
| Volumetric DDoS | ❌ |
| Network Layer attacks | ❌ |

Para eso se usa:
- Azure DDoS Protection
- Azure Firewall
- Front Door Edge protection

---

# 4. En qué capa trabaja

| Servicio | Capa |
|---|---|
| WAF | Layer 7 |
| DDoS Protection | Layer 3 / Layer 4 |
| Azure Firewall | Layer 3-Layer 7 |

---

# 5. Servicios Azure que soportan WAF

| Servicio Azure | Soporta WAF |
|---|---|
| Azure Application Gateway WAF v2 | ✅ |
| Azure Front Door Standard | ✅ |
| Azure Front Door Premium | ✅ |
| Azure CDN Premium | ✅ |

---

# 6. Qué es OWASP

OWASP significa:

```text
Open Worldwide Application Security Project
```

Es el estándar más usado del mundo para protección de aplicaciones web.

---

# 7. Qué es OWASP Top 10

Lista de vulnerabilidades web más críticas.

Ejemplos:

| Vulnerabilidad | Protegida por WAF |
|---|---|
| SQL Injection | ✅ |
| XSS | ✅ |
| Broken Authentication | ✅ parcialmente |
| Command Injection | ✅ |
| Path Traversal | ✅ |

---

# 8. Qué es una OWASP Policy

Una OWASP Policy es el conjunto de reglas que usa el WAF para:
- detectar ataques
- bloquear ataques
- generar logs
- aplicar exclusions
- aplicar rate limiting

---

# 9. OWASP CRS (Core Rule Set)

Azure WAF usa normalmente:

```text
OWASP Core Rule Set (CRS)
```

---

# Versiones típicas

| Versión | Estado |
|---|---|
| CRS 3.1 | Antigua |
| CRS 3.2 | Muy usada |
| CRS 3.3 | Recomendada |
| CRS 4.x | Nueva generación |

---

# 10. Modos de un WAF

| Modo | Qué hace |
|---|---|
| Detection | Sólo detecta y loggea |
| Prevention | Bloquea tráfico malicioso |

---

# 11. Detection Mode

## Qué hace

- analiza tráfico
- detecta ataques
- genera logs
- NO bloquea tráfico

---

## Uso típico

- pruebas iniciales
- tuning
- validación
- evitar falsos positivos

---

## Flujo

```text
Ataque detectado
   ↓
Log / Alert
   ↓
Tráfico permitido
```

---

# 12. Prevention Mode

## Qué hace

- detecta ataques
- bloquea tráfico
- devuelve HTTP 403

---

## Flujo

```text
Ataque detectado
   ↓
WAF bloquea request
   ↓
Backend protegido
```

---

# 13. Buenas prácticas enterprise

## Fase 1

```text
Detection Mode
```

para:
- analizar tráfico real
- detectar falsos positivos

---

## Fase 2

```text
Prevention Mode
```

cuando:
- reglas validadas
- exclusiones configuradas
- aplicaciones ajustadas

---

# 14. Qué es Application Gateway WAF v2

Es:
- reverse proxy
- load balancer Layer 7
- ingress controller
- WAF regional

---

# Funcionalidades principales

| Función | Soportado |
|---|---|
| WAF | ✅ |
| SSL termination | ✅ |
| Autoscaling | ✅ |
| Path routing | ✅ |
| URL routing | ✅ |
| Backend pools | ✅ |
| HTTP/HTTPS inspection | ✅ |
| OWASP protection | ✅ |

---

# Arquitectura típica

```text
Internet
   ↓
Application Gateway WAF v2
   ↓
Private Backend API
```

---

# 15. Qué es Azure Front Door WAF

Azure Front Door añade:
- edge global Microsoft
- CDN
- acceleration
- global routing
- global failover

más:
- WAF integrado.

---

# Arquitectura típica

```text
Internet
   ↓
Azure Front Door WAF
   ↓
Application Gateway / Backend
```

---

# 16. Diferencia entre App Gateway WAF y Front Door WAF

| Característica | App Gateway WAF | Front Door WAF |
|---|---|---|
| Scope | Regional | Global |
| Layer | L7 | L7 Global Edge |
| CDN | ❌ | ✅ |
| Global failover | ❌ | ✅ |
| Edge protection | ❌ | ✅ |
| Private backend | ✅ | Premium |
| Multi-region | Limitado | Excelente |

---

# 17. Costes aproximados

## Application Gateway WAF v2

| Servicio | Coste aproximado |
|---|---|
| App Gateway WAF v2 | ~200–500+ USD/mes |

Depende de:
- tráfico
- capacity units
- autoscaling
- región

---

## Azure Front Door Standard

| Servicio | Coste aproximado |
|---|---|
| Front Door Standard | ~35–150+ USD/mes |

---

## Azure Front Door Premium

| Servicio | Coste aproximado |
|---|---|
| Front Door Premium | ~300–1000+ USD/mes |

---

# 18. Dónde se despliega cada uno

| Servicio | Suscripción típica |
|---|---|
| Application Gateway WAF v2 | Workload / Spoke Subscription |
| Azure Front Door | Shared Services / Connectivity |
| Azure Front Door Premium | Shared Services / Connectivity |

---

# 19. Cómo se instala

# Application Gateway WAF v2

## Portal Azure

```text
Create Resource
→ Application Gateway
→ SKU = WAF_v2
→ Enable WAF
```

---

# Front Door Standard/Premium

## Portal Azure

```text
Create Resource
→ Front Door and CDN Profiles
→ Select Standard or Premium
→ Enable WAF Policy
```

---

# 20. Terraform — Application Gateway WAF

```hcl
resource "azurerm_web_application_firewall_policy" "waf" {
  name                = "waf-policy-prod"
  resource_group_name = azurerm_resource_group.network.name
  location            = azurerm_resource_group.network.location

  policy_settings {
    enabled = true
    mode    = "Prevention"
  }

  managed_rules {
    managed_rule_set {
      type    = "OWASP"
      version = "3.2"
    }
  }
}
```

---

# 21. Policies ALZ típicas

## Lo que suele forzarse

| Control | Azure Policy |
|---|---|
| Sólo WAF_v2 SKU | ✅ |
| WAF enabled | ✅ |
| Prevention mode | ✅ |
| Diagnostics enabled | ✅ |
| Standard Public IP only | ✅ |
| DDoS IP Protection enabled | ✅ parcialmente |

---

# 22. Qué NO garantiza Azure Policy

Azure Policy NO puede garantizar completamente:

```text
Public IP
+
DDoS Protection
+
WAF
+
correct routing
+
secure backend
```

como una única arquitectura lógica.

---

# 23. Qué hacen las empresas enterprise realmente

## Modelo maduro

```text
Policies
+
Reusable Terraform Modules
+
Pipelines
```

---

# 24. Qué es un Approved Terraform Module

NO es un producto Azure.

Es:
- un módulo Terraform corporativo
- creado por el equipo de plataforma
- validado por seguridad/networking

---

# Ejemplo

```text
module "secure_app_gateway"
```

que automáticamente:
- crea WAF_v2
- habilita WAF
- activa OWASP
- configura diagnostics
- crea Public IP segura
- habilita DDoS IP Protection

---

# 25. Arquitecturas que SÍ tienen sentido

| Arquitectura | Componentes | Coste aproximado | Nivel | Cuándo tiene sentido |
|---|---|---|---|---|
| Coste optimizado | DDoS Basic + Application Gateway WAF v2 | ~200–500+ USD/mes | Básico / Inicial | Primera API pública, coste limitado, entorno regional |
| Protección reforzada | DDoS IP Protection + Application Gateway WAF v2 | ~400–700+ USD/mes | Medio / Seguridad reforzada | API crítica, pocas Public IPs, necesidad DDoS avanzada |
| Enterprise moderado | Azure Front Door Standard + Application Gateway WAF v2 | ~250–650+ USD/mes | Enterprise | Usuarios globales, CDN, routing global, failover |
| Enterprise avanzado | Azure Front Door Premium + Application Gateway WAF v2 | ~500–1500+ USD/mes | Enterprise avanzado | Multi-región, backend privado, edge security avanzada |
| Enterprise completo | Azure Front Door Premium + Application Gateway WAF v2 + Azure Firewall HUB + DDoS Network Protection Plan | ~3500–5000+ USD/mes | Enterprise global | Muchas aplicaciones públicas, compliance alto, arquitectura global crítica |


# DDoS Basic + WAF

```text
Internet
   ↓
DDoS Basic
   ↓
Application Gateway WAF v2
   ↓
Backend API
```

Muy buena opción inicial.

---

# DDoS IP Protection + WAF

```text
Internet
   ↓
DDoS IP Protection
   ↓
Application Gateway WAF v2
   ↓
Backend API
```

Muy buena opción para una API crítica.

---

# Front Door + WAF

```text
Internet
   ↓
Azure Front Door WAF
   ↓
Application Gateway
   ↓
Backend
```

Muy buena opción enterprise.

---

# Front Door Premium + Private Backend

```text
Internet
   ↓
Front Door Premium
   ↓
Private Link
   ↓
Private Backend
```

Excelente arquitectura enterprise segura.

---

# 26. Arquitecturas que NO suelen tener sentido

| Arquitectura | Problema |
|---|---|
| WAF sin DDoS para apps críticas | No protege ataques volumétricos |
| Azure Firewall como único ingress HTTP | No sustituye WAF |
| VM pública directa | Rompe patrón seguro |
| DDoS Network Protection para una sola app pequeña | Coste muy alto |

---

# 27. Recomendación ALZ realista

## Coste optimizado

```text
DDoS Basic
   +
Application Gateway WAF v2
   +
Azure Firewall HUB
```
--- 
## Protección reforzada con coste contenido

```text
DDoS IP Protection
   +
Application Gateway WAF v2
   +
Azure Firewall HUB

---

## Enterprise futura

```text
Azure Front Door Premium
   +
Application Gateway WAF v2
   +
Azure Firewall HUB
   +
DDoS Protection
```

---

# 28. Resumen final

| Necesidad | Recomendación |
|---|---|
| API pública sencilla | App Gateway WAF v2 |
| API crítica | DDoS IP Protection + WAF |
| Multi-región | Front Door |
| Backend privado | Front Door Premium |
| Control egress | Azure Firewall HUB |
| Protección enterprise masiva | DDoS Network Protection |
