# Agentic AI - Conceptos básicos

## ¿Qué es un LLM?

**LLM** significa:

**Large Language Model**

En español:

**Modelo Grande de Lenguaje**

Un LLM es un modelo de Inteligencia Artificial entrenado con una gran cantidad de texto para poder:

- Entender preguntas.
- Generar texto.
- Resumir información.
- Escribir código.
- Analizar información.
- Razonar sobre un problema.
- Decidir qué acción realizar.

Ejemplos de LLM:

- GPT
- Claude
- Gemini
- Llama

### Ejemplo

Le preguntamos:

> ¿Cuál es la capital de Francia?

El LLM responde:

> París.

Funcionamiento:

```text
Usuario
   |
   v
  LLM
   |
   v
Respuesta
```

---

## ¿Qué es una Tool?

Una **Tool** es una herramienta que un LLM puede utilizar para realizar acciones externas.

Ejemplos:

- Buscar en Internet.
- Ejecutar código Python.
- Consultar una API.
- Leer un fichero.
- Consultar una base de datos.
- Ejecutar comandos.
- Consultar Azure.

### Ejemplo

Queremos saber el tiempo actual.

```text
Usuario
   |
   v
  LLM
   |
   v
Weather Tool
   |
   v
Resultado
   |
   v
  LLM
   |
   v
Respuesta
```

---

## ¿Qué es un Agent?

Una definición sencilla:

> An Agent is an LLM with tools in a loop to achieve a goal.

En español:

> Un agente es un LLM que utiliza herramientas dentro de un bucle para conseguir un objetivo.

Podemos resumirlo como:

```text
Agent = LLM + Tools + Loop + Goal
```

- **LLM** → decide qué hacer.
- **Tools** → permiten realizar acciones.
- **Loop** → permite repetir acciones.
- **Goal** → es el objetivo que queremos conseguir.
