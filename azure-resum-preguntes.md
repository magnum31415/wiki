
# Test 1

---
# Test 2

# Parte 1

**1.** Un **Storage Account Firewall** permite el acceso si la solicitud procede de una **IP pública permitida** o de una **Subnet autorizada** mediante **Virtual Network Rules**.
Si se cumplen ambos requisitos, cualquiera de ellos es suficiente para acceder al Storage Account.

**2.** Las reglas de **IP públicas** de un **Storage Account Firewall** tienen prioridad independiente de las **Virtual Network Rules**.
Una máquina virtual puede acceder utilizando su **Public IP** aunque pertenezca a una Subnet que no esté permitida.

**3.** Una máquina virtual ubicada en una **Subnet autorizada** **no está obligada** a utilizar su **Private IP** para acceder a un **Storage Account**.
Si su **Public IP** también está permitida por el Firewall, podrá utilizar cualquiera de las dos.

**4.** Para escalar automáticamente un **App Service Plan** cuando se cumpla una condición (por ejemplo, **CPU > 80%**), debe configurarse **Rule-based scaling** en **Scale out**.
**Scale up** aumenta la capacidad de una instancia; **Scale out** añade más instancias.

**5.** En Azure, cada **Subnet** reserva siempre las **4 primeras direcciones IP** y la **última**.
El número máximo de recursos que pueden conectarse a una Subnet es el número total de direcciones menos **5 IPs reservadas**.

**6.** Para acceder a un **Azure File Share** integrado con **Active Directory Domain Services**, el usuario debe existir en **Microsoft Entra ID**.
Si un usuario no se sincroniza mediante **Microsoft Entra Connect**, no podrá heredar permisos asignados mediante **Azure RBAC**.

**7.** Los permisos sobre un **Azure File Share** se conceden mediante **Azure RBAC** utilizando identidades sincronizadas con **Microsoft Entra ID**.
Un usuario sincronizado con el rol adecuado puede acceder al recurso.

**8.** Un usuario **cloud-only** de **Microsoft Entra ID** no obtiene acceso automáticamente a un **Azure File Share** integrado con **AD DS**.
Debe recibir explícitamente un rol compatible o cumplir la configuración de identidad establecida.

**9.** El **Global VNet Peering** permite conectar **Virtual Networks** situadas en **distintas regiones**, **distintas suscripciones** e incluso **distintos tenants**, siempre que los espacios de direcciones **no se solapen**.

**10.** Para restringir el acceso a un **Storage Account** desde determinadas redes, el primer paso es configurar **Public network access** para permitir únicamente **Selected networks**.
Después podrán añadirse **VNets** y **Public IPs** autorizadas.

**11.** **Azure Container Registry Tasks (ACR Tasks)** está disponible en los tres SKUs de **Azure Container Registry**: **Basic**, **Standard** y **Premium**.
Lo que cambia entre SKUs son las funcionalidades avanzadas y la capacidad disponible.

**12.** Los **Private Endpoints** para **Azure Container Registry** solo están disponibles en el **SKU Premium**.
Los SKUs **Basic** y **Standard** no admiten conectividad mediante **Private Link**.

**13.** Para habilitar un **Dedicated Data Endpoint** en un **Azure Container Registry**, primero debe utilizarse el **SKU Premium** y configurarlo desde **Properties**.
Posteriormente, la conectividad privada se configura desde **Networking**.

**14.** Para limitar las invitaciones de usuarios externos a un dominio concreto, debe configurarse **Collaboration restrictions** en **External collaboration settings** de **Microsoft Entra ID**.
Esta configuración controla **quién puede ser invitado**, no los permisos posteriores del invitado.

**15.** Si una **Virtual Machine** no puede cambiar de tamaño por falta de capacidad, el primer paso es **Deallocate** la máquina virtual.
Al liberar el hardware asignado, Azure puede volver a ubicar la VM en un host con capacidad suficiente.

**16.** Una **Public IP** puede asociarse directamente a una **Network Interface (NIC)** o al **Frontend** de un **Load Balancer**.
Nunca se asocia directamente a una **Virtual Machine** ni a una **Virtual Network**.

**17.** Para permitir que una red **on-premises** acceda a otra **VNet** utilizando una conexión VPN existente, la solución más económica consiste en utilizar **Service Chaining** junto con **User Defined Routes (UDRs)**.

**18.** La rotación automática de las **Access Keys** de un **Storage Account** se implementa mediante **Azure Key Vault**.
Ni **Lifecycle Management**, ni **Backup Vault**, ni **Recovery Services Vault** gestionan la rotación de claves.

**19.** Los únicos roles de Azure Storage que permiten **acceso directo a los blobs** y admiten **Role Assignment Conditions** son **Storage Blob Data Contributor** y **Storage Blob Data Owner**.
Los roles administrativos del Storage Account no conceden acceso a los datos.

**20.** Para importar usuarios masivamente y asignarlos automáticamente a grupos según un atributo como **Department**, deben utilizarse **Dynamic User Groups** junto con un **CSV** de importación.
La pertenencia al grupo se actualizará automáticamente cuando cambie el atributo del usuario.



**21.** La **Geo-replication** de un **Azure Container Registry (ACR)** solo está disponible en el **SKU Premium**.
Antes de configurarla, es obligatorio actualizar el registro desde **Basic** o **Standard** a **Premium**.

**22.** Para advertir cuando una **Shared Access Signature (SAS)** tenga una duración superior a un determinado límite, debe habilitarse **Allow recommended upper limit for SAS expiry interval**.
Esta opción **solo genera una advertencia**, no impide crear la SAS.

**23.** Un **Azure File Share** configurado con **Active Directory Domain Services (AD DS)** solo permite el acceso a usuarios **sincronizados** desde el **AD local**.
Un usuario **cloud-only** de Microsoft Entra ID no puede autenticarse mediante AD DS.

**24.** Cuando un **Storage Account** utiliza **AD DS** como origen de identidad y concede acceso a **Authenticated Users**, cualquier usuario **sincronizado desde AD DS** puede acceder si dispone de los permisos adecuados.

**25.** La configuración de **Identity-based Access** es **independiente para cada Storage Account**.
Configurar **AD DS** en un Storage Account **no** habilita automáticamente el acceso en otros Storage Accounts.

**26.** Un usuario de **Microsoft Entra ID** puede eliminarse aunque tenga **licencias asignadas**, ya sean **directas** o heredadas mediante grupos.
Al eliminar el usuario, las licencias quedan disponibles para reutilizarse.

**27.** Un **grupo con licencias asignadas directamente** **no puede eliminarse** hasta quitar previamente dichas licencias.
Los grupos **sin licencias directas** sí pueden eliminarse.

**28.** Para enviar los **Azure Backup Reports** a un **Storage Account**, éste debe encontrarse en la **misma región** que el **Recovery Services Vault**.

**29.** Un **Log Analytics Workspace** utilizado por **Azure Backup Reports** puede estar en **cualquier región**.
A diferencia del Storage Account, **no** necesita coincidir con la región del **Recovery Services Vault**.

**30.** Un **Connection Monitor** solo puede supervisar máquinas virtuales de la **misma región**.
Si existen VMs en varias regiones, será necesario crear **un Connection Monitor por región**.

**31.** Un **NSG** con únicamente las **reglas predeterminadas** **no permite** conexiones **RDP desde Internet**.
Es necesario crear una regla **Allow** para el puerto **3389** con mayor prioridad que las reglas por defecto.

**32.** Una regla **Allow TCP 3389** aplicada al **NSG** de una **NIC** permite conectarse por **RDP** desde Internet, aunque el NSG de la Subnet no tenga reglas específicas.

**33.** Las máquinas virtuales situadas en la **misma VNet** pueden comunicarse entre sí mediante sus **Private IPs** gracias a la ruta predeterminada **AllowVNetInBound**, salvo que un NSG lo impida.

**34.** Dos **Virtual Networks** solo pueden establecer un **VNet Peering** si sus **Address Spaces no se solapan (overlap)**.
Si ambos utilizan el mismo rango CIDR, primero debe modificarse el **Address Space** de una de ellas.

**35.** Un **Internal Load Balancer** distribuye tráfico **privado** entre máquinas virtuales de una **VNet**.
Es la solución adecuada para equilibrar el tráfico entre la **Web Tier** y la **Application Tier**.

**36.** Para proteger una aplicación frente a **SQL Injection**, **Cross-Site Scripting (XSS)** y otros ataques web, debe utilizarse un **Application Gateway WAF**.
Un **Load Balancer** o un **NSG** no ofrecen esta protección.

**37.** Una **Virtual Network** puede contener **varios Address Spaces**.
Antes de utilizar un nuevo rango de direcciones, primero debe añadirse como **Address Space** a la VNet.

**38.** Si el rango IP ya pertenece al **Address Space** de la VNet, únicamente es necesario crear una **Subnet** que utilice dicho rango.
No hace falta añadir un nuevo **Address Space**.

**39.** Los usuarios con **roles administrativos** de **Microsoft Entra ID** siguen una política especial de **Self-Service Password Reset (SSPR)**.
Los administradores **no pueden utilizar preguntas de seguridad** para restablecer su contraseña.

**40.** Los roles administrativos como **Security Administrator** o **Billing Administrator** utilizan una política de **SSPR para administradores**, basada en **dos métodos de autenticación**, excluyendo las **preguntas de seguridad**.


**41.** Los **usuarios normales** (no administradores) siguen la política estándar de **Self-Service Password Reset (SSPR)**.
Si las **preguntas de seguridad** están habilitadas, podrán utilizarlas durante el restablecimiento de la contraseña.

**42.** Una **Data Collection Rule (DCR)** solo puede utilizar como **origen de datos (Data Source)** recursos compatibles, como las **Virtual Machines**.
Recursos como **Storage Accounts**, **Log Analytics Workspaces** o **Azure SQL Database** no pueden ser Data Sources de una DCR.

**43.** El destino principal de una **Data Collection Rule (DCR)** es un **Log Analytics Workspace**.
Los **Storage Accounts** y **Azure SQL Databases** no son destinos válidos para una DCR.

**44.** Para recuperar **archivos individuales** de una **Azure VM** protegida con **Azure Backup**, debe utilizarse **File Recovery**.
El proceso consiste en seleccionar un **Recovery Point**, ejecutar el **script** que monta el volumen y copiar los archivos mediante **File Explorer**.

**45.** Cuando un **Recovery Point** coincide con varios períodos de retención (**diario**, **semanal**, **mensual** o **anual**), Azure conserva siempre la **retención más larga**.
Si coincide con la retención anual, se conservará durante los años configurados.

**46.** Si un **Recovery Point** coincide simultáneamente con una retención **semanal** y **mensual**, Azure mantiene la **retención mensual**, ya que es la de mayor duración.

**47.** Los **backups de Azure Virtual Machines** siempre se almacenan en un **Recovery Services Vault**.
No pueden almacenarse directamente en un **Storage Account**, **Blob Container** o **File Share**.

**48.** La programación y retención de las copias de seguridad de una **Virtual Machine** se configuran mediante una **Backup Policy**.
La política define la frecuencia de los backups y el tiempo durante el que se conservarán.

**49.** Para que **Azure Monitor** utilice exclusivamente la **red privada** de una **Virtual Network**, primero debe crearse un **Azure Monitor Private Link Scope (AMPLS)**.
Después se asocia un **Private Endpoint** al AMPLS para proporcionar conectividad privada.

**50.** En un **ARM Template**, la propiedad **copy** permite crear varias instancias de un mismo recurso o propiedad.
Se utiliza, por ejemplo, para crear dinámicamente varios **Data Disks** en una **Virtual Machine**.

**51.** La función **copyIndex()** devuelve el índice de cada iteración de un bloque **copy**.
Se utiliza para asignar valores únicos, como el **LUN** de cada disco de datos.

**52.** Para garantizar que todo el tráfico entre una **Virtual Machine** y un **Storage Account** permanezca dentro de la red privada de Azure, debe configurarse un **Private Endpoint**.
Una **SAS** o deshabilitar el acceso público no garantizan por sí solos el uso de la red privada.

**53.** Para cargar archivos en un **Blob Container** desde el **Azure Portal** siguiendo el principio de **mínimo privilegio**, deben asignarse los roles **Reader** y **Storage Blob Data Contributor**.
El primero permite navegar por el recurso y el segundo operar sobre los blobs.

**54.** Para impedir que los blobs puedan modificarse o eliminarse durante un período determinado, debe configurarse una **Time-based Retention Policy** (Inmutability Policy) desde **Access Policy** del contenedor.
Los permisos **IAM** o el **Access Tier** no proporcionan inmutabilidad.

**55.** Una **Alert Rule** cuyo ámbito incluye **todas las Resource Groups** también supervisa las **Resource Groups creadas en el futuro**.
La alerta aparecerá aunque el recurso se haya creado después de configurar la regla.

**56.** Una **Alert Processing Rule** de tipo **Suppress notifications** únicamente bloquea el envío de notificaciones.
La **Alert Rule** sigue evaluándose y la alerta continúa generándose en Azure Monitor.

**57.** Una modificación administrativa, como **añadir un Tag** a un recurso, puede generar una **Alert Rule** basada en **Administrative Operations**.
Si no existe una regla de supresión activa, también se ejecutarán las acciones del **Action Group**.

**58.** Para almacenar las copias de seguridad de una **Virtual Machine** en **tres Availability Zones**, primero debe crearse un **Recovery Services Vault**, configurar su redundancia como **Zone-Redundant Storage (ZRS)** y, por último, habilitar el backup.

**59.** Una licencia asignada mediante **Group-Based Licensing** **no puede eliminarse directamente del usuario**.
Para retirarla, es necesario quitar al usuario del grupo o eliminar la licencia asignada al grupo.

**60.** En **Microsoft Entra ID**, el **Group-Based Licensing** tiene prioridad sobre las asignaciones individuales.
Mientras el usuario pertenezca al grupo licenciado, seguirá heredando automáticamente esa licencia.


**61.** El **Group-Based Licensing** de **Microsoft Entra ID** **no admite grupos anidados**.
Si un grupo con licencia contiene otro grupo, **los miembros del grupo anidado no heredan la licencia**.

**62.** Una **Virtual Machine** cuenta para la **vCPU quota** aunque esté en estado **Stopped (Deallocated)**.
Para desplegar nuevas VMs deben respetarse tanto la cuota de la **familia de vCPUs** como la **Total Regional vCPU quota**.

**63.** Una nueva **Virtual Machine** solo puede desplegarse si **no supera** la **Total Regional vCPU quota**.
Las VMs **Deallocated** siguen consumiendo cuota, aunque no consuman capacidad de proceso.

**64.** Las cuotas de **vCPUs** se evalúan sobre **todas las VMs aprovisionadas**, independientemente de que estén **Running** o **Stopped (Deallocated)**.
Si la suma supera la cuota regional, el despliegue será rechazado.

**65.** Un **Dynamic User Group** añade automáticamente a los usuarios cuyos atributos cumplen la **Membership Rule**.
Las **licencias de Microsoft 365** no afectan a la pertenencia a grupos dinámicos.

**66.** Un usuario puede pertenecer simultáneamente a varios **Dynamic Groups** si cumple las reglas de cada uno.
La pertenencia depende únicamente de los **atributos del usuario**, no de las licencias asignadas.

**67.** Para instalar una extensión en una **Virtual Machine** mediante un **ARM Template**, el tipo de recurso debe ser **Microsoft.Compute/virtualMachines/extensions**.
Las extensiones son recursos hijos de la máquina virtual.

**68.** Los datos confidenciales de una **VM Extension**, como contraseñas o secretos, deben almacenarse en **ProtectedSettings**.
El bloque **Settings** solo debe contener información no sensible.

**69.** Si una aplicación solo es accesible desde una **VPN (P2S o S2S)**, pueden utilizarse tanto un **Internal Load Balancer** como un **Azure Application Gateway**.
Un **Public Load Balancer** o **Traffic Manager** no son necesarios para tráfico exclusivamente interno.

**70.** El rol **Contributor** permite crear y administrar tanto **Virtual Machines** como **Virtual Networks**, pero **no** permite asignar roles **RBAC**.
Es el rol que mejor cumple el principio de **mínimo privilegio** para administrar recursos.

**71.** Una **Azure Policy** con efecto **Deny** impide crear recursos del tipo bloqueado, independientemente de si el despliegue se realiza desde el **Portal**, **CLI**, **PowerShell** o un **ARM Template**.

**72.** Los **Resource Locks** (**Delete** y **Read-only**) **no impiden mover recursos** entre **Resource Groups**.
Los bloqueos solo evitan modificaciones o eliminaciones del recurso.

**73.** Un recurso puede moverse entre **Resource Groups** aunque el recurso o el grupo tengan un **Resource Lock**.
Los bloqueos **Delete** y **Read-only** no bloquean las operaciones de movimiento.

**74.** Si un requisito indica almacenar datos en el **Archive Tier**, debe utilizarse **Azure Blob Storage**.
El **Archive Tier** no está disponible para **Azure File Storage**.

**75.** **Azure Table Storage** solo debe utilizarse para almacenar datos **NoSQL clave-valor**.
No es adecuado para almacenar archivos como documentos, imágenes o planos.

**76.** **Azure File Storage** **no soporta** el **Archive Tier**.
Si un escenario requiere archivado de datos, debe utilizarse **Azure Blob Storage**.

**77.** Para copiar archivos locales a **Azure Blob Storage** a través de Internet utilizando una interfaz gráfica, la herramienta recomendada es **Azure Storage Explorer**.
No es necesario utilizar **Azure Import/Export** ni mapear unidades de red.

**78.** Antes de proteger una **Virtual Machine** con **Azure Backup**, primero debe crearse un **Recovery Services Vault**.
Después se configura la **Backup Policy** y se habilita la protección.

**79.** Los permisos sobre una **Subscription** se administran desde **Access Control (IAM)**.
Para asignar un usuario como administrador de la suscripción deben utilizarse las asignaciones de **Azure RBAC**.

**80.** Para publicar una aplicación web mediante **HTTPS**, el **NSG** debe permitir tráfico **Inbound TCP 443** desde **Internet** hacia la **Subnet** donde residen los **Web Servers**.
---
# Test 3
**1.** **Azure Bastion** permite conectarse a máquinas virtuales de la **misma VNet** o de **VNets directamente peered**, pero **no** a través de **peerings transitivos**.

**2.** Una **Network Interface (NIC)** debe crearse siempre en la **misma región** que la **Virtual Network** a la que se conecta.

**3.** Un **VNet Peering** permite la comunicación directa únicamente entre las **VNets** que están **directamente peered**.

**4.** Sin **Gateway Transit**, un **VNet Peering** **no** permite utilizar una VNet como red de tránsito hacia otra VNet.

**5.** **Azure Bastion** requiere una **Standard Public IP**, **IPv4**, **Static** y de tipo **Regional**.

**6.** Toda VNet que aloje **Azure Bastion** debe contener una subnet llamada exactamente **AzureBastionSubnet**.

**7.** La **AzureBastionSubnet** debe tener un tamaño mínimo de **/26**.

**8.** Para utilizar el **Native Client** de **Azure Bastion** es necesario disponer del **SKU Standard** y habilitar la característica **Native Client Support**.

**9.** La **Auto-registration** de una **Private DNS Zone** solo registra automáticamente las VMs pertenecientes a la **Registration Virtual Network**.

**10.** Una VM solo puede resolver nombres de una **Private DNS Zone** si su **VNet** está vinculada a dicha zona como **Resolution Virtual Network**.

**11.** La **Registration Virtual Network** también actúa como **Resolution Virtual Network** para la **Private DNS Zone**.

**12.** La **Auto-registration** de una **Private DNS Zone** únicamente crea registros **A** con la **Private IP** de las máquinas virtuales.

**13.** El **DNS suffix** configurado en una VM **no influye** en la **Auto-registration** de una **Private DNS Zone**; únicamente importa la **VNet**.

**14.** **Azure Container Instances**, **Azure App Service** y **Azure Kubernetes Service (AKS)** soportan **contenedores Windows**, mientras que **Azure Container Apps** solo admite **contenedores Linux**.

**15.** Los **contenedores Linux** pueden ejecutarse en **Azure Container Instances**, **Azure Container Apps**, **Azure Kubernetes Service (AKS)** y **Azure App Service**.

**16.** La configuración de **Self-Service Password Reset (SSPR)** solo puede ser administrada por los roles **Global Administrator** y **Authentication Policy Administrator**.

**17.** **SSPR** puede habilitarse para **Security Groups** y **Microsoft 365 Groups**, pero **no** para **Mail-enabled Security Groups**.

**18.** Para garantizar que solo se utilicen imágenes firmadas en un **Azure Container Registry**, debe habilitarse **Content Trust**.

**19.** Si un **Blob Container** necesita utilizar una clave distinta para el cifrado en reposo, debe configurarse un **Encryption Scope**.

**20.** Un **Blob Container** admite un máximo de **5 Stored Access Policies**.

**21.** Un **Blob Container** admite un máximo de **2 Immutable Blob Storage Policies**.

**22.** Para copiar un directorio local completo a un **Blob Container**, el comando recomendado es **`azcopy copy --recursive`**.

**23.** Una **Shared Access Signature (SAS)** solo concede acceso si se cumplen simultáneamente las restricciones de **IP**, **fecha**, **protocolo** y **permisos**.

**24.** Una **SAS** configurada para el servicio **File** no concede acceso a otros servicios del **Storage Account**.

**25.** Las **Stored Access Policies** solo pueden asociarse a **Blob Containers**, **File Shares**, **Queues** y **Tables**, **no** al **Storage Account**.

**26.** Una **Service SAS** hereda las restricciones definidas en la **Stored Access Policy** asociada.

**27.** Un **User Delegation SAS** requiere autenticación mediante **Microsoft Entra ID** y solo está disponible para el servicio **Blob**.

**28.** Una **Account SAS** puede conceder acceso a varios servicios del **Storage Account** mediante un único token.

**29.** Solo los roles **Global Administrator** y **Authentication Administrator** pueden modificar las **preguntas de seguridad** de **SSPR**.

**30.** Los roles administrativos de **Microsoft Entra ID** se asignan desde **Directory Roles**, no mediante **Licencias** ni **Grupos**.
**31.** Una asignación de **Azure Policy** puede realizarse sobre un **Management Group**, **Subscription**, **Resource Group** o **Resource** individual.

**32.** Las **Exclusions** de una **Azure Policy** pueden configurarse sobre **Management Groups**, **Subscriptions**, **Resource Groups** y **Resources**, pero **no** sobre el **Tenant Root Group**.

**33.** Para que **Azure Bastion** sea accesible desde Internet, el **NSG** debe permitir tráfico **HTTPS (TCP 443)** hacia el host de Bastion.

**34.** Una **Shared Access Signature (SAS)** solo permite acceder a los **servicios** (Blob, File, Queue o Table) para los que fue creada, independientemente de los roles **RBAC** del usuario.

**35.** Las **Access Keys** de un **Storage Account** conceden acceso completo a **todos los servicios** del almacenamiento, sin depender de **Azure RBAC**.

**36.** Un **Inbound NAT Rule** dirige el tráfico hacia una **VM concreta**, mientras que una **Load Balancing Rule** distribuye el tráfico entre varias VMs.

**37.** Un **Azure Firewall** solo puede desplegarse en una **VNet** ubicada en la **misma región** y en el **mismo Resource Group** que el Firewall.

**38.** **Azure Bastion** solo protege y proporciona acceso **RDP/SSH** a **máquinas virtuales**, no a **App Services** ni a **Microsoft Entra Domain Services**.

**39.** Un rol **Contributor** asignado a un **Management Group** se hereda automáticamente por todas las **Subscriptions**, **Resource Groups** y **Resources** descendientes.

**40.** El rol **Storage Account Contributor** permite administrar el **Storage Account**, pero **no** acceder a los **datos** almacenados.

**41.** El rol **User Access Administrator** permite crear y administrar **Role Assignments**, pero **no** administrar los recursos.

**42.** Los permisos de **Azure RBAC** siempre se **heredan** desde el ámbito superior hacia los recursos descendientes.

**43.** Un rol asignado directamente sobre un **Resource** solo concede permisos sobre ese recurso, no sobre el resto del **Resource Group**.

**44.** Un **Deny Assignment** tiene prioridad sobre cualquier permiso concedido mediante **Azure RBAC**.

**45.** El rol **Owner** incluye todos los permisos de **Contributor** y además puede administrar el acceso mediante **Azure RBAC**.

**46.** El rol **Contributor** puede administrar recursos, pero **no** puede crear ni modificar **Role Assignments**.

**47.** El rol **Reader** únicamente permite visualizar recursos y configuraciones, sin realizar modificaciones.

**48.** Para administrar los **datos** de un **Storage Account** deben utilizarse los roles **Storage Blob/File/Queue/Table Data**.

**49.** Los roles **Storage Data** controlan el acceso al **contenido** del almacenamiento, mientras que los roles de administración controlan el **Storage Account**.

**50.** Un **Storage Blob Data Contributor** permite leer, escribir y eliminar blobs, pero **no** administrar el **Storage Account**.

**51.** Una **Account SAS** puede conceder acceso simultáneo a varios servicios del **Storage Account**.

**52.** Una **Service SAS** únicamente concede acceso al servicio específico para el que fue creada.

**53.** Un **User Delegation SAS** requiere autenticación mediante **Microsoft Entra ID** y solo está disponible para **Blob Storage**.

**54.** Una **Stored Access Policy** permite modificar o revocar una **Service SAS** sin necesidad de regenerar el token.

**55.** Las **Stored Access Policies** solo pueden configurarse sobre **Blob Containers**, **File Shares**, **Queues** y **Tables**.

**56.** Una **Azure Policy** puede asignarse incluso a un **Resource** individual utilizando **Azure CLI** o **PowerShell**.

**57.** El **Tenant Root Group** **no** puede utilizarse como **Exclusion** de una **Azure Policy**.

**58.** **Azure Bastion** utiliza **HTTPS (TCP 443)** para encapsular las conexiones **RDP** y **SSH**, por lo que no es necesario abrir los puertos **3389** ni **22** hacia Internet.

**59.** Una **SAS** solo concede acceso a los recursos incluidos en su **Scope** y con los **Permisos** especificados, aunque el usuario tenga más permisos mediante **RBAC**.

**60.** Las **Access Keys** ignoran las restricciones de **Azure RBAC** y proporcionan acceso completo a todos los servicios del **Storage Account**.

**61.** Un **App Service Plan** debe estar en la **misma región** que la **Web App** que hospeda, aunque puede alojar varias aplicaciones.

**62.** Una **Web App** solo puede moverse entre **App Service Plans** compatibles que estén en la **misma región**.

**63.** Una máquina virtual solo puede utilizar una **Private IP** perteneciente al **Address Space** de la **VNet** y de la **Subnet** donde está conectada.

**64.** Las primeras **4 direcciones IP** y la **última** dirección de cada **Subnet** están reservadas por **Azure** y no pueden asignarse a recursos.

**65.** Todo **VPN Gateway** requiere una **GatewaySubnet** dedicada dentro de la **VNet**.

**66.** La **GatewaySubnet** no puede contener máquinas virtuales ni otros recursos; está reservada exclusivamente para el **Gateway**.

**67.** Un **ExpressRoute Gateway** también requiere una **GatewaySubnet** dedicada.

**68.** Una **Public IP** solo puede asociarse a recursos compatibles como **NICs**, **Load Balancers**, **VPN Gateways**, **Azure Bastion** o **Azure Firewall**.

**69.** El rol **Logic App Contributor** permite crear y administrar **Logic Apps**, pero no concede permisos sobre otros recursos del **Resource Group**.

**70.** Para redirigir **todo el tráfico** de una **VNet** mediante una **UDR**, el **Address Prefix** debe ser el **Address Space** completo de la VNet.

**71.** Una **User Defined Route (UDR)** puede enviar tráfico hacia una **Virtual Appliance**, un **VPN Gateway**, **Internet** o **None**.

**72.** Una **Virtual Appliance (NVA)** debe tener habilitado el **IP Forwarding** para poder enrutar tráfico de otras máquinas.

**73.** Una **Route Table** únicamente afecta a las **Subnets** a las que está asociada.

**74.** Una **UDR** tiene prioridad sobre las **System Routes**, excepto en determinadas rutas internas administradas por Azure.

**75.** Un **Custom DNS Server** debe ser accesible mediante conectividad IP para que las máquinas virtuales puedan resolver nombres.

**76.** Para que varias **VNets** utilicen un **DNS Server** ubicado en otra VNet, debe existir conectividad mediante **VNet Peering** o **VPN**.

**77.** **Connection Troubleshoot** verifica la **conectividad extremo a extremo** entre un origen y un destino.

**78.** **IP Flow Verify** determina si un paquete será **Allowed** o **Denied** por las reglas de un **NSG**.

**79.** **Next Hop** muestra la ruta efectiva que seguirá un paquete según la tabla de rutas.

**80.** Los **NSG Flow Logs** registran el tráfico permitido y denegado que atraviesa un **NSG**.

**81.** **Traffic Analytics** analiza los **NSG Flow Logs** para detectar patrones de tráfico y posibles problemas de seguridad.

**82.** Una conexión **Site-to-Site VPN** requiere un **VPN Gateway** en Azure y un dispositivo **VPN** compatible en el entorno local.

**83.** Una conexión **VNet-to-VNet VPN** requiere un **VPN Gateway** en **cada VNet**.

**84.** Una conexión **Site-to-Site VPN** solo puede crearse si existe una **GatewaySubnet** con espacio de direcciones suficiente.

**85.** Dos **VNets** pueden configurarse mediante **VNet Peering** aunque estén en distintas **regiones** o **suscripciones**, siempre que sus **Address Spaces** no se solapen.

**86.** El requisito imprescindible para crear un **VNet Peering** es que los **CIDR** de ambas **VNets** no tengan **overlapping**.

**87.** En un **Standard Public Load Balancer**, la **Public IP** debe estar asociada al **Frontend** del Load Balancer y no a las VMs del **Backend Pool**.

**88.** Un **Health Probe** determina automáticamente qué instancias del **Backend Pool** están disponibles para recibir tráfico.

**89.** Una **Inbound NAT Rule** publica un puerto hacia una única VM, mientras que una **Load Balancing Rule** distribuye el tráfico entre varias VMs.

**90.** Para registrar y analizar el tráfico permitido y denegado de una red virtual deben habilitarse los **NSG Flow Logs** sobre el **Network Security Group**.
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
