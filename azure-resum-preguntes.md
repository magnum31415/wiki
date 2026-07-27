
# Test 1

---
# Test 2

---
# Test 3

---
# Test 4

**1.** Un **Access Package** configurado para **All configured connected organizations** solo permite solicitudes de usuarios pertenecientes a **Connected Organizations**.

**2.** Cuando expira un **Access Package**, el usuario pierde inmediatamente el acceso a todos los recursos asignados, aunque su cuenta **Guest** permanezca temporalmente en el tenant.

**3.** Una cuenta **Guest** solo se elimina del tenant después de perder su último **Access Package** y de finalizar el período de retención configurado.

**4.** No se puede eliminar un **Recovery Services Vault** mientras contenga **Protected Items** o copias de seguridad activas; primero hay que detener la protección.

**5.** Una **Service Endpoint Policy** solo permite acceder, mediante **Service Endpoints**, a los recursos de Azure explícitamente autorizados en la política.

**6.** Una **Service Endpoint Policy** únicamente afecta a las **Subnets** donde está asociada.

**7.** Un **Service Endpoint** para **Microsoft Entra ID** sigue utilizando una **IP pública**; no convierte el servicio en privado.

**8.** Una regla de salida de un **NSG** hacia el **Service Tag Storage** permite acceder a Azure Storage incluso si se bloquea el acceso general a Internet.

**9.** Un **NSG** solo bloquea el tráfico que coincide con **origen**, **destino**, **puerto** y **protocolo** de la regla; en caso contrario se evalúan las siguientes reglas por prioridad.

**10.** Un **NSG** solo protege los recursos asociados a la **Subnet** o a la **NIC** donde está vinculado.

**11.** Un **Standard Load Balancer** no admite máquinas virtuales que tengan asociada una **Basic Public IP**.

**12.** Un **Standard Load Balancer** admite VMs con **Standard Public IP** o **sin Public IP**, pero nunca con **Basic Public IP**.

**13.** Para agregar una VM al backend de un **Standard Load Balancer**, es necesario eliminar cualquier **Basic Public IP** asociada.

**14.** Apagar una VM no elimina la incompatibilidad entre una **Basic Public IP** y un **Standard Load Balancer**.

**15.** Las **Built-in Azure Policies** no permiten imponer configuraciones específicas como crear reglas personalizadas de un **NSG**; para ello se necesita una **Custom Policy**.

**16.** Una **Custom Azure Policy** con efecto **DeployIfNotExists** puede desplegar automáticamente reglas o configuraciones en los recursos.

**17.** Desregistrar un **Resource Provider** impide crear nuevos recursos de ese tipo, pero no modifica la configuración de los recursos existentes.

**18.** Un **Resource Lock** protege un recurso frente a modificaciones o eliminaciones, pero no controla el acceso mediante **RBAC**.

**19.** Para restaurar una copia realizada con **MARS Agent** en otro servidor, primero debe instalarse el **MARS Agent** en el servidor de destino.

**20.** El historial de despliegues de un **Resource Group** permite consultar los **ARM Templates** utilizados durante un despliegue.

**21.** Un **Inbound NAT Rule** redirige un puerto de un **Load Balancer** hacia una única máquina virtual del **Backend Pool**.

**22.** Un **Azure Firewall** debe desplegarse en una **AzureFirewallSubnet** dentro de una **VNet** ubicada en la misma región que el Firewall.

**23.** **Azure Bastion** proporciona acceso **RDP** y **SSH** a las máquinas virtuales sin necesidad de asignarles una **Public IP**.

**24.** Un rol de **Azure RBAC** asignado a un **Management Group** se hereda automáticamente por todas las **Subscriptions**, **Resource Groups** y **Resources** descendientes.

**25.** Un rol asignado sobre un **Resource** solo concede permisos sobre ese recurso y no sobre el resto del **Resource Group**.

**26.** Un **Role Assignment** siempre se hereda hacia los recursos hijos, salvo que el recurso no admita ese tipo de herencia.

**27.** Los **Diagnostic Settings** son necesarios para enviar métricas y logs de un recurso a **Log Analytics**, **Storage** o **Event Hub**.

**28.** Las alertas basadas en **Logs** utilizan datos almacenados en un **Log Analytics Workspace** y se crean mediante consultas **KQL**.

**29.** Solo los roles **Global Administrator** y **Authentication Administrator** pueden administrar la configuración de **SSPR**.

**30.** **Azure Monitor** diferencia entre **Metric Alerts** y **Log Alerts**, según el origen de los datos que desencadenan la alerta.

**31.** Los **Resource Locks** solo pueden aplicarse a **Subscriptions**, **Resource Groups** y **Resources**, pero **no** a **Management Groups**.

**32.** Los **Tags** pueden aplicarse a **Subscriptions**, **Resource Groups** y **Resources**, pero **no** a **Management Groups**.

**33.** Los permisos de **Azure RBAC** se **heredan** desde el ámbito superior hacia todos los recursos descendientes.

**34.** El rol **Owner** incluye todos los permisos sobre los recursos, pero **no** puede administrar **Microsoft Entra ID**.

**35.** Un **Management Group** permite administrar de forma centralizada varias **Subscriptions** mediante **Azure RBAC** y **Azure Policy**.

**36.** Las asignaciones de **Azure RBAC** se evalúan utilizando el **ámbito más cercano** y los permisos heredados.

**37.** Un **VNet Peering** en estado **Disconnected** debe eliminarse y volver a crearse para recuperar el estado **Connected**.

**38.** Un **Basic Load Balancer** requiere que todas las VMs del **Backend Pool** pertenezcan al mismo **Availability Set**.

**39.** Un **Health Probe** determina qué instancias del **Backend Pool** reciben tráfico del **Load Balancer**.

**40.** Si un **Load Balancing Rule** se elimina, el **Load Balancer** deja de distribuir tráfico para ese servicio.

**41.** Un **NSG** permite el acceso únicamente a los puertos que no estén bloqueados por una regla de mayor prioridad.

**42.** Al eliminar una regla **Deny** de un **NSG**, vuelve a aplicarse la siguiente regla con mayor prioridad o la regla predeterminada correspondiente.

**43.** Las reglas de un **NSG** se procesan en **orden ascendente de prioridad**, deteniéndose en la primera coincidencia.

**44.** Las reglas **Default** de un **NSG** solo se aplican cuando ninguna regla personalizada coincide.

**45.** Una **Azure Firewall Policy** permite reutilizar la misma configuración de reglas en varios **Azure Firewalls**.

**46.** Una **User Defined Route (UDR)** puede redirigir tráfico hacia una **Virtual Appliance**, un **VPN Gateway** o **Internet**.

**47.** Una **Service Endpoint Policy** solo restringe el acceso mediante **Service Endpoints**, no mediante **Private Endpoints**.

**48.** Para descargar blobs mediante una **SAS**, se necesitan los permisos mínimos **Read** y **List** sobre el recurso **Container**.

**49.** Una **Managed Identity** debe recibir permisos mediante **Azure RBAC** para acceder de forma segura a un **Storage Account**.

**50.** Una **Shared Access Signature (SAS)** concede acceso temporal y limitado a los datos de un **Storage Account** sin exponer las **Access Keys**.

**51.** Los permisos de acceso a los datos se conceden mediante **Azure RBAC (IAM)**, mientras que el **Failover** geográfico depende de la configuración de **Redundancy** del **Storage Account**.

**52.** Un **App Service Plan** debe estar en la **misma región** que la **Web App** y ser compatible con el **sistema operativo** requerido.

**53.** Una **Web App** solo puede moverse entre **App Service Plans** compatibles en la **misma región**.

**54.** Una máquina virtual en Azure solo puede utilizar una **Private IP** perteneciente al **Address Space** de su **VNet**.

**55.** Las primeras **4 direcciones IP** y la **última** de cada **Subnet** están reservadas por **Azure**.

**56.** Un **VPN Gateway** requiere una **GatewaySubnet** dedicada dentro de la **VNet**.

**57.** Una **GatewaySubnet** no debe contener ningún otro recurso aparte del **VPN Gateway**.

**58.** Un **ExpressRoute Gateway** también requiere una **GatewaySubnet** dedicada.

**59.** Una **Public IP** solo puede asociarse a recursos compatibles como **NICs**, **Load Balancers**, **VPN Gateways** o **Azure Firewall**.

**60.** El rol **Logic App Contributor** permite crear y administrar **Logic Apps**, pero no concede permisos para administrar otros recursos del **Resource Group**.

**61.** Para redirigir **todo el tráfico** de una **VNet** mediante una **UDR**, el **Address Prefix** debe ser el **Address Space completo** de la VNet.

**62.** Una **User Defined Route (UDR)** puede enviar el tráfico a una **Virtual Appliance**, un **VPN Gateway**, **Internet** o **None**.

**63.** Una **Virtual Appliance (NVA)** debe tener habilitado el **IP Forwarding** para poder enrutar tráfico de otras máquinas.

**64.** Una **Route Table** solo afecta a las **Subnets** a las que está asociada.

**65.** Una **UDR** tiene prioridad sobre las **System Routes**, salvo las rutas de **VNet Local** y las necesarias para el funcionamiento interno de Azure.

**66.** Un **Custom DNS Server** debe ser accesible mediante conectividad IP para que las máquinas virtuales puedan resolver nombres.

**67.** Para que varias **VNets** utilicen un **DNS Server** ubicado en otra VNet, debe existir conectividad mediante **VNet Peering**.

**68.** **Connection Troubleshoot** verifica la **conectividad extremo a extremo** entre una máquina virtual y un destino.

**69.** **IP Flow Verify** determina si un paquete será **Allowed** o **Denied** por las reglas de un **NSG**.

**70.** **Next Hop** muestra el siguiente salto que seguirá un paquete según la tabla de rutas efectiva.

**71.** **NSG Flow Logs** registran el tráfico permitido y denegado que atraviesa un **NSG**, pero no validan la conectividad.

**72.** **Traffic Analytics** analiza los datos de **NSG Flow Logs** para identificar patrones y anomalías de tráfico.

**73.** Una conexión **VPN Site-to-Site** requiere un **VPN Gateway** en Azure y un dispositivo VPN compatible en el entorno local.

**74.** Una conexión **VNet-to-VNet VPN** requiere un **VPN Gateway** en **cada VNet**.

**75.** Una conexión **Site-to-Site VPN** requiere un **VPN Gateway**, y éste necesita una **GatewaySubnet** con espacio de direcciones disponible.

**76.** Dos **VNets** pueden configurarse mediante **VNet Peering** aunque estén en distintas **regiones** o **suscripciones**, siempre que sus espacios de direcciones **no se solapen**.

**77.** El requisito fundamental para crear un **VNet Peering** es que los **Address Spaces** de ambas VNets **no tengan solapamiento (overlapping CIDR)**.

**78.** En un **Standard Internet-facing Load Balancer**, la **Public IP** debe pertenecer al **Frontend** del Load Balancer y no a las VMs del **Backend Pool**.

**79.** Un **Health Probe** determina automáticamente qué instancias del **Backend Pool** reciben tráfico del **Load Balancer**.

**80.** Las **Inbound NAT Rules** permiten publicar un puerto de una única VM, mientras que las **Load Balancing Rules** distribuyen tráfico entre varias VMs.

**81.** Para registrar las conexiones permitidas y denegadas de una subnet se deben habilitar los **NSG Flow Logs** sobre el **Network Security Group**.

**82.** Un **Log Analytics Workspace** centraliza los **logs** enviados por distintos recursos de Azure mediante **Diagnostic Settings**.

**83.** Un **Diagnostic Setting** permite enviar métricas y logs a **Log Analytics**, **Storage Account** o **Event Hub**.

**84.** Las **Metric Alerts** monitorizan métricas numéricas, mientras que las **Log Alerts** utilizan consultas **KQL** sobre un **Log Analytics Workspace**.

**85.** Un **Action Group** puede reutilizarse en varias alertas siempre que los destinatarios y acciones sean los mismos.

**86.** En **Azure App Service**, una retención de **0 días** significa **conservar los backups indefinidamente**.

**87.** Los **Backups automáticos** de **Azure App Service** solo incluyen el **Production Slot**, salvo que otros slots se configuren explícitamente.

**88.** Un **ARM Template** que crea un **Resource Group** debe desplegarse a **nivel de suscripción** mediante **New-AzDeployment**.

**89.** **New-AzDeployment** se utiliza cuando un **ARM Template** despliega recursos a **nivel de Subscription**, como la creación de un **Resource Group**.

**90.** Antes de asociar un **Custom Domain** a una **Web App**, primero hay que crear el **registro DNS (CNAME o A)** para demostrar la propiedad del dominio.

---
# Test 5

---
# Test 6
