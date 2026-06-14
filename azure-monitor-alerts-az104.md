[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Monitor Alerts and Alert Processing Rules (AZ-104)

![Azure-Monitor-Alerting-Architecture](./img/azure/Azure-Monitor-Alerting-Architecture)

## Conceptos básicos

En Azure Monitor intervienen tres componentes principales:

- Alert Rule
- Action Group
- Alert Processing Rule

Es fundamental distinguir claramente su función, ya que es una pregunta muy habitual en el AZ-104.

---

# Arquitectura

```text
Azure Resource
       │
       ▼
Event / Metric / Log
       │
       ▼
Alert Rule
       │
       ▼
Alert Created
       │
       ▼
Alert Processing Rule
       │
       ▼
Action Group
       │
       ▼
Email / SMS / Webhook / Logic App / Function
```

---

# Alert Rule

Una Alert Rule es la encargada de detectar un evento o condición y generar una alerta.

Puede basarse en:

- Metrics
- Logs
- Activity Log
- Resource Health
- Service Health

Ejemplo:

```text
Condition:
CPU > 80%
        │
        ▼
Alert Created
```

Otro ejemplo:

```text
Condition:
Administrative Operation
        │
        ▼
Create Resource Group
        │
        ▼
Alert Created
```

---

# Action Group

Un Action Group define qué acciones ejecutar cuando una alerta se dispara.

Puede realizar acciones como:

- Email
- SMS
- Push Notification
- Voice Call
- Webhook
- Azure Function
- Logic App
- Automation Runbook

Ejemplo:

```text
Alert
    │
    ▼
Action Group
    │
    ├── Email
    ├── SMS
    ├── Webhook
    └── Logic App
```

Ejemplo concreto:

```text
Action Group

Name:
Action1
        │
        ▼
Send Email
        │
        ▼
admin1@company.com
```

---

# Alert Processing Rule

Una Alert Processing Rule modifica el comportamiento de una alerta DESPUÉS de que ésta haya sido creada.

No detecta eventos.

No crea alertas.

Simplemente modifica qué ocurre con ellas.

Puede:

- Suppress notifications
- Apply Action Groups
- Ejecutarse según un horario
- Aplicarse a un Scope determinado

Arquitectura:

```text
Alert Created
        │
        ▼
Alert Processing Rule
        │
        ├── Suppress Notifications
        └── Add Action Group
```

---

# Suppress Notifications

Esta es una de las preguntas más frecuentes del AZ-104.

Supongamos:

```text
Alert Rule

Administrative Operations
        │
        ▼
Alert Created
```

Existe además:

```text
Alert Processing Rule

Rule Type:
Suppress Notifications

Start:
10 Sep

End:
13 Sep
```

Durante ese periodo:

```text
Administrative Operation
        │
        ▼
Alert Created ✅
        │
        ▼
Notification Sent ❌
```

La alerta:

- existe
- aparece en Azure Portal
- aparece en Azure Monitor

Simplemente NO se envían:

- emails
- SMS
- webhooks

---

# Ejemplo completo

## Configuración

```text
ProdSub1
        │
        ▼
Alert Rule

Scope:
All Resource Groups

Condition:
Administrative Operations

Action:
Action1
```

Action1:

```text
Action Group
        │
        ▼
Email
        │
        ▼
admin1@company.com
```

Existe además:

```text
Alert Processing Rule

Suppress Notifications

10 Sep

↓

13 Sep
```

El día 11 de septiembre:

```text
Create Resource Group
        │
        ▼
Administrative Operation
        │
        ▼
Alert Created ✅
        │
        ▼
Alert appears in Azure Portal ✅
        │
        ▼

Email sent ❌
```

---

# Scope

Las Alert Rules pueden aplicarse a distintos ámbitos:

- Subscription
- Resource Group
- Resource
- Multiple Resources

Ejemplo:

```text
Subscription
        │
        ├── RG1
        ├── RG2
        └── RG3
```

Si el Scope es:

```text
All Resource Groups
including future Resource Groups
```

cualquier nuevo Resource Group quedará automáticamente cubierto.

---

# Schedule en Alert Processing Rules

Las Alert Processing Rules pueden activarse únicamente durante determinadas fechas u horarios.

Ejemplo:

```text
10 Sep
     │
     ├───────────────────────┐
     │                       │
     ▼                       ▼

Notifications Suppressed

until

13 Sep
```

Fuera de ese periodo:

```text
Alert
    │
    ▼
Email ✅
```

---

# Resumen

| Componente | Función |
|------------|----------|
| Alert Rule | Detecta eventos y crea alertas |
| Action Group | Ejecuta acciones (Email, SMS, Webhook...) |
| Alert Processing Rule | Modifica el tratamiento de las alertas existentes |
| Suppress Notifications | Suprime las notificaciones pero NO elimina la alerta |
| Scope | Determina sobre qué recursos aplica |
| Schedule | Determina cuándo aplica la regla |

---

# Trampas típicas del AZ-104

## Trampa 1

Pensar que Suppress Notifications evita crear la alerta.

❌ Incorrecto

```text
Event
    │
    ▼
Alert Created ✅
    │
    ▼
No Email ❌
```

---

## Trampa 2

Pensar que Alert Processing Rule detecta eventos.

❌ Incorrecto

Detecta eventos:

```text
Alert Rule
```

Modifica el comportamiento:

```text
Alert Processing Rule
```

---

## Trampa 3

Pensar que Action Group genera alertas.

❌ Incorrecto

```text
Alert Rule
        │
        ▼
Alert
        │
        ▼
Action Group
```

El Action Group solo ejecuta acciones.

---

# Chuleta AZ-104

| Si lees... | Piensa en... |
|------------|--------------|
| CPU > 80% | Alert Rule |
| Administrative Operation | Alert Rule |
| Create Alert | Alert Rule |
| Send Email | Action Group |
| Send SMS | Action Group |
| Webhook | Action Group |
| Logic App | Action Group |
| Suppress Notifications | Alert Processing Rule |
| Scheduled suppression | Alert Processing Rule |
| Alert visible in Portal | ✅ Sí, aunque se supriman las notificaciones |
| Email enviado | ❌ Puede estar suprimido |

---

# Regla rápida para memorizar

```text
Event
    │
    ▼
Alert Rule
    │
    ▼
Alert Created ✅
    │
    ▼
Alert Processing Rule
    │
    ├── Suppress Email ❌
    ├── Suppress SMS ❌
    └── Add Action Group
    │
    ▼
Alert still exists in Azure Portal ✅
```
