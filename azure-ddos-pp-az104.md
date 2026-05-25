[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# DDoS Protection en Azure — Documento de estudio

## 1. Qué es y para qué sirve

Azure DDoS Protection protege recursos expuestos a Internet frente a ataques Distributed Denial of Service.

Un ataque DDoS intenta saturar un servicio con mucho tráfico malicioso para dejarlo inaccesible.

Ejemplos:
- SYN Flood
- UDP Flood
- ataques volumétricos
- ataques de amplificación
- agotamiento de conexiones

Azure DDoS Protection trabaja principalmente en Layer 3 / Layer 4.

No sustituye a:
- WAF
- Azure Firewall
- Azure Front Door
- Application Gateway

Los complementa.

---

## 2. Servicios principales

| Servicio | Qué es | Dónde se aplica |
|---|---|---|
| Azure DDoS Basic | Protección básica incluida por defecto en Azure | Automática, sin configuración |
| Azure DDoS IP Protection | Protección avanzada para una Public IP concreta | Directamente sobre la Public IP |
| Azure DDoS Network Protection Plan | Protección avanzada enterprise para múltiples VNets | Asociado a VNets |

---

## 3. Azure DDoS Basic

Azure DDoS Basic está incluido automáticamente en Azure.

No hay que:
- crear ningún recurso
- asociarlo a una VNet
- activarlo en una Public IP
- pagar coste adicional

```text
Azure DDoS Basic
   ↓
Incluido automáticamente
   ↓
Sin configuración
```

Uso:
- protección básica por defecto
- entornos no críticos
- primera capa mínima de protección

Coste:

```text
Incluido
```

---

## 4. Azure DDoS IP Protection

Azure DDoS IP Protection protege una Public IP individual frente a ataques DDoS Layer 3 / Layer 4.

Es útil cuando sólo tienes una o pocas aplicaciones públicas.

```text
Internet
   ↓
Azure Global Edge
running Azure DDoS IP Protection
   ↓
Protected Public IP
   ↓
Application Gateway WAF v2
   ↓
Backend API
```

### Dónde se instala

Se habilita directamente en la Public IP.

```text
Public IP Address
→ DDoS Protection
→ Enable DDoS IP Protection
```

### En qué suscripción debería estar

Debe estar en la misma suscripción donde está la Public IP.

Ejemplo:

| Recurso | Suscripción |
|---|---|
| Application Gateway WAF v2 | Workload / Spoke Subscription |
| Public IP | Workload / Spoke Subscription |
| DDoS IP Protection | Misma Workload / Spoke Subscription |

### Coste aproximado

```text
~199 USD/mes por Public IP
```

---

## 5. Azure DDoS Network Protection Plan

Azure DDoS Network Protection Plan es el modelo enterprise.

Protege múltiples VNets mediante un plan centralizado.

```text
Connectivity Subscription
   ↓
DDoS Network Protection Plan
   ↓
Associated to:
   - Hub VNet
   - Shared Services VNet
   - Spoke VNet A
   - Spoke VNet B
```

### Dónde se instala

Se crea como recurso Azure:

```text
Microsoft.Network/ddosProtectionPlans
```

Normalmente en:

```text
Connectivity Subscription
```

### Cómo se activa

No se activa a nivel tenant.

Se crea el plan y luego se asocia VNet por VNet:

```text
VNet
→ DDoS Protection
→ Enable
→ Select existing DDoS Plan
```

### Importante

No existe:

```text
Enable DDoS for whole tenant
```

Azure no protege automáticamente todas las VNets con Network Protection.

### Coste aproximado

```text
~2,944 USD/mes
```

---

## 6. Tabla comparativa — DDoS IP Protection vs DDoS Network Protection Plan

| Característica | DDoS IP Protection | DDoS Network Protection Plan |
|---|---|---|
| Alcance | Una Public IP | Múltiples VNets |
| Coste aproximado | ~199 USD/mes por Public IP | ~2,944 USD/mes |
| Ideal para | Una API pública o pocas IPs críticas | Muchas aplicaciones públicas |
| Se asocia a | Public IP | VNet |
| Modelo ALZ | Workload / Spoke Subscription | Connectivity Subscription |
| Centralización | Baja / media | Alta |
| Coste para una sola app | Más razonable | Normalmente caro |
| Coste para muchas IPs | Puede crecer rápido | Más eficiente a escala |
| Protege Layer 3 / Layer 4 | Sí | Sí |
| Sustituye WAF | No | No |
| Sustituye Azure Firewall | No | No |

---

## 7. Tabla comparativa general

| Servicio | Coste aproximado | Capa | Para qué sirve | Dónde se aplica | Cuándo usarlo |
|---|---:|---|---|---|---|
| Azure DDoS Basic | Incluido | L3 / L4 | Protección DDoS básica | Automático en Azure | Siempre, viene por defecto |
| Azure DDoS IP Protection | ~199 USD/mes por Public IP | L3 / L4 | Protección avanzada por IP pública | Public IP | Una API pública crítica |
| Azure DDoS Network Protection Plan | ~2,944 USD/mes | L3 / L4 | Protección enterprise para muchas VNets/IPs | VNets | Muchas apps públicas o alto riesgo |
| Azure Application Gateway WAF v2 | ~200–500+ USD/mes según uso | L7 | Reverse proxy + WAF regional | Spoke o Shared Services | APIs/webs públicas regionales |
| Azure Front Door Standard | ~35–150+ USD/mes según tráfico | L7 Global | Ingress global, CDN, routing | Edge global Microsoft | Web/API pública simple o global |
| Azure Front Door Premium | ~300–1000+ USD/mes según tráfico | L7 Global | Ingress global avanzado, WAF, Private Link | Edge global Microsoft | Enterprise, multi-región, backend privado |

---

## 8. Diferencia entre DDoS, WAF, Front Door y Firewall

| Servicio | Protege contra | Capa |
|---|---|---|
| DDoS Protection | Ataques volumétricos, SYN Flood, UDP Flood | L3 / L4 |
| Application Gateway WAF v2 | SQL Injection, XSS, OWASP Top 10 | L7 |
| Azure Front Door | Ingress global, WAF, CDN, failover | L7 Global |
| Azure Firewall | Filtrado de red, egress, DNAT/SNAT | L3-L7 |

---

## 9. Combinaciones que SÍ tienen sentido

### 9.1 DDoS Basic + Application Gateway WAF v2

Tiene sentido para una primera fase con coste controlado.

```text
Internet
   ↓
DDoS Basic
   ↓
Application Gateway WAF v2
   ↓
Private Backend API
```

Por qué:
- DDoS Basic ya está incluido
- WAF protege Layer 7
- coste razonable
- patrón típico para una API pública

---

### 9.2 DDoS IP Protection + Application Gateway WAF v2

Muy buena opción para una sola API crítica.

```text
Internet
   ↓
DDoS IP Protection
   ↓
Protected Public IP
   ↓
Application Gateway WAF v2
   ↓
Backend API
```

Por qué:
- DDoS IP Protection protege la Public IP
- WAF protege la aplicación HTTP/HTTPS
- más barato que DDoS Network Protection

---

### 9.3 DDoS Network Protection Plan + Application Gateway WAF v2

Tiene sentido en entornos enterprise con muchas aplicaciones públicas.

```text
Internet
   ↓
DDoS Network Protection
   ↓
Application Gateway WAF v2
   ↓
Backend API
```

Por qué:
- DDoS protege L3/L4
- WAF protege L7
- modelo centralizado ALZ

No lo usaría para una única app salvo que sea muy crítica.

---

### 9.4 Azure Front Door Standard/Premium + Application Gateway WAF v2

Tiene sentido para arquitectura enterprise o multi-región.

```text
Internet
   ↓
Azure Front Door
   ↓
Application Gateway WAF v2
   ↓
Private Backend
```

Por qué:
- Front Door aporta edge global, CDN, routing y failover
- App Gateway aporta ingress regional y routing interno
- útil para multi-región o alta disponibilidad

---

### 9.5 Azure Front Door Premium + Private Backend

Muy buena arquitectura enterprise.

```text
Internet
   ↓
Azure Front Door Premium
   ↓
Private Link
   ↓
Private Backend
```

Por qué:
- evita exposición pública directa del backend
- soporta Private Link
- mejora seguridad de publicación

---

### 9.6 Application Gateway WAF v2 + Azure Firewall

Tiene sentido, pero cada uno protege un flujo distinto.

```text
Ingress:
Internet
   ↓
Application Gateway WAF v2
   ↓
Backend API

Egress:
Backend API
   ↓
UDR
   ↓
Azure Firewall HUB
   ↓
Internet
```

Por qué:
- WAF protege tráfico entrante HTTP/HTTPS
- Azure Firewall controla salida a Internet

---

## 10. Combinaciones que NO suelen tener sentido

| Combinación | Por qué no suele tener sentido |
|---|---|
| DDoS Network Protection Plan para una sola app no crítica | Coste fijo muy alto |
| DDoS IP Protection + DDoS Network Protection sobre el mismo escenario | Puede ser redundante si la VNet ya está cubierta por Network Protection |
| WAF sin ningún control DDoS adicional en apps muy críticas | WAF no protege bien ataques volumétricos L3/L4 |
| Sólo DDoS Protection sin WAF para una API pública | No protege ataques web como SQL Injection o XSS |
| Azure Firewall como único control para publicar APIs HTTP | Azure Firewall no sustituye un WAF |
| Public IP directa en VM + WAF separado | Rompe el patrón seguro de ingress |
| Front Door Premium + Application Gateway WAF v2 + DDoS Network Protection para una app pequeña | Puede ser técnicamente válido, pero demasiado caro y complejo |

---

## 11. Recomendación para una sola aplicación con coste limitado

Para una única API pública, empezaría con:

```text
DDoS Basic
   +
Application Gateway WAF v2
   +
Azure Firewall HUB para egress
```

Si la API es crítica o el riesgo DDoS es relevante:

```text
DDoS IP Protection
   +
Application Gateway WAF v2
   +
Azure Firewall HUB para egress
```

No empezaría con:

```text
DDoS Network Protection Plan
```

salvo que:
- haya varias aplicaciones públicas
- existan muchas Public IPs
- haya requisitos de compliance fuertes
- el impacto de caída sea muy alto
- el coste mensual esté justificado

---

## 12. Resumen final

| Escenario | Recomendación |
|---|---|
| Protección mínima | DDoS Basic |
| Una API pública con coste limitado | DDoS Basic + App Gateway WAF v2 |
| Una API pública crítica | DDoS IP Protection + App Gateway WAF v2 |
| Muchas aplicaciones públicas | DDoS Network Protection Plan + WAF |
| App global/multi-región | Front Door Standard/Premium |
| App enterprise con backend privado | Front Door Premium + Private Link |
| ALZ con control de salida | Azure Firewall HUB + UDR |
