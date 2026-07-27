[AZ104-INDEX](./readme.md)
# 04 - Network Security Groups (NSG) (AZ-104)

> Este documento resume la teoría de **Network Security Groups (NSG)** más preguntada en el examen **AZ-104**.

---

# Índice

- Network Security Group
- Asociación
- Security Rules
- Prioridades
- Default Rules
- Service Tags
- Application Security Groups
- Effective Security Rules
- IP Flow Verify
- NSG Flow Logs
- Buenas prácticas
- Preguntas trampa

---

# 1. ¿Qué es un Network Security Group?

Un **Network Security Group (NSG)** es un firewall de nivel 4 que controla el tráfico de red mediante reglas de:

- Entrada (Inbound)
- Salida (Outbound)

Filtra tráfico **TCP**, **UDP** e **ICMP** utilizando:

- Dirección IP origen
- Dirección IP destino
- Puerto
- Protocolo

---

# 2. Asociación de un NSG

Un NSG solo comienza a filtrar tráfico cuando está asociado a:

- Una **Subnet**
- Una **Network Interface (NIC)**

Si un NSG no está asociado a ningún recurso (**0 Subnets y 0 NICs**), **no filtra ningún tráfico**.

---

# 3. Reglas de seguridad

Cada regla contiene:

- Prioridad
- Dirección (Inbound / Outbound)
- Protocolo
- Puerto origen
- Puerto destino
- Dirección origen
- Dirección destino
- Acción (**Allow** o **Deny**)

Azure evalúa las reglas por orden de prioridad.

---

# 4. Prioridades

Las prioridades van desde:

```
100
```

hasta

```
4096
```

**Cuanto menor es el número, mayor prioridad tiene la regla.**

Ejemplo:

| Prioridad | Acción |
|-----------:|--------|
| 100 | Allow |
| 200 | Deny |

Se aplicará la regla con prioridad **100**.

---

# 5. Las reglas dejan de evaluarse

Cuando una regla coincide con el tráfico:

↓

Azure aplica esa regla.

↓

**No continúa evaluando el resto de reglas.**

Por ello, el orden de prioridad es fundamental.

---

# 6. Default Security Rules

Todos los NSG incluyen reglas predeterminadas.

## Inbound

Permiten:

- Tráfico dentro de la misma VNet.
- Tráfico procedente del Azure Load Balancer.

Deniegan:

- Todo el tráfico restante.

---

## Outbound

Permiten:

- Salida hacia Internet.
- Comunicación dentro de la VNet.

---

# 7. DenyAllInbound

La regla:

```
DenyAllInbound
```

bloquea todo el tráfico entrante que no haya sido permitido previamente.

Por ello, para permitir:

- RDP (3389)
- SSH (22)
- HTTPS (443)

es necesario crear una regla **Allow** con mayor prioridad.

---

# 8. Inbound vs Outbound

## Inbound

Controla el tráfico que entra al recurso.

Ejemplos:

- SSH
- RDP
- HTTPS

---

## Outbound

Controla el tráfico que sale del recurso.

Ejemplos:

- Internet
- Storage Account
- SQL Database

---

# 9. Service Tags

Los **Service Tags** representan grupos de direcciones IP administrados automáticamente por Microsoft.

Ejemplos:

- Internet
- VirtualNetwork
- AzureLoadBalancer
- AzureCloud
- Storage
- SQL
- AppService

No es necesario conocer las IP reales.

---

# 10. AzureCloud

El **Service Tag AzureCloud** representa las direcciones IP públicas utilizadas por los servicios de Azure.

Puede utilizarse para restringir el acceso a determinados servicios de Azure.

Por ejemplo, bloquear el acceso al **Azure Portal** desde una máquina virtual mediante una regla **Deny Outbound** hacia **AzureCloud**.

---

# 11. Application Security Groups (ASG)

Los **Application Security Groups (ASG)** permiten agrupar máquinas virtuales de forma lógica.

En lugar de crear reglas utilizando direcciones IP, se crean utilizando grupos.

Ejemplo:

```
WebServers

↓

DatabaseServers
```

Esto simplifica la administración de reglas.

---

# 12. Effective Security Rules

Las **Effective Security Rules** muestran las reglas que realmente afectan a una **NIC**.

Incluyen:

- Reglas del NSG de la NIC.
- Reglas heredadas del NSG asociado a la Subnet.

Son muy útiles para diagnosticar problemas de conectividad.

---

# 13. NSG en Subnet y NIC

Cuando existe un NSG asociado tanto a la **Subnet** como a la **NIC**, **ambos se aplican**.

El tráfico debe estar permitido por los dos NSG para poder pasar.

No existe prioridad entre ellos; el tráfico es evaluado por ambos.

---

# 14. IP Flow Verify

**IP Flow Verify** es una herramienta de **Network Watcher** que permite comprobar si un determinado flujo de red será:

- Permitido
- Denegado

Además indica exactamente qué regla del NSG ha tomado la decisión.

Es una de las herramientas de diagnóstico más utilizadas.

---

# 15. NSG Flow Logs

Los **NSG Flow Logs** registran todas las conexiones que atraviesan un NSG.

Incluyen información como:

- IP origen
- IP destino
- Puerto
- Protocolo
- Acción (Allow/Deny)

Son la base para herramientas como **Traffic Analytics** y **Network Insights**.

---

# 16. Traffic Analytics

**Traffic Analytics** analiza los **NSG Flow Logs** almacenados en un **Log Analytics Workspace**.

Permite identificar:

- Tráfico sospechoso.
- Conexiones bloqueadas.
- Patrones de comunicación.
- Flujos entre máquinas virtuales.

---

# 17. Network Watcher

**Network Watcher** proporciona herramientas para diagnosticar problemas de red, entre ellas:

- IP Flow Verify
- Connection Monitor
- NSG Flow Logs
- Topology
- Next Hop
- Effective Security Rules

---

# 18. Buenas prácticas

Microsoft recomienda:

- Aplicar NSG a nivel de **Subnet** cuando varias VMs comparten las mismas reglas.
- Utilizar NSG en la **NIC** únicamente cuando una VM necesite reglas específicas.
- Utilizar **Application Security Groups** para simplificar las reglas.
- Evitar reglas demasiado permisivas como:

```
Any → Any → Allow
```

---

# Preguntas trampa del AZ-104

✅ Un **NSG sin asociaciones** no protege ningún recurso.

✅ Azure evalúa primero la **prioridad más baja numéricamente**.

✅ Cuando una regla coincide, **Azure deja de evaluar** el resto.

✅ **DenyAllInbound** bloquea todo el tráfico no permitido previamente.

✅ **AzureLoadBalancer** está permitido por defecto.

✅ El **Service Tag AzureCloud** representa los servicios públicos de Azure.

✅ Los **NSG Flow Logs** registran conexiones, pero **no bloquean tráfico**.

✅ **IP Flow Verify** indica exactamente qué regla permite o deniega una conexión.

✅ Si existen NSG en la **Subnet** y en la **NIC**, **ambos deben permitir el tráfico**.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| NSG | Firewall de nivel 4 |
| Asociación | Subnet o NIC |
| Prioridad | Menor número = mayor prioridad |
| Evaluación | Se detiene en la primera coincidencia |
| Default Rules | VNet y Azure Load Balancer permitidos |
| DenyAllInbound | Bloquea el resto del tráfico |
| Service Tags | Grupos de IP administrados por Microsoft |
| AzureCloud | Servicios públicos de Azure |
| ASG | Agrupa VMs lógicamente |
| Effective Security Rules | Reglas realmente aplicadas |
| IP Flow Verify | Diagnóstico de una conexión |
| NSG Flow Logs | Registro de tráfico |
| Traffic Analytics | Análisis de NSG Flow Logs |
| Network Watcher | Herramientas de diagnóstico |
