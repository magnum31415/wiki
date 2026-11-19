[Azure](azure.md)
# 🌌 Azure Cosmos DB

# 📑 Índice

1. [Azure Cosmos DB](#-azure-cosmos-db)
   - [¿Qué es?](#-qué-es)
   - [Modelos soportados](#-modelos-soportados)
   - [Características clave](#-características-clave)
   - [Modelo de consumo](#-modelo-de-consumo)
   - [Casos de uso típicos](#-casos-de-uso-típicos)
   - [Clave examen AZ-305](#-clave-examen-az-305)

2. [Azure Synapse Link for Azure Cosmos DB](#-azure-synapse-link-for-azure-cosmos-db)
   - [¿Qué es?](#-qué-es-1)
   - [Qué resuelve](#-qué-resuelve)
   - [Cómo funciona](#-cómo-funciona)
   - [Beneficios](#-beneficios)
   - [Casos de uso](#-casos-de-uso)
   - [Clave examen AZ-305](#-clave-examen-az-305-1)
   - [Diferencia clara](#-diferencia-clara)

3. [Métodos de autenticación en Azure Cosmos DB (SQL API)](#-métodos-de-autenticación-en-azure-cosmos-db-sql-api)
   - [Key-Based Authentication (Modelo tradicional)](#1️⃣-key-based-authentication-modelo-tradicional)
     - [Master Key (Primary / Secondary)](#-master-key-primary--secondary)
     - [Read-Only Key](#-read-only-key)
     - [Resource Token](#-resource-token)
   - [Identity-Based Authentication (Recomendado)](#2️⃣-identity-based-authentication-recomendado)
     - [Azure RBAC (Control Plane)](#-azure-rbac-control-plane)
     - [Cosmos DB Data Plane RBAC](#-cosmos-db-data-plane-rbac-muy-importante)
   - [Comparativa rápida](#-comparativa-rápida)
   - [Reglas mentales para AZ-305](#-reglas-mentales-para-az-305)
   - [Arquitectura moderna recomendada](#-arquitectura-moderna-recomendada)


## 🔎 ¿Qué es?

Base de datos NoSQL totalmente gestionada y distribuida globalmente.

Diseñada para:
- Baja latencia (milisegundos)
- Alta disponibilidad
- Escalado automático
- Replicación multi-región

---

## 🧱 Modelos soportados

Cosmos DB es **multi-modelo**:

- Core (SQL API)
- MongoDB API
- Cassandra API
- Table API
- Gremlin (graph)


| API                | Tipo de dato ideal                   | Modelo         | Cuándo usarla                                                           | Ejemplo real                                                  |
| ------------------ | ------------------------------------ | -------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------- |
| **SQL (Core API)** | Documentos JSON estructurados        | Document-based | Aplicaciones web modernas, catálogos, perfiles de usuario, IoT          | App global que guarda pedidos en formato JSON                 |
| **MongoDB API**    | Documentos JSON/BSON                 | Document-based | Migraciones desde MongoDB on-prem o apps que ya usan drivers MongoDB    | Startup que ya usa MongoDB y quiere migrar sin cambiar código |
| **Cassandra API**  | Datos distribuidos de alta escritura | Wide-column    | Workloads masivos con alta concurrencia y baja latencia                 | Sistema de telemetría o IoT con millones de eventos           |
| **Gremlin API**    | Grafos (nodos y relaciones)          | Graph          | Modelar relaciones complejas                                            | Red social, motor de recomendaciones                          |
| **Table API**      | Datos clave-valor simples            | Key-Value      | Aplicaciones sencillas, bajo coste, migración desde Azure Table Storage | App ligera que almacena configuraciones por usuario           |



---

## 🚀 Características clave

- 🌍 Replicación global automática
- 📈 Escalado horizontal (particiones)
- ⚡ SLA de latencia < 10 ms (lecturas)
- 🔁 Multi-master opcional
- 🧠 Consistency levels configurables:
  - Strong
  - Bounded Staleness
  - Session (default)
  - Consistent Prefix
  - Eventual

---

## 💰 Modelo de consumo

- Basado en **Request Units (RU/s)**
- Provisioned throughput o Serverless
- Escalado manual o autoscale

---

## 🎯 Casos de uso típicos

- Aplicaciones web globales
- IoT
- Catálogos
- Gaming
- Sistemas con alta concurrencia y baja latencia

---

## 🧠 Clave examen AZ-305

Cosmos DB = Base de datos NoSQL global, baja latencia, multi-modelo y escalado horizontal automático.

---

# 🔗 Azure Synapse Link for Azure Cosmos DB

## 🔎 ¿Qué es?

Funcionalidad que permite analizar datos de Cosmos DB **sin moverlos ni impactar la carga OLTP**.

Conecta Cosmos DB directamente con:
- Azure Synapse Analytics

---

## 🧱 Qué resuelve

Normalmente:
- OLTP (Cosmos DB) → carga transaccional
- OLAP (Analytics) → requiere ETL

Synapse Link elimina:
- ETL
- Copias intermedias
- Impacto en rendimiento transaccional

---

## 🚀 Cómo funciona

- Activa un **Analytical Store** dentro de Cosmos DB
- Los datos se replican automáticamente a un formato columnar
- Synapse puede consultar directamente

---

## 🎯 Beneficios

- 🔄 Sin ETL
- ⚡ Análisis casi en tiempo real
- 📊 Separación OLTP / OLAP
- 📉 Sin impacto en RU del workload principal

---

## 🎯 Casos de uso

- Dashboards casi en tiempo real
- BI sobre datos operacionales
- Analítica sin afectar producción

---

## 🧠 Clave examen AZ-305

Synapse Link = Analítica sobre Cosmos DB sin ETL y sin afectar rendimiento OLTP.

---

# 🧩 Diferencia clara

| Servicio | Enfoque |
|----------|---------|
| Azure Cosmos DB | Base de datos NoSQL transaccional (OLTP) |
| Synapse Link | Analítica (OLAP) sobre datos de Cosmos DB sin moverlos |


# 🔐 Métodos de autenticación en Azure Cosmos DB (SQL API)

Azure Cosmos DB soporta **dos modelos principales de autenticación**:

1. Key-based (basado en claves)
2. Identity-based (basado en Microsoft Entra ID) ✅ Recomendado

---

# 1️⃣ Key-Based Authentication (Modelo tradicional)

Autenticación mediante claves compartidas.

## 🔹 Master Key (Primary / Secondary)

- Acceso completo a la cuenta
- Permite operaciones read/write/delete
- No es granular
- No está asociada a usuarios específicos
- Poco recomendable en producción moderna

Se envía en el header `Authorization` de la API REST.

---

## 🔹 Read-Only Key

- Permite solo lectura
- Sigue siendo a nivel cuenta
- No granular por usuario

---

## 🔹 Resource Token

Más seguro que usar la master key directamente.

- Se genera usando la master key
- Permisos limitados (read, write, etc.)
- Ámbito limitado (container o recurso específico)
- Tiene tiempo de expiración

### Uso típico:
- Acceso delegado
- Aplicaciones frontend
- Escenarios multi-tenant

⚠️ Sigue siendo key-based, pero con mayor control.

---

# 2️⃣ Identity-Based Authentication (Recomendado)

Basado en Microsoft Entra ID (Azure AD).

No requiere compartir claves.

---

## 🔹 Azure RBAC (Control Plane)

Controla la administración del recurso.

Ejemplos de roles:
- Cosmos DB Account Reader
- Contributor

Permite:
- Crear/modificar cuentas
- Configuración administrativa

---

## 🔹 Cosmos DB Data Plane RBAC (Muy importante)

Permite acceso real a los datos usando identidad Entra ID.

Ejemplos de roles:
- Cosmos DB Built-in Data Reader
- Cosmos DB Built-in Data Contributor

### Ventajas:
- Seguridad basada en identidad
- Sin compartir claves
- Integración con Conditional Access
- Control granular por usuario/grupo

---

# 🧠 Comparativa rápida

| Método | Tipo | Granularidad | Seguridad | Recomendado |
|--------|------|-------------|-----------|-------------|
| Primary Key | Key-based | ❌ Ninguna | Baja | ❌ |
| Read-Only Key | Key-based | ❌ Ninguna | Baja | ❌ |
| Resource Token | Key-based delegado | ⚠️ Limitada | Media | ⚠️ |
| Entra ID + RBAC | Identity-based | ✅ Alta | Alta | ✅ |

---

# 🎯 Reglas mentales para AZ-305

- “Acceso por usuario específico” → Entra ID + RBAC
- “Acceso temporal delegado” → Resource Token
- “No compartir claves maestras” → Identity-based
- “Legacy application” → Keys

---
# 🌐 Azure Table Storage vs Azure Cosmos DB  
## (con tipo de datos ideal para cada uno)

---

# 🔷 1️⃣ Azure Table Storage

## 🔎 Qué es
Servicio NoSQL simple dentro de una **Storage Account**.  
Modelo basado en:
- PartitionKey
- RowKey
- Entidades tipo clave-valor

---

## 📦 Tipo de datos ideal

Azure Table Storage es ideal para:

- Datos estructurados simples
- Entidades planas (sin relaciones complejas)
- Clave-valor
- Logs ligeros
- Metadatos
- Configuración de aplicaciones
- Datos con bajo volumen de consultas complejas
- Workloads donde el coste es crítico

### ❌ No es ideal para:
- Consultas complejas
- Relaciones entre entidades
- JSON anidado complejo
- Multi-región activa
- Workloads críticos con SLA estrictos

---

## 🎯 Escenario típico

Guardar:
- Eventos de auditoría
- Información de dispositivos IoT básica
- Configuraciones por usuario
- Catálogo pequeño sin joins

---

# 🔷 2️⃣ Azure Cosmos DB

## 🔎 Qué es
Base de datos NoSQL distribuida globalmente, con SLA de latencia y disponibilidad.

Soporta múltiples APIs:
- SQL (Core)
- MongoDB
- Cassandra
- Table
- Gremlin

---

## 📦 Tipo de datos ideal

Azure Cosmos DB es ideal para:

- Documentos JSON complejos
- Datos semiestructurados
- Datos jerárquicos
- Multi-tenant
- Alta concurrencia
- Datos distribuidos globalmente
- Modelos basados en particiones
- Workloads con latencia <10ms
- Eventos de alto volumen
- Aplicaciones SaaS

### Soporta muy bien:
- JSON anidado
- Documentos grandes
- Eventos en streaming
- Catálogos grandes
- Datos de usuario globales
- Gaming
- Telemetría IoT masiva

---

## 🎯 Escenario típico

- App web global con millones de usuarios
- Backend de e-commerce
- Plataforma SaaS multi-región
- Sistema IoT con alta escritura
- Aplicación con consistencia configurable

---

# 📊 Comparativa enfocada en tipo de datos

| Tipo de dato | Table Storage | Cosmos DB |
|--------------|--------------|------------|
| Clave-valor simple | ✅ Ideal | ✅ |
| JSON complejo | ❌ | ✅ Ideal |
| Datos jerárquicos | ❌ | ✅ |
| Alta concurrencia | ⚠️ Limitado | ✅ |
| Multi-región activa | ❌ | ✅ |
| Logs simples | ✅ Ideal | ⚠️ |
| IoT masivo | ⚠️ | ✅ Ideal |
| Catálogo global | ❌ | ✅ |
| Bajo coste prioritario | ✅ | ❌ |

---

# 🧠 Regla mental AZ-305

- Datos simples, bajo coste → **Table Storage**
- JSON complejo, global, crítico → **Cosmos DB**
- SLA de latencia y multi-región → **Cosmos DB**
- Solo necesitas tablas básicas → **Table Storage**

---

# 🎯 Frase final para memorizar

Table Storage = datos simples y baratos.  
Cosmos DB = datos complejos, globales y de misión crítica.



