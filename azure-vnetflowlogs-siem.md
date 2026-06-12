
# Fujo con CrowdStrike
````

                 Azure VNet
                    │
                    ▼
            Network Watcher
                    │
                    ▼
          VNet Flow Logs (raw data)
                    │
                    ▼
            Storage Account  
                    │
                    ▼
            Traffic Analytics
                    │
                    ▼
         Log Analytics Workspace
                    │
                    ▼
           NTANetAnalytics table
                    │
                    ▼
            Data Export Rule
                    │
                    ▼
               Azure Event Hub
                    │
                    ▼
         CrowdStrike Falcon Next-Gen SIEM
````

# Creacion de recursos

alz-platform-connectivity
- ✅ Network Watcher
- ✅ Storage Account GWC
- ✅ Storage Account SWC
- ✅ Blob Container vnet-flow-logs
- ✅ VNet Flow Logs

alz-platform-management
- ✅ Log Analytics Workspace
- ✅ Traffic Analytics
- ✅ Data Export Rule
- ✅ Event Hub Namespace
- ✅ Event Hub
- ✅ Alertas
- ✅ Workbooks
- ✅ Dashboards

----
# Bloques

## Log Analytics Workspace

Es el almacén de datos.
Piensa en él como una gran base de datos donde Azure guarda logs.

````
                Log Analytics Workspace

        +----------------------------------+
        |                                  |
        |  AzureActivity                   |
        |  NTANetAnalytics                 |
        |  AzureDiagnostics                |
        |  SecurityEvent                   |
        |  Heartbeat                       |
        |  ...                             |
        |                                  |
        +----------------------------------+
````

or ejemplo: ``law-platform-prod-001`` Todo acaba almacenado aquí.


## Log Analytics

````
                +-------------------------+
                | Log Analytics           |
                | (KQL Queries)           |
                +------------+------------+
                             |
                             |
                             v
                +-------------------------+
                | Log Analytics Workspace |
                +-------------------------+
````
Log Analytics es el servicio que permite consultar y analizar los datos almacenados en el Workspace.

Es decir:

- buscar
- filtrar
- correlacionar
- ejecutar consultas KQL
- crear alertas

No es un recurso distinto.

Es la funcionalidad que trabaja sobre el Workspace.

Ejemplo:
````
AzureActivity
| where OperationName contains "Delete"
````

## Traffic Analytics

- Traffic Analytics es un **motor de análisis**.
- No guarda los logs.
- Los procesa.
- Su función es transformar los VNet Flow Logs "crudos" en información útil.

**Entrada:**
````
Flow:

10.1.1.4 ---> 10.2.3.7 TCP 443 ALLOW
````

**Salida:**
````
Application A

Most connections:
443

Top Talkers:
10.1.1.4

Denied Flows:
15

Inbound:
1200

Outbound:
800
````

y además genera la tabla: ``NTANetAnalytics``

que es precisamente la que usa CrowdStrike.

Flujo:
````
Storage Account
        │
        ▼
Traffic Analytics
        │
        ▼
Log Analytics Workspace
        │
        ▼
NTANetAnalytics
````

## Data Export Rule

Es una regla automática.

Su función es: ``Todo lo que llegue a esta tabla, envíalo automáticamente a otro sitio.``

Por ejemplo:
````
NTANetAnalytics
        │
        ▼
Data Export Rule
        │
        ▼
Azure Event Hub
````

No almacena nada.Simplemente reenvía.

## Azure Event Hub

Es un sistema de streaming de eventos.

Puedes pensar en él como un Kafka gestionado por Microsoft.

Su trabajo:
````
Producer
     │
     ▼
 Event Hub
     │
     ▼
Consumer
````

En vuestro caso:
````
Log Analytics
      │
      ▼
Event Hub
      │
      ▼
CrowdStrike
````


- Event Hub tampoco analiza.
- Simplemente recibe eventos y los entrega a los consumidores.
