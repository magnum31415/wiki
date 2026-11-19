[Azure](azure.md)

# Disaster Recovery

# 📊 Azure Servicios Comunes vs Disaster Recovery Recomendado

| Servicio Azure | Tipo | DR recomendado | Cómo funciona el DR | Failover | Notas clave examen |
|---------------|------|----------------|----------------------|----------|-------------------|
| Virtual Machines (IaaS) | IaaS | Azure Site Recovery (ASR) | Replica discos a otra región y permite failover | Manual (automatizable con runbooks) | DR real = replicación continua |
| SQL Server en VM | IaaS | Always On AG + ASR | AG para HA, ASR para DR regional | AG: Automático (si síncrono) / ASR: Manual | HA ≠ DR |
| Azure SQL Database (PaaS) | PaaS | Auto-Failover Group | Replica base a región secundaria | Automático o Manual | DR gestionado por plataforma |
| Azure SQL Managed Instance | PaaS | Auto-Failover Group | Failover entre regiones | Automático o Manual | Similar a SQL DB pero a nivel instancia |
| Azure Storage Account | PaaS | GRS / RA-GRS / GZRS | Replicación automática entre regiones | Manual (Microsoft puede iniciar en desastre mayor) | Elegir redundancia correcta |
| Azure App Service | PaaS | Multi-region + Front Door | Despliegue en 2 regiones + balanceo global | Automático (si Front Door) | Slots ≠ DR |
| Azure Kubernetes Service (AKS) | PaaS | Multi-region + backup etcd | Cluster duplicado en otra región | Manual (requiere orquestación) | No tiene DR automático nativo |
| Azure Functions | Serverless | Multi-region deployment | Deploy en varias regiones + Front Door | Automático (si Front Door) | Stateless facilita DR |
| Azure Cosmos DB | PaaS | Multi-region replication | Replicación activa-activa opcional | Automático | SLA 99.999% multi-region |
| Azure Virtual Network | IaaS Networking | Re-deploy + IaC | ARM/Bicep/Terraform para recrear | Manual | Networking no se replica automáticamente |
| Azure Load Balancer | Networking | Re-deploy en región secundaria | Parte de arquitectura multi-región | Manual (depende del diseño) | Es regional |
| Azure Application Gateway | Networking | Multi-region + Front Door | Gateway por región | Automático (si Front Door) | WAF por región |
| Azure Key Vault | PaaS | Geo-redundant (Standard/Premium) | Replicación automática | Automático (gestionado por plataforma) | Managed HSM requiere diseño específico |
| Microsoft Entra ID | SaaS | N/A (global service) | Servicio global Microsoft | N/A | No requiere DR del cliente |


## Reglas mentales rápidas (AZ-305)

- IaaS → Azure Site Recovery
- SQL PaaS → Auto-Failover Group
- Storage → Elegir redundancia correcta (GRS/GZRS)
- Web/App → Multi-región + Front Door
- Cosmos DB → Multi-region nativo
- Entra ID → Servicio global (no diseñar DR)

  # 🏗 DR en Azure – Modelo por 4 Niveles (Enfoque Arquitectónico AZ-305)

---

## 1️⃣ Región / Zona (Infraestructura física)

Pregunta clave:
> ¿Contra qué tipo de fallo me estoy protegiendo?

| Nivel | Protege contra | Tecnología típica |
|-------|---------------|------------------|
| Locally Redundant | Fallo de hardware | LRS |
| Zone-Redundant | Caída de una Availability Zone | Zone Redundancy |
| Geo-Redundant | Caída regional | Geo-replication |
| Multi-region activo | DR completo empresarial | Arquitectura activa-activa |

🎯 Claves examen:
- "Protect against zone-level failure" → Zone-Redundant  
- "Protect against regional outage" → Geo-replication / Failover Group  

---

## 2️⃣ Plataforma (Compute Layer)

Pregunta clave:
> ¿Dónde corre mi aplicación?

| Plataforma | DR típico |
|------------|----------|
| VM (IaaS) | Azure Site Recovery |
| App Service | Deploy en región secundaria |
| AKS | Cluster secundario en otra región |
| Azure Functions | Re-deploy multi-región |

🎯 Regla:
- IaaS → necesitas configurar DR explícitamente (ASR).
- PaaS → muchas veces el DR viene integrado.

---

## 3️⃣ Servicio (Servicio gestionado específico)

Pregunta clave:
> ¿El servicio ya incluye DR nativo?

| Servicio | DR nativo |
|----------|-----------|
| Azure SQL Database | Failover Group |
| Azure SQL Managed Instance | Auto-Failover Group |
| Azure Storage | GRS / RA-GRS |
| Cosmos DB | Multi-region writes |

🎯 Clave examen:
Antes de proponer ASR, revisa si el servicio ya tiene DR integrado.

---

## 4️⃣ Datos (Persistencia y recuperación)

Pregunta clave:
> ¿Puedo recuperar datos borrados o corruptos?

| Mecanismo | Qué cubre | Impacto en RPO | Impacto en RTO |
|------------|----------|---------------|---------------|
| PITR | Restaurar a un punto exacto en el tiempo | Bajo (hasta el último log disponible) | Medio (minutos mientras restaura) |
| LTR | Retención de backups durante años | Alto (depende del último backup almacenado) | Alto (restauración completa) |
| Backup automático | Copias periódicas completas/diferenciales/log | Depende de frecuencia de backup | Medio |
| Snapshots | Restauración rápida basada en snapshot | Bajo | Bajo (recuperación rápida) |

---

### 📌 Recordatorio clave

| Concepto | Qué significa |
|----------|--------------|
| **RPO (Recovery Point Objective)** | Cuánta pérdida de datos es aceptable |
| **RTO (Recovery Time Objective)** | Cuánto tiempo puede tardar en recuperarse |

---

⚠ DR ≠ Backup  

- **DR** protege contra caída de infraestructura (región, zona, servidor).
- **Backup** protege contra borrado accidental, corrupción o errores lógicos.
- RPO y RTO son **objetivos de negocio**, no tecnologías.

---

# 🧠 Modelo mental completo

````
Infraestructura (zona/región)
↓
Compute (VM / App / AKS)
↓
Servicio (SQL / Storage / etc.)
↓
Datos (Backup / PITR / LTR)
````


Si falta una capa → la arquitectura está incompleta.

---

# 🎯 Ejemplo típico AZ-305

Escenario:
- Web App
- Azure SQL
- Requisito: minimizar downtime y pérdida de datos ante caída regional

Solución por capas:

1️⃣ Región → secundaria emparejada  
2️⃣ Plataforma → Web App desplegada en ambas regiones  
3️⃣ Servicio → Failover Group  
4️⃣ Datos → PITR + LTR configurado  

---

# 🏁 Resumen ultra-rápido

- Zona protege contra fallo local.
- Región protege contra desastre regional.
- Servicio puede tener DR nativo.
- Backup protege contra pérdida lógica de datos.

---

# Availability Group (AG)

**Un Availability Group (AG) es un sistema que mantiene una copia sincronizada de tu base de datos en otro servidor para que, 
si el principal se cae, otro tome el control automáticamente sin que la aplicación lo note.**

- Availability Grou es principalmente HA.
- Auto-Failover Group es DR regional en PaaS.

Sirve para:

- Alta disponibilidad (HA)
- Minimizar downtime
- Evitar pérdida de datos (si es síncrono)

````
                    La aplicación
                          │
                          ▼
                ┌─────────────────────┐
                │     AG Listener     │
                │   miapp-ag.domain   │
                └──────────┬──────────┘
                           │
                     Aplicación
                           │
        ┌──────────────────┴──────────────────┐
        │                                     │
 ┌───────────────┐                    ┌───────────────┐
 │ SQL VM 1      │ ◄── Síncrono ──►    │ SQL VM 2      │
 │ PRIMARY       │                    │ SECONDARY     │
 │ Read/Write    │                    │ Read-Only     │
 └───────────────┘                    └───────────────┘


````

- Si el **Primary** se cae:
  - **Secondary** → se convierte en Primary
  - La aplicación sigue usando el mismo Listener.


Usado en:
- SQL Server on-prem
- SQL Server en Azure VM (IaaS)

**Está pensado principalmente para: ✅ Alta disponibilidad (HA)**

- Fallo de VM
- Fallo de nodo
- Failover en segundos
- Puede ser síncrono → RPO ≈ 0

**¿Puede usarse para DR?**
Sí.
Pero:
 - Debes configurarlo tú entre regiones
 - Es más complejo
 - No es “automático PaaS”

👉 AG = HA fuerte + DR posible pero manual/arquitectónico

--- 

#🔷 Auto-Failover Group (FOG)

Usado en:
- Azure SQL Database
- Azure SQL Managed Instance

````
## Para Azure SQL Database y Azure SQL Managed Instance
                    APLICACIÓN

        ┌───────────────────────────────────────┐
        │                                       │
        │  🔵 Read/Write Endpoint               │
        │  miapp.database.windows.net           │
        │                                       │
        │  🟢 Read-Only Endpoint                │
        │  miapp.secondary.database.windows.net │
        │                                       │
        └───────────────┬───────────────┬───────┘
                        │               │
                        │               │
                        ▼               ▼


            ┌──────────────────────────────┐
            │     REGIÓN PRIMARIA          │
            │                              │
            │  Azure SQL Database /        │
            │  Azure SQL Managed Instance  │
            │  (Read/Write)                │
            └──────────────┬───────────────┘
                           │
                           │  Replicación
                           │  ASINCRÓNICA
                           ▼
            ┌──────────────────────────────┐
            │     REGIÓN SECUNDARIA        │
            │                              │
            │  Azure SQL Database /        │
            │  Azure SQL Managed Instance  │
            │  (Read-Only)                 │
            └──────────────────────────────┘


````

**Está pensado para: ✅ DR entre regiones**

- Replicación asincrónica
- Endpoint único
- Failover automático opcional
- RPO > 0
- RTO minutos

**¿Es HA local?**
- No.
- La HA local ya viene integrada en el servicio PaaS.

👉 FOG = DR regional gestionado por Azure

| Tecnología          | Principal objetivo | Tipo de servicio |
| ------------------- | ------------------ | ---------------- |
| AG                  | HA (y opcional DR) | IaaS             |
| Auto-Failover Group | DR regional        | PaaS             |


- SQL Server en VM → AG
- Azure SQL PaaS → Auto-Failover Group
- RPO = 0 obligatorio → AG síncrono
- DR sencillo entre regiones → Auto-Failover Group
- AG protege el servidor.
- Failover Group protege la región.
