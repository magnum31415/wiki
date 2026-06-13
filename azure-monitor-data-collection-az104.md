[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Monitor Data Collection, DCR and Azure Monitor Agent (AMA) (AZ-104)

- [Azure Monitor Data Collection (AZ-104)](#azure-monitor-data-collection-az-104)
  - [Azure Monitor Data Collection](#azure-monitor-data-collection)
  - [Qué tipos de datos puede recopilar](#qué-tipos-de-datos-puede-recopilar)
  - [Componentes principales](#componentes-principales)
  - [Regla mental para el examen](#regla-mental-para-el-examen)
  - [Azure Monitor Agent (AMA)](#azure-monitor-agent-ama)
  - [Recursos compatibles con DCR (Association Targets)](#recursos-compatibles-con-dcr-association-targets)
  - [Data Sources que puede configurar un DCR](#data-sources-que-puede-configurar-un-dcr)
  - [Destinations que puede configurar un DCR](#destinations-que-puede-configurar-un-dcr)

- [Data Collection Rule (DCR)](#data-collection-rule-dcr)
  - [Qué es](#qué-es)
  - [Concepto importante](#concepto-importante)
  - [Ejemplo típico](#ejemplo-típico)
  - [Importante para el examen](#importante-para-el-examen)
  - [Diferencia importante](#diferencia-importante)
  - [Reglas rápidas para el examen](#reglas-rápidas-para-el-examen)
  - [Frases clave AZ-104](#frases-clave-az-104)

- [Azure Monitor DCR Queries - XPath vs KQL vs WQL vs T-SQL](#azure-monitor-dcr-queries---xpath-vs-kql-vs-wql-vs-t-sql)
  - [Concepto clave para el examen](#concepto-clave-para-el-examen)
  - [XPath](#xpath)
  - [KQL (Kusto Query Language)](#kql-kusto-query-language)
  - [WQL (WMI Query Language)](#wql-wmi-query-language)
  - [T-SQL](#t-sql)
  - [Diferencia clave](#diferencia-clave)
  - [Flujo real](#flujo-real)
  - [Qué puede recopilar una DCR](#qué-puede-recopilar-una-dcr)
  - [Trampas típicas AZ-104](#trampas-típicas-az-104)
  - [Reglas rápidas](#reglas-rápidas)
  - [Frases clave AZ-104](#frases-clave-az-104)

---

# Azure Monitor Data Collection (AZ-104)

## Azure Monitor Data Collection

Azure Monitor Data Collection es el mecanismo que utiliza Azure Monitor para:

* recopilar datos
* filtrar datos
* transformar datos
* enviar datos

desde recursos compatibles (Azure VMs, Azure Arc, AKS y otros escenarios soportados) hacia uno o varios destinos de monitorización.

---

## Qué tipos de datos puede recopilar

| Tipo de dato         | Ejemplo                       |
| -------------------- | ----------------------------- |
| Windows Event Logs   | Security, System, Application |
| Syslog               | auth, daemon, kern            |
| Performance Counters | CPU %, Memory %, Disk IOPS    |
| IIS Logs             | Logs IIS                      |
| Custom Text Logs     | Logs personalizados           |
| Prometheus Metrics   | Métricas de AKS               |

---

## Componentes principales

```text
Azure VM / Arc Server / AKS
            │
            ▼
Azure Monitor Agent (AMA)
            │
            ▼
Data Collection Rule (DCR)
            │
    ├── Windows Event Logs
    ├── Syslog
    ├── Performance Counters
    ├── IIS Logs
    ├── Custom Text Logs
    └── Prometheus Metrics
            │
            ▼
      Destination(s)
            │
    ├── Log Analytics Workspace
    ├── Azure Monitor Metrics
    ├── Event Hub
    └── Storage Direct (casos específicos)
```

---

## Regla mental para el examen

```text
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

    - Windows Event Logs
    - Syslog
    - Performance Counters
    - IIS Logs
    - Custom Text Logs
    - Prometheus Metrics

          │

          ▼

(4) Destinations

    - Log Analytics Workspace
    - Azure Monitor Metrics
    - Event Hub
    - Storage Direct (casos específicos)
```

---

## Azure Monitor Agent (AMA)

Agente moderno de Azure Monitor.

Se instala en:

* Azure VMs
* Azure Arc-enabled Servers
* Máquinas híbridas

Es el responsable de recopilar los datos definidos por la Data Collection Rule (DCR).

---

## Recursos compatibles con DCR (Association Targets)

| Recurso                              | Compatible |
| ------------------------------------ | ---------- |
| ✅ Azure VM                           | Sí         |
| ✅ Azure Arc Server                   | Sí         |
| ✅ AKS (Managed Prometheus scenarios) | Sí         |
| ❌ Storage Account                    | No         |
| ❌ Log Analytics Workspace            | No         |
| ❌ Azure SQL Database                 | No         |
| ❌ Azure Firewall                     | No         |
| ❌ Application Gateway                | No         |
| ❌ Key Vault                          | No         |

---

## Data Sources que puede configurar un DCR

| Data Source            | Compatible |
| ---------------------- | ---------- |
| ✅ Windows Event Logs   | Sí         |
| ✅ Syslog               | Sí         |
| ✅ Performance Counters | Sí         |
| ✅ IIS Logs             | Sí         |
| ✅ Custom Text Logs     | Sí         |
| ✅ Prometheus Metrics   | Sí         |

---

## Destinations que puede configurar un DCR

| Destination               | Compatible | Comentario                                                       |
| ------------------------- | ---------- | ---------------------------------------------------------------- |
| ✅ Log Analytics Workspace | Sí         | Destino más habitual y el que aparece en AZ-104                  |
| ✅ Azure Monitor Metrics   | Sí         | Permite enviar datos al almacén de métricas                      |
| ✅ Event Hub               | Sí         | Para streaming e integración con terceros                        |
| ⚠️ Storage Blob Direct    | Sí*        | Disponible en escenarios específicos soportados por DCR modernos |
| ⚠️ Storage Table Direct   | Sí*        | Disponible en escenarios específicos soportados por DCR modernos |

**Nota importante para AZ-104**

En las preguntas clásicas del examen, si aparece:

* Azure VM
* Storage Account
* Log Analytics Workspace
* Azure SQL Database

y preguntan cuál puede ser el destino de un DCR, la respuesta esperada suele ser:

```text
Workspace only
```

aunque existan capacidades modernas relacionadas con Storage Direct.

---

# Data Collection Rule (DCR)

## Qué es

Una Data Collection Rule (DCR) es una configuración que le dice a Azure Monitor:

1. Qué datos recoger.
2. Desde qué recursos compatibles recogerlos.
3. Cómo filtrarlos o transformarlos.
4. A dónde enviarlos.

Es simplemente una regla de recopilación de datos.

Define:

```text
Qué recopilar

- Windows Event Logs
- Syslog
- Performance Counters
- IIS Logs
- Custom Logs

Desde qué recursos

- VM1
- VM2
- Azure Arc Server

Cómo filtrarlo

- XPath
- Transformations

Dónde enviarlo

- Log Analytics Workspace
- Azure Monitor Metrics
- Event Hub
```

---

## Concepto importante

Un DCR no sirve para leer cualquier recurso Azure.

Sirve para definir:

* qué datos recoger
* desde qué recursos compatibles
* cómo filtrarlos
* cómo transformarlos
* a qué destino enviarlos

---

## Ejemplo típico

Recopilar únicamente los eventos de seguridad con EventID 4625.

```text
Windows Security Events
```

↓

La DCR utiliza:

```text
XPath
```

para filtrar durante la recopilación.

---

## Importante para el examen

DCR reemplaza gradualmente al antiguo:

```text
Log Analytics Agent (MMA / OMS Agent)
```

---

## Diferencia importante

| Tecnología                | Estado              |
| ------------------------- | ------------------- |
| Log Analytics Agent (MMA) | Legacy / Deprecated |
| Azure Monitor Agent (AMA) | Recomendado         |

---

## Reglas rápidas para el examen

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

## Frases clave AZ-104

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

## Concepto clave para el examen

La pregunta típica AZ-104 evalúa:

```text
qué lenguaje utiliza Azure Monitor DCR
para filtrar Windows Event Logs
```

---

## XPath

### Qué es

Lenguaje utilizado para navegar y filtrar datos XML.

---

### Por qué Azure lo usa

Windows Event Logs internamente utilizan formato XML.

Por ello Azure Monitor utiliza:

```text
xPathQueries
```

para filtrar eventos.

---

### Ejemplo XPath

```text
*[System[(EventID=4648)]]
```

---

### Importante

XPath se usa:

* ✅ Durante la recopilación
* ✅ Dentro de la DCR
* ✅ Para decidir qué eventos ingerir

---

## KQL (Kusto Query Language)

### Qué es

Lenguaje de consultas utilizado en:

* Azure Monitor Logs
* Log Analytics
* Microsoft Sentinel
* Azure Data Explorer

---

### Para qué sirve

KQL se utiliza:

* ✅ Después de la ingestión
* ✅ Para consultar logs ya almacenados

---

### Ejemplo

```kusto
SecurityEvent
| where EventID == 4648
```

---

### Importante

KQL:

* ❌ No define filtros DCR para Windows Event Logs
* ✅ Consulta datos ya recopilados

---

## WQL (WMI Query Language)

### Qué es

Lenguaje asociado a:

```text
Windows Management Instrumentation (WMI)
```

---

### Uso típico

* Consultas WMI
* Inventario Windows
* Administración del sistema operativo

---

### Importante

WQL:

* ❌ No se utiliza en Azure Monitor DCR para Event Logs

---

## T-SQL

### Qué es

Lenguaje SQL de Microsoft.

Utilizado en:

* SQL Server
* Azure SQL Database

---

### Importante

T-SQL:

* ❌ No tiene relación con Azure Monitor DCR

---

## Diferencia clave

| Lenguaje | Uso principal                     |
| -------- | --------------------------------- |
| XPath    | Filtrar Windows Event Logs en DCR |
| KQL      | Consultar logs en Log Analytics   |
| WQL      | Consultas WMI                     |
| T-SQL    | Bases de datos SQL                |

---

## Flujo real

```text
Windows VM

      │

Azure Monitor Agent

      │

DCR
(with XPath filters)

      │

Azure Monitor ingestion

      │

Log Analytics Workspace

      │

KQL Queries
```

---

## Qué puede recopilar una DCR

| Tipo                 | Compatible |
| -------------------- | ---------- |
| Windows Event Logs   | ✅          |
| Syslog               | ✅          |
| Performance Counters | ✅          |
| IIS Logs             | ✅          |
| Custom Logs          | ✅          |
| Prometheus Metrics   | ✅          |

---

## Trampas típicas AZ-104

### Trampa 1

Pensar que:

```text
KQL define qué logs recopilar
```

❌ Incorrecto.

KQL consulta datos ya ingeridos.

---

### Trampa 2

Pensar que WQL se usa para Event Logs en DCR.

❌ Incorrecto.

DCR usa XPath.

---

### Trampa 3

Confundir:

```text
XPath
```

con:

```text
KQL
```

---

## Reglas rápidas

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

## Frases clave AZ-104

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
