
# Commands

## Account

- **Account set**: ``az account set --subscription "sub-alz-validation"``
- **Account show**: ``az account show``
 ````az account show \
  --query "{Name:name,SubscriptionId:id}" \
  -o table
````
- **Get Cloudshell IP**:
  ````curl -s https://api.ipify.org````

---
## Storage Account

````
az storage account network-rule add \
 --account-name stnetlogsprodswc001 \
 --resource-group rg-netlogs-prod-swc-001 \
 --ip-address X.Y.Z.K

az storage account network-rule list \
 --account-name stnetlogsprodswc001 \
 --resource-group rg-netlogs-prod-swc-001

az storage blob list \
  --account-name stnetlogsprodswc001 \
  --container-name insights-logs-flowlogflowevent \
  --auth-mode login \
  --output table

az storage blob download \
  --account-name stnetlogsprodswc001 \
  --container-name insights-logs-flowlogflowevent \
  --name "flowLogResourceID=/<subscription>_NETWORKWATCHERRG/NETWORKWATCHER_SWEDENCENTRAL_VNET-ALZ-VALIDATION-SWC-001-RG-ALZ-VALIDATION-NETWORK-SW-FLOWLOG/y=2026/m=08/d=05/h=14/m=00/macAddress=7CED8D2684E5/PT1H.json" \
  --file flowlog.json \
  --auth-mode login

cat flowlog.json | jq .



````

---

## Roles

- **List role assigments**:
  ````
   az role assignment list \
  --assignee "########-####-####-####-############" \
  --scope "/subscriptions/#####-###-####-####-#######9/resourceGroups/NetworkWatcherRG" \
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
  --assignee-object-id "AAAAAAA-AAA-AAAA-AAAA-AAAAAAA" \
  --assignee-principal-type ServicePrincipal \
  --role "Contributor" \
  --scope "/subscriptions/#####-###-####-####-#######/resourceGroups/rg-netlogs-prod-gwc-001/providers/Microsoft.Storage/storageAccounts/stnetlogsprodgwc001"
  ````
- **List the previously assigmed role**
  ````
  az role assignment list \
  --assignee-object-id "AAAAAAA-AAA-AAAA-AAAA-AAAAAAA" \
  --scope "/subscriptions/#####-###-####-####-#######/resourceGroups/rg-netlogs-prod-gwc-001/providers/Microsoft.Storage/storageAccounts/stnetlogsprodgwc001" \
  --query "[].{Name:name,Role:roleDefinitionName,PrincipalId:principalId,Scope:scope}" \
  -o table
  ````
 - **Qué roles RBAC tiene la Managed Identity Deploy-VNFL-GWC sobre este Log Analytics Workspace, incluyendo permisos heredados**
   ````
   az role assignment list \
   --assignee "IIIII-III-III-IIIIIIIIIIII" \
   --scope "/subscriptions/SSSSS-SSSS-SSSS-SSSS-SSSSSSS/resourceGroups/rg-netlogs-prod-gwc-001/providers/Microsoft.OperationalInsights/workspaces/law-connectivity-prod-gwc-001" \
   --include-inherited \
   -o table
   ````
## Policies

- **Force policy SCAN in a Resource Group**:solo fuerza una evaluación de compliance. Durante un evaluation cycle, una DINE detecta el recurso y lo marca Non-compliant, pero no ejecuta el deployment sobre ese recurso como consecuencia del scan.
    ``az policy state trigger-scan   --resource-group "rg-alz-flowlogs-validation-gwc-002"``

- **Policy definition show**:
    ``az policy definition show   --name "3e9965dc-cc13-47ca-8259-a4252fd0cf7b"``

- **Consultar las Policy Assignments**:Muestra la Policy Assignment Deploy-VNFL-GWC del MG landingzones y devuelve su nombre, Principal ID de la Managed Identity y Tenant ID.
  ````
  az policy assignment show \
  --name Deploy-VNFL-GWC \
  --scope /providers/Microsoft.Management/managementGroups/landingzones \
  --query "{Name:name,PrincipalId:identity.principalId,TenantId:identity.tenantId}" \
  -o table
  ````
  ````
  #verlas todas
  for mg in landingzones sandbox; do
  for policy in Deploy-VNFL-GWC Deploy-VNFL-SWC; do
    az policy assignment show \
      --name "$policy" \
      --scope "/providers/Microsoft.Management/managementGroups/$mg" \
      --query "{ManagementGroup:'$mg',Policy:name,PrincipalId:identity.principalId,TenantId:identity.tenantId}" \
      -o table
  done
done
  ````

## Log Analytics Workspace

- **Log Analytics Workspace → Settings → Data Export**: Cada regla indica qué tablas del LAW se exportan continuamente y a qué destino, por ejemplo Event Hubs
  ````
  az monitor log-analytics workspace data-export list
  --resource-group rg-netlogs-prod-gwc-001
  --workspace-name law-connectivity-prod-gwc-001
  --query "[].{Name:name,Tables:tableNames,Destination:destination.resourceId,Enabled:enable}"
  -o table
  ````
- **Log Analytics Workspace → Settings → Data Export → Table Name**: comprobar también explícitamente la tabla exportada
  ````
  az monitor log-analytics workspace data-export show \
  --resource-group rg-netlogs-prod-gwc-001 \
  --workspace-name law-connectivity-prod-gwc-001 \
  --name export-ntanetanalytics-crowdstrike-gwc \
  -o json
  ````
- **Resource ID completo del Log Analytics Workspace**
  ````
  az monitor log-analytics workspace show \
  --resource-group rg-netlogs-prod-gwc-001 \
  --workspace-name law-connectivity-prod-gwc-001 \
  --subscription sub-connectivity \
  --query id \
  -o tsv
  ````

## Entra Id
 - List
   ````
   az ad sp show \
   --id "AAAAA-AAA-AAA-AAA-AAAAAAAAA" \
   --query "{DisplayName:displayName,ObjectId:id,AppId:appId,ServicePrincipalType:servicePrincipalType}" \
   -o table

   #search by name
   az ad sp list \
   --display-name "Deploy-VNFL-SWC" \
   --query "[].{Name:displayName,ObjectID:id,ApplicationID:appId,ServicePrincipalType:servicePrincipalType}" \
   -o table
   ````

## EventHub

- **List Consumer Group**:
  ````
  az eventhubs eventhub consumer-group list \
  --resource-group rg-netlogs-prod-gwc-001 \
  --namespace-name evhns-crowdstrike-prod-gwc-001 \
  --eventhub-name evh-crowdstrike-vnet-flowlogs \
  --query "[].name" \
  -o table
  ````
