
# Commands

## Account

- **Account set**: ``az account set --subscription "sub-alz-validation"``
- **Account show**: ``az account show``
 ````az account show \
  --query "{Name:name,SubscriptionId:id}" \
  -o table
````
- 
## Roles

- **List role assigments**:
  ````
   az role assignment list \
  --assignee "########-####-####-####-############" \
  --scope "/subscriptions/#####-###-####-####-123456789/resourceGroups/NetworkWatcherRG" \
  --include-inherited \
  --query "[].{Role:roleDefinitionName,Scope:scope}" \
  -o table
  ````
- **List Changes**:
  ````
  az role assignment list-changelogs \
  --start-time "2026-08-10T00:00:00Z" \
  --end-time "2026-08-13T00:00:00Z" \
  -o json
  ````
- **Role Assigment specifiying GUID**:
  Microsoft admite az role assignment create para crear el assignment sobre un scope concreto y el parámetro --name permite establecer el GUID del Role Assignment.
  ````
   az role assignment create \
  --name "BBBBBB-c003-5d4e-9889-BBBBBBBB" \
  --assignee-object-id "AAAAAAA-019b-413c-ac88-AAAAAAA" \
  --assignee-principal-type ServicePrincipal \
  --role "Contributor" \
  --scope "/subscriptions/#####-###-####-####-12345678/resourceGroups/rg-netlogs-prod-gwc-001/providers/Microsoft.Storage/storageAccounts/stnetlogsprodgwc001"
  ````
  
## Policies

- **Force policy SCAN in a Resource Group**: ``az policy state trigger-scan   --resource-group "rg-alz-flowlogs-validation-gwc-002"``
  - solo fuerza una evaluación de compliance. Durante un evaluation cycle, una DINE detecta el recurso y lo marca Non-compliant, pero no ejecuta el deployment sobre ese recurso como consecuencia del scan.
- **Policy definition show**: ``az policy definition show   --name "3e9965dc-cc13-47ca-8259-a4252fd0cf7b"`` 
