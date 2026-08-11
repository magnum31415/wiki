
# Commands

## Account

- **Account set**: ``az account set --subscription "sub-alz-validation"``
- **Account show**: ``az account show``


## Policies

### Force policy SCAN in a Resource Group
solo fuerza una evaluación de compliance. Durante un evaluation cycle, una DINE detecta el recurso y lo marca Non-compliant, pero no ejecuta el deployment sobre ese recurso como consecuencia del scan.

``az policy state trigger-scan   --resource-group "rg-alz-flowlogs-validation-gwc-002"``
