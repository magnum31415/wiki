[AZ104-INDEX](./readme.md)

# 13 - ARM Templates (AZ-104)

> Este documento resume la teoría de **Azure Resource Manager (ARM) Templates** más preguntada en el examen **AZ-104**.

---

# Índice

- ¿Qué es un ARM Template?
- Estructura
- Parameters
- Variables
- Outputs
- Funciones
- copy
- copyIndex()
- resourceId()
- Deployment Modes
- Resource Group Deployment
- Subscription Deployment
- Buenas prácticas
- Preguntas trampa

---

# 1. ¿Qué es un ARM Template?

Un **ARM Template** es un archivo JSON que permite desplegar infraestructura en Azure mediante el paradigma **Infrastructure as Code (IaC)**.

Permite crear recursos de forma:

- Repetible.
- Automatizada.
- Declarativa.

---

# 2. Estructura básica

Un ARM Template contiene habitualmente:

- Parameters
- Variables
- Resources
- Outputs

Ejemplo:

```json
{
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {}
}
```

---

# 3. Parameters

Los **Parameters** permiten personalizar el despliegue sin modificar la plantilla.

Ejemplos:

- Nombre de la VM.
- Región.
- Tamaño de la VM.
- Nombre del Storage Account.

---

# 4. Variables

Las **Variables** almacenan valores calculados o reutilizables dentro del template.

Reducen la duplicación de código.

---

# 5. Outputs

Los **Outputs** muestran información al finalizar el despliegue.

Ejemplos:

- Dirección IP.
- Nombre de un recurso.
- Resource ID.

---

# 6. Función resourceId()

La función **resourceId()** devuelve el **Resource ID** de un recurso.

Se utiliza para crear referencias entre recursos del mismo template.

Ejemplo:

```json
resourceId(
'Microsoft.Storage/storageAccounts',
'storage1')
```

---

# 7. copy

La función **copy** permite crear múltiples recursos mediante un bucle.

Ejemplo:

- Varias máquinas virtuales.
- Varios discos.
- Varias NICs.

Evita duplicar código.

---

# 8. copyIndex()

La función **copyIndex()** devuelve el índice de cada iteración del bucle **copy**.

Es muy utilizada para:

- Numerar recursos.
- Asignar LUN a discos.
- Generar nombres únicos.

Ejemplo:

```
VM1

VM2

VM3
```

---

# 9. Deployment Modes

ARM Templates soportan dos modos.

## Incremental

Es el modo predeterminado.

Solo:

- crea recursos nuevos.
- actualiza recursos existentes.

**Nunca elimina recursos.**

---

## Complete

Azure elimina todos los recursos existentes del Resource Group que **no aparecen en la plantilla**.

Debe utilizarse con mucha precaución.

Pregunta muy habitual.

---

# 10. Resource Group Deployment

Permite desplegar recursos dentro de un **Resource Group** existente.

Ejemplo:

```powershell
New-AzResourceGroupDeployment
```

Es el despliegue más habitual.

---

# 11. Subscription Deployment

Permite desplegar recursos cuyo ámbito es la **Subscription**.

Ejemplo:

- Resource Groups.
- Policies.
- Role Assignments.

Cmdlet:

```powershell
New-AzSubscriptionDeployment
```

---

# 12. Parámetro -Location

En un **Subscription Deployment**, el parámetro:

```
-Location
```

**NO indica dónde se crearán los recursos.**

Solo especifica dónde Azure almacenará los metadatos del despliegue.

La ubicación real de los recursos viene definida en la propiedad **location** del template.

Es una pregunta muy repetida.

---

# 13. Recursos existentes

Si el template intenta crear un recurso que ya existe:

↓

Ese recurso producirá un error.

↓

El resto de recursos del despliegue pueden continuar creándose.

---

# 14. Dependencias

ARM determina automáticamente muchas dependencias entre recursos.

Cuando es necesario, puede utilizarse:

```json
dependsOn
```

para garantizar el orden correcto de creación.

---

# 15. Buenas prácticas

Microsoft recomienda:

- Utilizar **Parameters** para valores variables.
- Reutilizar código mediante **Variables**.
- Crear múltiples recursos mediante **copy**.
- Utilizar **Incremental Mode** salvo que sea imprescindible **Complete Mode**.
- Utilizar **resourceId()** para referenciar recursos.

---

# Preguntas trampa del AZ-104

✅ **Incremental** es el modo de despliegue predeterminado.

✅ **Complete Mode** elimina los recursos que no aparecen en la plantilla.

✅ **copy()** crea múltiples recursos automáticamente.

✅ **copyIndex()** devuelve el índice de la iteración.

✅ **resourceId()** devuelve el identificador de un recurso.

✅ **New-AzSubscriptionDeployment** despliega recursos a nivel de **Subscription**.

✅ El parámetro **-Location** de un Subscription Deployment **no determina la región de los recursos**, solo dónde se almacenan los metadatos del despliegue.

✅ La propiedad **location** del recurso dentro del template determina la región donde se creará.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| ARM Template | Infrastructure as Code |
| Parameters | Valores configurables |
| Variables | Valores reutilizables |
| Outputs | Información al finalizar |
| resourceId() | Resource ID |
| copy | Crear múltiples recursos |
| copyIndex() | Índice del bucle |
| Incremental | Crea y actualiza |
| Complete | Elimina recursos no definidos |
| Resource Group Deployment | New-AzResourceGroupDeployment |
| Subscription Deployment | New-AzSubscriptionDeployment |
| location | Región del recurso |
| -Location | Metadatos del despliegue |
| dependsOn | Orden de creación |
