[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# 🔄 Azure Logic Apps

---

# 📑 Índice

- [🔎 ¿Qué es?](#-qué-es)
- [🎯 ¿Qué resuelve?](#-qué-resuelve)
- [🧩 Cómo funciona](#-cómo-funciona)
- [🔌 Conectores](#-conectores)
- [🎨 Logic Apps Designer](#-logic-apps-designer)
  - [🔧 Elementos del Designer](#-elementos-del-logic-apps-designer)
- [🏗 Tipos de Logic Apps](#-tipos-de-logic-apps)
  - [📊 Consumption vs Standard](#-azure-logic-apps--consumption-vs-standard)
- [⚙️ Workflow Settings](#️-workflow-settings-en-azure-logic-apps)
  - [🔢 Concurrency por defecto](#-concurrency-por-defecto)
- [🎯 Cuándo usar Logic Apps](#-cuándo-usar-logic-apps)
- [🧠 Claves examen AZ-305](#-clave-examen-az-305)
- [🧠 Regla mental final](#-regla-mental)

---

# 🔎 ¿Qué es?

Azure Logic Apps es un servicio **serverless de automatización e integración** que permite crear flujos de trabajo (workflows) para conectar aplicaciones, datos y servicios sin escribir apenas código.

👉 Está orientado a **integración entre sistemas** y automatización de procesos.

---

# 🎯 ¿Qué resuelve?

- Automatizar procesos empresariales
- Integrar sistemas cloud y on-prem
- Orquestar APIs y servicios
- Reaccionar a eventos (event-driven)

Ejemplo típico:
- Cuando llega un email → guardar adjunto en Blob → enviar notificación Teams → registrar en base de datos.

---

# 🧩 Cómo funciona

Una Logic App se compone de:

## 1️⃣ Trigger
Evento que inicia el flujo.  
Solo puede haber **un trigger por Logic App**.

Ejemplos:
- When an HTTP request is received
- When a blob is created
- When an email arrives
- Recurrence (timer)

## 2️⃣ Actions
Pasos que se ejecutan después del trigger.

Ejemplos:
- Crear archivo
- Llamar API
- Insertar en SQL
- Enviar correo
- Postear en Teams

---

# 🔌 Conectores

Logic Apps tiene cientos de conectores predefinidos:

- Microsoft 365
- SharePoint
- SQL Server
- Azure Storage
- Service Bus
- Salesforce
- SAP
- HTTP / REST APIs

👉 Permite integración sin desarrollar código complejo.

---

# 🎨 Logic Apps Designer

## 🔎 ¿Qué es?

Es la **interfaz visual (low-code)** para crear y configurar workflows de Azure Logic Apps.

Permite:

- Arrastrar y configurar triggers
- Añadir acciones
- Definir condiciones (if/else)
- Manejar bucles (for each)
- Ver el flujo en formato gráfico

## 🧠 Qué hace el Designer

- Genera automáticamente el JSON del workflow
- Simplifica la configuración
- Reduce necesidad de programación manual

Internamente, una Logic App es un archivo JSON,
pero el Designer lo abstrae visualmente.

---

# 🔧 Elementos del Logic Apps Designer

| Elemento | Qué hace |
|----------|----------|
| Trigger | Inicia el flujo |
| Action | Ejecuta una tarea |
| Condition | Lógica if/else |
| For each | Iteración |
| Switch | Múltiples decisiones |
| Scope | Agrupar acciones |
| Variables | Guardar datos temporales |
| HTTP | Llamar APIs |

### Control Blocks

- **For each** → Itera sobre colecciones
- **Until** → Ejecuta hasta cumplir condición
- **Scope** → Agrupa acciones
- **Switch** → Múltiples casos
- **Variables** → Guardar datos temporales
- **HTTP** → Llamar APIs externas

---

# 🏗 Tipos de Logic Apps

## 1️⃣ Consumption (Multitenant)

- Pago por ejecución
- Escalado automático
- Ideal para flujos intermitentes
- SLA 99.9%

## 2️⃣ Standard (Single-tenant)

- Más control
- Mejor rendimiento
- Integración con VNet
- Más parecido a App Service
- SLA 99.95%

---

# 📊 Azure Logic Apps – Consumption vs Standard

| Característica | Consumption | Standard |
|---------------|-------------|----------|
| Modelo de ejecución | Multitenant | Single-tenant |
| Modelo de pago | Pago por ejecución | Pago por instancia |
| Escalado | Automático | Manual / automático |
| VNet Integration | Limitada | Nativa |
| Built-in connectors | No | Sí |
| Acceso a sistema de archivos | No | Sí |
| Desarrollo local | No | Sí |
| Integration Account | Externo | Integrable |
| Cold start | Puede existir | Menor impacto |
| Casos ideales | Flujos ligeros | Integraciones críticas |

---

# ⚙️ Workflow Settings en Azure Logic Apps

## 🔎 ¿Qué son?

Configuraciones avanzadas que controlan cómo se ejecuta el workflow.

No definen qué hace el flujo,  
sino cómo se comporta.

---

## 🧠 Configuraciones principales

### 1️⃣ Concurrency
Controla ejecuciones paralelas.

### 2️⃣ Retry Policy
Controla reintentos ante fallo.

### 3️⃣ Timeout
Tiempo máximo de ejecución.

### 4️⃣ Secure Inputs / Outputs
Protección de datos sensibles.

### 5️⃣ Run History Retention
Tiempo de retención de logs.

### 6️⃣ Trigger Conditions
Condiciones adicionales en el trigger.

### 7️⃣ Managed Identity
Autenticación segura sin secretos.

### 8️⃣ Integration Account
Integración B2B (EDI, AS2, XSLT).

---

# 🔢 Concurrency por defecto

## Consumption

- Ejecuta múltiples instancias en paralelo por defecto.
- Escala automáticamente.
- Límite configurable hasta 50 ejecuciones paralelas.

## Standard

- Depende del App Service Plan.
- No hay valor fijo universal.

---

# 🎯 Cuándo usar Logic Apps

- Integración entre sistemas
- Automatización empresarial
- Orquestación de APIs
- Workflows sin desarrollar backend completo

---

# 🧠 Clave examen AZ-305

| Problema | Setting |
|----------|--------|
| Control de carga | Concurrency |
| API inestable | Retry policy |
| Seguridad datos | Secure inputs/outputs |
| Acceso sin secretos | Managed Identity |
| Escenario B2B | Integration Account |

---

# 🧠 Regla mental

Logic Apps = Automatización sin código  
Designer = Editor visual  
Workflow Settings = Cómo se ejecuta el flujo
