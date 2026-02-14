# Servicios Azure

| Servicio                    | Para qué sirve realmente                                  |
| --------------------------- | --------------------------------------------------------- |
| **Azure Service Bus**       | Mensajería empresarial entre sistemas (backend ↔ backend) |
| **Azure Notification Hubs** | Enviar notificaciones push a móviles y dispositivos       |



## 📦 Azure Service Bus – Resumen para estudiar

### 🔎 ¿Qué es?
Servicio de mensajería empresarial (Enterprise Messaging) para conectar aplicaciones y servicios de forma desacoplada.

Permite comunicación confiable entre sistemas distribuidos.

### 🧱 Modelos de mensajería

- 📬 **Queues (colas)** → Comunicación punto a punto
- 📢 **Topics + Subscriptions** → Modelo publish/subscribe
- 🔁 Mensajes ordenados, con entrega garantizada
- ⏱ Soporte para reintentos y dead-letter queues

### 🚀 Características clave

- ✅ Desacopla aplicaciones
- ✅ Alta fiabilidad
- ✅ Entrega garantizada
- ✅ Transacciones
- ✅ Soporte para patrones enterprise

### 🎯 Casos de uso típicos

- Integración entre microservicios
- Procesamiento asíncrono
- Workflows distribuidos
- Sistemas financieros o críticos
- Arquitecturas event-driven

### ❌ No es para

- Enviar notificaciones push móviles
- Segmentación por usuarios finales
- Localización de notificaciones

### 🧠 Idea clave para examen

**Azure Service Bus = Mensajería entre aplicaciones (colas y publish/subscribe).**
No está orientado a dispositivos móviles.


## 🏢 Azure Stack Hub – Resumen para estudiar

### 🔎 ¿Qué es?
Extensión de Azure que permite ejecutar servicios Azure **en tu propio datacenter** (on-premises).

Pensado para escenarios híbridos.

### 🏗 Qué permite

- Ejecutar máquinas virtuales
- Usar servicios PaaS seleccionados
- Mantener consistencia con Azure público
- Cumplir requisitos regulatorios o de latencia

### 🎯 Casos de uso típicos

- Requisitos de soberanía de datos
- Entornos sin conectividad constante a Azure
- Edge computing
- Organizaciones gubernamentales

### ❌ No es para

- Notificaciones push móviles
- Mensajería entre apps
- Escenarios puramente cloud

### 🧠 Idea clave para examen

**Azure Stack Hub = Azure en tu datacenter (híbrido).**


---------------------------------------------------------------------


## 🌐 Azure Arc – Resumen para estudiar

### 🔎 ¿Qué es?
Servicio que extiende la gestión de Azure a recursos que están:

- On-premises
- En otras nubes (AWS, GCP)
- En edge locations

No mueve recursos a Azure; solo los gestiona desde Azure.

---

### 🧱 Qué permite gestionar

- Servidores
- Kubernetes clusters
- Bases de datos
- Aplicar Azure Policy
- Seguridad centralizada

---

### 🎯 Casos de uso típicos

- Gobierno multi-cloud
- Gestión centralizada
- Aplicación de políticas en híbrido
- Inventario unificado de recursos

---

### ❌ No es para

- Enviar notificaciones push
- Mensajería empresarial
- Comunicación directa entre aplicaciones

---

### 🧠 Idea clave para examen

**Azure Arc = Gestión y gobierno centralizado de infra híbrida y multi-cloud.**

## Azure Notification Hubs
Servicio altamente escalable para enviar **notificaciones push masivas** a dispositivos móviles:

- iOS (APNs)
- Android (FCM / antes GCM)
- Windows (WNS)
- Kindle y otros

Permite enviar millones de notificaciones rápidamente con muy poco código.

---

### 🚀 Características clave

- ✅ Alta escalabilidad
- ✅ Envío multiplataforma
- ✅ Segmentación por usuarios o grupos
- ✅ Personalización de mensajes
- ✅ Integración directa con servicios de notificación nativos

---

### 🎯 Casos de uso típicos

- 📰 Breaking news a millones de usuarios
- 📍 Cupones basados en ubicación
- 🏟 Notificaciones de eventos (deportes, finanzas, gaming)
- 📢 Campañas promocionales
- 💼 Alertas empresariales (mensajes, tareas)
- 🔐 Envío de códigos para MFA

### 🧠 Idea clave para examen

**Azure Notification Hubs = Servicio para enviar notificaciones push masivas a dispositivos móviles.**
