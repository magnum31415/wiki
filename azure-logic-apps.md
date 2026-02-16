
# 🔄 Azure Logic Apps

## 🔎 ¿Qué es?

Azure Logic Apps es un servicio **serverless de automatización e integración** que permite crear flujos de trabajo (workflows) para conectar aplicaciones, datos y servicios sin escribir apenas código.

👉 Está orientado a **integración entre sistemas** y automatización de procesos.

---

## 🎯 ¿Qué resuelve?

- Automatizar procesos empresariales
- Integrar sistemas cloud y on-prem
- Orquestar APIs y servicios
- Reaccionar a eventos (event-driven)

Ejemplo típico:
- Cuando llega un email → guardar adjunto en Blob → enviar notificación Teams → registrar en base de datos.

---

## 🧩 Cómo funciona

Una Logic App se compone de:

### 1️⃣ Trigger
Evento que inicia el flujo.
Ejemplos:
- When an HTTP request is received
- When a blob is created
- When an email arrives
- Recurrence (timer)

### 2️⃣ Actions
Pasos que se ejecutan después del trigger.
Ejemplos:
- Crear archivo
- Llamar API
- Insertar en SQL
- Enviar correo
- Postear en Teams

---

## 🔌 Conectores

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

---

## 🧠 Qué hace el Designer

- Genera automáticamente el JSON del workflow
- Simplifica la configuración
- Reduce necesidad de programación manual

Internamente, una Logic App es un archivo JSON,
pero el Designer lo abstrae visualmente.


# 🔧 Elementos del Logic Apps Designer

En **Azure Logic Apps Designer**, construyes un workflow usando bloques visuales.  
Cada bloque tiene un tipo y una función específica.

---

####  🧠 Resumen rápido

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

**🎯 Regla mental examen AZ-305**

Trigger → Evento  
Action → Tarea  
Condition → Decisión  
Control → Lógica avanzada

### 1️⃣ Trigger (Disparador)

#### 🔎 ¿Qué es?
Es el evento que **inicia el workflow**.

Solo puede haber **un trigger por Logic App**.

#####  📌 Ejemplos
- When an HTTP request is received
- When a blob is created
- When a new email arrives
- Recurrence (cada X minutos)

👉 Sin trigger, el flujo no empieza.


### 2️⃣ Action (Acción)

#### 🔎 ¿Qué es?
Es un paso que ejecuta una tarea después del trigger.

Puedes tener múltiples actions en secuencia.

#### 📌 Ejemplos
- Create blob
- Insert row in SQL
- Send email
- Call HTTP endpoint
- Post message in Teams

👉 Son las “instrucciones” del flujo.



### 3️⃣ Condition (Control de condición)

#### 🔎 ¿Qué es?
Bloque de control lógico tipo **if/else**.

Evalúa una condición y ejecuta acciones distintas según el resultado.

#### 📌 Ejemplo
Si `Amount > 1000`  
→ Enviar aprobación  
Si no  
→ Procesar automáticamente

👉 Permite tomar decisiones dentro del flujo.



### 4️⃣ Control (Bloques de control)

Son estructuras que organizan la lógica del flujo.

#### 🔹 For each
Itera sobre una colección (array).

Ejemplo:
- Para cada archivo en una carpeta → copiarlo.

#### 🔹 Until
Ejecuta acciones hasta que se cumpla una condición.

#### 🔹 Scope
Agrupa varias acciones dentro de un bloque lógico.


### 5️⃣ Switch

#### 🔎 ¿Qué es?
Alternativa avanzada a Condition.

Permite múltiples casos (como switch/case en programación).

Ejemplo:
Según el tipo de pedido:
- Case A → Flujo 1
- Case B → Flujo 2
- Default → Flujo estándar

### 6️⃣ Variables

Permiten almacenar valores temporales dentro del flujo.

Tipos comunes:
- String
- Integer
- Array
- Boolean

Se usan para:
- Acumular resultados
- Guardar estados intermedios



### 7️⃣ HTTP

Acción especial para:

- Llamar APIs externas
- Consumir servicios REST
- Integrar sistemas personalizados

Muy usada en integración enterprise.







---

[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# 🏗 Tipos de Logic Apps

## 1️⃣ Consumption (Multitenant)

- Pago por ejecución
- Escalado automático
- Ideal para flujos intermitentes

## 2️⃣ Standard (Single-tenant)

- Más control
- Mejor rendimiento
- Integración con VNet
- Más parecido a App Service

---

# 🧠 Diferencia rápida para examen AZ-305

| Concepto | Qué es |
|----------|--------|
| Azure Logic Apps | Servicio serverless para automatizar e integrar sistemas |
| Logic Apps Designer | Interfaz visual para crear los workflows |

---

# 🎯 Cuándo usar Logic Apps

- Integración entre sistemas
- Automatización empresarial
- Orquestación de APIs
- Workflows sin desarrollar backend completo

---

# 🧠 Regla mental

Logic Apps = Automatización sin código  
Designer = Editor visual del flujo
