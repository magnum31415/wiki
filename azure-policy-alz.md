[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Index

- [Cómo se organizan las policies en ALZ](#cómo-se-organizan-las-policies-en-alz)
- [Lo que ocurre en tu repositorio](#lo-que-ocurre-en-tu-repositorio)
- [Entonces ¿cómo añado una policy nueva?](#entonces-cómo-añado-una-policy-nueva)
- [¿De dónde sale Deny-AppGateway-Without-WAF?](#de-dónde-sale-deny-appgateway-without-waf)

----
# Azure Landing Zones Library - platform/alz


- [Azure Landing Zones Library - platform/alz](https://github.com/Azure/Azure-Landing-Zones-Library/tree/main/platform/alz)

La librería: ``platform/alz`` Es el **catálogo oficial de componentes reutilizables de Azure Landing Zones.** Microsoft la creó para que todas las implementaciones ALZ (Terraform, Bicep, Accelerator, etc.) consuman los mismos artefactos de gobierno y no tengan que reinventarlos en cada proyecto.

## ¿Para qué sirve?

Contiene definiciones reutilizables de:

- Management Groups
- Archetypes
- Policy Definitions
- Policy Assignments
- Policy Set Definitions (Initiatives)
- Role Definitions

Por ejemplo:
````
platform/alz
│
├── archetype_definitions
├── policy_assignments
├── policy_definitions
├── policy_set_definitions
└── role_definitions
````

La idea es que tú no tengas que crear desde cero cosas como:

````
Deny-Public-IP-On-NIC
Audit-PeDnsZones
Deploy-Private-DNS-Zones
Enable-DDoS-VNET
````

porque Microsoft ya las ha modelado y probado.



# Cómo se organizan las policies en ALZ

La cadena mental correcta es esta:
````
Policy Definition                   = la regla técnica
Policy Set Definition / Initiative  = Un conjunto de Policy Definitions
Policy Assignment                   = Una Policy Definition o Initiative aplicada a un scope
Archetype                           = Un conjunto de Policy Assignments, Policies definitions, Initiatives y Roles. 
Management Group                    = scope donde se aplica uno o varios archetypes
````

**Architecture Definition:** define la jerarquía de Management Groups y qué archetype usa cada MG

| Concepto          | ¿Qué es?                                             | ¿Aplica algo?    | Ejemplo                                                       | ¿Dónde estaría?             |
| ----------------- | ---------------------------------------------------- | ---------------- | ------------------------------------------------------------- | --------------------------- |
| Policy Definition | Define una regla                                     | ❌ No             | `Deny-AppGateway-Without-WAF`                                 | `policy_definitions/`       |
| Policy Assignment | Aplica una Policy Definition o Initiative a un scope | ✅ Sí             | Aplicar `Deny-AppGateway-Without-WAF` al MG `corp-enforced`   | `policy_assignments/`       |
| Archetype         | Agrupa Assignments, Policies, Initiatives y Roles    | ✅ Indirectamente | `corp_enforced_custom` contiene `Deny-AppGateway-Without-WAF` | `archetype_definitions/`    |
| Management Group  | Scope donde se aplican los Archetypes                | ✅ Sí             | `corp-enforced`                                               | `architecture_definitions/` |


````
lib/
│
├── alz_library_metadata.json
│       ↓
│   Importa la librería ALZ oficial
│
├── architecture_definitions/
│       ↓
│   Define Management Groups
│
├── archetype_definitions/
│       ↓
│   Define qué policies tiene cada MG
│
├── policy_definitions/
│       ↓
│   Policies personalizadas
│
├── policy_assignments/
│       ↓
│   Assignments personalizados
│
└── policy_set_definitions/
        ↓
    Initiatives personalizadas

````


## Nivel 1 - Policy Definition

Ejemplos
````
Allowed-Locations
Deny-Public-IP
Audit-VM-Backup
Deploy-Private-DNS-Zones
````

- Una definición de policy es simplemente una regla. 
- Por sí sola no hace nada. 
- Simplemente existe.

````
Policy Definition
    ↓
Todavía no se aplica
````

## Nivel 2 - Policy Set Definition (Initiative)

Microsoft suele agrupar varias policies.

Ejemplo:
````
Corp-Security-Baseline
│
├── Deny-Public-IP
├── Audit-VM-Backup
├── Audit-NSG
└── Audit-Diagnostics
````

Tampoco hace nada todavía.
````
Initiative
    ↓
Todavía no se aplica
````

## Nivel 3 - Policy Assignment

Aquí es donde realmente se activa.

Ejemplo:
````
Assign Corp-Security-Baseline
to
corp-enforced
````
o 
````
Assign Deny-Public-IP
to
corp-enforced
````

Ahora sí se evalúa.

````
Policy Assignment
    ↓
La policy empieza a funcionar
````

## Nivel 4 - Archetype

Aquí está la magia de ALZ.

Un Archetype es simplemente:

````
Una lista de Policy Assignments
+
Una lista de Policy Definitions
+
Una lista de Initiatives
+
Una lista de RBAC Roles
````

Ejemplo:

````
corp
│
├── Audit-PeDnsZones
├── Deny-HybridNetworking
├── Deny-Public-Endpoints
├── Deny-Public-IP-On-NIC
└── Deploy-Private-DNS-Zones
````

Todo eso junto forma:  ``Archetype = corp``

## Nivel 5 - Management Group

Finalmente aplicas el Archetype.

````
Management Group
    corp-enforced
````

usa

````
Archetype
    corp_enforced_custom
````

# Lo que ocurre en tu repositorio

Tu fichero: ``lib/archetype_definitions/corp_enforced_custom.alz_archetype_override.yaml``

contiene algo parecido a:

````
base_archetype: corp
name: corp_enforced_custom
policy_assignments_to_add: []
policy_assignments_to_remove: []
policy_definitions_to_add: []
policy_definitions_to_remove: []
````


Lo que significa:

````
corp_enforced_custom
      hereda
          ↓
        corp
````

y no añade nada extra.

Visualmente:

````
corp
│
├── Audit-PeDnsZones
├── Deny-HybridNetworking
├── Deny-Public-Endpoints
├── Deny-Public-IP-On-NIC
└── Deploy-Private-DNS-Zones

        ↓ heredado por

corp_enforced_custom
````

# Entonces ¿cómo añado una policy nueva?

- No modificas Terraform.
- No modificas el módulo AVM.
- No modificas la librería oficial.

Añades el assignment al archetype.

Ejemplo:

````
base_archetype: corp

name: corp_enforced_custom

policy_assignments_to_add:
  - Deny-AppGateway-Without-WAF

policy_assignments_to_remove: []
````

Resultado:

````
corp
│
├── Audit-PeDnsZones
├── Deny-HybridNetworking
├── Deny-Public-Endpoints
├── Deny-Public-IP-On-NIC
├── Deploy-Private-DNS-Zones
└── Deny-AppGateway-Without-WAF
````

# ¿De dónde sale Deny-AppGateway-Without-WAF?

Queremos conseguir: ``No permitir Application Gateway sin WAF``

La cadena completa sería:
````
1) Policy Definition: Deny-AppGateway-Without-WAF
                ↓
2) Policy Assignment: Deny-AppGateway-Without-WAF
                ↓
3) Archetype: corp_enforced_custom
                ↓
4) Management Group:  corp-enforced
````

Hay tres posibilidades:

| Caso                  | Policy Definition | Policy Assignment | Archetype |
| --------------------- | ----------------- | ----------------- | --------- |
| Ya existe todo en ALZ | ❌                 | ❌                 | ✅         |
| Existe la Definition  | ❌                 | ✅                 | ✅         |
| No existe nada        | ✅                 | ✅                 | ✅         |


## Caso 1 - Ya existe en la librería ALZ.

Supongamos que Microsoft ya tiene:
````
Policy Definition:
Deny-AppGateway-Without-WAF
````
y además ya existe:

````
Policy Assignment:
Deny-AppGateway-Without-WAF
````
en la librería.

### Sólo tocas

``lib/archetype_definitions/``

Ejemplo:

````yaml
# corp_enforced_custom.alz_archetype_override.yaml

base_archetype: corp

name: corp_enforced_custom

policy_assignments_to_add:
  - Deny-AppGateway-Without-WAF
````

### No tocas
````
policy_definitions/
policy_assignments/
policy_set_definitions/
````
porque ya existen.

### Diagrama
````
ALZ Library
│
├── Policy Definition
├── Policy Assignment
│
└── Tú sólo añades

archetype_definitions/
````

Simplemente la añades al archetype.

## Caso 2 - Existe la Policy Definition pero no el Assignment.

Microsoft tiene:

````
Policy Definition:
Deny-AppGateway-Without-WAF
````
pero no la aplica en ningún sitio.

### Debes crear

``Policy Assignment``

### Directorios a tocar

````
lib/policy_assignments/
lib/archetype_definitions/
````

### Paso 1

Crear Assignment:
````
lib/policy_assignments/
    Deny-AppGateway-Without-WAF.alz_policy_assignment.json
````

Ejemplo:
````
{
  "name": "Deny-AppGateway-Without-WAF",
  "policyDefinitionId": "...",
  "parameters": {}
}
````

### Paso 2

Añadir Assignment al Archetype
````
policy_assignments_to_add:
  - Deny-AppGateway-Without-WAF
````

### Diagrama

````
ALZ Library
│
└── Policy Definition

Tú creas
│
├── policy_assignments/
│
└── archetype_definitions/
````

## Caso 3 - No existe nada

Ni Microsoft ni tú tenéis nada.

Hay que crear todo.

### Directorios a tocar
````
lib/policy_definitions/
lib/policy_assignments/
lib/archetype_definitions/
````

### Paso 1

Crear Policy Definition

````
lib/policy_definitions/
    Deny-AppGateway-Without-WAF.alz_policy_definition.json
````

Contiene la lógica:

````
if
    AppGateway
    and WAF disabled

then
    deny
````

### Paso 2

Crear Policy Assignment

````
lib/policy_assignments/
    Deny-AppGateway-Without-WAF.alz_policy_assignment.json
````


Que referencia la Definition.

### Paso 3

Añadir Assignment al Archetype

````
policy_assignments_to_add:
  - Deny-AppGateway-Without-WAF
````

### Diagrama

````
Tú creas

policy_definitions/
        ↓

policy_assignments/
        ↓

archetype_definitions/
        ↓

corp-enforced
````

## Resumen práctico

| Quieres hacer                          | Fichero/directorio que tocas                                                             |
| -------------------------------------- | ---------------------------------------------------------------------------------------- |
| Añadir una policy que ya existe en ALZ | `lib/archetype_definitions/*.yaml`                                                       |
| Crear un assignment propio             | `lib/policy_assignments/` + `lib/archetype_definitions/`                                 |
| Crear una policy propia                | `lib/policy_definitions/` + `lib/policy_assignments/` + `lib/archetype_definitions/`     |
| Quitar una policy heredada             | `policy_assignments_to_remove` en el archetype                                           |
| Añadir una initiative propia           | `lib/policy_set_definitions/` + `lib/policy_assignments/` + `lib/archetype_definitions/` |



