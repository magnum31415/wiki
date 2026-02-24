# Entrevista Técnica – Responsable Oracle & Linux 

# 📑 Índice – Entrevista Técnica Responsable Oracle & Linux

## 🖥️ Unix

1. [Parámetros importantes del kernel para Oracle en Linux](#1-parámetros-importantes-del-kernel-para-oracle-en-linux-donde-se-configurarn-y-como-se-aplican)
2. [Diferencias entre Suse y Redhat](#2-diferencias-entre-suse-y-redhat)
3. [Borrar un fichero en uso por un proceso](#3-si-borras-un-fichero-que-esta-en-uso-por-un-proceso-que-ocurre-realmente)
4. [Servidor Linux lento con CPU baja](#4-un-servidor-linux-va-lento-pero-su-cpu-es-baja-que-revisarias)

---

## 🗄️ Oracle

1. [¿Qué ocurre en un COMMIT en Oracle?](#1-qué-ocurre-en-un-commit-en-oracle)
2. [Oracle Transparent Data Encryption (TDE)](#2-qué-es-oracle-transparent-data-encryption-tde-y-cómo-funciona)
3. [Tuning básico en Oracle](#3-cómo-harías-tuning-básico-en-oracle)

---

## 🔁 Oracle Data Guard

1. [Qué aporta Oracle Data Guard (HA / DR)](#1-desde-el-punto-de-vista-técnico-y-de-continuidad-de-servicio-qué-aporta-oracle-data-guard)
2. [Modos de protección y RPO/RTO](#2-cuales-son-los-modos-de-protección-de-dataguard-y-su-rpo--rto)
3. [¿Por qué el failover no es automático por defecto?](#3-porque-el-failover-en-dataguard-no-es-automatico-por-defecto)
4. [Physical vs Logical Standby](#physical-standby-y-logical-standby)
5. [Switchover vs Failover](#switchover-y-failover)
6. [Diagnóstico de Apply Lag](#5-si-el-apply-lag-empieza-a-crecer-cómo-lo-diagnosticas)
7. [Standby desincronizado varios días](#6-qué-harías-si-el-standby-está-desincronizado-por-varios-días)
8. [Migración 12c → 19c con Data Guard](#7-cómo-planificarías-migración-12c--19c-con-data-guard)
9. [Estrategia de migración de datacenter con Data Guard y mínimo downtime](#8-actualmente-tenemos-una-base-de-datos-productiva-que-ya-funciona-con-oracle-data-guard-primary--standby-vamos-a-migrar-a-un-nuevo-proveedor-de-datacenterqué-estrategia-plantearías-para-hacer-la-migración-con-mínimo-downtime-y-mínimo-riesgo-manteniendo-alta-disponibilidad-durante-el-proceso)
---

## 🌐 Subnetting

1. [Hosts válidos en una red /27](#1-cuántos-hosts-válidos-hay-en-una-red-27)
2. [Dividir 192.168.1.0/24 en 4 subredes](#2-tienes-la-red-1921681024-si-necesitas-4-subredes-iguales-qué-máscara-usarías-y-cuántos-hosts-tendría-cada-una)


## 🔧 Automatización

1. [Despliegue y mantenimiento automatizado de agente en 500 servidores](#1-tenemos-que-desplegar-y-mantener-actualizado-un-agente-en-500-servidores-linux-productivos-el-proceso-debe-ser-automatizado-seguro-auditable-y-con-mínimo-riesgo-cómo-lo-diseñarías-de-extremo-a-extremo)


## Unix

### 1. Parámetros importantes del kernel para Oracle en Linux, donde se configurarn y como se aplican 

- kernel.shmmax
- kernel.shmall
- kernel.sem
- fs.file-max
- ulimit -n
- Transparent HugePages deshabilitado
- swappiness bajo
- noatime en filesystem

Estos se configuran en: ``/etc/sysctl.conf o /etc/sysctl.d/99-oracle.conf``
Se aplican con: ``sysctl -p o sysctl -p /etc/sysctl.d/99-oracle.conf``

Debe entender shared memory y SGA.


### 2. Diferencias entre Suse y Redhat

**gestor de paquetes**

- **RHEL** → yum / dnf (RPM)
- **SUSE** → zypper (RPM también)

**Herramientas de administración**
- **RHEL** → systemctl, nmcli, firewalld
- **SUSE** → YaST (herramienta gráfica/CLI potente)

**Gestión de parches**
- **RHEL** → Red Hat Satellite
- **SUSE** → SUSE Manager

---

### 3. Si borras un fichero que esta en uso por un proceso que ocurre realmente

- Se elimina la entrada en el filesystem (inode link).
- Pero si un proceso tiene el fichero abierto…
- El kernel mantiene el inode activo.

Cómo detectarlo: ``lsof | grep deleted``
Cómo liberar el espacio: ``> /proc/<PID>/fd/<FD>``

---

### 4. Un servidor Linux va lento pero su CPU es baja que revisarias?

Revisar:

- `top`
- `vmstat`
- `iostat`
- I/O wait
- Memoria (`free -m`)
- Swapping
- Load average
- Procesos bloqueados
- Locks en base

Debe entender que CPU baja ≠ servidor sano.


## Oracle

### 1. Qué ocurre en un COMMIT en Oracle?
Un COMMIT en Oracle no escribe directamente en los datafiles.
Lo que garantiza es la durabilidad de la transacción escribiendo primero el redo en disco.

#### Flujo interno de un COMMIT**
1. Se genera un commit record.
2. Se asigna un SCN (System Change Number).
3. El contenido del Redo Log Buffer (en la SGA) se prepara para escritura.
4. El proceso LGWR (Log Writer) hace flush hacia los Online Redo Logs.
5. Cuando el redo está físicamente en disco → Oracle devuelve “Commit complete”.

⚠️ El commit no espera a que los datafiles se escriban.

####  Procesos involucrados

**🔹 Server Process**

- Ejecuta la transacción.
- Genera redo.
- Solicita el commit.

**🔹 LGWR (Log Writer)**

- Escribe el Redo Log Buffer en los Online Redo Logs.
- Es el proceso crítico del commit.
- Sin escritura exitosa → no hay confirmación.

**🔹 DBWR (Database Writer)**

- Escribe buffers sucios a datafiles.
- No participa directamente en el commit.
- Puede escribir tiempo después.


---

### 2. ¿Qué es Oracle Transparent Data Encryption (TDE) y cómo funciona?

### Respuesta esperada:

TDE permite cifrar datos en reposo sin modificar la aplicación.

- Column-level encryption
- Tablespace encryption
- Uso de Wallet / Keystore
- Master Encryption Key
- Protege contra robo de discos o backups

Debe diferenciar:
- Cifrado en reposo
- Cifrado en tránsito (TLS)

Pregunta de seguimiento:
¿Qué ocurre si se pierde el wallet?

Respuesta correcta:
No se pueden descifrar los datos cifrados ni abrir la base correctamente.


---

### 3. ¿Cómo harías tuning básico en Oracle?

1. Revisar AWR / ADDM
2. Identificar top wait events
3. Analizar SQL más costoso
4. Revisar planes de ejecución
5. Verificar estadísticas
6. Revisar índices
7. Ajustar memoria solo si aplica

Mala señal si responde solo “aumentar memoria”.




---
## Oracle DataGuard

### 1. ¿Desde el punto de vista técnico y de continuidad de servicio qué aporta Oracle Data Guard?

- Que Data Guard replica una base de datos primaria a una standby.
- Que proporciona Alta Disponibilidad (HA) ante fallos del servidor primario.
- Que es una solución de Disaster Recovery (DR) ante caída del site completo.
- Que permite minimizar RPO y RTO.
- Que no es un cluster activo-activo.
-´Que se puede automatizar el failover.

---

### 2. ¿Cuales son los modos de protección de DataGuard y su RPO / RTO?

| Modo de Protección        | Tipo de Transporte | RPO (Recovery Point Objective) | RTO (Recovery Time Objective) | Comentarios Clave |
|---------------------------|-------------------|----------------------------------|--------------------------------|-------------------|
| Maximum Protection        | SYNC              | 0 (cero pérdida de datos)        | Bajo (minutos)                 | Si standby falla → primary se detiene. Máxima seguridad. |
| Maximum Availability      | SYNC (con fallback a ASYNC) | 0 en condiciones normales | Bajo (minutos) | Si el standby falla, puede degradar a ASYNC para mantener disponibilidad. |
| Maximum Performance       | ASYNC             | > 0 (posible pérdida de segundos/minutos) | Bajo–Medio (minutos) | Modo más usado en producción. Prioriza rendimiento sobre RPO cero. |

Para entorno financiero: Maximum Availability o Maximum Protection.

---

### 3. ¿Porque el failover en dataguard no es automatico por defecto?

Oracle Data Guard en configuración básica no incluye mecanismo automático de decisión porque:

- Se prioriza la integridad de datos.
- Se debe evitar split-brain.
- Se necesita validar que no sea un problema de red.

El failover automático requiere:
- **Data Guard Broker** : Es una herramienta de administración y automatización de Data Guard.
- **Fast-Start Failover (FSFO)** : Es una funcionalidad específica dentro del Broker que permite:
  - Failover automático
  - Sin intervención humana
  - Basado en reglas y monitoreo continuo
  - Requiere un Observer
- **Observer configurado**

---

### 4. Diferencias entre:

#### Physical Standby y Logical Standby

- **Physical Standby:**
  - Replica a nivel de bloques
  - Usa Redo Apply 
  - Copia exacta
  - Mejor rendimiento 
  - Compatible con Active Data Guard

- **Logical Standby:**
  - Replica a nivel SQL
  - Usa SQL Apply
  - Permite reporting más flexible
  - Más limitaciones de objetos

Debe indicar que Physical es más común en entornos críticos.


| Característica | Physical Standby | Logical Standby |
|---------------|------------------|-----------------|
| Tipo de replicación | A nivel de bloques (Redo Apply) | A nivel SQL (SQL Apply) |
| Tipo de copia | Copia binaria exacta del Primary | Estructura lógica equivalente |
| Aplicación de redo | Media Recovery (MRP) | Conversión redo → sentencias SQL |
| Rendimiento | Más eficiente y estable | Mayor consumo de CPU |
| Uso para reporting | Solo lectura (Active Data Guard) | Lectura y objetos adicionales |
| Cambios estructurales independientes | No permitido | Permitido en ciertos casos |
| Soporte de tipos de objetos | Completo | Algunas limitaciones |
| Complejidad de administración | Más simple | Más compleja |
| Uso típico | Alta disponibilidad y DR crítico | Reporting avanzado o upgrades |
| Compatibilidad rolling upgrade | Más limitado | Más flexible |
| Riesgo de inconsistencias | Muy bajo | Mayor si hay objetos no soportados |
| Recomendado para entornos críticos | Sí | Solo en casos específicos |


#### Switchover y Failover

- **Switchover**: Es un **cambio de roles planificado y controlado** entre el Primary y el Standby.
  - El Primary pasa a ser Standby.
  - El Standby pasa a ser Primary.
  - No hay pérdida de datos.
  - Ambos servidores están operativos.
  - Es una operación reversible.

- **Failover**: Es un **cambio de roles forzado debido a la caída del Primary**.
  - El Primary no está disponible.
  - Se promueve el Standby a Primary.
  - Puede haber pérdida de datos.
  - No siempre es reversible automáticamente.


| Característica | Switchover | Failover |
|---------------|------------|----------|
| Planificado | Sí | No |
| Primary disponible | Sí | No |
| Pérdida de datos | No | Puede haber |
| Reversible fácilmente | Sí | No siempre |
| Uso típico | Mantenimiento | Emergencia |

---

### 5. Si el apply lag empieza a crecer, ¿cómo lo diagnosticas?

- Revisar `v$dataguard_stats`
- Revisar `v$managed_standby`
- Diferenciar transport lag vs apply lag
  - **transport lag** : retraso en el envío de redo desde el primary hacia el standby. El primary genera redo, pero todavía no ha llegado al standby.
     -  **Dónde ocurre el problema**: Entre los servidores (red).
     -  **Causas típicas**
        - Latencia o pérdida de red
        - Problemas en LOG_ARCHIVE_DEST
        - Saturación del network
        - Firewall / DNS
        - Standby caído o sin listener
     - **Cómo verlo** ``SELECT NAME, VALUE FROM v$dataguard_stats;``
  - **apply lag** : retraso en aplicar el redo que YA llegó al standby.   El redo está en el standby, pero aún no se ha aplicado a los datafiles.
    - **Dónde ocurre el problema**: En el servidor standby.
    - **Causas típicas**: 
      - I/O lento
      - CPU saturado
      - Datafiles lentos 
      - Bloqueos internos
      - Standby muy cargado (reporting pesado)
      - Redo logs pequeños generando mucha rotación
    - **Cómo verlo** ``SELECT NAME, VALUE FROM v$dataguard_stats; SELECT PROCESS, STATUS FROM v$managed_standby;``
- Revisar red
- Revisar I/O en standby
- Revisar CPU
- Verificar tamaño de redo logs

Debe saber distinguir problema de transporte vs problema de aplicación.

---

### 6. ¿Qué harías si el standby está desincronizado por varios días?

- El desfase de redo es enorme.
- Aplicar todos los archivelogs tardaría demasiado.
- O incluso algunos archivelogs ya no existen.

Opciones:

1. **Incremental backup from SCN con RMAN** : Es un método para re-sincronizar el standby sin reconstruirlo completo, usando un backup incremental desde el SCN donde el standby se quedó. En lugar de enviar miles de archivelogs: Se envían solo los bloques que cambiaron desde un SCN específico.
  - STANDBY: Identificar el SCN del standby: ``SELECT CURRENT_SCN FROM V$DATABASE;``
  - PRIMARY: rear backup incremental desde ese SCN:  ``RMAN> BACKUP INCREMENTAL FROM SCN <scn_standby> DATABASE;`` Esto genera un backup con solo los bloques modificados desde ese SCN.
  - Transferir el backup al standby
  - Aplicar el backup en el standby: Montar la base en modo MOUNT y aplicar el incremental con RMAN.
  - Reiniciar managed recovery: ``ALTER DATABASE RECOVER MANAGED STANDBY DATABASE USING CURRENT LOGFILE DISCONNECT;``
   
2. **Flashback Database (si está habilitado)**
  - **El standby solo estuvo caído**: Estuvo apagado varios días Pero no hizo nada incorrecto y Solo dejó de aplicar redo, **Flashback NO sirve aquí**.
  - **El standby se desincronizó por divergencia**: (El standby estuvo aislado por red, Se hizo un failover,El antiguo primary siguió generando cambios, Ahora tienes dos líneas de tiempo distintas (divergencia de SCN). Aquí puedes usar Flashback Database para: Retroceder el standby a un SCN anterior a la divergencia. Volverlo al punto común con el primary. Reanudar sincronización.
3. **Rebuild completo del standby**: Borrar el standby actual y recrearlo desde cero usando un backup del primary.

Debe mencionar:
- Identificar SCN actual
- Evaluar tamaño del desfase
- Minimizar impacto

---
### 7. ¿Cómo planificarías migración 12c → 19c con Data Guard?

- **Uso de preupgrade tool**
- **Backup completo previo**
- **Rolling upgrade usando standby**: es un método de actualización que permite actualizar una base de datos con mínimo o casi cero downtime, aprovechando una configuración de Oracle Data Guard o entornos RAC.
- **Upgrade primero standby**
- **Switchover**
- **Minimizar downtime**
- **Pruebas previas**

Debe demostrar visión arquitectónica.

---

### 8. Actualmente tenemos una base de datos productiva que ya funciona con Oracle Data Guard (Primary + Standby). Vamos a migrar a un nuevo proveedor de datacenter.¿Qué estrategia plantearías para hacer la migración con mínimo downtime y mínimo riesgo, manteniendo alta disponibilidad durante el proceso?

**Supongamos escenario actual:**
- DC1 → Primary
- DC2 → Standby
Y ahora quieres ir a:
- DC3 (nuevo proveedor)


``Añadiría el nuevo datacenter como standby adicional, sincronizaría completamente, validaría su estado y luego haría un switchover planificado hacia el nuevo site. Mantendría el antiguo primary como standby durante un periodo de transición para garantizar rollback.``

**1. Crear nuevo Standby en el nuevo datacenter (DC3)**

- Montar infraestructura nueva.
- Instalar Oracle misma versión/parches.
- Añadir DC3 como nuevo standby adicional.

Es decir, pasar temporalmente a:
- DC1 → Primary
- DC2 → Standby
- DC3 → Standby

Aquí debe mencionar:

- Posibilidad de múltiples standbys.
- Ajustar LOG_ARCHIVE_DEST.
- Validar red y latencia.
- Verificar apply lag.

Si no sabe que Data Guard permite múltiples standbys → nivel medio-bajo.

**2. Validar sincronización completa en DC3**

- Lag = 0.
- Pruebas de apertura read-only.
- Validar backups en nuevo site.
- Validar rendimiento.

**3. Switchover hacia el nuevo datacenter**

Cuando DC3 está 100% sincronizado:
- Switchover planificado.
- DC3 pasa a ser Primary.
- DC1 o DC2 pueden quedar como standby.
Sin pérdida de datos.

**4. Mantener el antiguo site como standby temporal**

Muy importante:
- No desmontar inmediatamente DC1.
- Mantenerlo como standby durante periodo de estabilización.
- Permite rollback rápido con switchover inverso.


---

## Subnetting

### 1. ¿Cuántos hosts válidos hay en una red /27?
- 32 - 27 = 5 bits para hosts
- 2⁵ = 32 direcciones
- Hosts válidos = 32 - 2 = 30

| Tipo               | Dirección    |
| ------------------ | ------------ |
| Dirección de red   | 192.168.1.0  |
| Primer host válido | 192.168.1.1  |
| Último host válido | 192.168.1.30 |
| Broadcast          | 192.168.1.31 |


### 2. Tienes la red 192.168.1.0/24. Si necesitas 4 subredes iguales, ¿qué máscara usarías y cuántos hosts tendría cada una?

**¿Qué significa /24?** : 
Un prefijo **/24** indica:
- 24 bits para red
- 32 - 24 = **8 bits para hosts**

Direcciones totales: 2⁸ = 256 direcciones

Rango original: 192.168.1.0 – 192.168.1.255

**¿Cuántos bits necesito para 4 subredes?**

- Queremos 4 subredes.
- Buscamos un número n tal que:
- 2ⁿ = 4
Eso ocurre con: n = 2 👉 Necesitamos tomar **2 bits** del campo de hosts.

**Nueva máscara** 

- Si antes era:  /24
- Y tomamos 2 bits más: /24 + 2 = /26

✅ Nueva máscara: **/26**  y en formato decimal: 255.255.255.192

**¿Cuántos hosts por subred?**

Ahora quedan: 32 - 26 = 6 bits para hosts
Direcciones totales por subred: 2⁶ = 64 direcciones
Hosts válidos: 64 - 2 = 62 hosts (Se restan la dirección de red y broadcast)

**Subredes resultantes**

- En /26 los saltos son de 64 direcciones.

Las 4 subredes son:
1. 192.168.1.0/26     → 0 – 63  
2. 192.168.1.64/26    → 64 – 127  
3. 192.168.1.128/26   → 128 – 191  
4. 192.168.1.192/26   → 192 – 255  

 **Respuesta final**

- Máscara: **/26 (255.255.255.192)**
- Hosts por subred: **62**
- Total subredes creadas: **4**


---

## Subnetting

### 1. ¿Cuántos hosts válidos hay en una red /27?
- 32 - 27 = 5 bits para hosts
- 2⁵ = 32 direcciones
- Hosts válidos = 32 - 2 = 30

| Tipo               | Dirección    |
| ------------------ | ------------ |
| Dirección de red   | 192.168.1.0  |
| Primer host válido | 192.168.1.1  |
| Último host válido | 192.168.1.30 |
| Broadcast          | 192.168.1.31 |


### 2. Tienes la red 192.168.1.0/24. Si necesitas 4 subredes iguales, ¿qué máscara usarías y cuántos hosts tendría cada una?

**¿Qué significa /24?** : 
Un prefijo **/24** indica:
- 24 bits para red
- 32 - 24 = **8 bits para hosts**

Direcciones totales: 2⁸ = 256 direcciones

Rango original: 192.168.1.0 – 192.168.1.255

**¿Cuántos bits necesito para 4 subredes?**

- Queremos 4 subredes.
- Buscamos un número n tal que:
- 2ⁿ = 4
Eso ocurre con: n = 2 👉 Necesitamos tomar **2 bits** del campo de hosts.

**Nueva máscara** 

- Si antes era:  /24
- Y tomamos 2 bits más: /24 + 2 = /26

✅ Nueva máscara: **/26**  y en formato decimal: 255.255.255.192

**¿Cuántos hosts por subred?**

Ahora quedan: 32 - 26 = 6 bits para hosts
Direcciones totales por subred: 2⁶ = 64 direcciones
Hosts válidos: 64 - 2 = 62 hosts (Se restan la dirección de red y broadcast)

**Subredes resultantes**

- En /26 los saltos son de 64 direcciones.

Las 4 subredes son:
1. 192.168.1.0/26     → 0 – 63  
2. 192.168.1.64/26    → 64 – 127  
3. 192.168.1.128/26   → 128 – 191  
4. 192.168.1.192/26   → 192 – 255  

 **Respuesta final**

- Máscara: **/26 (255.255.255.192)**
- Hosts por subred: **62**
- Total subredes creadas: **4**


---

## Automatización 

### 1. Tenemos que desplegar y mantener actualizado un agente en 500 servidores Linux productivos. El proceso debe ser automatizado, seguro, auditable y con mínimo riesgo. ¿Cómo lo diseñarías de extremo a extremo?


- Infraestructura como código
- Versionado en Git
- Entornos separados (dev/pre/prod)
- Despliegue progresivo (canary)
- Control de fallos
- Validación automática
- Gestión segura de secretos
- Idempotencia
- Monitoreo post-deploy
- Plan de rollback


- Objetivo: que el playbook sea idempotente, seguro, auditable y operable.

Role oracledb_exporter con:
- instalación binaria (descarga + checksum) o paquete interno
- usuario/grupo dedicados
- directorios /opt/oracledb_exporter, /etc/oracledb_exporter, /var/lib/oracledb_exporter
- config + credenciales (Vault)
- unit file systemd
- healthcheck
- Inventario por entornos (dev/pre/prod) y por site
- Variables en group_vars y host_vars
- Secretos: DSN / password con ansible-vault (o integración con Vault corporativo)
- Rollout: canary + batches con serial, max_fail_percentage, strategy: free si quieres velocidad
- Objetivo: que el playbook

Observabilidad del despliegue:
salida a log
verificación de puerto /metrics
handlers para restart solo si cambia config/binary


---


