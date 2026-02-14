[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# 🌌 Azure Cosmos DB

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
