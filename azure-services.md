[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# 📚 Resumen comparativo de servicios Azure

## 📑 Índice

- [Tabla comparativa rápida](#tabla-comparativa-rápida)
- [Azure Arc](#azure-arc)
- [Azure Event Hubs](#azure-event-hubs)
- [Azure Notification Hubs](#azure-notification-hubs)
- [Azure Service Bus](#azure-service-bus)
- [Azure Stack Hub](#azure-stack-hub)

---

# 📊 Tabla comparativa rápida

| Servicio | Resumen en pocas palabras | Propósito principal | Orientado a |
|-----------|--------------------------|--------------------|-------------|
| Azure Arc | Gestión híbrida y multi-cloud | Gobierno centralizado | Infraestructura |
| Azure Event Hubs | Ingesta masiva de eventos en tiempo real | Streaming de datos | Big Data / Telemetría |
| Azure Notification Hubs | Push notifications móviles | Notificaciones masivas | Usuarios finales |
| Azure Service Bus | Mensajería empresarial | Comunicación entre aplicaciones | Backend |
| Azure Stack Hub | Azure en tu datacenter | Extensión híbrida | Infraestructura |

---

## Azure Arc

### 🔎 ¿Qué es?
Servicio que permite gestionar recursos fuera de Azure (on-premises, multi-cloud, edge) desde el portal de Azure.

### 🎯 Para qué se usa
- Gobierno y cumplimiento en entornos híbridos  
- Gestión centralizada de servidores y Kubernetes  
- Aplicar Azure Policy fuera de Azure  

### ❌ No es para
Mensajería ni notificaciones.

### 🧠 Idea clave examen
**Azure Arc = Gestión y gobierno híbrido/multi-cloud.**

---

## Azure Event Hubs

### 🔎 ¿Qué es?
Servicio de **ingesta masiva de eventos en tiempo real** (streaming platform).

Diseñado para capturar millones de eventos por segundo.

### 🎯 Para qué se usa
- Telemetría IoT  
- Logs de aplicaciones  
- Streaming de datos  
- Integración con Spark, Databricks o Synapse  

### 🚀 Características clave
- Alta escalabilidad  
- Arquitectura basada en particiones  
- Integración con análisis en tiempo real  
- Retención temporal de eventos  

### ❌ No es para
Colas empresariales con transacciones o workflows complejos.

### 🧠 Idea clave examen
**Event Hubs = Streaming masivo de eventos.**

---

## Azure Notification Hubs

### 🔎 ¿Qué es?
Servicio altamente escalable para enviar **notificaciones push masivas a dispositivos móviles** (iOS, Android, Windows, etc.).

### 🎯 Para qué se usa
- Breaking news  
- Promociones  
- Alertas empresariales  
- Códigos MFA  
- Segmentación por usuarios o grupos  

### ❌ No es para
Comunicación entre microservicios backend.

### 🧠 Idea clave examen
**Notification Hubs = Push notifications móviles.**

---

## Azure Service Bus

### 🔎 ¿Qué es?
Servicio de **mensajería empresarial** para comunicación desacoplada entre aplicaciones.

### 🧱 Modelos de mensajería
- Queues (punto a punto)  
- Topics + Subscriptions (publish/subscribe)  

### 🎯 Para qué se usa
- Integración de microservicios  
- Procesamiento asíncrono  
- Workflows distribuidos  
- Sistemas críticos  

### ❌ No es para
Streaming masivo de datos ni push móvil.

### 🧠 Idea clave examen
**Service Bus = Mensajería confiable entre aplicaciones.**

---

## Azure Stack Hub

### 🔎 ¿Qué es?
Extensión de Azure que permite ejecutar servicios Azure en tu propio datacenter.

### 🎯 Para qué se usa
- Soberanía de datos  
- Requisitos regulatorios  
- Edge computing  
- Escenarios híbridos  

### ❌ No es para
Mensajería ni notificaciones.

### 🧠 Idea clave examen
**Azure Stack Hub = Azure on-premises (híbrido).**
