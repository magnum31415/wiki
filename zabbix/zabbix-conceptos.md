[ZABBIX ](https://github.com/magnum31415/wiki/tree/main/zabbix)

# Zabbix - Conceptos

# Zabbix 7.0 LTS — Glosario de conceptos

> Guía de estudio y referencia para administración de Zabbix. Los nombres de los conceptos se mantienen en inglés para facilitar su localización en la interfaz y la documentación oficial.
>
> **Alcance:** selección amplia de los conceptos esenciales y más útiles para administrar Zabbix; no pretende enumerar cada parámetro interno de configuración.


# Diagrama 

````mermaid
flowchart TB
    S["Zabbix Server"]
    P["Zabbix Proxy"]
    A["Zabbix Agent 2"]

    S -->|"Proxy PASSIVE MODE - TCP 10051"| P
    P -->|"Proxy ACTIVE MODE - TCP 10051"| S

    P -->|"Agent PASSIVE MODE - TCP 10050"| A
    A -->|"Agent ACTIVE MODE - TCP 10051"| P

    classDef server fill:#dbeafe,stroke:#2563eb,color:#1e40af
    classDef proxy fill:#fef3c7,stroke:#d97706,color:#92400e
    classDef agent fill:#dcfce7,stroke:#16a34a

    class S server
    class P proxy
    class A agent
````

| Origen | Destino | Puerto destino | Modo |
|---|---|---|---|
| Zabbix Proxy | Zabbix Agent | TCP 10050 | Agent PASSIVE MODE |
| Zabbix Agent | Zabbix Proxy | TCP 10051 | Agent ACTIVE MODE |
| Zabbix Proxy | Zabbix Server | TCP 10051 | Proxy ACTIVE MODE |
| Zabbix Server | Zabbix Proxy | TCP 10051 | Proxy PASSIVE MODE |

## 1. Arquitectura y componentes

| Concepto | Función | Ejemplo |
|---|---|---|
| Zabbix Server | Componente central que procesa datos, evalúa triggers y coordina la monitorización. | Servidor central Zabbix. |
| Zabbix Frontend | Interfaz web para configurar, consultar y administrar Zabbix. | Página `Monitoring > Problems`. |
| Zabbix Database | Almacena configuración, históricos, tendencias y eventos. | PostgreSQL o MySQL. |
| Zabbix Proxy | Recoge y transmite datos de los hosts en nombre del server; permite distribuir la carga. | Proxy en un datacenter remoto. |
| Proxy Group | Agrupa proxies para distribuir hosts y facilitar la alta disponibilidad de la monitorización. | `DC-DUS-PG` con varios proxies. |
| Zabbix Agent | Proceso instalado en un host para obtener métricas del sistema. | CPU, memoria y filesystem. |
| Zabbix Agent 2 | Nueva generación del agente, extensible mediante plugins. | Monitorizar Linux y aplicaciones mediante plugins. |
| Agent Passive Checks | El server/proxy solicita la métrica al agent. | Proxy → Agent TCP 10050. |
| Agent Active Checks | El agent solicita la lista de checks y envía sus valores al server/proxy. | Agent → Proxy TCP 10051. |
| Zabbix Sender | Utilidad que envía valores a items de tipo trapper. | `zabbix_sender` publica un resultado. |
| Zabbix Get | Utilidad que consulta una key del agent para hacer pruebas. | `zabbix_get -s 10.80.1.6 -k agent.ping` (sin TLS). |
| Java Gateway | Componente para monitorización mediante JMX. | Métricas de una JVM. |
| Web Service | Componente usado para ciertas tareas, como la generación de informes programados. | PDF de un dashboard. |

## 2. Hosts, grupos y configuración reutilizable

| Concepto | Función | Ejemplo |
|---|---|---|
| Host | Entidad lógica que agrupa los parámetros que se monitorizan. | `de-dus0-prkw02`. |
| Host Group | Agrupa hosts y ayuda a organizar permisos. | `Linux Production`. |
| Template | Conjunto reutilizable de items, triggers, gráficos, reglas LLD y otros objetos. | Template de Linux. |
| Template Group | Organiza templates y sus permisos. | `Templates/Operating systems`. |
| Template Linking | Asocia un template a un host o a otro template. | Aplicar template Linux a 100 hosts. |
| Host Interface | Dirección y puerto utilizados por determinados checks. | Agent: `10.80.1.6:10050`. |
| Host Status | Determina si la monitorización del host está habilitada. | `Enabled` / `Disabled`. |
| Availability | Indicador del estado de disponibilidad de interfaces de monitorización. | `ZBX` rojo, verde o gris. |
| Host Inventory | Campos de inventario asociados al host. | Ubicación, número de serie, contacto. |
| Tags | Metadatos clave-valor utilizados para filtrado, correlación y acciones. | `environment: production`. |
| Maintenance | Periodo que modifica el tratamiento de datos, problemas y acciones durante una intervención. | Ventana de mantenimiento de servidores. |
| Host Encryption | Parámetros TLS de comunicación para las interfaces de agente. | Conexiones mediante PSK. |

## 3. Datos e items

| Concepto | Función | Ejemplo |
|---|---|---|
| Item | Define una métrica o dato que se obtiene de un host. | Uso de CPU = 85 %. |
| Item Key | Identificador de la métrica dentro de un tipo de check. | `system.cpu.util`. |
| Item Type | Define cómo se obtiene el dato. | Zabbix agent, HTTP agent, SNMP agent. |
| Update Interval | Frecuencia prevista de recogida de un item. | Cada 60 segundos. |
| Value Type | Tipo de dato guardado. | Float, unsigned integer, text o log. |
| Units | Unidad en que se muestra el valor. | `%`, `B`, `bps`. |
| History | Valores individuales almacenados durante un tiempo determinado. | CPU medida cada minuto. |
| Trends | Valores agregados por hora para datos numéricos (mínimo, máximo, media y recuento). | Evolución durante 12 meses. |
| Latest Data | Vista de los valores recientes de los items. | Último valor de memoria libre. |
| Preprocessing | Transforma o filtra valores antes de almacenarlos. | Extraer un campo de JSON con JSONPath. |
| Dependent Item | Obtiene su valor a partir de otro item maestro. | Obtener varias métricas de una respuesta JSON. |
| Master Item | Item cuyo valor alimenta a uno o varios dependent items. | Petición HTTP que devuelve JSON. |
| Calculated Item | Calcula valores mediante expresiones sobre datos disponibles. | Combinar métricas de varios items. |
| Trapper Item | Recibe valores enviados desde una fuente externa. | Resultado enviado con `zabbix_sender`. |
| HTTP Agent Item | Obtiene información mediante HTTP/HTTPS. | Consultar un endpoint REST. |
| SNMP Item | Recoge datos de dispositivos vía SNMP. | Contador de octetos de un switch. |
| External Check | Ejecuta un programa externo desde el server o proxy. | Script de comprobación personalizado. |
| UserParameter | Define una key personalizada ejecutada por el agente. | `app.queue.depth`. |
| Unsupported Item | Item que no puede obtener o procesar su valor. | Key inválida o error de preprocesamiento. |
| Timeout | Tiempo máximo de espera de determinadas operaciones de recogida. | Comprobación que excede el timeout. |

## 4. Triggers, eventos y problemas

| Concepto | Función | Ejemplo |
|---|---|---|
| Trigger | Evalúa una expresión y determina un estado OK o PROBLEM. | CPU > 80 % durante 5 min. |
| Trigger Expression | Condición lógica que evalúa datos de items. | `avg(/host/system.cpu.util,5m)>80`. |
| Recovery Expression | Condición adicional para determinar la recuperación de un trigger. | Recuperar al bajar de 70 %. |
| Hysteresis | Separación entre umbrales de problema y recuperación para evitar oscilaciones. | Alerta a 80 %, recuperación a 70 %. |
| Trigger Dependency | Evita problemas derivados cuando existe una causa superior. | No alertar por HTTP si cae el router del sitio. |
| Severity | Nivel de gravedad configurado para un trigger/problema. | Warning, High, Disaster. |
| Event | Registro puntual de un cambio o suceso significativo. | Transición de trigger OK → PROBLEM. |
| Problem | Situación problemática detectada, normalmente por un trigger. | Disco sin espacio suficiente. |
| Recovery Event | Evento que indica la recuperación de una situación problemática. | Disco vuelve a condiciones normales. |
| Acknowledgement | Marca que un operador ha reconocido un problema. | Técnico reconoce la alerta. |
| Problem Update | Operaciones sobre un problema, según permisos y configuración. | Añadir comentario o cambiar gravedad. |
| Event Correlation | Relaciona eventos y permite resolver problemas según reglas. | Correlacionar un fallo y su recuperación. |
| Event Tags | Etiquetas asignadas a eventos para filtrar, correlacionar o enrutar. | `service: database`. |
| Problem Suppression | Suprime determinadas notificaciones o el tratamiento visible de problemas según contexto. | Supresión durante mantenimiento. |
| Flapping | Oscilaciones repetidas de estado; se mitigan con expresiones, histéresis o dependencias. | Servicio que cae y vuelve repetidamente. |

## 5. Discovery y automatización

| Concepto | Función | Ejemplo |
|---|---|---|
| Network Discovery | Busca hosts o servicios en rangos IP mediante comprobaciones configuradas. | Detectar hosts con SNMP. |
| Discovery Rule (Network) | Define rangos, checks y frecuencia de descubrimiento. | Escanear `10.80.0.0/24`. |
| Active Agent Autoregistration | Permite registrar hosts cuando agentes activos desconocidos contactan con Zabbix. | VM recién desplegada con Agent 2 activo. |
| Low-Level Discovery (LLD) | Descubre entidades dentro de un host y genera objetos usando prototipos. | Filesystems e interfaces de red. |
| LLD Rule | Regla que obtiene la lista de entidades detectables. | `vfs.fs.discovery`. |
| LLD Macro | Variable extraída del descubrimiento para crear objetos por entidad. | `{#FSNAME}`. |
| Item Prototype | Plantilla de item que genera un item por cada entidad descubierta. | Espacio libre de cada filesystem. |
| Trigger Prototype | Plantilla de trigger aplicada a cada entidad descubierta. | Disco lleno en cada filesystem. |
| Graph Prototype | Plantilla de gráfico aplicada a entidades descubiertas. | Gráfico por filesystem. |
| Host Prototype | Permite crear hosts a partir de entidades descubiertas mediante LLD. | Descubrir instancias monitorizables. |
| LLD Filter | Selecciona qué entidades descubiertas deben procesarse. | Excluir interfaces loopback. |
| Discovery Action | Ejecuta operaciones a partir de eventos de descubrimiento de red. | Añadir host y vincular templates. |

> **No confundir:** Network Discovery busca dispositivos/servicios en la red; Autoregistration incorpora agentes activos que se identifican ante Zabbix; LLD descubre componentes *dentro* de un host o fuente de datos.

## 6. Visualización e informes

| Concepto | Función | Ejemplo |
|---|---|---|
| Graph | Visualiza series históricas de items. | CPU de las últimas 24 horas. |
| Simple Graph | Gráfico disponible para un item numérico sin crear uno personalizado. | Evolución de memoria libre. |
| Custom Graph | Gráfico configurado con uno o varios items. | CPU y load average. |
| Dashboard | Panel con widgets de monitorización. | Estado global de producción. |
| Widget | Componente visual dentro de un dashboard. | Problems, Graph, Gauge. |
| Map | Representación gráfica de infraestructura y dependencias. | Mapa de sedes y enlaces. |
| Latest Data | Consulta los últimos valores e históricos de un host. | Ver CPU y memoria actual. |
| Problems View | Lista problemas abiertos o históricos, según los filtros. | Incidencias High sin reconocer. |
| Scheduled Report | Envía informes de dashboards cuando se configura el servicio necesario. | Informe semanal por correo. |
| SLA Report | Presenta métricas de cumplimiento de acuerdos de servicio. | SLA mensual de una aplicación. |

## 7. Alertas, acciones y notificaciones

| Concepto | Función | Ejemplo |
|---|---|---|
| Action | Define qué operaciones ejecutar ante eventos y bajo qué condiciones. | Notificar si severity = High. |
| Action Condition | Filtro que determina cuándo se activa una acción. | Tag `environment=production`. |
| Operation | Trabajo ejecutado por una acción. | Enviar mensaje o ejecutar comando permitido. |
| Recovery Operation | Operación al producirse recuperación. | Enviar aviso de resolución. |
| Update Operation | Operación ligada a actualizaciones del problema. | Avisar al reconocer una incidencia. |
| Escalation | Secuencia temporal de operaciones configuradas. | Avisar a N1 y después a N2. |
| Media Type | Mecanismo técnico de entrega de notificaciones. | Email, webhook o SMS. |
| User Media | Canal/destino concreto configurado para un usuario. | Correo del equipo de operaciones. |
| Notification | Mensaje enviado a destinatarios mediante un media type. | Email de fallo de disco. |
| Webhook | Integración HTTP basada en scripts para entregar eventos. | Enviar problema a sistema de tickets. |
| Remote Command | Comando ejecutado bajo condiciones y permisos establecidos. | Reiniciar un servicio autorizado. |
| Message Template | Plantilla del contenido de las notificaciones. | Asunto y cuerpo de alerta. |

## 8. Usuarios y control de acceso

| Concepto | Función | Ejemplo |
|---|---|---|
| User | Cuenta de acceso al frontend o a la API. | Operador N1. |
| User Group | Agrupa usuarios para administrar permisos y notificaciones. | `Linux Operators`. |
| User Role | Define capacidades disponibles para el usuario, incluida UI y API. | Rol de lectura. |
| Host Group Permissions | Definen el acceso Read, Read-write o Deny a grupos de hosts para grupos de usuarios. | Equipo Linux con escritura sobre Linux. |
| Template Group Permissions | Controlan acceso a grupos de templates. | Administración de templates Linux. |
| Authentication | Comprueba identidad del usuario. | Autenticación interna o LDAP. |
| MFA | Añade un segundo factor cuando está configurado. | TOTP para acceso web. |
| API Token | Credencial para autenticar solicitudes a la API. | Automatizar alta de hosts. |
| Audit Log | Registra acciones administrativas auditables. | Cambio en configuración de un host. |

## 9. Seguridad y cifrado

| Concepto | Función | Ejemplo |
|---|---|---|
| TLS | Protege comunicaciones entre componentes compatibles. | Tráfico cifrado agent ↔ proxy. |
| PSK | Clave precompartida utilizada para autenticación/cifrado TLS. | Secreto aleatorio gestionado en Vault. |
| PSK Identity | Identificador que permite seleccionar la PSK correspondiente; no es la clave. | `linux-prod-host01`. |
| TLS Certificate | Certificado utilizado en conexiones TLS basadas en certificados. | Certificados de agente y proxy. |
| TLSConnect | Determina cómo establece conexiones TLS salientes un agente. | `TLSConnect=psk`. |
| TLSAccept | Determina qué conexiones entrantes acepta un agente. | `TLSAccept=psk`. |
| Server | Parámetro del agente que autoriza direcciones de server/proxy para checks pasivos. | `Server=10.80.1.10`. |
| ServerActive | Parámetro del agente que indica los destinos de checks activos. | `ServerActive=proxy.example:10051`. |
| Hostname | Identificador del host para checks activos; debe concordar con la configuración de Zabbix. | `de-dus0-prkw02`. |
| AllowKey / DenyKey | Controlan qué keys del agente están permitidas o denegadas. | Restringir `system.run[*]`. |
| Secret Macro | Macro cuyo valor se protege en la interfaz según su tipo. | Token de integración. |
| Vault Secret Macro | Macro cuyo secreto puede obtenerse desde un gestor externo admitido. | Credencial almacenada en Vault. |

> **Puertos habituales:** Agent pasivo TCP 10050; Server/Proxy para checks activos y comunicaciones de proxy TCP 10051. Son valores predeterminados, no una garantía sobre todos los entornos.

## 10. Disponibilidad, rendimiento y retención

| Concepto | Función | Ejemplo |
|---|---|---|
| Zabbix Server HA | Permite desplegar varios nodos de server en configuración de alta disponibilidad. | Nodo activo y standby. |
| Proxy HA / Proxy Group | Facilita la continuidad de recogida mediante grupos de proxies. | Tres proxies en una sede. |
| Proxy Buffer | Conserva datos temporalmente si se interrumpe la comunicación con el server. | Retención durante caída WAN. |
| Data Collection Poller | Proceso del server/proxy que ejecuta determinados checks. | Recoger métricas periódicas. |
| Cache | Memoria utilizada para acelerar procesamiento y consultas internas. | Cache de configuración. |
| Queue | Muestra retrasos de items pendientes de recogida. | Checks con más de 5 minutos de demora. |
| Internal Item | Métrica interna del propio Zabbix. | `zabbix[queue]`. |
| Housekeeping | Elimina datos antiguos según reglas de retención. | Borrar histórico expirado. |
| History Storage Period | Tiempo de conservación de valores individuales. | 30 días. |
| Trend Storage Period | Tiempo de conservación de agregados horarios. | 365 días. |
| TimescaleDB | Extensión de PostgreSQL opcional para gestión de grandes volúmenes de series temporales. | Base histórica de monitorización. |

## 11. API e integraciones

| Concepto | Función | Ejemplo |
|---|---|---|
| Zabbix API | Interfaz JSON-RPC para administrar y consultar datos. | `host.get`. |
| API Method | Operación concreta de la API. | `host.create`, `item.get`. |
| API Token | Autentica automatizaciones sin enviar credenciales interactivas en cada llamada. | Script Python de inventario. |
| Webhook Media Type | Envía notificaciones integrándose con servicios externos. | Crear incidente en plataforma ITSM. |
| HTTP Agent | Recoge datos de servicios HTTP sin instalar un agent en ellos. | Endpoint de salud de aplicación. |
| Script | Permite ejecutar acciones o integraciones configuradas bajo permisos. | Herramienta administrativa desde la UI. |
| Export / Import | Traslada configuración compatible mediante ficheros de exportación. | Exportar un template para Git. |
| Infrastructure as Code | Automatización externa de configuración, normalmente usando API o herramientas compatibles. | Ansible o Terraform con proveedor adecuado. |

## 12. Monitorización de servicios y técnicas avanzadas

| Concepto | Función | Ejemplo |
|---|---|---|
| Service | Representa un servicio de negocio o TI y su estado. | Aplicación de facturación. |
| Service Tree | Organiza dependencias y relaciones entre servicios. | Aplicación → base de datos. |
| SLA | Acuerdo que define el nivel de disponibilidad comprometido. | Disponibilidad contractual del 99,9 %. |
| SLI | Indicador utilizado para medir comportamiento del servicio. | Porcentaje de disponibilidad observado. |
| Web Scenario | Secuencia de comprobaciones HTTP sobre una web. | Login o respuesta de una página. |
| Web Scenario Step | Una petición dentro de un escenario web. | Solicitud a `/login`. |
| JMX Monitoring | Obtiene información de aplicaciones Java a través de JMX. | Heap JVM. |
| IPMI Monitoring | Obtiene información de hardware compatible con IPMI. | Temperatura o sensores del servidor. |
| SNMP Monitoring | Monitoriza equipos compatibles con SNMP. | Interfaces de un switch. |
| ODBC Monitoring | Obtiene métricas mediante consultas a bases de datos compatibles. | Resultado de una consulta SQL. |
| Log Monitoring | Lee y analiza logs mediante items apropiados. | Buscar `ERROR` en un log. |
| Log Rotation | Cambio de ficheros de log que debe contemplarse al monitorizarlos. | `app.log` → `app.log.1`. |
| VMware Monitoring | Descubre y monitoriza recursos VMware desde Zabbix. | Estado de ESXi y VM. |
| Anomaly Detection | Uso de funciones de predicción o comparación para detectar valores anómalos. | Métrica fuera de patrón histórico. |
| Baseline | Referencia histórica empleada para comparar el comportamiento actual. | CPU mayor que en periodos equivalentes. |

## Relaciones fundamentales

```text
Host ── vincula ──> Template
                        ├── Items ──> History / Trends ──> Graphs / Dashboards
                        ├── Triggers ──> Events / Problems ──> Actions ──> Notifications
                        └── LLD Rules ──> Prototypes ──> Items / Triggers / Graphs

Zabbix Agent ──> Zabbix Proxy (opcional) ──> Zabbix Server ──> Database
                                                    │
                                               Frontend / API
```

## Conceptos que suelen confundirse

| No son lo mismo | Diferencia clave |
|---|---|
| Item vs Trigger | El item obtiene un dato; el trigger evalúa una condición sobre datos. |
| Trigger vs Event | El trigger es una regla; el evento es un suceso registrado. |
| Event vs Problem | No todos los eventos son problemas; también hay recuperaciones o descubrimientos. |
| Trigger vs Action | El trigger detecta; la action decide qué hacer. |
| Host Group vs Proxy Group | El primero agrupa hosts y facilita permisos; el segundo agrupa proxies para distribución y disponibilidad. |
| Network Discovery vs LLD | El primero descubre hosts/servicios de red; el segundo descubre entidades monitorizables y crea objetos con prototipos. |
| Passive vs Active Agent | En pasivo consulta el server/proxy; en activo el agente solicita checks y envía valores. |
| History vs Trends | History contiene muestras individuales; trends almacena agregados horarios numéricos. |
| PSK Identity vs PSK | La identity identifica qué clave utilizar; la PSK es el secreto. |
| Enabled vs Available | Enabled indica configuración; Available refleja el estado observado de disponibilidad de la interfaz. |

## Orden recomendado para estudiar

1. **Fundamentos:** Host, Host Group, Template, Item, Trigger, Event, Problem, Graph.
2. **Operación:** Latest Data, Problems, Dashboard, Acknowledgement, Maintenance.
3. **Arquitectura:** Server, Proxy, Proxy Group, Agent 2, Passive/Active Checks.
4. **Configuración automática:** LLD, prototipos, Network Discovery, Autoregistration.
5. **Alertas:** Action, Condition, Escalation, Media Type, Webhook.
6. **Seguridad:** Roles, permisos, PSK, certificados, macros secretas.
7. **Administración avanzada:** Preprocessing, Dependent Items, API, HA, Queue, Housekeeping, SLA.

## Documentación oficial (Zabbix 7.0)

- [Definiciones](https://www.zabbix.com/documentation/7.0/es/manual/definitions)
- [Manual principal](https://www.zabbix.com/documentation/7.0/es/manual)
- [Descubrimiento de bajo nivel](https://www.zabbix.com/documentation/7.0/es/manual/discovery/low_level_discovery)
- [Descubrimiento de red](https://www.zabbix.com/documentation/7.0/es/manual/discovery/network_discovery)
- [Autoregistro](https://www.zabbix.com/documentation/7.0/es/manual/discovery/auto_registration)
- [Preprocesamiento](https://www.zabbix.com/documentation/7.0/es/manual/config/items/preprocessing)
- [Eventos](https://www.zabbix.com/documentation/7.0/en/manual/config/events)
- [Actions](https://www.zabbix.com/documentation/7.0/en/manual/config/notifications/action)

---

**Nota:** Los ejemplos ilustran el comportamiento, no son parámetros listos para copiar a producción. Según el tipo de item, la versión exacta y la configuración de cada componente pueden existir particularidades adicionales.
