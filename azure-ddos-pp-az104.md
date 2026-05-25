# Azure DDoS Protection Plan

## Área / Categoría

| Área | Categoría |
|---|---|
| Networking | Seguridad de Red |
| Servicio Azure | Azure DDoS Protection |
| Dominio ALZ | Connectivity / Security |
| Capa OSI | Protección Layer 3 / Layer 4 |

---

# Descripción General

Azure DDoS Protection es un servicio administrado de Azure que protege recursos expuestos a Internet frente a ataques Distributed Denial of Service (DDoS).

Proporciona mitigación automática frente a:
- ataques volumétricos
- ataques de protocolo
- agotamiento de recursos

antes de que el tráfico malicioso llegue al entorno del cliente.

---

# Tipos de Servicio

| Tipo | Incluido | Descripción |
|---|---|---|
| DDoS Basic | Sí (por defecto) | Protección básica incluida automáticamente en Azure |
| DDoS Network Protection | De pago | Protección avanzada enterprise para VNets |

---

# Concepto Arquitectónico Importante

## DDoS Protection se asocia a una VNet

El DDoS Protection Plan NO se asocia directamente a:
- Public IPs
- Application Gateway
- Azure Firewall

Se asocia a una:

```text
Virtual Network (VNet)
```

---

# Qué queda protegido automáticamente

Cuando una VNet está asociada a un DDoS Protection Plan, Azure protege automáticamente los recursos públicos dentro de esa VNet, incluyendo:

- Public IP Addresses
- Application Gateway
- Azure Firewall
- Load Balancers
- Workloads expuestos a Internet

---

# Cómo funciona la mitigación

La mitigación DDoS NO ocurre dentro de la VNet.

Azure realiza la mitigación en la:

```text
Microsoft Azure Global Edge Network
```

antes de que el tráfico malicioso llegue a los recursos Azure.

---

# Flujo de Tráfico

```text
Internet
   ↓
Azure Global Edge Network
(Detección y Mitigación DDoS)
   ↓
Public IP
   ↓
Application Gateway / Firewall
   ↓
Backend Workload
```

---

# Diferencia entre DDoS Protection y WAF

| DDoS Protection | WAF |
|---|---|
| Protección Layer 3 / Layer 4 | Protección Layer 7 |
| Protege frente a ataques volumétricos | Protege frente a ataques web |
| SYN Flood, UDP Flood | SQL Injection, XSS |
| Mitigación en el edge de Azure | Protección a nivel aplicación |

---

# Diferencia entre DDoS Protection y Azure Firewall

| DDoS Protection | Azure Firewall |
|---|---|
| Mitigación DDoS | Filtrado e inspección de tráfico |
| Mitigación automática | Reglas configurables |
| Azure global edge | Desplegado dentro de Azure |
| Absorción de ataques | Gobernanza y seguridad de red |

---

# Arquitectura Típica ALZ

```text
Connectivity Subscription
   ↓
DDoS Protection Plan
   ↓
Asociado a:
   - Spoke VNet A
   - Spoke VNet B
   - Shared Services VNet
```

---

# Modelo de Gobernanza Recomendado en ALZ

| Componente | Ownership |
|---|---|
| DDoS Protection Plan | GroupIT Cloud |
| Asociación a VNets | GroupIT Cloud |
| Exposición pública de aplicaciones | Application Teams mediante patrones aprobados |
| Gobernanza WAF | GroupIT Cloud |

---

# Conceptos Importantes para Examen

## Concepto Clave 1

Los DDoS Protection Plans se asocian a VNets, NO directamente a Public IPs.

## Concepto Clave 2

La mitigación ocurre en el Azure global edge network.

## Concepto Clave 3

DDoS Protection complementa:
- Azure Firewall
- Application Gateway WAF
- Azure Front Door

No los reemplaza.

---

# Ejemplo

```text
Ataque SYN Flood de 500 Gbps
        ↓
Azure Edge detecta el ataque
        ↓
El tráfico es mitigado antes de llegar a la Public IP
        ↓
Sólo tráfico limpio llega al Application Gateway
```

# Coste Aproximado

## DDoS Basic

```text
Incluido gratuitamente en Azure
```

---

## DDoS Network Protection

Modelo de coste:
- coste fijo mensual por plan
- permite proteger múltiples VNets bajo el mismo plan

### Coste aproximado

| Servicio | Coste aproximado |
|---|---|
| DDoS Network Protection Plan | ~2.500 - 3.000 USD/mes |

⚠️ El precio puede variar según región y cambios de Microsoft.

---

# Consideraciones Importantes de Coste

## El coste NO es por VNet

El coste es:
```text
por DDoS Protection Plan
```

Por eso normalmente en ALZ:
- se centraliza
- se comparte entre múltiples VNets

---

# Ejemplo Enterprise

```text
Connectivity Subscription
   ↓
1 DDoS Protection Plan
   ↓
Protege:
- Shared Services VNet
- Spoke VNet A
- Spoke VNet B
- Spoke VNet C
```

---

# Beneficios Enterprise

| Beneficio | Explicación |
|---|---|
| Cost sharing | Un único plan protege muchas VNets |
| Central governance | Gobernanza centralizada |
| Attack telemetry | Métricas y alertas avanzadas |
| Cost protection | Protección frente a costes derivados de ataques |
| Integration | Azure Monitor / Sentinel |

---

# Concepto Arquitectónico Importante

## DDoS Protection se asocia a una VNet

El DDoS Protection Plan NO se asocia directamente a:
- Public IPs
- Application Gateway
- Azure Firewall

Se asocia a una:

```text
Virtual Network (VNet)
```

---

# Qué queda protegido automáticamente

Cuando una VNet está asociada a un DDoS Protection Plan, Azure protege automáticamente los recursos públicos dentro de esa VNet, incluyendo:

- Public IP Addresses
- Application Gateway
- Azure Firewall
- Load Balancers
- Workloads expuestos a Internet

---

# Cómo funciona la mitigación

La mitigación DDoS NO ocurre dentro de la VNet.

Azure realiza la mitigación en la:

```text
Microsoft Azure Global Edge Network
```

antes de que el tráfico malicioso llegue a los recursos Azure.

---

# Flujo de Tráfico

```text
Internet
   ↓
Azure Global Edge Network
(Detección y Mitigación DDoS)
   ↓
Public IP
   ↓
Application Gateway / Firewall
   ↓
Backend Workload
```

---

# Diferencia entre DDoS Protection y WAF

| DDoS Protection | WAF |
|---|---|
| Protección Layer 3 / Layer 4 | Protección Layer 7 |
| Protege frente a ataques volumétricos | Protege frente a ataques web |
| SYN Flood, UDP Flood | SQL Injection, XSS |
| Mitigación en el edge de Azure | Protección a nivel aplicación |

---

# Diferencia entre DDoS Protection y Azure Firewall

| DDoS Protection | Azure Firewall |
|---|---|
| Mitigación DDoS | Filtrado e inspección de tráfico |
| Mitigación automática | Reglas configurables |
| Azure global edge | Desplegado dentro de Azure |
| Absorción de ataques | Gobernanza y seguridad de red |

---

# Arquitectura Típica ALZ

```text
Connectivity Subscription
   ↓
DDoS Protection Plan
   ↓
Asociado a:
   - Spoke VNet A
   - Spoke VNet B
   - Shared Services VNet
```

---

# Modelo de Gobernanza Recomendado en ALZ

| Componente | Ownership |
|---|---|
| DDoS Protection Plan | GroupIT Cloud |
| Asociación a VNets | GroupIT Cloud |
| Exposición pública de aplicaciones | Application Teams mediante patrones aprobados |
| Gobernanza WAF | GroupIT Cloud |

---

# Conceptos Importantes para Examen

## Concepto Clave 1

Los DDoS Protection Plans se asocian a VNets, NO directamente a Public IPs.

## Concepto Clave 2

La mitigación ocurre en el Azure global edge network.

## Concepto Clave 3

DDoS Protection complementa:
- Azure Firewall
- Application Gateway WAF
- Azure Front Door

No los reemplaza.

---

# Ejemplo

```text
Ataque SYN Flood de 500 Gbps
        ↓
Azure Edge detecta el ataque
        ↓
El tráfico es mitigado antes de llegar a la Public IP
        ↓
Sólo tráfico limpio llega al Application Gateway
```
