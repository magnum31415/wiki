[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)





# 📂 AZ-104 – Azure File Sync (Teoría + Orden Correcto)

---

# 1️⃣ Teoría que debes saber para el examen AZ-104

## 🔹 ¿Qué es Azure Files?

Azure Files es un servicio que permite crear **file shares en la nube** accesibles vía:

- SMB
- REST

Permite:

- Compartir archivos entre VMs
- Migrar file servers on-prem
- Centralizar datos

---

## 🔹 ¿Qué es Azure File Sync?

Azure File Sync permite:

> Sincronizar un servidor Windows on-premises con un Azure File Share.

Convierte tu Windows Server en:

🗂️ Una **caché local** de un Azure File Share.

---

## 🔹 Arquitectura de Azure File Sync

Componentes clave:

| Componente | Función |
|------------|----------|
| **Azure File Share** | Donde viven los datos en la nube |
| **Storage Sync Service** | Orquesta la sincronización |
| **Sync Group** | Define qué endpoints se sincronizan |
| **Cloud Endpoint** | El Azure File Share dentro del Sync Group |
| **Server Endpoint** | Carpeta específica en el servidor on-prem |
| **Azure File Sync Agent** | Software instalado en Windows Server |


![azure-files](img/azure/azure-files.png)


---

# 🧠 Concepto clave de examen

Azure File Sync funciona así:

Azure File Share  
⬇  
Storage Sync Service  
⬇  
Sync Group  
⬇  
Cloud Endpoint  
⬇  
Server Endpoint  
⬇  
Servidor Windows (con agente instalado)

---

# 2️⃣ Análisis de la pregunta

## 📌 Escenario

Tienes:

- File Share en Azure
- Storage Sync Service creado
- Servidor on-prem TDFileServer1
- Quieres sincronizar archivos

---

## 📌 ¿Qué hay que hacer?

Para que un servidor on-prem sincronice con Azure necesitas:

1️⃣ Instalar el agente  
2️⃣ Registrar el servidor  
3️⃣ Crear Sync Group  
4️⃣ Configurar endpoints  

El orden es importante.

---

# 3️⃣ Orden correcto explicado

## ✅ Paso 1 – Deploy the Azure File Sync agent

Primero debes instalar el agente en:

TDFileServer1

Sin el agente el servidor no puede hablar con Azure.

---

## ✅ Paso 2 – Register TDFileServer1 with Storage Sync Service

Esto:

- Establece relación de confianza
- Vincula el servidor al servicio de sincronización

Un servidor solo puede registrarse en un Storage Sync Service.

---

## ✅ Paso 3 – Set up a sync group and configure a cloud endpoint

Aquí defines:

- Qué Azure File Share participa
- Qué grupo de sincronización se crea

El Cloud Endpoint representa el Azure File Share.

---

## ✅ Paso 4 – Create a server endpoint

Aquí defines:

- Qué carpeta local del servidor se sincroniza

Ejemplo:
D:\Shares\Finance

Ese es el Server Endpoint.

---

# 🎯 Orden final correcto

1. Deploy the Azure File Sync agent to TDFileServer1  
2. Register TDFileServer1 with Storage Sync Service  
3. Set up a sync group and configure a cloud endpoint  
4. Create a server endpoint  

---

# 🧩 Por qué ese orden

| Paso | Por qué va aquí |
|------|-----------------|
| Instalar agente | Sin agente no hay comunicación |
| Registrar servidor | Sin registro no puedes crear endpoints |
| Crear sync group | Define la topología |
| Crear server endpoint | Conecta carpeta local al grupo |

---

# 🧠 Regla mental para el examen

Si la pregunta menciona:

- Sincronizar servidor on-prem
- Azure File Share
- File Recovery
- Cache local

👉 Estás en Azure File Sync

Y el orden siempre es:

Agente → Registro → Sync Group → Endpoints

---

# 🔥 Trampa típica de examen

Muchos fallan porque ponen:

Crear Sync Group antes de registrar el servidor.

Eso es incorrecto porque:

No puedes crear un Server Endpoint si el servidor no está registrado.

---

# 🏁 Resumen ultra corto para memorizar

Azure File Sync siempre sigue este patrón:

Instalar → Registrar → Definir Cloud → Definir Server

Si recuerdas eso, esta pregunta nunca la fallas.
