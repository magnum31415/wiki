# # Entrevista Técnica – Responsable Oracle & Linux 

## Oracle DataGuar

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

## 3. ¿Porque el failover en dataguard no es automatico por defecto?

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

## 4. Diferencias entre:

### Physical Standby y Logical Standby

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


### Switchover y Failover

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

## 5. Si el apply lag empieza a crecer, ¿cómo lo diagnosticas?

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

## 6. ¿Qué harías si el standby está desincronizado por varios días?

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

## 7. Servidor Linux lento pero CPU baja

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

---

## 8. Parámetros importantes del kernel para Oracle en Linux, donde se configurarn y como se aplican 

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

---

## 9. ¿Cómo harías tuning básico en Oracle?

1. Revisar AWR / ADDM
2. Identificar top wait events
3. Analizar SQL más costoso
4. Revisar planes de ejecución
5. Verificar estadísticas
6. Revisar índices
7. Ajustar memoria solo si aplica

Mala señal si responde solo “aumentar memoria”.

---

## 10. ¿Cómo planificarías migración 12c → 19c con Data Guard?

- **Uso de preupgrade tool**
- **Backup completo previo**
- **Rolling upgrade usando standby**: es un método de actualización que permite actualizar una base de datos con mínimo o casi cero downtime, aprovechando una configuración de Oracle Data Guard o entornos RAC.
- **Upgrade primero standby**
- **Switchover**
- **Minimizar downtime**
- **Pruebas previas**

Debe demostrar visión arquitectónica.

---

## 11.  ¿Qué es Oracle Transparent Data Encryption (TDE) y cómo funciona?

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

# Señales de alerta

- No menciona vistas dinámicas
- No habla de SCN
- No diferencia transport lag vs apply lag
- No conoce RMAN incremental from SCN
- No menciona Broker para failover automático
