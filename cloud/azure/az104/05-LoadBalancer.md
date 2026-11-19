[AZ104-INDEX](./readme.md)

# 05 - Azure Load Balancer (AZ-104)

> Este documento resume la teoría de **Azure Load Balancer** y el resto de soluciones de balanceo de carga que aparecen con mayor frecuencia en el examen **AZ-104**.

---

# Índice

- Azure Load Balancer
- Tipos de Load Balancer
- SKU Basic vs Standard
- Frontend IP
- Backend Pool
- Health Probe
- Load Balancing Rules
- Inbound NAT Rules
- Outbound Rules
- Public IP
- Internal Load Balancer
- Application Gateway
- Traffic Manager
- Preguntas trampa

---

# 1. Azure Load Balancer

**Azure Load Balancer** distribuye tráfico de red de **capa 4 (TCP/UDP)** entre varias máquinas virtuales.

Se utiliza para:

- Alta disponibilidad.
- Escalabilidad.
- Distribución automática del tráfico.

No inspecciona el contenido HTTP o HTTPS.

---

# 2. Capas OSI

Azure dispone de varias soluciones de balanceo.

| Servicio | Capa | Uso |
|----------|------|-----|
| **Load Balancer** | L4 | TCP / UDP |
| **Application Gateway** | L7 | HTTP / HTTPS |
| **Traffic Manager** | DNS | Balanceo global |

Es una de las comparativas más preguntadas del examen.

---

# 3. Tipos de Load Balancer

## Public Load Balancer

Expone un servicio a Internet mediante una **Public IP**.

Ejemplo:

```
Internet

↓

Public IP

↓

Azure Load Balancer

↓

VM1
VM2
VM3
```

---

## Internal Load Balancer (ILB)

Solo dispone de una **Private IP**.

Se utiliza para distribuir tráfico interno dentro de una Virtual Network.

No es accesible desde Internet.

---

# 4. SKU Basic vs Standard

## Basic

- SKU antiguo.
- Menos funcionalidades.
- No recomendado para nuevas implementaciones.

---

## Standard

Microsoft recomienda siempre **Standard Load Balancer**.

Ventajas:

- Mayor seguridad.
- Alta disponibilidad.
- Availability Zones.
- Backend más flexible.
- Integración con Standard Public IP.

---

# 5. Frontend IP

El **Frontend IP Configuration** representa la dirección IP mediante la que los clientes acceden al Load Balancer.

Puede ser:

- Public IP
- Private IP

---

# 6. Backend Pool

El **Backend Pool** contiene las máquinas virtuales que recibirán el tráfico.

Puede incluir:

- NICs
- Virtual Machine Scale Sets

Una máquina virtual debe pertenecer al **Backend Pool** para recibir tráfico.

---

# 7. Public IP y Standard Load Balancer

Cuando una VM pasa a formar parte del **Backend Pool** de un **Standard Public Load Balancer**, **no puede mantener una Public IP propia**.

Es necesario eliminar la Public IP de la NIC antes de añadir la VM al Backend Pool.

Esta es una pregunta clásica del AZ-104.

---

# 8. Health Probe

El **Health Probe** comprueba continuamente si cada máquina virtual está disponible.

Si una VM deja de responder:

↓

Azure deja de enviarle tráfico.

Puede utilizar:

- TCP
- HTTP
- HTTPS

---

# 9. Load Balancing Rule

Una **Load Balancing Rule** relaciona:

- Frontend IP
- Backend Pool
- Health Probe

Define cómo se distribuye el tráfico.

Ejemplo:

```
443

↓

Frontend

↓

Backend Pool

↓

VMs
```

---

# 10. Inbound NAT Rule

Las **Inbound NAT Rules** permiten redirigir un puerto específico hacia una máquina virtual concreta.

Ejemplo:

```
3389

↓

VM1

3390

↓

VM2

3391

↓

VM3
```

Muy utilizada para:

- RDP
- SSH

---

# 11. Load Balancing Rule vs NAT Rule

## Load Balancing Rule

Distribuye tráfico entre varias máquinas.

---

## NAT Rule

Redirige tráfico hacia una única máquina.

---

# Comparativa

| Load Balancing Rule | NAT Rule |
|---------------------|----------|
| Varias VMs | Una VM |
| Balanceo | Redirección |
| Alta disponibilidad | Administración |

---

# 12. Outbound Rules

Las **Outbound Rules** controlan cómo las máquinas del Backend Pool acceden a Internet.

En un **Standard Load Balancer**, las VMs **no disponen de acceso saliente automático**.

Debe configurarse:

- Outbound Rule
- NAT Gateway
- Public IP propia

---

# 13. Network Security Group

Un **Standard Load Balancer** requiere un **NSG** que permita el tráfico correspondiente.

Sin reglas adecuadas en el NSG, el Load Balancer no podrá entregar tráfico a las máquinas virtuales.

---

# 14. Availability Zones

El **Standard Load Balancer** puede distribuir tráfico entre máquinas situadas en distintas **Availability Zones**.

Esto aumenta la disponibilidad frente a fallos de un datacenter.

---

# 15. Application Gateway

**Azure Application Gateway** es un balanceador de carga de **capa 7**.

Características:

- HTTP
- HTTPS
- URL Routing
- SSL Offloading
- Session Affinity
- Web Application Firewall (WAF)

Es la solución recomendada para aplicaciones web.

---

# 16. Web Application Firewall (WAF)

El **WAF** protege aplicaciones web frente a ataques como:

- SQL Injection
- Cross Site Scripting (XSS)
- OWASP Top 10

Solo está disponible en:

- Application Gateway WAF
- Azure Front Door WAF

No existe WAF en Azure Load Balancer.

---

# 17. Azure Traffic Manager

**Traffic Manager** realiza balanceo mediante **DNS**.

No distribuye conexiones TCP.

Redirige a los clientes hacia distintos endpoints utilizando políticas como:

- Priority
- Weighted
- Performance
- Geographic
- Multivalue

Ideal para aplicaciones distribuidas globalmente.

---

# 18. Azure Front Door

Azure Front Door es un servicio global de **capa 7 (HTTP/HTTPS)** que proporciona:

- Balanceo de carga global.
- Aceleración del tráfico.
- Alta disponibilidad.
- Terminación SSL.
- Web Application Firewall (WAF).
- CDN integrada.

Distribuye las solicitudes HTTP/HTTPS entre distintos backends utilizando políticas como:

- Priority
- Weighted
- Latency
- Session Affinity

Ideal para aplicaciones web distribuidas globalmente.

---

# Azure Front Door vs Traffic Manager

| Azure Front Door | Traffic Manager |
|------------------|-----------------|
| Capa 7 (HTTP/HTTPS) | DNS |
| Proxy inverso | DNS Redirect |
| Inspecciona el tráfico | No inspecciona tráfico |
| Soporta WAF | No |
| Soporta CDN | No |
| SSL Termination | Sí | No |
| Session Affinity | Sí | No |
| Failover rápido | Sí | Depende del TTL DNS |

---

# Casos de uso

Azure Front Door es la mejor opción cuando:

- Se publican aplicaciones Web.
- Se necesita WAF.
- Se requiere aceleración global.
- Se necesita balanceo HTTP/HTTPS entre regiones.
- Se desea proteger aplicaciones frente a ataques web.

---

# Preguntas trampa del AZ-104

✅ Azure Front Door funciona en la **capa 7 (HTTP/HTTPS)**.

✅ Es un **proxy inverso global**.

✅ Puede balancear tráfico entre distintas regiones.

✅ Puede integrar **Web Application Firewall (WAF)**.

✅ Puede realizar **SSL Offloading (Terminación SSL)**.

✅ Traffic Manager realiza balanceo mediante **DNS**.

✅ Front Door distribuye el tráfico **HTTP/HTTPS**, mientras que Traffic Manager solo responde consultas DNS.

---

# 18. Comparativa

| Servicio | Nivel | Uso |
|----------|-------|-----|
| Azure Load Balancer | L4 | TCP / UDP |
| Application Gateway | L7 | HTTP / HTTPS |
| Traffic Manager | DNS | Balanceo global |
| Azure Front Door | L7 Global | Aplicaciones web globales |

---

# 19. Buenas prácticas

Microsoft recomienda:

- Utilizar siempre **Standard Load Balancer**.
- Utilizar **Health Probes**.
- Eliminar la **Public IP** de las VMs que formen parte de un **Backend Pool** de un Standard Public Load Balancer.
- Utilizar **Application Gateway** para aplicaciones web.
- Utilizar **Traffic Manager** para distribuir tráfico entre regiones.

---

# Preguntas trampa del AZ-104

✅ **Azure Load Balancer** trabaja en **capa 4**.

✅ **Application Gateway** trabaja en **capa 7**.

✅ **Traffic Manager** realiza balanceo mediante **DNS**.

✅ Una VM perteneciente al **Backend Pool** de un **Standard Public Load Balancer** **no puede conservar una Public IP**.

✅ El **Health Probe** determina qué máquinas reciben tráfico.

✅ Una **Load Balancing Rule** distribuye tráfico entre varias VMs.

✅ Una **Inbound NAT Rule** redirige tráfico hacia una única VM.

✅ El **WAF** solo está disponible en **Application Gateway** y **Front Door**, no en Azure Load Balancer.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Azure Load Balancer | Balanceador L4 |
| Public Load Balancer | Acceso desde Internet |
| Internal Load Balancer | Solo red privada |
| Standard SKU | Recomendado |
| Backend Pool | VMs que reciben tráfico |
| Frontend IP | IP pública o privada |
| Health Probe | Comprueba disponibilidad |
| Load Balancing Rule | Balanceo entre VMs |
| NAT Rule | Redirección a una VM |
| Outbound Rules | Salida a Internet |
| Application Gateway | Balanceador L7 + WAF |
| Traffic Manager | Balanceo DNS global |
| WAF | Solo App Gateway / Front Door |
