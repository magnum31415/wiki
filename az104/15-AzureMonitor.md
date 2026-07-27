[AZ104-INDEX](./readme.md)

# 15 - Azure Monitor (AZ-104)

> Este documento resume la teoría de **Azure Monitor** más preguntada en el examen **AZ-104**.

---

# Índice

- Azure Monitor
- Metrics
- Logs
- Log Analytics Workspace
- Diagnostic Settings
- Azure Monitor Agent (AMA)
- Data Collection Rules (DCR)
- Alerts
- Action Groups
- Alert Processing Rules
- Activity Log
- Connection Monitor
- Network Watcher
- Buenas prácticas
- Preguntas trampa

---

# 1. ¿Qué es Azure Monitor?

**Azure Monitor** es el servicio centralizado de monitorización de Azure.

Permite recopilar información sobre:

- Recursos Azure.
- Máquinas virtuales.
- Redes.
- Aplicaciones.

Los datos recopilados pueden utilizarse para:

- Alertas.
- Informes.
- Dashboards.
- Consultas.

---

# 2. Metrics

Las **Metrics** son datos numéricos recopilados en tiempo real.

Ejemplos:

- CPU.
- Memoria.
- IOPS.
- Latencia.
- Throughput.

Se utilizan principalmente para crear alertas.

---

# 3. Logs

Los **Logs** contienen información detallada sobre los recursos.

Permiten realizar consultas mediante:

**Kusto Query Language (KQL)**

Los Logs se almacenan en un **Log Analytics Workspace**.

---

# 4. Log Analytics Workspace

El **Log Analytics Workspace (LAW)** es el repositorio donde Azure Monitor almacena los registros.

Puede recibir información de:

- Máquinas virtuales.
- Azure Firewall.
- NSG Flow Logs.
- Azure Backup.
- Azure Monitor Agent.

---

# 5. Diagnostic Settings

Los **Diagnostic Settings** permiten enviar Logs y Metrics de un recurso a distintos destinos.

Destinos soportados:

- Log Analytics Workspace.
- Storage Account.
- Event Hub.

Es el primer paso para recopilar logs de muchos servicios de Azure.

---

# 6. Azure Monitor Agent (AMA)

El **Azure Monitor Agent (AMA)** es el agente recomendado por Microsoft para recopilar datos de máquinas virtuales.

Sustituye a los agentes antiguos.

Puede recopilar:

- Eventos.
- Performance Counters.
- Syslog.
- Logs personalizados.

---

# 7. Data Collection Rules (DCR)

Las **Data Collection Rules (DCR)** definen:

- Qué datos recopilar.
- Desde qué máquinas.
- A qué destino enviarlos.

AMA necesita una DCR para funcionar.

---

# 8. Alerts

Las **Alert Rules** permiten generar alertas automáticamente cuando se cumple una condición.

Ejemplos:

- CPU > 80%.
- Disco lleno.
- VM detenida.
- Error en Backup.

---

# 9. Action Groups

Un **Action Group** define qué ocurre cuando se dispara una alerta.

Acciones habituales:

- Email.
- SMS.
- Push Notification.
- Webhook.
- Azure Function.
- Logic App.

Las Alert Rules utilizan uno o varios Action Groups.

---

# 10. Alert Processing Rules

Las **Alert Processing Rules** permiten modificar el comportamiento de las alertas después de generarse.

Ejemplos:

- Silenciar alertas durante mantenimiento.
- Cambiar el Action Group.
- Filtrar determinadas alertas.

No modifican la Alert Rule.

---

# 11. Activity Log

El **Activity Log** registra operaciones realizadas sobre recursos Azure.

Ejemplos:

- Crear recursos.
- Eliminar recursos.
- Modificar configuraciones.
- Asignar permisos RBAC.

No registra eventos del sistema operativo.

---

# 12. Azure Monitor vs Activity Log

| Azure Monitor Logs | Activity Log |
|--------------------|--------------|
| Rendimiento | Operaciones Azure |
| Eventos del SO | Operaciones ARM |
| Métricas | Auditoría |

---

# 13. Connection Monitor

**Connection Monitor** comprueba la conectividad entre dos puntos.

Puede monitorizar:

- VM ↔ VM.
- VM ↔ Storage.
- VM ↔ SQL.
- Azure ↔ On-Premises.

Es una herramienta de **Network Watcher**.

---

# 14. Network Watcher

**Network Watcher** proporciona herramientas de diagnóstico de red.

Incluye:

- Connection Monitor.
- IP Flow Verify.
- Next Hop.
- Effective Security Rules.
- NSG Flow Logs.

---

# 15. NSG Flow Logs

Los **NSG Flow Logs** registran las conexiones que atraviesan un Network Security Group.

Se almacenan inicialmente en un:

**Storage Account**

Posteriormente pueden analizarse mediante:

**Traffic Analytics**

---

# 16. Azure Backup Reports

Los informes de Azure Backup utilizan:

- Azure Monitor.
- Log Analytics Workspace.

Permiten consultar:

- Backups correctos.
- Errores.
- Retención.
- Cumplimiento.

---

# 17. Buenas prácticas

Microsoft recomienda:

- Utilizar **Azure Monitor Agent (AMA)**.
- Configurar **Diagnostic Settings**.
- Centralizar los logs en un **Log Analytics Workspace**.
- Crear **Action Groups** reutilizables.
- Utilizar **Alert Processing Rules** para periodos de mantenimiento.

---

# Preguntas trampa del AZ-104

✅ Las **Metrics** son datos numéricos y casi en tiempo real.

✅ Los **Logs** se almacenan en un **Log Analytics Workspace**.

✅ Los **Diagnostic Settings** permiten enviar datos a **LAW**, **Storage Account** o **Event Hub**.

✅ **Azure Monitor Agent (AMA)** necesita una **Data Collection Rule (DCR)**.

✅ Un **Action Group** define qué hacer cuando se dispara una alerta.

✅ Las **Alert Processing Rules** modifican el tratamiento de una alerta, **no la condición que la genera**.

✅ El **Activity Log** registra operaciones realizadas sobre recursos Azure, no eventos del sistema operativo.

✅ **Connection Monitor** forma parte de **Network Watcher**.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Azure Monitor | Monitorización centralizada |
| Metrics | Datos numéricos |
| Logs | Información detallada |
| Log Analytics Workspace | Almacena Logs |
| Diagnostic Settings | Envía Logs y Metrics |
| Azure Monitor Agent | Agente recomendado |
| Data Collection Rule | Configura AMA |
| Alert Rule | Genera alertas |
| Action Group | Acción tras la alerta |
| Alert Processing Rule | Modifica el tratamiento |
| Activity Log | Auditoría de Azure |
| Connection Monitor | Comprueba conectividad |
| Network Watcher | Diagnóstico de red |
| NSG Flow Logs | Registro de tráfico |
| Azure Backup Reports | Basados en Azure Monitor |
