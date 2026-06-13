[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# ARM Templates - `copy`, `copyIndex()` y `dependsOn` (AZ-104)

## Concepto

En Azure Resource Manager (ARM) Templates es habitual necesitar:

* crear varios recursos iguales
* crear varias propiedades iguales
* controlar el orden de creación de los recursos

Para ello existen conceptos como:

* `copy`
* `copyIndex()`
* `dependsOn`

---

# `copy`

## Qué es

La propiedad `copy` permite crear múltiples instancias de un recurso o de una propiedad de forma dinámica.

Es equivalente a un bucle (`for`) en un lenguaje de programación.

---

## Cuándo se utiliza

Por ejemplo para crear:

* varias Data Disks
* varias NICs
* varias máquinas virtuales
* varias reglas
* varios recursos iguales

---

## Ejemplo conceptual

```text
numberOfDisks = 4

        │

        ▼

      copy

        │

 ┌──────┼──────┬──────┬──────┐

 ▼      ▼      ▼      ▼

disk0  disk1  disk2  disk3
```

---

## Ejemplo ARM

```json
"copy": [
  {
    "name": "dataDisks",
    "count": "[parameters('numberOfDataDisks')]",
    "input": {
      ...
    }
  }
]
```

---

# `copyIndex()`

## Qué es

`copyIndex()` es una función que devuelve el índice de la iteración actual dentro de un bloque `copy`.

No crea recursos.

Simplemente devuelve:

```text
0
1
2
3
...
```

---

## Ejemplo conceptual

```text
copy count = 4

Iteration 1

copyIndex() = 0

----------------

Iteration 2

copyIndex() = 1

----------------

Iteration 3

copyIndex() = 2

----------------

Iteration 4

copyIndex() = 3
```

---

## Uso típico

Generar nombres distintos:

```json
"name": "[concat('disk', copyIndex())]"
```

Resultado:

```text
disk0

disk1

disk2

disk3
```

---

## Importante para el examen

`copyIndex()`:

* ✅ es una función
* ❌ NO es una propiedad del template

Por tanto:

Correcto:

```json
"copy": [
   ...
]
```

y dentro:

```json
"[copyIndex()]"
```

Incorrecto:

```json
"copyIndex": [
```

---

# `dependsOn`

## Qué es

Permite indicar que un recurso depende de otro.

Azure no creará el recurso hasta que sus dependencias estén completadas.

---

## Ejemplo conceptual

```text
Virtual Network

        │

dependsOn

        ▼

Network Interface

        │

dependsOn

        ▼

Virtual Machine
```

---

## Otro ejemplo

```text
Storage Account

        │

dependsOn

        ▼

Virtual Machine
```

---

## Ejemplo ARM

```json
"dependsOn": [
    "[resourceId('Microsoft.Network/networkInterfaces','nic1')]"
]
```

---

# Diferencia entre `copy` y `dependsOn`

| Concepto      | Función                                    |
| ------------- | ------------------------------------------ |
| `copy`        | Repetir recursos o propiedades             |
| `copyIndex()` | Obtener el índice de la repetición         |
| `dependsOn`   | Controlar el orden de creación de recursos |

---

# Comparación rápida

## `copy`

```text
Crear 5 discos

        │

        ▼

disk0

disk1

disk2

disk3

disk4
```

---

## `copyIndex()`

```text
Iteración

0

1

2

3

4
```

---

## `dependsOn`

```text
Virtual Network

        │

        ▼

Network Interface

        │

        ▼

Virtual Machine
```

---

# Relación entre `copy` y `copyIndex()`

```text
             copy

               │

        ┌──────┼──────┐

        ▼      ▼      ▼

      iter0  iter1  iter2

        │      │      │

        ▼      ▼      ▼

 copyIndex=0  =1     =2
```

---

# Trampas típicas del AZ-104

## Trampa 1

Pensar que `copyIndex()` crea recursos.

❌ Incorrecto.

Solo devuelve el índice de la iteración.

---

## Trampa 2

Usar:

```json
"copyIndex": [
```

❌ Incorrecto.

`copyIndex()` es una función.

---

## Trampa 3

Pensar que `dependsOn` sirve para crear varios recursos.

❌ Incorrecto.

Solo controla dependencias.

---

# Regla rápida para memorizar

```text
Need multiple resources?

        │

        ▼

      copy
```

```text
Need the current iteration number?

        │

        ▼

    copyIndex()
```

```text
Need to control creation order?

        │

        ▼

    dependsOn
```

---

# Chuleta AZ-104

| Si ves...                        | Piensa en...  |
| -------------------------------- | ------------- |
| Crear varias Data Disks          | `copy`        |
| Crear varias NICs                | `copy`        |
| Crear varios recursos iguales    | `copy`        |
| Obtener 0,1,2,3...               | `copyIndex()` |
| Nombrar recursos dinámicamente   | `copyIndex()` |
| Crear un recurso después de otro | `dependsOn`   |
| Controlar dependencias           | `dependsOn`   |

## Regla de oro

```text
copy
    ↓
Repite recursos
```

```text
copyIndex()
    ↓
Devuelve el índice de la repetición
```

```text
dependsOn
    ↓
Controla el orden de creación
```
