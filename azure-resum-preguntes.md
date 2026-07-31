[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

- [Test 1](#test-1)
- [Test 2](#test-2)
- [Test 3](#test-3)
- [Test 4](#test-4)
- [Test 5](#test-5)
- [Test 6](#test-6)
- [Test 7](#test-7)

  
# Test 1

### **1. Website Contributor**

El rol **Website Contributor** permite administrar y desplegar aplicaciones en **Azure App Service** utilizando autenticación de **Microsoft Entra ID**, sin conceder permisos administrativos completos.

Es el rol recomendado cuando los desarrolladores solo necesitan publicar aplicaciones mediante **Web Deploy**, ya que sigue el principio de **mínimo privilegio**. 


### **2. Azure RBAC Conditions - Lectura de Blobs**

Las **Azure RBAC Conditions** permiten restringir un rol mediante condiciones adicionales, como el nombre del **Container**, la ruta del **Blob** o el tipo de operación.

Aunque un usuario tenga el rol **Storage Blob Data Reader**, solo podrá acceder a los blobs que cumplan la condición definida en la asignación del rol.


### **3. Azure RBAC Conditions - Escritura de Blobs**

Las **Azure RBAC Conditions** también pueden limitar operaciones de escritura (**Write**) realizadas por roles como **Storage Blob Data Owner**.

Las condiciones se evalúan en cada solicitud y pueden permitir o denegar el acceso según atributos como el nombre del blob, la ruta o el contenedor. 


### **4. Azure RBAC Conditions**

Las **Conditions** son un filtro adicional aplicado sobre un rol de **Azure RBAC**. Primero el usuario debe tener el rol y, además, la condición debe evaluarse como **True**.

Si la condición devuelve **False**, la operación se deniega aunque el usuario tenga asignado el rol correspondiente.


### **5. Virtual Machine Redeploy**

La operación **Redeploy** mueve una **Virtual Machine** a un **nuevo host físico** dentro de la misma región de Azure.

Se utiliza principalmente para resolver problemas relacionados con el host físico o para evitar una **Planned Maintenance**. 

### **6. Mover una VM entre Resource Groups**

Mover una **Virtual Machine** entre **Resource Groups** es únicamente una operación de **Azure Resource Manager**.

Este movimiento **no cambia el host físico** donde se ejecuta la VM y, por tanto, **no sirve** para evitar una tarea de mantenimiento de Azure. 


### **7. Updates de una Virtual Machine**

Habilitar **Updates** o **Automatic Guest Patching** administra las actualizaciones del sistema operativo invitado.

Esta configuración **no mueve** la máquina virtual a otro servidor físico y **no evita** una operación de mantenimiento de la plataforma Azure. 

### **8. Azure Virtual Machine Redeploy + Reapply**

La opción **Redeploy + Reapply** reprovisiona la máquina virtual sobre un **nuevo host físico** manteniendo la configuración de Azure.

Es la acción correcta cuando el objetivo es cambiar inmediatamente de servidor físico sin modificar la configuración de la VM. 



### **9. Azure Virtual Machine Scale Set - Scale Out**

Cuando la utilización de **CPU** supera el umbral configurado durante el tiempo especificado, un **Virtual Machine Scale Set** ejecuta automáticamente un **Scale Out**.

El número de instancias añadidas depende de la configuración de la regla de escalado automático. 


### **10. Azure Virtual Machine Scale Set - Scale In**

Si la utilización de **CPU** permanece por debajo del umbral de **Scale In**, Azure reduce progresivamente el número de instancias hasta alcanzar el valor mínimo configurado.

El escalado nunca reducirá las instancias por debajo del parámetro **Minimum instance count** definido en el Scale Set. 


### **11. Azure Container Registry Tasks**

**ACR Tasks** permite compilar, probar y publicar automáticamente imágenes de contenedor directamente en **Azure Container Registry**.

Es una característica disponible en los tres SKUs (**Basic**, **Standard** y **Premium**). Lo que cambia entre ellos son los límites de capacidad y las funcionalidades avanzadas, no la disponibilidad de ACR Tasks.



### **12. Private Endpoints en Azure Container Registry**

Los **Private Endpoints** para **Azure Container Registry (ACR)** solo están disponibles en el **SKU Premium**.

Si un escenario requiere acceder al registro mediante **Private Link**, primero será necesario actualizar el registro al SKU **Premium**.



### **13. Dedicated Data Endpoint en ACR**

Para habilitar un **Dedicated Data Endpoint** en un **Azure Container Registry**, primero debe utilizarse el **SKU Premium**.

Después se habilita el endpoint desde **Properties** y, posteriormente, se configura la conectividad mediante **Networking** y **Private Endpoints** si es necesario.



### **14. External Collaboration Restrictions**

Las **External Collaboration Settings** de **Microsoft Entra ID** permiten restringir qué dominios externos pueden ser invitados al tenant.

La configuración **Collaboration restrictions** controla los dominios permitidos o bloqueados para usuarios invitados, aumentando la seguridad de la colaboración B2B.



### **15. Cambiar el tamaño de una Virtual Machine**

Si una **Virtual Machine** no puede cambiar de tamaño por falta de capacidad en el clúster actual, el primer paso es **Deallocate** la máquina.

Al quedar desasignada, Azure puede moverla a otro clúster con capacidad disponible y permitir el cambio de tamaño.



### **16. Public IP y Network Interface**

Una **Public IP** siempre se asocia a una **Network Interface (NIC)** o al **Frontend** de un **Load Balancer**.

Nunca se asigna directamente a una **Virtual Machine**; la VM accede a ella a través de la NIC.



### **17. Service Chaining**

**Service Chaining** permite redirigir tráfico entre redes mediante una **User Defined Route (UDR)** hacia una **Network Virtual Appliance (NVA)** o un dispositivo de red.

Es una solución habitual para hacer que varias VNets compartan un firewall o una conexión hacia una red **on-premises**.



### **18. Rotación de Access Keys**

La rotación automática de las **Access Keys** de un **Storage Account** se implementa utilizando **Azure Key Vault** junto con procesos automatizados.

Ni **Recovery Services Vault**, ni **Backup Vault**, ni **Lifecycle Management** gestionan la renovación de claves.



### **19. Storage Blob Data Roles**

Los roles **Storage Blob Data Contributor** y **Storage Blob Data Owner** permiten acceder directamente a los **Blob Data** y admiten **Azure RBAC Conditions**.

Los roles administrativos del **Storage Account** permiten administrar el recurso, pero no acceder al contenido de los blobs.



### **20. Dynamic User Groups**

Los **Dynamic User Groups** agregan o eliminan usuarios automáticamente según reglas basadas en atributos como **Department**, **City** o **Job Title**.

Son especialmente útiles para automatizar la asignación de licencias, aplicaciones y permisos sin necesidad de administrar manualmente la pertenencia a los grupos.

# Parte 1C

### **21. Geo-replication en Azure Container Registry**

La **Geo-replication** permite que un **Azure Container Registry (ACR)** replique automáticamente las imágenes en varias regiones, mejorando la disponibilidad y reduciendo la latencia.

Esta funcionalidad **solo está disponible en el SKU Premium**, por lo que el primer paso siempre será actualizar el registro desde **Basic** o **Standard** a **Premium**. 

### **22. SAS Expiry Recommendation**

Un **Storage Account** puede advertir cuando un usuario crea una **Shared Access Signature (SAS)** con una duración superior a la recomendada.

La opción **Allow recommended upper limit for shared access signature (SAS) expiry interval** **no bloquea** la creación de la SAS; únicamente muestra una advertencia para fomentar buenas prácticas de seguridad. 

### **23. Azure Files + Active Directory Domain Services**

Cuando un **Azure File Share** utiliza **Active Directory Domain Services (AD DS)** para la autenticación, los usuarios deben existir en dicho dominio.

Los usuarios **cloud-only** de **Microsoft Entra ID** no pueden autenticarse mediante AD DS, ya que no disponen de una identidad en el dominio local sincronizado. 

### **24. Azure Files + Usuarios sincronizados**

Un usuario sincronizado desde **Active Directory** mediante **Microsoft Entra Connect** puede acceder a un **Azure File Share** integrado con AD DS si dispone de los permisos adecuados.

La autenticación la realiza **AD DS**, mientras que la autorización se controla mediante **Azure RBAC** y los permisos del recurso compartido. 

### **25. Identity-based Access en Storage Accounts**

La configuración de **Identity-based Access** es independiente para cada **Storage Account**.

Configurar un Storage Account para utilizar **AD DS** no implica que el resto de Storage Accounts del tenant adopten automáticamente la misma configuración. 

### **26. Eliminar usuarios con licencias**

Un usuario de **Microsoft Entra ID** puede eliminarse aunque tenga licencias asignadas, ya sean directas o heredadas mediante grupos.

Al eliminar el usuario, las licencias quedan automáticamente disponibles para volver a asignarlas a otros usuarios. 

### **27. Eliminar grupos con licencias**

Un grupo que tenga **licencias asignadas directamente** no puede eliminarse hasta retirar previamente dichas licencias.

Esta restricción evita dejar asignaciones de licencias huérfanas dentro del tenant. 

### **28. Azure Backup Reports + Storage Account**

Si **Azure Backup Reports** exporta información a un **Storage Account**, éste debe encontrarse en la **misma región** que el **Recovery Services Vault**.

Este requisito solo aplica al Storage Account utilizado para almacenar los informes exportados. 

### **29. Azure Backup Reports + Log Analytics**

El **Log Analytics Workspace** utilizado por **Azure Backup Reports** puede encontrarse en una región distinta al **Recovery Services Vault**.

A diferencia del **Storage Account**, **no existe un requisito de coincidencia de región** entre ambos recursos. 

### **30. Connection Monitor**

**Connection Monitor** supervisa la conectividad entre recursos de Azure y permite detectar problemas de red mediante pruebas periódicas.

Las máquinas virtuales supervisadas deben pertenecer a la **misma región**, por lo que si existen VMs en varias regiones será necesario crear **un Connection Monitor por cada región**. 

# Parte 2A

### **31. Reglas predeterminadas de un NSG**

Un **Network Security Group (NSG)** con únicamente las **reglas predeterminadas** **no permite** conexiones **RDP (TCP 3389)** desde Internet. La regla **DenyAllInbound (65500)** bloquea todo el tráfico entrante que no haya sido permitido explícitamente.

Para publicar una máquina virtual mediante **Remote Desktop**, es obligatorio crear una regla **Allow** con una prioridad superior a **65500**. 

### **32. NSG asociado a una Network Interface**

Un **NSG** puede asociarse tanto a una **Subnet** como a una **Network Interface (NIC)**. Si existe una regla **Allow TCP 3389** en el NSG de la NIC, la máquina virtual podrá aceptar conexiones RDP siempre que ninguna otra regla lo impida.

En el examen recuerda que **las reglas del NSG de la Subnet y de la NIC se evalúan conjuntamente**, y el tráfico solo se permite si **ninguna** de ellas lo bloquea. 

### **33. Comunicación entre máquinas virtuales de una misma VNet**

Las máquinas virtuales situadas en una **misma Virtual Network** pueden comunicarse mediante sus **Private IPs** gracias a la regla predeterminada **AllowVNetInBound**.

Esta comunicación solo dejará de funcionar si un **NSG**, una **User Defined Route (UDR)** o un **Firewall** bloquean explícitamente el tráfico. 

### **34. Overlapping de Address Spaces**

Dos **Virtual Networks** únicamente pueden establecer un **VNet Peering** si sus **Address Spaces no se solapan (CIDR Overlap)**.

Si ambos utilizan el mismo rango de direcciones, primero deberá modificarse el **Address Space** de una de las VNets antes de crear el peering. 

### **35. Internal Load Balancer**

Un **Internal Load Balancer (ILB)** distribuye tráfico utilizando una **Private IP**, por lo que solo puede ser utilizado desde la red privada (VNet, VPN o ExpressRoute).

Es la solución recomendada para balancear tráfico entre capas internas de una aplicación, por ejemplo entre la **Web Tier** y la **Application Tier**. 
---

### **36. Azure Application Gateway WAF**

Si el objetivo es proteger una aplicación web frente a ataques como **SQL Injection**, **Cross-Site Scripting (XSS)** o **OWASP Top 10**, debe utilizarse un **Web Application Firewall (WAF)**.

Ni un **Load Balancer** ni un **Network Security Group** inspeccionan el contenido HTTP/HTTPS de las peticiones. 

---

### **37. Address Spaces en una Virtual Network**

Una **Virtual Network** puede contener **varios Address Spaces**. Esto permite ampliar la red sin necesidad de crear una nueva VNet.

Antes de crear una **Subnet** utilizando un nuevo rango IP, dicho rango debe añadirse previamente como **Address Space** de la VNet. 

---

### **38. Crear una nueva Subnet**

Si el rango IP ya pertenece al **Address Space** de la **Virtual Network**, únicamente será necesario crear una nueva **Subnet** utilizando ese rango.

No es necesario modificar el **Address Space** siempre que el nuevo CIDR esté incluido dentro de él y no se solape con otras subredes. 

---

### **39. SSPR para administradores**

Los usuarios que poseen **roles administrativos** en **Microsoft Entra ID** siguen una política especial de **Self-Service Password Reset (SSPR)**.

Por motivos de seguridad, **no pueden utilizar preguntas de seguridad** como método de autenticación durante el restablecimiento de la contraseña. 

---

### **40. Métodos de autenticación para administradores**

Los administradores de **Microsoft Entra ID** deben utilizar **dos métodos de autenticación** para realizar un **Self-Service Password Reset**.

Los métodos admitidos incluyen, por ejemplo, **Microsoft Authenticator**, **SMS**, **llamada telefónica** o **correo alternativo**, pero **nunca** las **preguntas de seguridad**.
---
# Parte 2B

### **41. SSPR para usuarios estándar**

Los **usuarios que no tienen un rol administrativo** siguen la política estándar de **Self-Service Password Reset (SSPR)**. Si las **preguntas de seguridad** están habilitadas, Azure puede solicitar cualquiera de las preguntas configuradas como método de autenticación.

La diferencia importante respecto a los administradores es que **los usuarios normales sí pueden utilizar preguntas de seguridad**, mientras que los administradores no. 

### **42. Data Collection Rule (DCR) - Data Sources**

Una **Data Collection Rule (DCR)** define **qué datos se recopilan**, **desde dónde** y **a dónde se envían**. Como origen (**Data Source**) solo admite recursos compatibles, siendo las **Virtual Machines** el caso más habitual.

Ni un **Storage Account**, ni un **Log Analytics Workspace**, ni una **Azure SQL Database** pueden configurarse como **Data Sources** de una DCR. 

### **43. Data Collection Rule (DCR) - Destinations**

Los datos recopilados por una **Data Collection Rule** pueden enviarse a destinos compatibles como un **Log Analytics Workspace**, **Azure Monitor Metrics** o **Event Hubs**, dependiendo del escenario.

Un **Storage Account** o una **Azure SQL Database** no son destinos directos de una DCR estándar para Azure Monitor. 

### **44. Azure Backup - File Recovery**

Cuando una **Azure Virtual Machine** está protegida mediante **Azure Backup**, es posible recuperar únicamente archivos o carpetas sin restaurar toda la máquina.

Azure monta temporalmente el **Recovery Point** como una unidad en un equipo Windows, permitiendo copiar los archivos deseados mediante el Explorador de archivos. 

### **45. Azure Backup Retention**

Si un mismo **Recovery Point** coincide con varias reglas de retención (**diaria**, **semanal**, **mensual** o **anual**), Azure siempre conserva la **retención más larga**.

Por ejemplo, si una copia coincide con la retención mensual y anual, permanecerá almacenada durante el período anual configurado.

### **46. Prioridad entre reglas de retención**

Las políticas de retención de **Azure Backup** son acumulativas. Un mismo punto de recuperación puede pertenecer a varias categorías de retención simultáneamente.

En caso de coincidencia, **siempre prevalece el período de retención más largo**, evitando eliminar copias que aún deban conservarse. 

### **47. Recovery Services Vault**

Las copias de seguridad de las **Azure Virtual Machines** siempre se administran desde un **Recovery Services Vault**.

No es posible almacenar directamente los backups de una VM en un **Storage Account**, un **Blob Container** o un **Azure File Share**. :

### **48. Backup Policy**

Una **Backup Policy** define **cuándo** se realizan las copias de seguridad y **cuánto tiempo** se conservan los distintos **Recovery Points**.

La política puede incluir retenciones **diarias**, **semanales**, **mensuales** y **anuales**, todas gestionadas automáticamente por Azure Backup. 


### **49. Azure Monitor Private Link Scope (AMPLS)**

Para que **Azure Monitor** sea accesible únicamente mediante **red privada**, primero debe crearse un **Azure Monitor Private Link Scope (AMPLS)**.

Después se crea un **Private Endpoint** asociado al AMPLS, evitando que las comunicaciones con Azure Monitor utilicen la red pública. 



### **50. ARM Template - copy**

La función **copy** de un **ARM Template** permite crear múltiples instancias de un mismo recurso o propiedad utilizando un único bloque de definición.

Normalmente se utiliza para desplegar varios **Data Disks**, **NICs** u otros recursos repetitivos, combinándola con **copyIndex()** para generar valores únicos en cada iteración. 


### **51. ARM Template - copyIndex()**

La función **copyIndex()** devuelve el índice de cada iteración de un bloque **copy** dentro de un **ARM Template**. Se utiliza para generar valores únicos, como el **LUN** de un disco, el nombre de un recurso o un número secuencial.

Normalmente se combina con **copy** para desplegar varios recursos similares sin repetir código.



### **52. Private Endpoint para Azure Storage**

Si el requisito es que todo el tráfico entre una **Virtual Machine** y un **Storage Account** permanezca dentro de la red privada de Azure, debe utilizarse un **Private Endpoint**.

Los **Service Endpoints** siguen accediendo al servicio mediante su **IP pública**, mientras que **Private Endpoint** asigna una **Private IP** dentro de la VNet.



### **53. Acceso a Blob Storage con mínimo privilegio**

Para cargar archivos desde el **Azure Portal** a un **Blob Container**, el usuario necesita permisos tanto sobre el recurso como sobre los datos.

La combinación habitual es **Reader** (para visualizar el Storage Account) y **Storage Blob Data Contributor** (para leer y escribir blobs), siguiendo el principio de **mínimo privilegio**.



### **54. Inmutabilidad de Blob Storage**

Si un blob no debe modificarse ni eliminarse durante un período determinado, debe configurarse una **Time-based Retention Policy** o una **Legal Hold**.

Los permisos **IAM**, los **Access Keys** o el **Access Tier** no proporcionan protección frente a modificaciones o eliminaciones accidentales.



### **55. Ámbito de una Alert Rule**

Una **Alert Rule** cuyo ámbito es una **Subscription** o **todas las Resource Groups** supervisará también los recursos que se creen en el futuro dentro de ese ámbito.

No es necesario modificar la alerta cada vez que se crea una nueva **Resource Group** o un nuevo recurso.



### **56. Alert Processing Rule**

Una **Alert Processing Rule** permite modificar el tratamiento de una alerta después de que ésta se haya generado.

Por ejemplo, una regla de tipo **Suppress notifications** evita el envío de correos o SMS, pero **la alerta sigue generándose y registrándose** en Azure Monitor.



### **57. Alertas administrativas**

Las operaciones administrativas, como **crear**, **eliminar** o **añadir un Tag** a un recurso, pueden generar alertas si existe una **Alert Rule** basada en **Administrative Activity**.

Estas alertas se basan en el **Activity Log**, no en métricas ni en registros de Log Analytics.



### **58. Recovery Services Vault con ZRS**

Si el requisito indica que las copias de seguridad deben almacenarse en **Availability Zones**, el **Recovery Services Vault** debe configurarse con **Zone-Redundant Storage (ZRS)** antes de habilitar el backup.

Una vez que el Vault contiene elementos protegidos, **no es posible cambiar posteriormente el tipo de redundancia**.


### **59. Group-Based Licensing**

Cuando una licencia se asigna mediante un **grupo**, el usuario la recibe automáticamente mientras pertenezca a ese grupo.

No puede eliminarse la licencia directamente del usuario; para retirarla es necesario **quitar al usuario del grupo** o **eliminar la licencia del grupo**.



### **60. Prioridad del Group-Based Licensing**

El **Group-Based Licensing** se administra automáticamente por **Microsoft Entra ID** y vuelve a aplicar la licencia si el usuario continúa perteneciendo al grupo.

Por ello, quitar manualmente la licencia al usuario **no tiene efecto permanente**, ya que la heredará de nuevo mientras siga siendo miembro del grupo.



### **61. Group-Based Licensing y grupos anidados**

El **Group-Based Licensing** de **Microsoft Entra ID** **no admite grupos anidados**. Si un grupo con una licencia asignada contiene otro grupo, los miembros del grupo interno **no heredarán la licencia**.

Para que un usuario reciba una licencia mediante este mecanismo, debe ser **miembro directo** del grupo al que se ha asignado la licencia

### **62. Cuotas de vCPU en Azure**

Azure controla el número máximo de **vCPUs** que pueden desplegarse en cada región mediante las **vCPU Quotas**. Estas cuotas se aplican tanto a una familia de máquinas virtuales como al total regional.

Antes de crear una nueva VM, Azure comprueba que ambas cuotas dispongan de capacidad suficiente.

### **63. Máquinas virtuales Deallocated y cuotas**

Una **Virtual Machine** en estado **Stopped (Deallocated)** deja de consumir recursos de proceso, pero **sigue contando para la cuota de vCPUs** de la suscripción.

Si se supera la cuota regional, Azure impedirá crear nuevas máquinas virtuales aunque algunas estén desasignadas


### **64. Total Regional vCPU Quota**

La **Total Regional vCPU Quota** representa el número máximo de vCPUs que una suscripción puede tener aprovisionadas en una región determinada.

Para desplegar una nueva VM es necesario cumplir tanto la **cuota regional** como la **cuota de la familia** de máquinas virtuales elegida.


### **65. Dynamic User Groups**

Un **Dynamic User Group** agrega y elimina usuarios automáticamente según reglas basadas en atributos como **Department**, **Country** o **Job Title**.

La pertenencia al grupo depende exclusivamente de la **Membership Rule**, no de las licencias asignadas al usuario.



### **66. Pertenencia a varios Dynamic Groups**

Un usuario puede pertenecer simultáneamente a varios **Dynamic User Groups** siempre que cumpla las reglas de cada uno.

Cada grupo evalúa sus reglas de forma independiente, por lo que un cambio en los atributos del usuario puede modificar automáticamente su pertenencia.


### **67. Virtual Machine Extensions**

Las **Virtual Machine Extensions** permiten instalar software o ejecutar tareas de configuración después del despliegue de una VM.

En un **ARM Template**, las extensiones se implementan mediante el recurso **Microsoft.Compute/virtualMachines/extensions**, que es un recurso hijo de la máquina virtual.


### **68. Protected Settings**

Las **Virtual Machine Extensions** disponen de dos bloques de configuración: **Settings** y **ProtectedSettings**.

Toda la información confidencial, como **contraseñas**, **claves** o **tokens**, debe almacenarse en **ProtectedSettings**, ya que Azure la cifra automáticamente.



### **69. Publicar aplicaciones solo para redes privadas**

Si una aplicación únicamente debe ser accesible desde una **VPN** o mediante **ExpressRoute**, debe utilizarse un **Internal Load Balancer** o un **Application Gateway interno**.

Ambas soluciones utilizan una **Private IP**, evitando exponer el servicio a Internet.

---

### **70. Rol Contributor**

El rol **Contributor** permite crear, modificar y eliminar prácticamente todos los recursos de Azure, como **Virtual Machines**, **Virtual Networks** o **Storage Accounts**.

Sin embargo, **no puede asignar permisos RBAC** ni administrar el acceso de otros usuarios; para ello se requiere **Owner** o **User Access Administrator**.


# Parte 3B

### **71. Azure Policy - Efecto Deny**

Una **Azure Policy** con efecto **Deny** impide la creación o modificación de recursos que no cumplan la política definida.

El bloqueo se aplica independientemente del método utilizado (**Portal**, **Azure CLI**, **PowerShell**, **ARM Templates** o **Terraform**), ya que la validación se realiza en Azure Resource Manager.

---

### **72. Resource Locks**

Los **Resource Locks** protegen un recurso frente a modificaciones o eliminaciones accidentales. Existen dos tipos:

| Tipo de bloqueo | Impide |
|-----------------|---------|
| **CanNotDelete (Delete Lock)** | Impide eliminar el recurso. También impide mover recursos entre Resource Groups o Subscriptions, ya que el movimiento requiere operaciones internas de modificación. Permite modificar el recurso. |
| **ReadOnly (Read-only Lock)** | Impide cualquier modificación del recurso, incluida su eliminación y su movimiento entre Resource Groups o Subscriptions. El recurso solo puede leerse. |

Un **Resource Lock** no controla los permisos mediante **Azure RBAC**. Aunque un usuario tenga permisos de **Owner** o **Contributor**, el bloqueo seguirá aplicándose hasta que sea eliminado.

---

### **73. Mover recursos y Resource Locks**

Un recurso **no puede moverse** entre **Resource Groups** o **Subscriptions** si el propio recurso o el Resource Group de origen o destino tiene un **Resource Lock** (**CanNotDelete** o **ReadOnly**).

Esto se debe a que la operación de movimiento requiere que Azure realice modificaciones internas sobre el recurso.

Antes de mover un recurso, es necesario eliminar los **Resource Locks** y volver a aplicarlos una vez finalizado el movimiento.

---

### **74. Archive Tier**

El **Archive Tier** es el nivel de almacenamiento de menor coste para datos que apenas se consultan y pueden permanecer archivados durante largos periodos.

Solo está disponible para **Azure Blob Storage** y requiere una rehidratación antes de poder acceder nuevamente a los datos.

---

### **75. Azure Table Storage**

**Azure Table Storage** es un servicio **NoSQL** orientado al almacenamiento de grandes volúmenes de datos estructurados mediante **Partition Key** y **Row Key**.

No está pensado para almacenar archivos, imágenes o documentos, sino información estructurada de alta escalabilidad.

---

### **76. Azure File Storage**

**Azure File Storage** proporciona recursos compartidos compatibles con el protocolo **SMB** y **NFS**, permitiendo montar unidades de red desde Windows y Linux.

A diferencia de **Blob Storage**, **no admite el Archive Tier**, por lo que no debe utilizarse para archivado de datos.

---

### **77. Azure Storage Explorer**

**Azure Storage Explorer** es la herramienta gráfica recomendada para administrar **Storage Accounts**, **Blob Containers**, **File Shares**, **Queues** y **Tables**.

Permite cargar, descargar y administrar datos de forma sencilla sin necesidad de utilizar comandos como **AzCopy**.

---

### **78. Recovery Services Vault**

Antes de proteger una **Virtual Machine** con **Azure Backup**, es obligatorio crear un **Recovery Services Vault**.

Después se configura una **Backup Policy** y, finalmente, se habilita la protección de la máquina virtual.

---

### **79. Access Control (IAM)**

Los permisos sobre una **Subscription**, **Resource Group** o **Resource** se administran desde **Access Control (IAM)** mediante **Azure RBAC**.

Los roles se heredan desde el ámbito donde se asignan, simplificando la administración de permisos en Azure.

---

### **80. NSG y HTTPS**

Para publicar una aplicación web mediante **HTTPS**, el **Network Security Group (NSG)** debe permitir tráfico **Inbound TCP 443** desde el origen correspondiente.

Si no existe una regla **Allow** con mayor prioridad que la regla **DenyAllInbound**, Azure bloqueará las conexiones HTTPS.


# Parte 3C

### **81. Azure Resource Tags**

Los **Tags** permiten clasificar y organizar los recursos mediante pares **Nombre = Valor**, facilitando la administración, el filtrado y el control de costes.

Pueden aplicarse a **Subscriptions**, **Resource Groups** y **Resources**, pero **no** a los **Management Groups**.

---

### **82. Azure Management Groups**

Los **Management Groups** permiten organizar varias **Subscriptions** en una jerarquía para administrar de forma centralizada **Azure Policy** y **Azure RBAC**.

Las políticas y permisos asignados a un **Management Group** se heredan automáticamente por todas las suscripciones y recursos descendientes.

---

### **83. Azure Policy vs Azure RBAC**

**Azure RBAC** controla **quién** puede realizar una acción sobre un recurso, mientras que **Azure Policy** controla **qué configuraciones están permitidas**.

Ambos servicios son complementarios: un usuario puede tener permisos RBAC y, aun así, una **Azure Policy** impedir la operación.

---

### **84. Service Endpoint**

Un **Service Endpoint** mantiene el acceso a un servicio PaaS mediante su **IP pública**, pero el tráfico viaja por la **red troncal de Microsoft**.

No asigna una **Private IP** al servicio; para ello debe utilizarse un **Private Endpoint**.

---

### **85. Private Endpoint**

Un **Private Endpoint** asigna una **Private IP** de la **Virtual Network** a un servicio PaaS de Azure mediante **Azure Private Link**.

Desde la VNet, el servicio se consume como si fuera un recurso interno, eliminando la necesidad de acceder a su dirección pública.

---

### **86. Service Endpoint Policy**

Una **Service Endpoint Policy** permite restringir el acceso desde una **Subnet** únicamente a determinados **Storage Accounts** cuando se utilizan **Service Endpoints**.

Si un Storage Account no está incluido en la política, el acceso quedará bloqueado aunque pertenezca al servicio **Microsoft.Storage**.

---

### **87. Private DNS Zone**

Una **Private DNS Zone** permite resolver automáticamente el nombre DNS de un **Private Endpoint** hacia su **Private IP**.

Sin esta resolución DNS, las aplicaciones podrían seguir intentando acceder al servicio mediante su dirección pública.

---

### **88. Recovery Services Vault**

Un **Recovery Services Vault** no puede eliminarse mientras contenga **Protected Items**, **Recovery Points** o copias de seguridad activas.

Antes de eliminar el Vault o el **Resource Group** que lo contiene, es necesario **detener la protección** y eliminar todos los elementos protegidos.

---

### **89. App Service Plan**

Un **App Service Plan** define el **SKU**, el **sistema operativo**, la **región** y los recursos de proceso (**CPU** y **memoria**) que compartirán todas las **Web Apps** hospedadas en él.

Varias aplicaciones pueden compartir el mismo App Service Plan, compartiendo también sus recursos.

---

### **90. Web App**

Una **Azure Web App** siempre debe ejecutarse dentro de un **App Service Plan** compatible con su **región** y **sistema operativo**.

Por ejemplo, una aplicación **.NET 8** puede ejecutarse tanto en **Windows** como en **Linux**, siempre que el **App Service Plan** sea compatible.


------------------------

# Test 2

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
### **1. Azure App Service Deployment Slots**

Los **Deployment Slots** permiten desplegar una nueva versión de una aplicación en un entorno de **Staging** antes de pasarla a **Producción**, evitando tiempos de inactividad.

La forma correcta de publicar una nueva versión es **Deploy → Test → Swap**. Si el despliegue falla, puede realizarse otro **Swap** para volver inmediatamente a la versión anterior.

---

### **2. ARM Template - resourceId()**

La función **resourceId()** de un **ARM Template** construye el identificador completo (**Resource ID**) de un recurso de Azure.

Debe utilizarse cuando otro recurso necesita referenciar una **NIC**, una **Virtual Machine** o cualquier otro recurso dentro del mismo despliegue.

---

### **3. ARM Template - imageReference**

La propiedad **imageReference** especifica la imagen del **Marketplace** que utilizará una **Virtual Machine** durante su creación.

Incluye valores como **publisher**, **offer**, **sku** y **version**, y se encuentra dentro de **storageProfile**.

---

### **4. Azure App Service - Custom Domain**

Antes de asociar un **Custom Domain** a un **Azure App Service**, Azure debe comprobar que eres el propietario del dominio.

Para ello, primero debe crearse el registro **TXT asuid**, y únicamente después podrán configurarse los registros **CNAME** o **A**.

---

### **5. Azure Container Instance - Public IP**

Un **Azure Container Instance** configurado con **IP Address Type = Public** expone el contenedor directamente a Internet mediante una **Public IP**.

Todos los puertos publicados en el **Container Group** podrán ser accesibles desde Internet, siempre que estén definidos en la configuración.

---

### **6. Azure Container Instance - Restart Policy**

La propiedad **Restart Policy** determina cuándo Azure reinicia automáticamente un contenedor.

Con **OnFailure**, el contenedor solo se reiniciará cuando finalice con un error; con **Always** se reiniciará siempre y con **Never** nunca se reiniciará.

---

### **7. Azure Traffic Analytics**

**Traffic Analytics** analiza los **NSG Flow Logs** para mostrar información sobre el tráfico de red y detectar patrones de comunicación.

Para funcionar necesita un **Storage Account**, donde se almacenan los Flow Logs, y un **Log Analytics Workspace**, donde se analizan los datos.

---

### **8. Azure Monitor - Email Alerts**

Las acciones de tipo **Email** de un **Action Group** no tienen limitación de frecuencia por parte de Azure Monitor.

Si una alerta se genera cada minuto, Azure enviará un correo electrónico por cada activación de la alerta.

---

### **9. Azure Monitor - SMS Alerts**

Las notificaciones mediante **SMS** sí tienen limitación para evitar envíos masivos.

Azure Monitor envía como máximo **1 SMS cada 5 minutos**, lo que equivale a un máximo de **12 SMS por hora**.

---

### **10. Recovery Services Vault**

Un **Recovery Services Vault** únicamente puede proteger recursos que se encuentren en la **misma región** que el propio Vault.

Antes de planificar una estrategia de backup, es importante comprobar que el **Vault** y los recursos protegidos comparten la misma región de Azure.
---
### **11. Azure Container Registry Tasks (ACR Tasks)**

**ACR Tasks** permite compilar, probar y publicar automáticamente imágenes Docker directamente en **Azure Container Registry**, sin necesidad de un servidor de compilación externo.

Está disponible en los SKUs **Basic**, **Standard** y **Premium**. Lo que cambia entre ellos son las funcionalidades avanzadas y los límites de capacidad, no la disponibilidad de ACR Tasks.

---

### **12. Azure Container Registry - Private Endpoint**

Los **Private Endpoints** permiten acceder a un **Azure Container Registry** mediante una **Private IP** dentro de una **Virtual Network**, evitando el acceso por Internet.

Esta funcionalidad **solo está disponible en el SKU Premium** de Azure Container Registry.

---

### **13. Azure Container Registry - Dedicated Data Endpoint**

El **Dedicated Data Endpoint** separa el tráfico de administración del tráfico utilizado para descargar y subir imágenes a un **Azure Container Registry**.

Para utilizar esta característica es necesario que el registro sea de tipo **Premium**.

---

### **14. Microsoft Entra ID - External Collaboration**

Las **External Collaboration Settings** permiten controlar qué organizaciones externas pueden colaborar con tu tenant mediante usuarios **B2B Guest**.

Las **Collaboration Restrictions** permiten crear listas de dominios permitidos o bloqueados para impedir invitaciones no autorizadas.

---

### **15. Cambiar el tamaño de una Virtual Machine**

Si Azure no puede cambiar el tamaño de una **Virtual Machine** por falta de capacidad en el host actual, primero debe ejecutarse **Deallocate** sobre la VM.

Al quedar desasignada, Azure podrá moverla a otro clúster con recursos disponibles y completar el cambio de tamaño.

---

### **16. Public IP y Network Interface**

Una **Public IP Address** siempre se asocia a una **Network Interface (NIC)** o al **Frontend** de un **Load Balancer**.

Las máquinas virtuales utilizan la Public IP a través de su NIC; la IP pública nunca se asigna directamente a la VM.

---

### **17. Service Chaining**

**Service Chaining** permite redirigir tráfico entre distintas redes utilizando una **User Defined Route (UDR)** hacia una **Network Virtual Appliance (NVA)**.

Se utiliza habitualmente para compartir un **Firewall**, una **VPN** o cualquier otro dispositivo de red entre varias Virtual Networks.

---

### **18. Rotación de Access Keys**

La rotación automática de las **Access Keys** de un **Storage Account** suele implementarse mediante **Azure Key Vault** junto con procesos automatizados.

Ni **Recovery Services Vault**, ni **Backup Vault**, ni **Lifecycle Management** gestionan la renovación automática de claves.

---

### **19. Storage Blob Data Roles**

Los roles **Storage Blob Data Contributor** y **Storage Blob Data Owner** permiten acceder directamente al contenido de los blobs y admiten **Azure RBAC Conditions**.

Los roles administrativos del Storage Account administran el recurso, pero **no conceden acceso a los datos** almacenados en los blobs.

---

### **20. Dynamic User Groups**

Los **Dynamic User Groups** agregan y eliminan usuarios automáticamente en función de atributos como **Department**, **Country** o **Job Title**.

Son la solución recomendada para automatizar la asignación de **licencias**, **aplicaciones** y **permisos**, evitando la administración manual de los miembros del grupo.

### **21. Azure Container Registry - Geo-replication**

La **Geo-replication** replica automáticamente un **Azure Container Registry** en varias regiones, mejorando la disponibilidad y reduciendo la latencia para los usuarios.

Esta característica **solo está disponible en el SKU Premium**, por lo que un registro **Basic** o **Standard** debe actualizarse antes de poder habilitarla.

---

### **22. Shared Access Signature (SAS) - Expiry Recommendation**

Un **Storage Account** puede generar una advertencia cuando una **Shared Access Signature (SAS)** supera el tiempo máximo recomendado de validez.

La opción **Allow recommended upper limit for SAS expiry interval** **no bloquea** la creación de la SAS; únicamente informa de que la duración excede la recomendación de Microsoft.

---

### **23. Azure Files + Active Directory Domain Services**

Cuando un **Azure File Share** utiliza **Active Directory Domain Services (AD DS)** para la autenticación, los usuarios deben existir en el dominio local o estar sincronizados mediante **Microsoft Entra Connect**.

Un usuario creado únicamente en **Microsoft Entra ID (cloud-only)** no podrá autenticarse utilizando **AD DS**.

---

### **24. Azure Files + Usuarios sincronizados**

Los usuarios sincronizados desde **Active Directory** pueden acceder a un **Azure File Share** integrado con **AD DS** siempre que dispongan de los permisos adecuados.

La autenticación la realiza **Active Directory**, mientras que la autorización se controla mediante **Azure RBAC** y los permisos configurados sobre el recurso compartido.

---

### **25. Identity-based Access en Storage Accounts**

La configuración de **Identity-based Access** es independiente para cada **Storage Account** y debe habilitarse individualmente.

Configurar un Storage Account para utilizar **AD DS** o **Microsoft Entra Kerberos** no afecta automáticamente al resto de Storage Accounts de la suscripción.

---

### **26. Eliminar usuarios con licencias**

Un usuario de **Microsoft Entra ID** puede eliminarse aunque tenga licencias asignadas, tanto de forma directa como mediante **Group-Based Licensing**.

Al eliminar el usuario, las licencias quedan disponibles para reutilizarse automáticamente con otros usuarios.

---

### **27. Eliminar grupos con licencias**

Un grupo que tenga **licencias asignadas directamente** no puede eliminarse hasta retirar previamente dichas licencias.

Esta restricción evita dejar asignaciones de licencias sin un grupo válido que las gestione.

---

### **28. Azure Backup Reports - Storage Account**

Si **Azure Backup Reports** exporta información a un **Storage Account**, éste debe encontrarse en la **misma región** que el **Recovery Services Vault**.

Este requisito solo aplica al Storage Account utilizado para almacenar los informes exportados.

---

### **29. Azure Backup Reports - Log Analytics Workspace**

El **Log Analytics Workspace** utilizado por **Azure Backup Reports** puede encontrarse en una región diferente a la del **Recovery Services Vault**.

A diferencia del **Storage Account**, **no existe un requisito de coincidencia de región** entre ambos recursos.

---

### **30. Connection Monitor**

**Connection Monitor** supervisa la conectividad entre recursos de Azure mediante pruebas periódicas de red, ayudando a detectar problemas de comunicación.

Para supervisar recursos situados en distintas regiones, normalmente será necesario crear un **Connection Monitor independiente para cada región**.
### **31. Reglas predeterminadas de un Network Security Group (NSG)**

Un **Network Security Group (NSG)** con únicamente las **reglas predeterminadas** **no permite** conexiones **RDP (TCP 3389)** desde Internet. La regla **DenyAllInbound (65500)** bloquea todo el tráfico entrante que no haya sido permitido previamente.

Para publicar una máquina virtual mediante **Remote Desktop**, es necesario crear una regla **Allow** con una prioridad superior a **65500**.

---

### **32. NSG asociado a una Network Interface**

Un **NSG** puede asociarse tanto a una **Subnet** como a una **Network Interface (NIC)**. Para que una conexión sea válida, **debe estar permitida por ambos NSGs** si existen.

Si cualquiera de ellos contiene una regla **Deny**, el tráfico será bloqueado, aunque el otro NSG lo permita.

---

### **33. Comunicación entre máquinas virtuales de una misma VNet**

Las máquinas virtuales situadas en una misma **Virtual Network** pueden comunicarse utilizando sus **Private IPs** gracias a la regla predeterminada **AllowVNetInBound**.

Esta comunicación solo dejará de funcionar si un **NSG**, una **User Defined Route (UDR)** o un **Azure Firewall** bloquean explícitamente el tráfico.

---

### **34. Overlapping de Address Spaces**

Dos **Virtual Networks** solo pueden establecer un **VNet Peering** si sus **Address Spaces no se solapan (Overlapping CIDR)**.

Si ambos utilizan rangos IP que se superponen, primero será necesario modificar el **Address Space** de una de las VNets.

---

### **35. Internal Load Balancer**

Un **Internal Load Balancer (ILB)** distribuye tráfico utilizando una **Private IP**, por lo que únicamente puede ser utilizado desde redes privadas (VNet, VPN o ExpressRoute).

Es la opción adecuada para equilibrar el tráfico entre capas internas de una aplicación, como la **Web Tier** y la **Application Tier**.

---

### **36. Web Application Firewall (WAF)**

Para proteger aplicaciones web frente a ataques como **SQL Injection**, **Cross-Site Scripting (XSS)** o vulnerabilidades del **OWASP Top 10**, debe utilizarse un **Application Gateway con WAF**.

Un **Load Balancer** o un **NSG** solo controlan el tráfico de red y **no inspeccionan el contenido HTTP/HTTPS**.

---

### **37. Address Spaces en una Virtual Network**

Una **Virtual Network** puede contener **varios Address Spaces**, permitiendo ampliar el espacio de direcciones sin crear una nueva VNet.

Antes de utilizar un nuevo rango IP, éste debe añadirse primero como **Address Space** de la Virtual Network.

---

### **38. Crear una nueva Subnet**

Si el rango IP ya pertenece al **Address Space** de la Virtual Network, basta con crear una nueva **Subnet** utilizando ese rango.

No es necesario modificar el **Address Space**, siempre que el nuevo CIDR no se solape con otras subredes existentes.

---

### **39. Self-Service Password Reset (SSPR) para administradores**

Los usuarios con **roles administrativos** de **Microsoft Entra ID** siguen una política especial de **Self-Service Password Reset (SSPR)**.

Por motivos de seguridad, **no pueden utilizar preguntas de seguridad** como método para restablecer su contraseña.

---

### **40. Métodos de autenticación para administradores**

Los administradores de **Microsoft Entra ID** deben utilizar **dos métodos de autenticación** para realizar un **Self-Service Password Reset**.

Los métodos permitidos incluyen **Microsoft Authenticator**, **SMS**, **llamada telefónica** o **correo alternativo**, pero **nunca las preguntas de seguridad**.

### **41. Self-Service Password Reset (SSPR) para usuarios**

Los **usuarios estándar** (que no son administradores) siguen la política normal de **Self-Service Password Reset (SSPR)**. Si las **preguntas de seguridad** están habilitadas, Azure podrá utilizarlas como uno de los métodos para verificar la identidad.

La diferencia importante respecto a los administradores es que **los usuarios normales sí pueden utilizar preguntas de seguridad** durante el restablecimiento de la contraseña.

---

### **42. Data Collection Rule (DCR) - Orígenes de datos**

Una **Data Collection Rule (DCR)** define **qué datos se recopilan**, **desde qué recursos** y **a qué destino** se enviarán. Como origen (**Data Source**) admite recursos compatibles como las **Virtual Machines**.

Recursos como un **Storage Account**, un **Log Analytics Workspace** o una **Azure SQL Database** no pueden configurarse como **Data Sources** de una DCR.

---

### **43. Data Collection Rule (DCR) - Destinos**

Los datos recopilados por una **Data Collection Rule** pueden enviarse a destinos compatibles como un **Log Analytics Workspace**, **Azure Monitor Metrics** o **Event Hubs**, según el escenario.

Un **Storage Account** o una **Azure SQL Database** no son destinos directos de una **Data Collection Rule** para Azure Monitor.

---

### **44. Azure Backup - File Recovery**

Cuando una **Virtual Machine** está protegida mediante **Azure Backup**, es posible recuperar únicamente archivos o carpetas sin restaurar toda la máquina.

Azure descarga un **script**, monta temporalmente el **Recovery Point** como una unidad local y permite copiar los archivos necesarios mediante el Explorador de Windows.

---

### **45. Azure Backup - Retención de Recovery Points**

Un mismo **Recovery Point** puede coincidir con varias reglas de retención (**diaria**, **semanal**, **mensual** y **anual**) definidas en la **Backup Policy**.

En ese caso, Azure siempre conserva la copia durante el **período de retención más largo**, evitando eliminar datos antes de tiempo.

---

### **46. Prioridad entre reglas de retención**

Las reglas de retención de **Azure Backup** son acumulativas. Si una copia pertenece simultáneamente a varias categorías de retención, **prevalece siempre la de mayor duración**.

Por ejemplo, si un Recovery Point coincide con la retención **mensual** y **anual**, se conservará hasta finalizar la retención anual.

---

### **47. Recovery Services Vault**

Todas las copias de seguridad de **Azure Virtual Machines** se almacenan y administran desde un **Recovery Services Vault**.

No es posible guardar directamente el backup de una VM en un **Storage Account**, un **Blob Container** o un **Azure File Share**.

---

### **48. Backup Policy**

La **Backup Policy** define la programación de las copias de seguridad y el tiempo que Azure conservará cada **Recovery Point**.

Una misma política puede incluir retenciones **diarias**, **semanales**, **mensuales** y **anuales**, gestionadas automáticamente por Azure Backup.

---

### **49. Azure Monitor Private Link Scope (AMPLS)**

Para que **Azure Monitor** sea accesible únicamente mediante la **red privada**, primero debe crearse un **Azure Monitor Private Link Scope (AMPLS)**.

Después se crea un **Private Endpoint** asociado al AMPLS, permitiendo que todas las comunicaciones con Azure Monitor permanezcan dentro de la red privada de Azure.

---

### **50. ARM Template - copy**

La función **copy** de un **ARM Template** permite crear múltiples instancias de un mismo recurso o propiedad utilizando una única definición.

Se utiliza habitualmente para desplegar varios **Data Disks**, **Network Interfaces** u otros recursos repetitivos, combinándola con **copyIndex()** para generar valores únicos.

### **51. ARM Template - copyIndex()**

La función **copyIndex()** devuelve el índice de cada iteración dentro de un bloque **copy** de un **ARM Template**. Se utiliza para generar nombres, números o configuraciones diferentes en cada recurso creado automáticamente.

Es habitual utilizarla para asignar valores como el **LUN** de varios discos de datos o para numerar máquinas virtuales creadas mediante un bucle.

---

### **52. Private Endpoint para Azure Storage**

Si el requisito es que una **Virtual Machine** acceda a un **Storage Account** sin salir nunca de la red privada de Azure, debe utilizarse un **Private Endpoint**.

Un **Service Endpoint** sigue accediendo al servicio mediante su **IP pública**, mientras que un **Private Endpoint** asigna una **Private IP** dentro de la Virtual Network.

---

### **53. Acceso a Blob Storage con mínimo privilegio**

Para cargar archivos desde el **Azure Portal** a un **Blob Container**, el usuario necesita permisos tanto sobre el recurso como sobre los datos.

La combinación recomendada es **Reader** (para visualizar el Storage Account) y **Storage Blob Data Contributor** (para leer y escribir blobs), siguiendo el principio de **mínimo privilegio**.

---

### **54. Inmutabilidad en Blob Storage**

Si los blobs no deben modificarse ni eliminarse durante un período determinado, debe configurarse una **Time-based Retention Policy** o una **Legal Hold**.

Los permisos **IAM**, las **Access Keys** o el **Access Tier** no protegen los datos frente a modificaciones o eliminaciones accidentales.

---

### **55. Ámbito de una Alert Rule**

Una **Alert Rule** cuyo ámbito es una **Subscription** o todas las **Resource Groups** supervisará automáticamente los recursos que se creen en el futuro dentro de ese ámbito.

No es necesario modificar la alerta cada vez que se crea una nueva máquina virtual, Storage Account o Resource Group.

---

### **56. Alert Processing Rule**

Una **Alert Processing Rule** permite modificar el comportamiento de una alerta una vez generada, por ejemplo suprimiendo las notificaciones.

Una regla de tipo **Suppress notifications** **no evita que la alerta exista**, únicamente impide el envío de correos, SMS u otras acciones del **Action Group**.

---

### **57. Alertas del Activity Log**

Las operaciones administrativas, como **crear**, **eliminar** o **añadir un Tag** a un recurso, pueden generar alertas basadas en el **Azure Activity Log**.

Estas alertas no dependen de métricas ni de Log Analytics, sino de los eventos administrativos registrados por Azure Resource Manager.

---

### **58. Recovery Services Vault con ZRS**

Si las copias de seguridad deben almacenarse de forma redundante entre **Availability Zones**, el **Recovery Services Vault** debe configurarse con **Zone-Redundant Storage (ZRS)** antes de habilitar el backup.

Una vez que el Vault contiene elementos protegidos, **ya no es posible cambiar el tipo de redundancia**.

---

### **59. Group-Based Licensing**

Cuando una licencia se asigna mediante un **grupo**, todos los miembros directos del grupo reciben automáticamente esa licencia.

La licencia **no puede eliminarse manualmente del usuario**; para retirarla es necesario quitar al usuario del grupo o eliminar la licencia asignada al grupo.

---

### **60. Prioridad del Group-Based Licensing**

Mientras un usuario continúe perteneciendo a un grupo con **Group-Based Licensing**, Microsoft Entra volverá a asignarle automáticamente la licencia si ésta se elimina manualmente.

Para retirar definitivamente la licencia, es obligatorio **eliminar la pertenencia al grupo** o **quitar la licencia del propio grupo**.

### **61. Group-Based Licensing y grupos anidados**

El **Group-Based Licensing** de **Microsoft Entra ID** **no admite grupos anidados**. Si un grupo con una licencia asignada contiene otro grupo, los miembros del grupo interno **no heredarán la licencia**.

Para que un usuario reciba una licencia mediante este mecanismo, debe ser **miembro directo** del grupo al que se ha asignado la licencia.

---

### **62. Cuotas de vCPU en Azure**

Azure limita el número de **vCPUs** que una suscripción puede utilizar en cada región mediante las **vCPU Quotas**, tanto por familia de máquinas virtuales como por el total regional.

Antes de desplegar una nueva VM, Azure comprueba ambas cuotas y rechazará el despliegue si cualquiera de ellas se supera.

---

### **63. Virtual Machines Deallocated y cuotas**

Una **Virtual Machine** en estado **Stopped (Deallocated)** deja de consumir recursos de proceso, pero **sigue consumiendo cuota de vCPUs**.

Por este motivo, desasignar una VM **no libera cuota** para crear nuevas máquinas virtuales; únicamente deja de generar costes de computación.

---

### **64. Total Regional vCPU Quota**

La **Total Regional vCPU Quota** representa el número máximo de **vCPUs aprovisionadas** que una suscripción puede tener en una región.

Para crear una nueva VM deben cumplirse simultáneamente la **cuota regional** y la **cuota específica de la familia** de máquinas virtuales elegida.

---

### **65. Dynamic User Groups**

Los **Dynamic User Groups** agregan y eliminan usuarios automáticamente según atributos como **Department**, **Country**, **Job Title** o cualquier otro atributo de Microsoft Entra ID.

La pertenencia al grupo depende únicamente de la **Membership Rule** y se actualiza automáticamente cuando cambian los atributos del usuario.

---

### **66. Pertenencia a varios Dynamic Groups**

Un mismo usuario puede pertenecer simultáneamente a varios **Dynamic User Groups** si cumple las reglas de cada uno de ellos.

Cada grupo evalúa su **Membership Rule** de forma independiente, por lo que un cambio en un atributo puede añadir o eliminar automáticamente al usuario.

---

### **67. Virtual Machine Extensions**

Las **Virtual Machine Extensions** permiten instalar software o ejecutar tareas de configuración automáticamente después de crear una máquina virtual.

En un **ARM Template**, las extensiones se implementan mediante el recurso **Microsoft.Compute/virtualMachines/extensions**, que es un recurso hijo de la VM.

---

### **68. Protected Settings**

Las **Virtual Machine Extensions** distinguen entre **Settings** y **ProtectedSettings** para almacenar su configuración.

Toda la información sensible, como **contraseñas**, **tokens** o **claves**, debe almacenarse en **ProtectedSettings**, ya que Azure la cifra automáticamente.

---

### **69. Internal Load Balancer**

Cuando una aplicación solo debe ser accesible desde una **VPN**, **ExpressRoute** o desde la propia **Virtual Network**, debe utilizarse un **Internal Load Balancer**.

El **Internal Load Balancer** utiliza una **Private IP**, por lo que nunca expone la aplicación directamente a Internet.

---

### **70. Rol Contributor**

El rol **Contributor** permite crear, modificar y eliminar prácticamente todos los recursos de Azure, como **Virtual Machines**, **Virtual Networks** y **Storage Accounts**.

Sin embargo, **no puede administrar permisos RBAC** ni asignar roles a otros usuarios; para ello se requiere **Owner** o **User Access Administrator**.
### **71. Azure Policy - Efecto Deny**

Una **Azure Policy** con efecto **Deny** impide crear o modificar recursos que no cumplan las reglas definidas por la organización.

La validación se realiza en **Azure Resource Manager (ARM)**, por lo que el bloqueo se aplica tanto desde el **Portal**, como desde **Azure CLI**, **PowerShell**, **ARM Templates** o **Terraform**.

---

### **72. Resource Locks**

Los **Resource Locks** protegen un recurso frente a modificaciones o eliminaciones accidentales. Existen dos tipos: **CanNotDelete** y **ReadOnly**.

Los bloqueos **no sustituyen a Azure RBAC**; simplemente impiden determinadas operaciones incluso aunque el usuario tenga permisos suficientes.

---

### **73. Mover recursos y Resource Locks**

Los **Resource Locks** están diseñados para impedir modificaciones o eliminaciones, **no para controlar el movimiento de recursos** entre **Resource Groups** o **Subscriptions**.

Para mover un recurso deben cumplirse los requisitos de Azure Resource Manager y de los tipos de recursos implicados, independientemente de RBAC.

---

### **74. Archive Tier**

El **Archive Tier** es el nivel de almacenamiento de menor coste de **Azure Blob Storage**, pensado para datos que apenas se consultan y pueden permanecer archivados durante largos periodos.

Los datos archivados deben **rehidratarse** antes de poder leerse, por lo que no es adecuado para información de acceso frecuente.

---

### **75. Azure Table Storage**

**Azure Table Storage** es un servicio **NoSQL** diseñado para almacenar grandes cantidades de datos estructurados utilizando una **Partition Key** y una **Row Key**.

No debe utilizarse para almacenar archivos, imágenes o documentos; para ello existen servicios como **Blob Storage** o **Azure Files**.

---

### **76. Azure File Storage**

**Azure File Storage** proporciona recursos compartidos compatibles con los protocolos **SMB** y **NFS**, permitiendo montar unidades de red desde Windows y Linux.

A diferencia de **Azure Blob Storage**, **no admite el Archive Tier**, por lo que no puede utilizarse para archivado de datos.

---

### **77. Azure Storage Explorer**

**Azure Storage Explorer** es la herramienta gráfica recomendada para administrar **Storage Accounts**, **Blob Containers**, **File Shares**, **Queues** y **Tables**.

Permite cargar, descargar y administrar datos de forma sencilla sin necesidad de utilizar comandos como **AzCopy** o **Azure CLI**.

---

### **78. Recovery Services Vault**

Antes de proteger una **Virtual Machine** mediante **Azure Backup**, es obligatorio crear un **Recovery Services Vault** y configurar una **Backup Policy**.

Una vez habilitada la protección, todas las copias de seguridad y los **Recovery Points** serán administrados desde ese Vault.

---

### **79. Access Control (IAM)**

Los permisos sobre una **Subscription**, un **Resource Group** o un **Resource** se administran desde **Access Control (IAM)** utilizando **Azure Role-Based Access Control (RBAC)**.

Los roles asignados se heredan automáticamente hacia los recursos inferiores, simplificando la administración de permisos.

---

### **80. Network Security Group y HTTPS**

Para permitir el acceso a una aplicación web mediante **HTTPS**, el **Network Security Group (NSG)** debe contener una regla **Allow Inbound TCP 443** con una prioridad superior a la regla **DenyAllInbound**.

Si no existe esa regla, Azure bloqueará las conexiones HTTPS aunque la máquina virtual esté funcionando correctamente.

### **81. Azure Resource Tags**

Los **Tags** permiten clasificar y organizar los recursos de Azure mediante pares **Nombre = Valor**, facilitando la administración, el filtrado, la automatización y el análisis de costes.

Pueden aplicarse a **Subscriptions**, **Resource Groups** y **Resources**, pero **no** a los **Management Groups**, una pregunta muy habitual en el AZ-104.

---

### **82. Azure Management Groups**

Los **Management Groups** permiten organizar varias **Subscriptions** en una estructura jerárquica para administrar de forma centralizada **Azure Policy** y **Azure RBAC**.

Las políticas y permisos asignados en un **Management Group** se heredan automáticamente por todas las **Subscriptions**, **Resource Groups** y **Resources** situados por debajo.

---

### **83. Azure RBAC vs Azure Policy**

**Azure RBAC** determina **quién puede realizar una acción** sobre un recurso, mientras que **Azure Policy** determina **qué configuraciones están permitidas** dentro de la organización.

Aunque un usuario tenga permisos RBAC suficientes, una **Azure Policy** con efecto **Deny** puede impedir la operación.

---

### **84. Service Endpoint**

Un **Service Endpoint** conecta una **Virtual Network** con un servicio PaaS de Azure utilizando la **red troncal de Microsoft**, pero el servicio **sigue utilizando su dirección IP pública**.

No asigna una **Private IP** al servicio; simplemente restringe el acceso para que solo pueda realizarse desde las subredes autorizadas.

---

### **85. Private Endpoint**

Un **Private Endpoint** utiliza **Azure Private Link** para asignar una **Private IP** de la **Virtual Network** a un servicio PaaS, como **Storage**, **Key Vault** o **SQL Database**.

Desde la VNet, el servicio se comporta como si estuviera dentro de la red privada, eliminando la necesidad de acceder mediante una IP pública.

---

### **86. Service Endpoint Policy**

Una **Service Endpoint Policy** permite limitar el acceso desde una **Subnet** únicamente a determinados **Storage Accounts** cuando se utilizan **Service Endpoints**.

Si un Storage Account no está incluido en la política, el acceso será denegado aunque pertenezca al servicio **Microsoft.Storage**.

---

### **87. Private DNS Zone**

Una **Private DNS Zone** resuelve automáticamente el nombre DNS de un **Private Endpoint** hacia su **Private IP**, permitiendo que las aplicaciones sigan utilizando el mismo nombre del servicio.

Sin una resolución DNS adecuada, los clientes podrían seguir resolviendo el nombre público y acceder al servicio por Internet.

---

### **88. Eliminar un Recovery Services Vault**

Un **Recovery Services Vault** no puede eliminarse mientras contenga **Protected Items**, **Recovery Points** o elementos protegidos por **Azure Backup**.

Antes de eliminar el Vault o el **Resource Group**, es obligatorio **detener la protección** y eliminar todos los elementos protegidos.

---

### **89. App Service Plan**

Un **App Service Plan** define la **región**, el **sistema operativo (Windows/Linux)**, el **SKU** y los recursos de proceso (**CPU**, **memoria** y escalabilidad**) que compartirán todas las **Web Apps** alojadas en él.

Varias aplicaciones pueden ejecutarse sobre el mismo App Service Plan compartiendo los mismos recursos.

---

### **90. Azure Web App**

Una **Web App** siempre debe ejecutarse dentro de un **App Service Plan** compatible tanto con la **región** como con el **sistema operativo** requerido por la aplicación.

Por ejemplo, una aplicación **.NET 8 (LTS)** puede ejecutarse tanto en **Windows** como en **Linux**, siempre que el **App Service Plan** esté en la **misma región**.


###################
# Test 6


### **1. Resource Guard (MAU)**

Para habilitar **Multi-User Authorization (MAU)** en un **Recovery Services Vault**, es obligatorio crear previamente un **Resource Guard**.

El **Resource Guard** protege operaciones críticas (como detener copias de seguridad o deshabilitar **Soft Delete**) exigiendo una autorización adicional.

---

### **2. Azure Firewall - Public IP**

**Azure Firewall** solo admite **Public IP Standard SKU**, **IPv4** y con asignación **Static**.

Las direcciones **Basic SKU** o **IPv6** no pueden asociarse a un **Azure Firewall**.

---

### **3. Co-Administrator**

El rol **Co-Administrator** únicamente puede asignarse al **nivel de Subscription**.

No puede asignarse directamente sobre **Management Groups**, **Resource Groups** ni **Resources**.

---

### **4. ARM Template Deployment**

Para desplegar un **ARM Template** dentro de un **Resource Group** mediante PowerShell se utiliza el cmdlet:

**`New-AzResourceGroupDeployment`**

Es el cmdlet específico para desplegar recursos contenidos en un **Resource Group**.

---

### **5. Resource Group Deployment**

Cuando se utiliza **New-AzResourceGroupDeployment**, el parámetro obligatorio es:

**`-ResourceGroupName`**

El despliegue siempre necesita conocer el **Resource Group** donde crear los recursos.

---

### **6. App Service Autoscale**

Cuando una regla de **Autoscale** incrementa las instancias en **1** y el número inicial es **1**, tras cumplirse la condición existirán **2 instancias**.

Azure nunca escala directamente hasta el máximo; cada regla realiza únicamente la acción configurada.

---

### **7. Autoscale Cooldown**

Después de una operación de **Scale Out**, Azure espera el tiempo definido en **Cooldown** antes de volver a evaluar la regla.

Mientras dura el **Cooldown**, no se realiza ninguna operación adicional de escalado.

---

### **8. Azure Files RBAC**

Para asignar permisos sobre un **Azure File Share** mediante **RBAC**, debe utilizarse **Access Control (IAM)** del propio **File Share**.

Los roles como **Storage File Data SMB Share Contributor** se asignan sobre el recurso **File Share**, no sobre el **Storage Account**.

---

### **9. Storage Account Routing Preference**

Para reducir el coste del tráfico de red de un **Storage Account**, debe modificarse el **Default Routing Tier**.

El uso de **Internet Routing** puede reducir el coste frente al enrutamiento por la red global de Microsoft.

---

### **10. Storage Encryption**

Después de crear un **Storage Account**, todavía puede modificarse el **Encryption Type**, por ejemplo cambiando entre claves administradas por Microsoft o por el cliente.

Sin embargo, opciones como **Infrastructure Encryption** solo pueden configurarse durante la creación del **Storage Account**.


### **11. Blob Point-in-Time Restore**

El **Point-in-Time Restore** únicamente permite restaurar datos hasta el número de días configurado en la política de restauración.

Si la política está configurada para **6 días**, no será posible restaurar datos modificados hace **7 días**. :contentReference[oaicite:0]{index=0}

---

### **12. Public Access de un Storage Account**

Cuando un **Storage Account** tiene **Public Network Access** habilitado y no existen restricciones de red, puede ser accesible desde cualquier ubicación de Internet.

La región donde se encuentra el Storage Account **no limita** desde qué región pueden conectarse los clientes. :contentReference[oaicite:1]{index=1}

---

### **13. LRS (Locally Redundant Storage)**

La redundancia **LRS** mantiene **tres copias** de los datos dentro de la **misma región** de Azure.

Estas copias protegen frente a fallos de hardware locales, pero **no** frente a la pérdida completa de una región. :contentReference[oaicite:2]{index=2}

---

### **14. Azure Disk Encryption**

**Azure Disk Encryption (ADE)** cifra tanto el **disco del sistema operativo** como los **discos de datos** utilizando claves almacenadas en **Azure Key Vault**.

La protección permanece incluso si el disco virtual se descarga fuera de Azure, a diferencia de **Encryption at Host**. :contentReference[oaicite:3]{index=3}

---

### **15. Azure Private DNS Zone**

Las máquinas virtuales que utilizan el **Azure-provided DNS** resolverán los registros de una **Private DNS Zone** siempre que su **Virtual Network** esté vinculada a dicha zona.

En ese caso, la resolución se realiza utilizando los registros almacenados en la **Private DNS Zone**, no en un servidor DNS personalizado. :contentReference[oaicite:4]{index=4}

---

### **16. Custom DNS vs Private DNS Zone**

Si una **Virtual Network** está configurada para utilizar un **Custom DNS Server**, las máquinas virtuales enviarán todas las consultas DNS a ese servidor.

En ese escenario, los registros de una **Private DNS Zone** **no se utilizarán automáticamente**, aunque la VNet esté vinculada a dicha zona. :contentReference[oaicite:5]{index=5}

---

### **17. Custom DNS Server**

Las máquinas virtuales utilizan siempre el **servidor DNS configurado en la Virtual Network**.

Si la VNet utiliza un **Custom DNS Server**, las consultas se resolverán utilizando ese servidor y sus registros DNS. :contentReference[oaicite:6]{index=6}

---

### **18. Lifecycle Management - Archive Tier**

Para mover automáticamente blobs al nivel de menor coste, una **Lifecycle Management Policy** debe utilizar la acción **tierToArchive**.

El **Archive Tier** es el nivel de almacenamiento más económico para datos que rara vez se consultan. :contentReference[oaicite:7]{index=7}

---

### **19. Lifecycle Management - prefixMatch**

Para aplicar una **Lifecycle Management Policy** únicamente a un **Blob Container**, debe utilizarse el filtro **prefixMatch**.

El valor de **prefixMatch** corresponde al nombre del contenedor (o a una ruta dentro de él), limitando la política únicamente a esos blobs. :contentReference[oaicite:8]{index=8}

### **20. Blob Versioning**

**Blob Versioning** crea automáticamente una nueva versión de un blob cada vez que se modifica o sobrescribe.

Permite recuperar versiones anteriores de un archivo sin necesidad de restaurar un backup completo.

---

### **21. Soft Delete para Blobs**

**Blob Soft Delete** protege frente a eliminaciones accidentales conservando temporalmente los blobs eliminados.

Durante el período de retención configurado, los blobs pueden restaurarse sin necesidad de utilizar Azure Backup.

---

### **22. Soft Delete para Containers**

**Container Soft Delete** permite recuperar un **Blob Container** eliminado junto con todo su contenido.

La recuperación solo es posible mientras no haya finalizado el período de retención configurado.

---

### **23. Change Feed**

El **Change Feed** registra de forma permanente todas las operaciones realizadas sobre los blobs de un **Storage Account**.

Se utiliza para auditoría, sincronización y procesamiento de eventos, pero **no permite restaurar datos**.

---

### **24. Azure Files Snapshot**

Un **Snapshot** de un **Azure File Share** permite recuperar archivos o carpetas individuales sin restaurar todo el recurso compartido.

Los snapshots son de solo lectura y se almacenan dentro del propio **Storage Account**.

---

### **25. Azure File Sync**

**Azure File Sync** sincroniza un **Azure File Share** con uno o varios servidores Windows mediante el agente **Azure File Sync Agent**.

Permite mantener una copia local de los archivos y utilizar **Cloud Tiering** para reducir el almacenamiento local.

---

### **26. Cloud Tiering**

**Cloud Tiering** mantiene únicamente los archivos más utilizados en el servidor local y traslada automáticamente el resto al **Azure File Share**.

Los archivos permanecen visibles para los usuarios y se descargan automáticamente cuando vuelven a abrirse.

---

### **27. Storage Account Firewall**

El **Storage Account Firewall** permite restringir el acceso al Storage Account mediante **IP públicas**, **Virtual Networks** o **Private Endpoints**.

Cuando el firewall está habilitado, solo podrán acceder los orígenes explícitamente autorizados.

---

### **28. Trusted Microsoft Services**

La opción **Allow trusted Microsoft services** permite que determinados servicios administrados por Microsoft accedan al **Storage Account** aunque el firewall esté habilitado.

Esta opción **no concede acceso a usuarios** ni a aplicaciones externas.

---

### **29. Azure Storage Replication**

La **Redundancy** de un **Storage Account** (**LRS**, **ZRS**, **GRS**, **RA-GRS** o **GZRS**) se configura a nivel del **Storage Account**.

La replicación se realiza automáticamente sobre **todos los datos** almacenados; no es posible seleccionar únicamente determinados contenedores o blobs.

### **30. Azure File Share Backup**

Los **Azure File Shares** solo pueden protegerse mediante un **Recovery Services Vault**.

Un **Backup Vault** admite copias de seguridad de **Azure Blob Storage**, pero **no** de **Azure File Shares**.

---

### **31. Storage Account Firewall y Service Endpoints**

Cuando un **Storage Account Firewall** permite acceso únicamente desde **subredes específicas**, solo las máquinas virtuales ubicadas en esas **Subnets autorizadas** podrán acceder al Storage Account.

Pertenecer a la misma **Virtual Network** no es suficiente; el acceso se controla a nivel de **Subnet** cuando se utilizan **Service Endpoints**.

---

### **32. Service Endpoint Scope**

Un **Service Endpoint** se habilita siempre sobre una **Subnet**, no sobre toda la **Virtual Network**.

Solo los recursos conectados a esa Subnet podrán acceder al servicio PaaS cuando el firewall del recurso esté configurado para permitir dicha Subnet.

---

### **33. Network Routing Preference**

La opción **Microsoft Network Routing** utiliza la red troncal global de Microsoft para transportar el tráfico hacia el servicio de Azure.

Esta configuración **no modifica** las reglas del firewall del Storage Account ni concede acceso a subredes no autorizadas.

---

### **34. Backup Vault**

Un **Backup Vault** es el almacén utilizado para proteger servicios modernos como **Azure Blob Storage**.

No puede utilizarse para realizar copias de seguridad de **Azure Virtual Machines** ni de **Azure File Shares**.

---

### **35. Recovery Services Vault**

Un **Recovery Services Vault** permite proteger recursos como **Azure Virtual Machines**, **Azure File Shares** y **SQL Server en Azure VM**.

No admite la protección de **Azure Blob Containers**, que utilizan **Backup Vault**.

### **36. Backup Policy - Retención anual**

Si un **Recovery Point** coincide con la programación de retención **anual**, Azure conservará esa copia durante el período definido para la retención anual.

Cuando una copia coincide con varias reglas de retención, **siempre prevalece la retención más larga**.

---

### **37. Backup Policy - Retención mensual**

Si un **Recovery Point** coincide simultáneamente con una retención **semanal** y **mensual**, Azure conservará la copia utilizando la **retención mensual**, por ser la de mayor duración.

Las reglas de retención de **Azure Backup** son acumulativas y siempre prevalece el período más largo.

---

### **38. Azure VM Backup**

Las copias de seguridad de una **Azure Virtual Machine** siempre se almacenan en un **Recovery Services Vault**.

No es posible almacenar backups de máquinas virtuales directamente en un **Storage Account**, un **Blob Container** o un **Azure File Share**.

---

### **39. Backup Policy**

Una **Backup Policy** define tanto la **programación** de las copias de seguridad como los **períodos de retención** de los Recovery Points.

Para proteger una máquina virtual con Azure Backup es necesario asociarla a una **Backup Policy**.

---

### **40. File Recovery**

Para restaurar únicamente archivos individuales de una **Azure Virtual Machine**, debe utilizarse la opción **File Recovery** del **Recovery Services Vault**.

Azure descarga un script que monta temporalmente el Recovery Point como una unidad local para copiar los archivos mediante **File Explorer**.

---

### **41. File Recovery vs Restore VM**

La opción **Restore VM** recupera una máquina virtual completa, mientras que **File Recovery** permite restaurar únicamente archivos o carpetas individuales.

Para recuperar unos pocos archivos, **File Recovery** es el método más rápido y recomendado.

---

### **42. Azure Backup Reports - Storage Account**

Si **Azure Backup Reports** exporta información a un **Storage Account**, éste debe encontrarse en la **misma región** que el **Recovery Services Vault**.

Este requisito solo aplica al **Storage Account** utilizado para almacenar los datos exportados.

---

### **43. Azure Backup Reports - Log Analytics Workspace**

El **Log Analytics Workspace** utilizado por **Azure Backup Reports** puede encontrarse en cualquier región.

No es necesario que el **Log Analytics Workspace** esté en la misma región que el **Recovery Services Vault**.

---

### **44. Connection Monitor**

**Connection Monitor** debe crearse en la **misma región** que la máquina virtual desde la que se realizan las comprobaciones de conectividad.

Si existen máquinas virtuales en varias regiones, será necesario crear un **Connection Monitor** independiente para cada región.

---

### **45. Network Security Group - RDP**

Las reglas predeterminadas de un **Network Security Group (NSG)** **no permiten** conexiones **RDP (TCP 3389)** desde Internet.

Para acceder por **Remote Desktop** es necesario crear una regla **Allow** específica con mayor prioridad que **DenyAllInbound**.

### **46. Network Security Group (NSG)**

Un **Network Security Group (NSG)** solo aplica sus reglas cuando está asociado a una **Subnet** o a una **Network Interface (NIC)**.

Un NSG sin asociaciones (**0 subnets, 0 network interfaces**) **no filtra ningún tráfico**, aunque sus reglas estén correctamente configuradas.

---

### **47. Azure Backup - Replace Existing Restore**

La opción **Replace existing** restaura el contenido de los discos de una **Virtual Machine** al estado del **Recovery Point** seleccionado.

Cualquier archivo creado o modificado después del backup deberá recuperarse manualmente, mientras que los cambios de configuración de la VM (como el tamaño o los discos adjuntos) normalmente se conservan.

---

### **48. Azure Policy - Append Tag**

La política **Append a tag and its value to resources** solo agrega el tag a los **nuevos recursos** creados dentro del ámbito de la política.

No modifica los **Resource Groups** existentes, aunque la política esté asignada a nivel de **Subscription**.

---

### **49. Azure Policy - Append Tag a nuevos recursos**

Cuando una política **Append** está asignada a una **Subscription**, cualquier recurso nuevo creado heredará automáticamente el **tag** definido por la política.

Los tags especificados durante la creación del recurso se conservan y Azure simplemente añade el tag que falte.

---

### **50. Network Insights**

**Network Insights** proporciona un panel centralizado para supervisar recursos de red y visualizar la **topología** de una infraestructura de Azure.

A diferencia de **Log Analytics**, **Network Watcher** o las **Data Collection Rules**, ofrece una vista gráfica de la conectividad y dependencias de la red.

### **51. ARM Template - copyIndex()**

La función **copyIndex()** devuelve el índice de cada iteración dentro de un bloque **copy** de un **ARM Template**.

Se utiliza para generar valores únicos, como el **LUN** de varios discos de datos o nombres secuenciales de recursos creados mediante un bucle.

---

### **52. Private Endpoint para Azure Storage**

Si el requisito es que el tráfico entre una **Virtual Machine** y un **Storage Account** permanezca siempre dentro de la red privada de Azure, debe configurarse un **Private Endpoint**.

Un **Private Endpoint** asigna una **Private IP** al servicio dentro de la **Virtual Network**, evitando el uso de Internet.

---

### **53. Acceso a Blob Storage con mínimo privilegio**

Para cargar blobs desde el **Azure Portal**, un usuario necesita el rol **Reader** para visualizar el recurso y **Storage Blob Data Contributor** para leer y escribir datos.

Esta combinación sigue el principio de **mínimo privilegio**, evitando conceder permisos administrativos innecesarios.

---

### **54. Blob Immutability**

Si los blobs no deben modificarse ni eliminarse durante un período determinado, debe configurarse una **Time-based Retention Policy** o un **Legal Hold**.

Las **Access Keys**, los permisos **IAM** o el **Access Tier** no proporcionan protección frente a modificaciones o eliminaciones.

---

### **55. Azure Monitor Alert Scope**

Una **Alert Rule** configurada a nivel de **Subscription** supervisa automáticamente los recursos existentes y los que se creen posteriormente dentro de esa suscripción.

No es necesario modificar la alerta cada vez que se crea una nueva **Resource Group** o un nuevo recurso.

---

### **56. Alert Processing Rules**

Una **Alert Processing Rule** modifica el tratamiento de una alerta después de que ésta se haya generado.

Por ejemplo, una regla de **Suppress notifications** impide el envío de correos o SMS, pero **la alerta sigue existiendo** en Azure Monitor.

---

### **57. Activity Log Alerts**

Las operaciones administrativas, como **crear**, **eliminar** o **añadir Tags** a un recurso, pueden generar alertas utilizando el **Azure Activity Log**.

Estas alertas no dependen de métricas ni de Log Analytics, sino de los eventos registrados por **Azure Resource Manager**.

---

### **58. Recovery Services Vault - ZRS**

Si las copias de seguridad deben almacenarse con redundancia entre **Availability Zones**, el **Recovery Services Vault** debe configurarse con **Zone-Redundant Storage (ZRS)** antes de habilitar el backup.

Una vez que el Vault contiene elementos protegidos, **no es posible cambiar posteriormente el tipo de redundancia**.

---

### **59. Group-Based Licensing**

Cuando una licencia se asigna mediante **Group-Based Licensing**, todos los miembros directos del grupo reciben automáticamente esa licencia.

Para retirar la licencia de un usuario es necesario **eliminarlo del grupo** o **quitar la licencia del grupo**, no basta con retirarla manualmente del usuario.

---

### **60. Group-Based Licensing - Reasignación automática**

Mientras un usuario continúe perteneciendo a un grupo con **Group-Based Licensing**, Microsoft Entra volverá a asignarle automáticamente la licencia si ésta se elimina manualmente.

La pertenencia al grupo tiene prioridad sobre cualquier cambio manual realizado sobre las licencias del usuario.

### **61. Group-Based Licensing y grupos anidados**

El **Group-Based Licensing** de **Microsoft Entra ID** **no admite grupos anidados**. Si un grupo con una licencia asignada contiene otro grupo, los miembros del grupo interno **no heredarán la licencia**.

Solo los **miembros directos** del grupo reciben automáticamente las licencias asignadas al grupo.

---

### **62. Servicios compatibles con contenedores Windows**

Los **contenedores Windows** solo pueden ejecutarse en **Azure App Service** y **Azure Container Instances (ACI)**.

**Azure Container Apps** únicamente admite **contenedores Linux**, por lo que no puede hospedar imágenes basadas en Windows Server.

---

### **63. Azure Monitor - Estado de una alerta cerrada**

El estado (**User response**) de una alerta de **Azure Monitor** puede modificarse en cualquier momento.

Una alerta marcada como **Closed** puede volver a cambiarse a **New** o **Acknowledged** si es necesario reabrir la incidencia.

---

### **64. Azure Monitor - Estado de una alerta nueva**

Una alerta cuyo estado es **New** puede actualizarse a **Acknowledged** o directamente a **Closed**.

El estado de una alerta es únicamente un mecanismo de seguimiento y **no afecta** al funcionamiento de la regla de alerta.

---

### **65. ARM Template - Complete Mode**

Cuando un **ARM Template** se despliega utilizando el modo **Complete**, Azure elimina automáticamente todos los recursos existentes del **Resource Group** que no estén definidos en la plantilla.

El modo **Incremental** conserva los recursos existentes y solo crea o actualiza los definidos en el template.

---

### **66. ARM Template - Incremental Mode**

El modo **Incremental** es el modo de despliegue predeterminado de los **ARM Templates**.

Solo crea o actualiza los recursos definidos en la plantilla y **nunca elimina** recursos existentes del **Resource Group**.

---

### **67. ARM Template - copy**

La función **copy** permite crear múltiples recursos automáticamente dentro de un **ARM Template** mediante un bucle.

Normalmente se utiliza junto con **copyIndex()** para generar nombres o configuraciones diferentes en cada iteración.

---

### **68. ARM Template - Subscription Deployment**

El cmdlet **New-AzSubscriptionDeployment** permite desplegar recursos con ámbito de **Subscription**, como **Resource Groups**, **Policies** o **Role Assignments**.

El parámetro **-Location** indica únicamente dónde se almacenan los metadatos del despliegue, **no la ubicación de los recursos creados**.

---

### **69. ARM Template - Resource Group Location**

La región donde se crea un **Resource Group** viene determinada por la propiedad **location** definida dentro del **ARM Template**.

El parámetro **-Location** utilizado en **New-AzSubscriptionDeployment** **no cambia** la región de los recursos creados.

---

### **70. ARM Template - Recursos existentes**

Cuando un **ARM Template** intenta crear un recurso que ya existe y la plantilla no contempla ese caso, Azure genera un error para ese recurso.

Por ejemplo, si un template intenta crear **RG1** y **RG1** ya existe, esa iteración fallará mientras que el resto de recursos podrán crearse correctamente.

### **71. Azure Policy - Not allowed resource types**

Una **Azure Policy** con efecto **Deny** y tipo **Not allowed resource types** impide crear recursos de los tipos especificados, independientemente de si el despliegue se realiza desde el **Portal**, **Azure CLI**, **PowerShell** o un **ARM Template**.

Si la política bloquea **Microsoft.Compute/virtualMachines**, primero debe eliminarse ese tipo de recurso de la política antes de crear una nueva máquina virtual.

---

### **72. Resource Locks**

Los **Resource Locks** protegen los recursos frente a modificaciones o eliminaciones accidentales mediante los tipos **CanNotDelete** y **ReadOnly**.

Un bloqueo aplicado sobre un **Resource Group** se hereda automáticamente por todos los recursos contenidos en él.

---

### **73. User Access Administrator**

El rol **User Access Administrator** permite administrar las asignaciones de **Azure RBAC**, pero **no** crear, modificar ni eliminar recursos de Azure.

Es el rol de **mínimo privilegio** cuando únicamente se necesita asignar permisos a otros usuarios.

---

### **74. Storage Blob Data Reader**

El rol **Storage Blob Data Reader** permite leer el contenido de los blobs almacenados en un **Storage Account**, pero **no** crear, modificar ni eliminar datos.

Es el rol recomendado cuando un usuario únicamente necesita consultar información almacenada en Blob Storage.

---

### **75. VNet Peering**

Un **VNet Peering** solo puede crearse entre **Virtual Networks cuyos Address Spaces no se solapen**.

Si existe **CIDR Overlap**, primero será necesario modificar el **Address Space** de alguna de las VNets.

---

### **76. Azure CDN**

**Azure CDN** distribuye contenido estático desde ubicaciones cercanas al usuario mediante una red global de **Points of Presence (PoP)**.

Su objetivo es reducir la latencia y acelerar la entrega de contenido, no balancear tráfico entre máquinas virtuales.

---

### **77. Azure Load Balancer**

**Azure Load Balancer** distribuye tráfico de red de nivel **4 (TCP/UDP)** entre varias máquinas virtuales.

No inspecciona el contenido HTTP/HTTPS ni proporciona funcionalidades de **Web Application Firewall (WAF)**.

---

### **78. Azure Application Gateway**

**Azure Application Gateway** es un balanceador de carga de nivel **7 (HTTP/HTTPS)** que admite funcionalidades como **URL-based routing**, **SSL termination** y **Web Application Firewall (WAF)**.

Es la solución recomendada para publicar aplicaciones web.

---

### **79. Azure Traffic Manager**

**Azure Traffic Manager** realiza el balanceo a nivel **DNS**, dirigiendo a los clientes hacia distintos endpoints según criterios como prioridad, rendimiento, geografía o estado.

No distribuye tráfico directamente entre máquinas virtuales de una misma **Virtual Network**.

---

### **80. Azure Monitor Network Insights**

**Network Insights** necesita que estén habilitados los **NSG Flow Logs** para analizar el tráfico de red y detectar comportamientos anómalos.

Sin **NSG Flow Logs**, Azure Monitor no dispone de la información necesaria para generar alertas basadas en tráfico de red.

### **81. NSG Flow Logs**

Para capturar las **IPs origen y destino** de las conexiones que atraviesan una subred, deben habilitarse los **NSG Flow Logs** sobre el **Network Security Group (NSG)** asociado.

Los registros pueden enviarse a un **Log Analytics Workspace**, donde es posible realizar consultas interactivas mediante **Kusto Query Language (KQL)**.

---

### **82. Azure Container Registry - Push de imágenes**

Después de autenticarse en un **Azure Container Registry (ACR)** mediante **Azure CLI**, el siguiente paso es asignar a la imagen un **tag** que incluya el **Login Server** del registro.

Solo las imágenes etiquetadas con el nombre del registro (por ejemplo, `myacr.azurecr.io/app:v1`) pueden publicarse mediante `docker push`.

---

### **83. Azure Container Registry - Push**

El comando **docker push** envía al **Azure Container Registry** la imagen cuyo nombre incluye el **Login Server** del ACR.

Si la imagen no está correctamente etiquetada, Docker no sabrá en qué registro debe almacenarla.

---

### **84. Azure Container Registry - Login Server**

Cada **Azure Container Registry** dispone de un **Login Server** único con formato:

`<nombre>.azurecr.io`

Este nombre debe formar parte del **tag** de la imagen antes de realizar el **push**.

---

### **85. Azure CLI - az acr login**

Antes de enviar imágenes a un **Azure Container Registry**, debe ejecutarse el comando **`az acr login`** para autenticarse en el registro.

La autenticación por sí sola no publica la imagen; todavía es necesario etiquetarla y ejecutar **docker push**.

---

### **86. Docker Tag**

El comando **docker tag** crea una nueva referencia de una imagen Docker sin duplicar su contenido.

Se utiliza para asociar la imagen local con el repositorio del **Azure Container Registry** donde será almacenada.

---

### **87. Docker Push**

El comando **docker push** publica una imagen Docker previamente etiquetada en un registro remoto como **Azure Container Registry**.

Solo se enviarán las capas que todavía no existan en el registro, reduciendo el tráfico de red.

---

### **88. Azure Container Registry Repository**

Un **Repository** de **Azure Container Registry** puede almacenar múltiples versiones de una misma imagen utilizando distintos **tags**.

Cada **tag** identifica una versión concreta de la imagen, como `v1`, `latest` o `prod`.

---

### **89. Azure Container Registry Authentication**

Para poder subir imágenes a un **Azure Container Registry**, el usuario debe tener permisos suficientes, como el rol **AcrPush**.

Disponer únicamente de permisos de lectura (**AcrPull**) no permite publicar nuevas imágenes.

---

### **90. Azure Container Registry**

**Azure Container Registry (ACR)** es un registro privado de imágenes Docker y OCI totalmente administrado por Azure.

Se integra con servicios como **AKS**, **Container Apps**, **App Service** y **Azure Container Instances** para desplegar contenedores de forma segura.
