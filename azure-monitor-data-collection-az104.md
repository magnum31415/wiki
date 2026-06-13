[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Monitor Data Collection (AZ-104)

# Índice

- [Azure Monitor DCR Queries - XPath vs KQL vs WQL vs T-SQL](#azure-monitor-dcr-queries---xpath-vs-kql-vs-wql-vs-t-sql-az-104)
- [Azure Monitor Data Collection](#azure-monitor-data-collection)
  
---

# Azure Monitor Data Collection

Azure Monitor Data Collection es el mecanismo que utiliza Azure Monitor para:

- recopilar datos
- filtrar datos
- transformar datos
- enviar datos

desde recursos Azure y máquinas hacia destinos de monitorización.

---

# Qué tipos de datos puede recopilar

Azure Monitor puede recopilar:

| Tipo de dato | Ejemplo |
|---|---|
| Logs | Windows Event Logs, Syslog |
| Métricas | CPU, memoria |
| Performance Counters | % CPU, Disk IOPS |
| Application Logs | Logs aplicaciones |
| Security Events | Eventos seguridad Windows |
| Custom Logs | Logs personalizados |

---

# Componentes principales

```                  Azure VM / Arc Server
                           │
                           │ (Association)
                           ▼
                 Data Collection Rule (DCR)
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
      Windows Events     Syslog    Performance Counters
             │
             ├── IIS Logs
             ├── Custom Text Logs
             └── Prometheus Metrics (AKS)
                           │
                           ▼
                    Destination(s)
                           │
          ├── Log Analytics Workspace
          ├── Azure Monitor Metrics
          ├── Event Hub
          └── (Storage Direct en escenarios específicos)
```

**La regla mental que usaría para el examen**
````
(1) Resource
    Azure VM
    Azure Arc Server
    AKS
          │
          ▼
(2) Azure Monitor Agent (AMA)
          │
          ▼
(3) Data Sources
    - Windows Events
    - Syslog
    - Performance Counters
    - IIS Logs
    - Custom Logs
    - Prometheus
          │
          ▼
(4) Destinations
    - Log Analytics Workspace
    - Azure Monitor Metrics
    - Event Hub
    - Storage Direct (casos específicos)
````


## 1. Azure Monitor Agent (AMA)

Agente moderno de Azure Monitor.

Se instala en:

- Azure VMs
- Arc-enabled servers
- máquinas híbridas

## 2. Recursos compatibles con DCR (Association Targets)

| Recurso                    | Compatible |
| -------------------------- | ---------- |
| ✅ Azure VM                 | Sí         |
| ✅ Azure Arc Server         | Sí         |
| ✅ AKS (Managed Prometheus) | Sí         |
| ❌ Storage Account          | No         |
| ❌ Log Analytics Workspace  | No         |
| ❌ Azure SQL Database       | No         |
| ❌ Azure Firewall           | No         |
| ❌ Application Gateway      | No         |
| ❌ Key Vault                | No         |

## 3. Data Sources que puede configurar un DCR

| Data Source            | Compatible |
| ---------------------- | ---------- |
| ✅ Windows Event Logs   | Sí         |
| ✅ Syslog               | Sí         |
| ✅ Performance Counters | Sí         |
| ✅ IIS Logs             | Sí         |
| ✅ Custom Text Logs     | Sí         |
| ✅ Prometheus Metrics   | Sí         |

## 4. Destinations que puede configurar un DCR


| Destination | ¿Compatible? | Comentario |
|-------------|------------|------------|
| ✅ Log Analytics Workspace | Sí | Destino más habitual y el que aparece en AZ-104. |
| ✅ Azure Monitor Metrics | Sí | Permite enviar datos al almacén de métricas. |
| ✅ Event Hub | Sí | Para streaming e integración con terceros. |
| ✅ Storage Blob Direct | Sí* | Disponible para escenarios específicos soportados por DCR modernos. |
| ✅ Storage Table Direct | Sí* | Disponible para escenarios específicos soportados por DCR modernos. |
   
# Data Collection Rule (DCR)

## Què és?
Una Data Collection Rule (DCR) es una configuración que le dice a Azure Monitor:

1. Qué datos recoger.
2. Desde dónde recogerlos.
3. A dónde enviarlos.
4. (Opcionalmente) Cómo transformarlos antes de almacenarlos.

Es decir, una DCR es simplemente una regla de recopilación de datos.

Definen:

```text
qué recopilar
   - Windows Events
    - Syslog
    - Performance Counters
cómo filtrarlo
    - VM1
dónde enviarlo
    - Log Analytics Workspace
```

**qué es realmente un Data Collection Rule (DCR) en Azure Monitor.**

Un DCR no sirve para “leer cualquier recurso Azure”.

Sirve para definir:

- qué datos recoger
- desde qué origen compatible
- y a dónde enviarlos

Normalmente se usa con:

- Azure Monitor Agent (AMA)
- máquinas virtuales
- servidores híbridos
- eventos Windows
- syslog
- performance counters
- logs personalizados





# Ejemplo típico

## Recopilar solo Event ID 4625

```text
Windows Security Events
```
↓
La DCR utiliza:

```text
XPath
```

para filtrar.

---

# Importante examen
DCR reemplaza gradualmente al antiguo:
```text
Log Analytics Agent (MMA/OMS Agent)
```

---

# Diferencia importante examen

| Tecnología | Estado |
|---|---|
| Log Analytics Agent (MMA) | Legacy / Deprecated |
| Azure Monitor Agent (AMA) | Recomendado |

---

# Regla rápida examen

```text
DCR defines what data Azure Monitor collects.
```

```text
Azure Monitor Agent sends data according to DCR rules.
```

```text
XPath filters Windows Event Logs during ingestion.
```

```text
KQL queries data after ingestion.
```

---

# Frases clave AZ-104

```text
Data Collection Rules define data collection behavior in Azure Monitor.
```

```text
Azure Monitor Agent uses DCRs to collect and route monitoring data.
```

```text
DCRs support filtering, transformation, and routing of monitoring data.
```

---

# Azure Monitor DCR Queries - XPath vs KQL vs WQL vs T-SQL

---

# Concepto clave examen

La pregunta típica AZ-104 evalúa:

```text
qué lenguaje utiliza Azure Monitor DCR
para filtrar Windows Event Logs
```

---

# XPath

## Qué es

Lenguaje utilizado para navegar y filtrar datos XML.

---

# Por qué Azure lo usa

Windows Event Logs internamente utilizan formato XML.

Por eso Azure Monitor usa:

```text
xPathQueries
```

para filtrar eventos.

---

# Ejemplo XPath

```text
*[System[(EventID=4648)]]
```

---

# Importante examen

XPath se usa:

✅ DURANTE la recopilación  
✅ dentro de la DCR  
✅ para decidir qué eventos ingerir  

---

# KQL (Kusto Query Language)

## Qué es

Lenguaje de consultas de:

- Azure Monitor Logs
- Log Analytics
- Microsoft Sentinel
- Azure Data Explorer

---

# Para qué sirve

KQL se usa:

✅ DESPUÉS de la ingestión  
✅ para consultar logs ya almacenados  

---

# Ejemplo KQL

```kql
SecurityEvent
| where EventID == 4648
```

---

# Importante examen

KQL:

❌ NO define filtros DCR para Windows Event Logs  
✅ consulta datos ya recopilados  

---

# WQL (WMI Query Language)

## Qué es

Lenguaje asociado a:

```text
Windows Management Instrumentation (WMI)
```

---

# Uso típico

WQL se utiliza para:

- consultas WMI
- inventario Windows
- administración SO

---

# Importante examen

WQL:

❌ NO se usa en Azure Monitor DCR para Event Logs  

---

# T-SQL

## Qué es

Lenguaje SQL de Microsoft.

Usado en:

- SQL Server
- Azure SQL Database

---

# Importante examen

T-SQL:

❌ NO tiene relación con Azure Monitor DCRs  

---

# Diferencia clave examen

| Lenguaje | Uso principal |
|---|---|
| XPath | Filtrar Windows Event Logs en DCR |
| KQL | Consultar logs en Log Analytics |
| WQL | Consultas WMI |
| T-SQL | Bases de datos SQL |

---

# Flujo real

```text
Windows VM
   ↓
DCR with XPath filter
   ↓
Azure Monitor ingestion
   ↓
Log Analytics
   ↓
KQL queries
```

---

# Diferencia importante examen

| Tecnología | Estado |
|---|---|
| Log Analytics Agent (MMA) | Legacy / Deprecated |
| Azure Monitor Agent (AMA) | Recomendado |

---

# Qué puede recopilar una DCR

| Tipo | Compatible |
|---|---|
| Windows Event Logs | ✅ |
| Syslog | ✅ |
| Performance Counters | ✅ |
| IIS Logs | ✅ |
| Custom Logs | ✅ |

---

# Trampas típicas AZ-104

## Trampa 1

Pensar que:

```text
KQL define qué logs recopilar
```

❌ Incorrecto.

↓

KQL consulta datos ya ingeridos.

---

## Trampa 2

Pensar que WQL se usa para Event Logs en DCR.

❌ Incorrecto.

↓

DCR usa XPath.

---

## Trampa 3

Confundir:

```text
XPath
```

con:

```text
KQL
```

---

# Reglas rápidas examen

```text
DCR defines what data Azure Monitor collects.
```

```text
Azure Monitor Agent sends data according to DCR rules.
```

```text
XPath filters Windows Event Logs during ingestion.
```

```text
KQL queries data after ingestion.
```

---

# Frases clave AZ-104

```text
Data Collection Rules define data collection behavior in Azure Monitor.
```

```text
Azure Monitor Agent uses DCRs to collect and route monitoring data.
```

```text
XPath is used in DCRs to filter Windows Event Logs.
```

```text
KQL is used to query logs already stored in Log Analytics.
```
