[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# 📚 Resumen comparativo de servicios Azure

## 📑 Índice

- [Tabla comparativa rápida](#tabla-comparativa-rápida)
- [Azure Arc](#azure-arc)
- [Azure Data Box Gateway](#azure-data-box-gateway)
- [Azure Data Lake Storage](#azure-data-lake-storage)
- [Azure Event Hubs](#azure-event-hubs)
- [Azure Import/Export service](#azure-importexport-service)
- [Azure Notification Hubs](#azure-notification-hubs)
- [Azure Service Bus](#azure-service-bus)
- [Azure Stack Hub](#azure-stack-hub)
- [Azure Storage Sync](#azure-storage-sync)

---

# 📊 Tabla comparativa rápida

| Servicio | Resumen en pocas palabras | Propósito principal | Orientado a |
|-----------|--------------------------|--------------------|-------------|
| Azure Arc | Gestión híbrida y multi-cloud | Gobierno centralizado | Infraestructura |
| Azure Data Box Gateway | Pasarela híbrida con caché local | Transferir datos a Azure Storage | Híbrido / Migración / DR |
| Azure Data Lake Storage | Data lake escalable para analítica | Almacenamiento masivo estructurado y no estructurado | Big Data / Analytics |
| Azure Event Hubs | Ingesta masiva de eventos en tiempo real | Streaming de datos | Big Data / Telemetría |
| Azure Import/Export service | Transferencia física de datos | Migraciones offline con discos | Migración masiva |
| Azure Notification Hubs | Push notifications móviles | Notificaciones masivas | Usuarios finales |
| Azure Service Bus | Mensajería empresarial | Comunicación entre aplicaciones | Backend |
| Azure Stack Hub | Azure en tu datacenter | Extensión híbrida | Infraestructura |
| Azure Storage Sync | Sincronización de file servers con Azure | Extender almacenamiento on-prem a la nube | Híbrido / Files |

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

## Azure Data Box Gateway

### 🔎 ¿Qué es?
Dispositivo **virtual** que permite transferir datos a Azure Storage usando **NFS o SMB** a través de red.

Incluye **caché local configurable** para mejorar rendimiento.

### 🎯 Para qué se usa
- Archivado en la nube  
- Disaster Recovery  
- Migraciones híbridas  
- Procesamiento de datos a escala cloud  

### 🧠 Idea clave examen
**Data Box Gateway = Pasarela híbrida con caché para enviar datos a Azure Storage.**

---

## Azure Data Lake Storage

### 🔎 ¿Qué es?
Servicio de almacenamiento optimizado para **Big Data y analítica avanzada**.

Basado en Azure Blob Storage (Gen2) con jerarquía de directorios y control POSIX.

### 🎯 Para qué se usa
- Data lakes empresariales  
- Procesamiento con Spark, Databricks o Synapse  
- Machine Learning  
- Grandes volúmenes de datos estructurados y no estructurados  

### 🚀 Características clave
- Escalabilidad masiva  
- Control de acceso granular  
- Integración nativa con herramientas de análisis  

### ❌ No es para
Sincronización de file servers tradicionales.

### 🧠 Idea clave examen
**Data Lake Storage = Almacenamiento masivo optimizado para analítica.**

---

## Azure Event Hubs

### 🔎 ¿Qué es?
Servicio de **ingesta masiva de eventos en tiempo real**.

Diseñado para capturar millones de eventos por segundo.

### 🎯 Para qué se usa
- Telemetría IoT  
- Logs de aplicaciones  
- Streaming de datos  

### 🧠 Idea clave examen
**Event Hubs = Streaming masivo de eventos.**

---

## Azure Import/Export service

### 🔎 ¿Qué es?
Servicio que permite transferir grandes volúmenes de datos a Azure enviando **discos físicos** a Microsoft.

### 🎯 Para qué se usa
- Migraciones iniciales masivas  
- Subida o descarga offline de datos  
- Entornos con ancho de banda limitado  

### 🚀 Características clave
- Uso de discos cifrados  
- Importación y exportación de Azure Storage  
- Alternativa cuando la red no es viable  

### ❌ No es para
Sincronización continua de datos.

### 🧠 Idea clave examen
**Import/Export = Migración física de datos con discos.**

---

## Azure Notification Hubs

### 🔎 ¿Qué es?
Servicio para enviar **notificaciones push masivas a dispositivos móviles**.

### 🎯 Para qué se usa
- Promociones  
- Alertas  
- MFA  

### 🧠 Idea clave examen
**Notification Hubs = Push notifications móviles.**

---

## Azure Service Bus

### 🔎 ¿Qué es?
Servicio de **mensajería empresarial** para comunicación desacoplada entre aplicaciones.

### 🎯 Para qué se usa
- Microservicios  
- Procesamiento asíncrono  
- Workflows críticos  

### 🧠 Idea clave examen
**Service Bus = Mensajería confiable entre aplicaciones.**

---

## Azure Stack Hub

### 🔎 ¿Qué es?
Extensión de Azure que permite ejecutar servicios Azure en tu propio datacenter.

### 🎯 Para qué se usa
- Escenarios híbridos  
- Soberanía de datos  

### 🧠 Idea clave examen
**Azure Stack Hub = Azure on-premises.**

---

## Azure Storage Sync

### 🔎 ¿Qué es?
Servicio que sincroniza **file servers on-premises con Azure File Share**.

Permite usar Azure como extensión del almacenamiento local.

### 🎯 Para qué se usa
- Reemplazo o extensión de NAS  
- Backup en la nube  
- Caché local con cloud tiering  

### 🚀 Características clave
- Sincronización bidireccional  
- Cloud tiering (archivos fríos en Azure)  
- Gestión centralizada  

### ❌ No es para
Big Data ni analítica avanzada.

### 🧠 Idea clave examen
**Storage Sync = Sincroniza file servers locales con Azure Files.**
