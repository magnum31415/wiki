[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure IT Service Management Connector (ITSM Connector) - AZ-104

---

# Qué es ITSM Connector

Azure IT Service Management Connector (ITSM Connector) permite integrar:

```text
Azure Monitor / Azure Alerts
```

con herramientas ITSM externas.

---

# Qué significa ITSM

```text
ITSM
```

=

```text
IT Service Management
```

---

# Objetivo principal

Permite convertir automáticamente:

- alertas Azure
- incidents
- monitor events

en:

- tickets
- incidents
- work items

dentro de plataformas ITSM.

---

# Qué integra normalmente

| Azure | Plataforma ITSM |
|---|---|
| Azure Monitor Alerts | ServiceNow |
| Azure Monitor Alerts | System Center Service Manager |
| Azure Monitor Alerts | Provance |
| Azure Monitor Alerts | Cherwell |

---

# Flujo típico

```text
Azure Monitor Alert
        ↓
ITSM Connector
        ↓
Ticket / Incident
        ↓
ITSM Platform
```

---

# Ejemplo práctico

## Escenario

Una VM:

```text
CPU > 95%
```

↓

Azure Monitor genera alerta.

↓

ITSM Connector crea automáticamente:

```text
INC000123
```

en:

```text
ServiceNow
```

---

# Qué permite automatizar

| Acción | Compatible |
|---|---|
| Crear incidentes | ✅ |
| Crear tickets | ✅ |
| Crear work items | ✅ |
| Sincronizar alertas | ✅ |

---

# Componentes relacionados

| Servicio | Función |
|---|---|
| Azure Monitor | Detecta alertas |
| Action Groups | Ejecutan acciones |
| ITSM Connector | Integra con ITSM |
| ServiceNow/Cherwell/etc | Gestión tickets |

---

# Relación con Action Groups

Azure Monitor normalmente usa:

```text
Action Groups
```

↓

para ejecutar acciones cuando ocurre una alerta.

Una de esas acciones puede ser:

```text
ITSM
```

---

# Arquitectura conceptual

```text
Azure Resource
      ↓
Azure Monitor Alert
      ↓
Action Group
      ↓
ITSM Connector
      ↓
ServiceNow / ITSM Tool
```

---

# Casos típicos de uso

| Escenario | Resultado |
|---|---|
| VM caída | Ticket automático |
| Alta CPU | Incident automático |
| Servicio crítico down | Escalado automático |
| Alertas seguridad | Work item SOC |

---

# Importante examen

ITSM Connector:

```text
NO monitoriza recursos
```

↓

La monitorización la hace:

```text
Azure Monitor
```

---

# Qué hace realmente

ITSM Connector:

```text
integra alertas Azure con plataformas ITSM
```

---

# Diferencia importante examen

| Servicio | Función |
|---|---|
| Azure Monitor | Detectar eventos |
| Action Groups | Ejecutar acciones |
| ITSM Connector | Integración ITSM |
| Logic Apps | Automatización workflows |

---

# Trampas típicas AZ-104

## Trampa 1

Pensar que ITSM Connector genera alertas.

❌ Incorrecto.

↓

Las genera Azure Monitor.

---

# Trampa 2

Pensar que reemplaza ServiceNow.

❌ Incorrecto.

↓

Solo integra.

---

# Trampa 3

Confundir:

```text
ITSM Connector
```

con:

```text
Azure Automation
```

---

# Servicios ITSM comunes

| Plataforma | Compatible |
|---|---|
| ServiceNow | ✅ |
| System Center Service Manager | ✅ |
| Cherwell | ✅ |
| Provance | ✅ |

---

# Tabla resumen examen

| Feature | ITSM Connector |
|---|---|
| Integra Azure Alerts | ✅ |
| Crea tickets automáticos | ✅ |
| Requiere Azure Monitor | ✅ |
| Genera alertas | ❌ |
| Automatización ITSM | ✅ |

---

# Reglas rápidas AZ-104

```text
ITSM Connector integrates Azure Monitor alerts with ITSM tools.
```

```text
Azure Monitor generates alerts.
```

```text
ITSM Connector creates incidents or tickets in external systems.
```

---

# Frases clave AZ-104

```text
ITSM stands for IT Service Management.
```

```text
ITSM Connector integrates Azure Monitor with ticketing systems.
```

```text
ServiceNow is a common ITSM integration target.
```
