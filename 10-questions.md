# 10-questions


## Oracle DataGuar

### ¿Desde el punto de vista técnico y de continuidad de servicio qué aporta Oracle Data Guard?

- Que Data Guard replica una base de datos primaria a una standby.
- Que proporciona Alta Disponibilidad (HA) ante fallos del servidor primario.
- Que es una solución de Disaster Recovery (DR) ante caída del site completo.
- Que permite minimizar RPO y RTO.
- Que no es un cluster activo-activo.
-´Que se puede automatizar el failover.



### RPO / RTO

| Modo de Protección        | Tipo de Transporte | RPO (Recovery Point Objective) | RTO (Recovery Time Objective) | Comentarios Clave |
|---------------------------|-------------------|----------------------------------|--------------------------------|-------------------|
| Maximum Protection        | SYNC              | 0 (cero pérdida de datos)        | Bajo (minutos)                 | Si el standby no está disponible, el primary se detiene. Máxima seguridad. |
| Maximum Availability      | SYNC (con fallback a ASYNC) | 0 en condiciones normales | Bajo (minutos) | Si el standby falla, puede degradar a ASYNC para mantener disponibilidad. |
| Maximum Performance       | ASYNC             | > 0 (posible pérdida de segundos/minutos) | Bajo–Medio (minutos) | Modo más usado en producción. Prioriza rendimiento sobre RPO cero. |


### ¿Porque el failover en dataguard no es automatico?

Sí puede ser automático, pero no lo es por defecto.

** Por defecto NO es automático**
Oracle Data Guard estándar requiere:
- Detección manual del fallo.
- Decisión humana.
- Ejecución manual del failover.
- 
¿Por qué?  Porque Oracle prioriza:
- Evitar split-brain.
- Evitar activaciones incorrectas.
- Evitar pérdida innecesaria de datos.
- 
Un failover mal ejecutado puede provocar:
- Divergencia de bases.
- Pérdida de datos.
- Problemas de reintegración.
