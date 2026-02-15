[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure App Service


# 🔵 ¿Qué es Azure App Service?

Azure App Service es un servicio **PaaS (Platform as a Service)** para alojar:

- Aplicaciones web
- APIs REST
- Backends móviles
- Aplicaciones en contenedores

Sin tener que gestionar:

- Servidores
- Sistema operativo
- Parches
- Infraestructura

---

# 🧱 Qué te proporciona

- Hosting gestionado (Windows o Linux)
- Escalado automático o manual
- Alta disponibilidad
- Integración con Azure AD
- CI/CD desde GitHub, Azure DevOps, etc.
- SSL y dominios personalizados

---

# 🔷 Cómo funciona

Cuando creas una app, la asignas a un:

## App Service Plan

El App Service Plan define:

- CPU
- RAM
- Número de instancias
- Región
- Precio

Varias apps pueden compartir el mismo plan y recursos.

---

# 🔷 Tipos principales

- **Web App** → aplicaciones web y APIs
- **Web App for Containers** → contenedores Docker
- **Mobile App** → backend móvil

---

# 🔷 Diferencia con otros servicios

| Servicio | Uso principal |
|----------|--------------|
| App Service | Apps siempre activas |
| Azure Functions | Código event-driven |
| Container Apps | Microservicios en contenedores |
| AKS | Kubernetes gestionado |

---

# 🎯 Cuándo usarlo

- Aplicación web pública
- API corporativa
- Backend estable
- Necesitas despliegue sin downtime (Deployment Slots)

---

# 🧠 Resumen para examen AZ-305

App Service =  
Hosting web gestionado en Azure con escalado y alta disponibilidad sin gestionar infraestructura.

# 📘 AZ-305 – Resumen clave para preguntas tipo *App Service Case Study*

Este resumen cubre la teoría que necesitas dominar para responder correctamente preguntas como las del escenario de CVD.

---

# 🔷 1️⃣ App Service Plan vs App Service Environment (ASE)

## 🔵 App Service Plan

- Define los recursos (CPU, RAM, almacenamiento).
- Varias apps pueden compartir el mismo plan.
- Todas las apps deben:
  - Ser Windows o Linux (no mezclar)
  - Usar el mismo runtime base
- Es la opción **más rentable**.

### 🔎 Claves examen
- “Cost-effective” → App Service Plan
- “Apps públicas accesibles desde Internet” → Plan normal
- “Compartir recursos para ahorrar costes” → Mismo plan

---

## 🔴 App Service Environment (ASE)

- Entorno totalmente aislado.
- Integración profunda con VNet.
- Infraestructura dedicada.
- Mucho más caro.

### 🔎 Cuándo usarlo
- Requisitos de aislamiento extremo
- Cumplimiento normativo estricto
- Aplicaciones internas privadas

⚠️ Si no se menciona aislamiento o red privada estricta → NO usar ASE.

---

## 🎯 Regla mental

| Requisito | Solución correcta |
|------------|------------------|
| Minimizar coste | App Service Plan |
| Aislamiento completo | ASE |
| Multi-región | Plan por región |
| Por availability zone | ❌ No es coste-efectivo |

---

# 🔷 2️⃣ Alta disponibilidad y Zonas

Si activas **Availability Zones** en App Service:

- Azure exige mínimo 3 instancias.
- Pagas por las 3 instancias.
- No es la opción más económica.

👉 Si el requisito dice “cost-effective” → no uses por zona.

---

# 🔷 3️⃣ Deployment Slots (MUY IMPORTANTE)

Permiten:

- Tener entorno staging y producción.
- Probar nueva versión antes de publicar.
- Hacer *swap* sin downtime.
- Warm-up antes del swap.
- Rollback inmediato.

### 🎯 Si lees:
- “Replace production without interruption”
- “Deploy staging before production”

👉 Respuesta: **Create a deployment slot**

---

# 🔷 4️⃣ Load Balancing + WAF

Si el requisito dice:

- “Actively load balanced”
- “Pass through a WAF”
- “Traffic routing by region”

Soluciones típicas:

| Necesidad | Servicio |
|------------|----------|
| WAF + HTTP/S | Azure Application Gateway |
| Routing por región | Azure Front Door |
| Balanceo interno | Azure Load Balancer |

Para tráfico global → **Front Door + WAF**

---

# 🔷 5️⃣ Multi-región y enrutamiento

Si dice:

- “North America → West US”
- “Others → East Asia”

Eso es:
👉 Routing basado en geografía  
👉 Azure Front Door o Traffic Manager

---

# 🔷 6️⃣ Azure Storage + SMB + On-prem

Si el requisito dice:

- Acceso SMB
- LAN on-prem
- Replicación a on-prem

Necesitas:
👉 Azure Files (SMB)
👉 Azure File Sync

---

# 🔷 7️⃣ Key Vault Integration

Si la app necesita:

- Credenciales
- Connection strings

Usar:
👉 Managed Identity + Azure Key Vault

Nunca hardcodear secretos.

---

# 🔷 8️⃣ Monitorizar rendimiento sin cambiar código

Si dice:

- “Analyze performance”
- “Without modifying application code”

Solución:
👉 Application Insights
👉 Azure Monitor

---

# 🔷 9️⃣ Resumen mental rápido AZ-305

| Si lees… | Piensa en… |
|------------|------------|
| Cost-effective multi-region | App Service Plan per region |
| Aislamiento total | ASE |
| Zero downtime deployment | Deployment Slots |
| Global routing | Front Door |
| WAF requerido | Application Gateway / Front Door WAF |
| Secretos | Key Vault + Managed Identity |
| SMB acceso on-prem | Azure Files |
| Analizar rendimiento sin tocar código | Application Insights |

---

# 🧠 Concepto clave

Azure App Service = hosting PaaS compartido  
ASE = hosting aislado y caro  
Deployment Slot = zero downtime swap  

---

Si quieres, puedo prepararte una hoja de “trampas típicas” específicas para App Service en AZ-305.

