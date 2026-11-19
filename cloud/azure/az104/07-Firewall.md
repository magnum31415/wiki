[AZ104-INDEX](./readme.md)

# 07 - Azure Firewall (AZ-104)

> Este documento resume la teoría de **Azure Firewall** más preguntada en el examen **AZ-104**.

---

# Índice

- ¿Qué es Azure Firewall?
- AzureFirewallSubnet
- Azure Firewall SKU
- Firewall Policy
- Tipos de reglas
- DNAT
- SNAT
- Threat Intelligence
- Forced Tunneling
- Routing
- Integración con UDR
- Buenas prácticas
- Preguntas trampa

---

# 1. ¿Qué es Azure Firewall?

**Azure Firewall** es un firewall administrado de **capa 3 a 7**, totalmente gestionado por Azure.

Permite controlar el tráfico:

- Norte-Sur (Internet ↔ Azure)
- Este-Oeste (entre VNets)

Se utiliza para centralizar la seguridad de la red.

---

# 2. AzureFirewallSubnet

Azure Firewall debe desplegarse obligatoriamente en una Subnet llamada:

```
AzureFirewallSubnet
```

El nombre es obligatorio.

No puede utilizarse ninguna otra Subnet.

---

# 3. Tamaño de la Subnet

Microsoft recomienda que la subnet tenga un tamaño mínimo de:

```
/26
```

Esto permite futuras ampliaciones del servicio.

---

# 4. SKU disponibles

Azure Firewall dispone de tres SKUs.

| SKU | Características |
|------|-----------------|
| **Basic** | Funcionalidades básicas |
| **Standard** | Network + Application Rules |
| **Premium** | TLS Inspection + IDPS |

En el examen normalmente se trabaja con **Standard** o **Premium**.

---

# 5. Firewall Policy

Una **Firewall Policy** permite administrar de forma centralizada la configuración del Firewall.

Incluye:

- Reglas
- Threat Intelligence
- TLS Inspection
- IDPS

Puede reutilizarse en varios Firewalls.

---

# 6. Tipos de reglas

Azure Firewall soporta tres tipos de reglas.

## Network Rules

Filtran tráfico mediante:

- IP origen
- IP destino
- Puerto
- Protocolo

Trabajan en capa 3 y 4.

---

## Application Rules

Filtran tráfico mediante:

- FQDN
- URL
- HTTP
- HTTPS

Ejemplo:

```
login.microsoftonline.com
```

Muy utilizadas para permitir acceso a servicios SaaS.

---

## NAT Rules

Permiten publicar servicios internos hacia Internet.

Utilizan:

**DNAT**

---

# 7. DNAT

**Destination NAT (DNAT)** redirige conexiones entrantes.

Ejemplo:

```
Internet

↓

Firewall Public IP

↓

VM privada
```

Permite publicar:

- RDP
- SSH
- HTTP
- HTTPS

---

# 8. SNAT

Azure Firewall realiza automáticamente:

**Source NAT (SNAT)**

cuando los recursos internos acceden a Internet.

Esto permite que varias máquinas compartan la IP pública del Firewall.

---

# 9. Threat Intelligence

Azure Firewall puede utilizar información proporcionada por Microsoft para detectar tráfico malicioso.

Modos disponibles:

- Alert
- Alert and Deny
- Off

---

# 10. Azure Firewall Premium

La versión **Premium** añade funcionalidades avanzadas como:

- TLS Inspection
- IDPS (Intrusion Detection and Prevention System)
- URL Filtering
- Web Categories

Es la opción recomendada para entornos con mayores requisitos de seguridad.

---

# 11. Forced Tunneling

Mediante **User Defined Routes (UDR)** es posible forzar que todo el tráfico pase por Azure Firewall.

Ejemplo:

```
0.0.0.0/0

↓

Azure Firewall
```

Esta técnica se conoce como:

**Forced Tunneling**

---

# 12. Azure Firewall y UDR

Azure Firewall **no modifica automáticamente las rutas**.

Para que el tráfico pase por el Firewall es necesario:

1.

Crear una **Route Table**.

↓

2.

Añadir una UDR.

↓

3.

Asociar la Route Table a la Subnet.

---

# 13. Routing Intent (Virtual WAN)

En **Azure Virtual WAN** puede configurarse un **Routing Intent** para enviar automáticamente el tráfico:

- Internet
- Privado

hacia Azure Firewall.

No es necesario crear UDR manualmente.

---

# 14. Azure Firewall vs NSG

| Azure Firewall | NSG |
|----------------|-----|
| Firewall administrado | Filtro de red |
| L3-L7 | L4 |
| Reglas por FQDN | IP y puertos |
| DNAT | No |
| Threat Intelligence | No |
| TLS Inspection (Premium) | No |

Ambos servicios son complementarios.

---

# 15. Azure Firewall vs Application Gateway

| Azure Firewall | Application Gateway |
|----------------|---------------------|
| Todo tipo de tráfico | Solo HTTP/HTTPS |
| L3-L7 | L7 |
| Firewall de red | Balanceador Web |
| No balancea tráfico | Balancea tráfico |

---

# 16. Casos de uso

Azure Firewall es recomendable para:

- Centralizar la seguridad de varias VNets.
- Filtrar acceso a Internet.
- Permitir únicamente determinados FQDN.
- Publicar servicios mediante DNAT.
- Arquitecturas Hub & Spoke.

---

# 17. Buenas prácticas

Microsoft recomienda:

- Utilizar **Firewall Policy**.
- Centralizar el Firewall en el **Hub**.
- Enviar el tráfico mediante **UDR** o **Routing Intent**.
- Utilizar **Application Rules** cuando sea posible.
- Utilizar **Premium** si se necesita inspección TLS.

---

# Preguntas trampa del AZ-104

✅ Azure Firewall debe desplegarse en una Subnet llamada **AzureFirewallSubnet**.

✅ El tamaño mínimo recomendado de la subnet es **/26**.

✅ Azure Firewall utiliza **Network Rules**, **Application Rules** y **NAT Rules**.

✅ Las **Application Rules** utilizan **FQDN**, no direcciones IP.

✅ **DNAT** publica recursos internos hacia Internet.

✅ Azure Firewall realiza **SNAT** automáticamente para conexiones salientes.

✅ Para enviar tráfico al Firewall normalmente se utilizan **UDR**.

✅ En **Azure Virtual WAN**, el **Routing Intent** puede dirigir automáticamente el tráfico al Firewall.

✅ Azure Firewall y **NSG** no son alternativas; son **complementarios**.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Azure Firewall | Firewall administrado L3-L7 |
| AzureFirewallSubnet | Nombre obligatorio |
| Tamaño recomendado | **/26** |
| Firewall Policy | Administración centralizada |
| Network Rules | IP + Puerto |
| Application Rules | FQDN |
| NAT Rules | DNAT |
| SNAT | Automático |
| Threat Intelligence | Alert / Alert and Deny |
| Premium | TLS Inspection + IDPS |
| Forced Tunneling | UDR hacia Firewall |
| Routing Intent | Virtual WAN |
| NSG | Complementario al Firewall |
