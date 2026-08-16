# Responsible AI – Six Principles

Microsoft define **seis principios fundamentales de Responsible AI** que deben guiar el diseño, desarrollo, implementación y uso de soluciones de Inteligencia Artificial.

---

## 1. Fairness

**Fairness** significa que los sistemas de IA deben tratar a las personas de forma **justa y equitativa**, evitando discriminaciones o sesgos injustificados.

Los modelos de IA aprenden a partir de datos. Si los datos utilizados para entrenarlos contienen sesgos históricos o determinados grupos están poco representados, el modelo puede reproducir o incluso amplificar esos sesgos.

### Puntos clave

* Utilizar datos de entrenamiento representativos.
* Identificar y reducir posibles **bias** (sesgos).
* Evaluar el comportamiento del modelo para diferentes grupos.
* Evitar que características personales provoquen discriminación.
* Monitorizar el modelo después de desplegarlo.

### Ejemplo

Un sistema de IA utilizado para conceder préstamos no debería ofrecer sistemáticamente peores condiciones a determinados grupos de personas debido a su género, edad u origen.

> **Idea clave:** Fairness → evitar **bias** y discriminación.

---

## 2. Reliability and Safety

**Reliability and Safety** significa que un sistema de IA debe funcionar de manera **fiable, consistente y segura**, incluso ante situaciones inesperadas.

El sistema debe probarse antes de llegar a producción y continuar siendo monitorizado durante todo su ciclo de vida.

### Puntos clave

* Probar el sistema en condiciones normales y excepcionales.
* Conocer las limitaciones del modelo.
* Monitorizar su funcionamiento.
* Detectar posibles fallos.
* Implementar mecanismos de recuperación o **fallback**.
* Evaluar los riesgos antes del despliegue.
* Incorporar supervisión humana cuando el riesgo lo requiera.

### Ejemplo

Una IA utilizada para ayudar en un diagnóstico médico debe estar ampliamente probada y disponer de mecanismos de seguridad y supervisión humana.

> **Idea clave:** Reliability and Safety → evitar **fallos o comportamientos peligrosos**.

---

## 3. Privacy and Security

**Privacy and Security** significa que los sistemas de IA deben **proteger los datos de los usuarios y evitar accesos o usos no autorizados**.

Esto es especialmente importante porque los sistemas de IA pueden procesar grandes cantidades de información personal o sensible.

### Puntos clave

* Proteger información personal y sensible.
* Cifrar los datos cuando sea necesario.
* Implementar autenticación y autorización.
* Aplicar **least privilege**.
* Recoger únicamente los datos necesarios.
* Cumplir las regulaciones de privacidad.
* Proteger modelos, infraestructura y datos frente a ataques.

La privacidad debe considerarse durante todo el ciclo de vida:

**Data collection → Training → Deployment → Storage → Deletion**

### Ejemplo

Una aplicación de IA que procesa historiales médicos debe garantizar que únicamente usuarios autorizados puedan acceder a esa información.

> **Idea clave:** Privacy and Security → proteger **datos, usuarios y sistemas**.

---

## 4. Inclusiveness

**Inclusiveness** significa diseñar sistemas de IA que puedan ser utilizados y aprovechados por **personas con diferentes capacidades, circunstancias y características**.

El objetivo es evitar que determinados usuarios queden excluidos por el diseño de la solución.

### Puntos clave

* Considerar personas con discapacidades.
* Diseñar interfaces accesibles.
* Considerar diferentes idiomas y contextos.
* Probar la solución con usuarios diversos.
* Detectar y eliminar barreras de acceso.

### Ejemplo

Un sistema de reconocimiento de voz debería funcionar adecuadamente con diferentes acentos y patrones de habla.

Una aplicación también debería considerar mecanismos de accesibilidad para personas con dificultades visuales.

> **Idea clave:** Inclusiveness → evitar **exclusión**.

---

## 5. Transparency

**Transparency** significa que los usuarios deben poder **entender cuándo están interactuando con una IA y comprender suficientemente cómo funciona o cómo llega a determinadas decisiones**.

También deben conocer sus capacidades y limitaciones.

### Puntos clave

* Informar al usuario cuando está interactuando con una IA.
* Explicar decisiones importantes cuando sea posible.
* Documentar cómo funciona el sistema.
* Comunicar las limitaciones conocidas.
* Explicar qué factores influyen en las decisiones.
* No presentar resultados inciertos como hechos indiscutibles.

Transparency está estrechamente relacionada con **Explainability**.

### Ejemplo

Si una IA rechaza una solicitud de crédito, debería ser posible proporcionar información comprensible sobre qué factores han influido en la decisión.

> **Idea clave:** Transparency → poder **entender la IA y sus decisiones**.

---

## 6. Accountability

**Accountability** significa que las **personas y organizaciones siguen siendo responsables** de los sistemas de IA que desarrollan y utilizan.

La responsabilidad de una decisión no puede trasladarse simplemente al algoritmo.

### Puntos clave

* Definir responsabilidades.
* Establecer mecanismos de governance.
* Mantener supervisión humana.
* Realizar evaluaciones de riesgo.
* Auditar los sistemas de IA.
* Monitorizar los sistemas después del despliegue.
* Permitir revisar o corregir decisiones.
* Actuar cuando se detecten problemas.

### Ejemplo

Si una IA toma una decisión incorrecta que perjudica a un cliente, la organización que utiliza el sistema continúa siendo responsable de investigar y solucionar el problema.

> **Idea clave:** Accountability → siempre debe existir un **responsable humano u organizativo**.

---

# Resumen para el examen

| Principle                  | Concepto clave        | Pregunta para identificarlo                                 |
| -------------------------- | --------------------- | ----------------------------------------------------------- |
| **Fairness**               | Bias / discriminación | ¿La IA trata justamente a diferentes grupos?                |
| **Reliability and Safety** | Funcionamiento seguro | ¿La IA funciona correctamente y de forma segura?            |
| **Privacy and Security**   | Protección de datos   | ¿Los datos y el sistema están protegidos?                   |
| **Inclusiveness**          | Accesibilidad         | ¿La solución puede ser utilizada por todo tipo de personas? |
| **Transparency**           | Explainability        | ¿Entendemos que se utiliza IA y cómo toma decisiones?       |
| **Accountability**         | Responsabilidad       | ¿Quién responde por las decisiones de la IA?                |

## Regla rápida para memorizar

```text
Fairness                → Bias
Reliability and Safety  → Failure
Privacy and Security    → Data
Inclusiveness           → Exclusion
Transparency            → Explain
Accountability          → Responsibility
```

Para preguntas tipo examen, intenta localizar primero **el problema principal del escenario**:

* Si habla de **sesgo o discriminación** → **Fairness**
* Si habla de **errores, riesgos o funcionamiento seguro** → **Reliability and Safety**
* Si habla de **datos personales, acceso o protección** → **Privacy and Security**
* Si habla de **discapacidad o accesibilidad** → **Inclusiveness**
* Si habla de **explicar una decisión** → **Transparency**
* Si habla de **quién es responsable** → **Accountability**
