
| Concept | Qué es | Dónde se usa |
|---|---|---|
| **Claim** | Información sobre una identidad incluida en un token de autenticación | OAuth / OpenID Connect / Entra ID |
| **Access Key (Storage)** | Clave secreta que da acceso directo a la Storage Account | Autenticación basada en claves |


## Modelos de autenticación en Azure Storage

| Modelo | Usa claims | Usa claves |
|---|---|---|
| Microsoft Entra ID (RBAC) | ✔ | ❌ |
| Shared Key (Access Keys) | ❌ | ✔ |
| SAS Token | ❌ | ✔ |
