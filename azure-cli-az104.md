[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

- [Resumen comandos `az acr` (Azure Container Registry) - AZ-104](#resumen-comandos-az-acr-azure-container-registry---az-104)

  
# Resumen comandos `az acr` (Azure Container Registry) - AZ-104

| Comando | Función | Ejemplo |
|---|---|---|
| `az acr create` | Crear un Azure Container Registry | `az acr create --resource-group rg1 --name corpregistry --sku Basic` |
| `az acr list` | Listar registries ACR | `az acr list --output table` |
| `az acr show` | Mostrar información de un ACR | `az acr show --name corpregistry` |
| `az acr delete` | Eliminar un ACR | `az acr delete --name corpregistry` |
| `az acr login` | Login Docker contra ACR | `az acr login --name corpregistry` |
| `az acr build` | Build + push usando ACR Tasks | `az acr build --registry corpregistry --image app:v1 .` |
| `az acr repository list` | Listar repositories | `az acr repository list --name corpregistry --output table` |
| `az acr repository show-tags` | Mostrar tags de una imagen | `az acr repository show-tags --name corpregistry --repository app` |
| `az acr repository delete` | Eliminar imagen/repository | `az acr repository delete --name corpregistry --repository app` |
| `az acr credential show` | Ver credenciales Admin User | `az acr credential show --name corpregistry` |
| `az acr update` | Modificar configuración ACR | `az acr update --name corpregistry --admin-enabled true` |
| `az acr check-health` | Validar conectividad y estado ACR | `az acr check-health --name corpregistry` |

---

## Flujo típico completo

| Paso | Comando |
|---|---|
| Crear ACR | `az acr create` |
| Login ACR | `az acr login` |
| Tag imagen | `docker tag` |
| Push imagen | `docker push` |
| Ver repositories | `az acr repository list` |

---

### Ejemplo completo AZ-104

### 1. Crear registry

```bash
az acr create \
  --resource-group rg-containers \
  --name corpregistry \
  --sku Basic
```

---

### 2. Login contra ACR

```bash
az acr login --name corpregistry
```

---

### 3. Tag imagen Docker

```bash
docker tag image1 corpregistry.azurecr.io/image1:v1
```

---

### 4. Push imagen

```bash
docker push corpregistry.azurecr.io/image1:v1
```

---

### 5. Ver imágenes almacenadas

```bash
az acr repository list \
  --name corpregistry \
  --output table
```

---

## Comandos importantes examen

| Comando | Muy preguntado |
|---|---|
| `az acr create` | ✅ |
| `az acr login` | ✅ |
| `az acr build` | ✅ |
| `az acr repository list` | ✅ |

---

## Diferencia importante

| Comando | Qué hace |
|---|---|
| `az acr create` | Crear registry |
| `az acr build` | Build + push imagen |
| `az acr login` | Autenticación Docker |
| `docker push` | Subir imagen |

---

## Trampa típica AZ-104

### `az acr build`

Muchos candidatos creen que:

```bash
az acr build
```

crea el registry.

❌ Incorrecto.

Solo:

- construye imagen
- sube imagen

sobre un ACR ya existente.

---

## Regla rápida examen

```text
az acr create → crea el registry
```

```text
az acr build → build + push imagen
```

```text
az acr login → login Docker contra ACR
```
