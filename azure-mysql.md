# Azure Database for MySQL

## ¿Qué es Azure Database for MySQL – Flexible Server?

Es un servicio PaaS totalmente gestionado para MySQL que ofrece:
- Más control sobre configuración
- Control del mantenimiento
- Opciones de alta disponibilidad
- Mejor optimización de costes

Es la versión moderna (reemplaza progresivamente a Single Server).

**🧩 Resumen mental rápido (tipo examen)**

Si lees:
- “Custom maintenance window” → Flexible Server
- “Zone redundant HA” → Flexible Server General Purpose
- “Cost saving dev/test” → Burstable
- “Predictable workload” → Reserved Instances

## Azure Database for MySQL – Comparativa

| Característica                | **Single Server**            | **Flexible Server – Burstable** | **Flexible Server – General Purpose** | **Flexible Server – Memory Optimized** |
| ----------------------------- | ---------------------------- | ------------------------------- | ------------------------------------- | -------------------------------------- |
| Estado del servicio           | Modelo antiguo (en retirada) | Actual                          | Actual                                | Actual                                 |
| Control ventana mantenimiento | ❌ No                         | ✅ Sí                            | ✅ Sí                                  | ✅ Sí                                   |
| Stop / Start                  | ❌ No                         | ✅ Sí                            | ✅ Sí                                  | ✅ Sí                                   |
| Alta disponibilidad (HA)      | Limitada                     | ❌ No                            | ✅ Sí                                  | ✅ Sí                                   |
| HA Zone-redundant             | ❌ No                         | ❌ No                            | ✅ Sí (solo al crear)                  | ✅ Sí (solo al crear)                   |
| Tipo replicación HA           | Interna gestionada           | —                               | Síncrona entre zonas                  | Síncrona entre zonas                   |
| Tier de computación           | Basic / GP                   | Burstable (B-series)            | D-series equivalente                  | E-series equivalente                   |
| Ideal para                    | Workloads legacy             | Dev/Test                        | Producción estándar                   | Producción intensiva                   |
| Soporta Reserved Instances    | Limitado                     | ✅ Sí                            | ✅ Sí                                  | ✅ Sí                                   |
| Coste                         | Medio                        | Bajo                            | Medio-Alto                            | Alto                                   |


![azure-mysql-flexible-server](./img/azure/azure-mysql-flexible-server.png)


### 🏗 Qué lo hace “Flexible”
- 1️⃣ Alta disponibilidad configurable

  Puedes elegir:
  - 🔹 Sin HA
  - 🔹 HA dentro de la misma zona
  - 🔹 HA zone-redundant (entre Availability Zones)

 Pero:

  - ⚠️ Solo disponible en General Purpose
  - ❌ No disponible en Burstable

### 2️⃣ Control del mantenimiento

  A diferencia de Single Server:

  - Puedes elegir ventana de mantenimiento
  - O dejar que sea gestionado por el sistema

  Esto es clave en entornos productivos.

### 3️⃣ Optimización de costes

  Flexible Server permite:

  - ⏸ Stop / Start del servidor (ahorro en entornos no productivos)
  - ⚡ Burstable tier (para cargas ligeras intermitentes)
  - 💰 Reserved Instances (hasta ~63% ahorro si uso predecible)

### 🧠 Diferencia entre tiers

  | Tier                 | Para qué sirve        | HA soportada |
| -------------------- | --------------------- | ------------ |
| **Burstable**        | Dev/Test o carga baja | ❌ No         |
| **General Purpose**  | Producción            | ✅ Sí         |
| **Memory Optimized** | Workloads intensivos  | ✅ Sí         |


