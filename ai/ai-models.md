

# Principales modelos, proveedores y plataformas de IA

## Resumen rápido

| Empresa / Producto | Tipo | Pequeño / rápido / barato | Medio | Grande / potente | Explicación simple |
|---|---|---|---|---|---|
| **OpenAI** | Creador de modelos | GPT Nano | GPT Mini | GPT / modelos grandes | Crea los modelos GPT |
| **Anthropic** | Creador de modelos | Claude Haiku | Claude Sonnet | Claude Opus | Crea la familia Claude |
| **Google** | Creador de modelos | Gemini Flash-Lite | Gemini Flash | Gemini Pro | Crea la familia Gemini |
| **DeepSeek** | Creador de modelos | Depende de la generación | Depende de la generación | DeepSeek V-series / R-series | Crea modelos, muchos con pesos abiertos |
| **Groq** | Proveedor de inferencia | Depende del modelo | Depende del modelo | Depende del modelo | Ejecuta modelos de otros fabricantes extremadamente rápido |
| **Ollama** | Software local | Modelos pequeños | Modelos medianos | Modelos grandes si tu hardware puede | Permite ejecutar modelos en tu propio ordenador |
| **OpenRouter** | Router / API Gateway de LLMs | Cualquier modelo | Cualquier modelo | Cualquier modelo | Una API para acceder a muchos proveedores |

---

# OpenAI

OpenAI es la empresa que desarrolla los modelos **GPT**.

La idea general es disponer de modelos de distintos tamaños:

```text
Small / Nano
      ↓
Medium / Mini
      ↓
Large
```

Simplificando:

| Tamaño | Características | Uso típico |
|---|---|---|
| **Nano** | Muy rápido y barato | Clasificación, extracción de datos, tareas simples |
| **Mini** | Equilibrio coste/rendimiento | Agents, automatización, programación |
| **Large** | Mayor capacidad | Razonamiento complejo, análisis avanzado |

Ejemplo:

```text
Necesito comprobar si CPU > 90%
              ↓
           Nano
```

Pero:

```text
Analiza métricas + logs + procesos
y determina la causa raíz
              ↓
        Modelo grande
```

Los nombres concretos de los modelos de OpenAI cambian con bastante frecuencia, por lo que es mejor pensar en:

```text
Nano → barato / rápido

Mini → equilibrio

Large → más inteligente / más caro
```

---

# Anthropic

Anthropic desarrolla todos sus modelos bajo el nombre:

# Claude

Su clasificación tradicional es especialmente fácil de entender:

| Modelo | Tamaño | Características |
|---|---|---|
| **Claude Haiku** | Small | Rápido y barato |
| **Claude Sonnet** | Medium | Equilibrio entre capacidad, precio y velocidad |
| **Claude Opus** | Large | Modelo de mayor capacidad |

Visualmente:

```text
Haiku
  ↓
Sonnet
  ↓
Opus
```

O:

```text
Cheap / Fast
     ↓
   Haiku

Balanced
     ↓
   Sonnet

Powerful
     ↓
    Opus
```

Para construir Agents, **Sonnet suele representar el tipo de modelo "equilibrado"** que usarías para muchas tareas.

---

# Google

Google desarrolla la familia:

# Gemini

Simplificando sus gamas:

| Modelo | Tamaño aproximado | Características |
|---|---|---|
| **Gemini Flash-Lite** | Small | Muy barato y rápido |
| **Gemini Flash** | Medium | Rápido y bastante capaz |
| **Gemini Pro** | Large | Mayor capacidad y razonamiento |

Visualmente:

```text
Flash-Lite
     ↓
   Flash
     ↓
    Pro
```

Ejemplo:

```text
clasificar 100.000 logs
        ↓
   Flash-Lite
```

frente a:

```text
analizar un incidente complejo
con logs, métricas y documentación
        ↓
       Pro
```

---

# DeepSeek

**DeepSeek** es una empresa china de IA que desarrolla sus propios modelos.

Ha ganado mucha popularidad especialmente por publicar modelos con **open weights**.

Tiene diferentes familias de modelos.

Conceptualmente puedes pensar en dos grandes tipos:

```text
DeepSeek V-series
        ↓
modelo general

DeepSeek R-series
        ↓
modelo especializado en reasoning
```

Por ejemplo:

```text
DeepSeek
   |
   ├── V models
   │      ↓
   │   General purpose
   │
   └── R models
          ↓
       Reasoning
```

No conviene memorizar algo como:

```text
DeepSeek Small
DeepSeek Medium
DeepSeek Large
```

porque su nomenclatura no sigue exactamente el esquema:

```text
Nano
Mini
Large
```

de otros fabricantes.

Además, los nombres y versiones de DeepSeek evolucionan rápidamente.

---

# Groq

**Groq no es principalmente una familia de modelos como GPT, Claude o Gemini.**

Groq es principalmente un:

> **Inference Provider**

Es decir:

```text
Modelo
   ↓
Groq infrastructure
   ↓
Respuesta extremadamente rápida
```

Groq permite ejecutar modelos de distintos fabricantes y proyectos open source.

Por ejemplo:

```text
Llama
  |
  ↓
Groq
  |
  ↓
Very fast inference
```

La característica por la que Groq es especialmente conocido es:

# SPEED

Puede generar una gran cantidad de:

```text
tokens / second
```

Por ejemplo:

```text
Application
     |
     ↓
Groq API
     |
     ↓
Llama / otros modelos
```

Por tanto:

```text
OpenAI    → crea modelos

Anthropic → crea modelos

Google    → crea modelos

DeepSeek  → crea modelos

Groq      → ejecuta modelos
```

---

# Ollama

**Ollama tampoco es un modelo.**

Ollama es un programa que permite ejecutar LLMs directamente en tu ordenador.

Por ejemplo:

```text
MacBook
   |
   ↓
 Ollama
   |
   ├── Llama
   ├── Mistral
   ├── Qwen
   ├── DeepSeek
   └── otros modelos
```

Ejemplo:

```bash
ollama run llama
```

La gran diferencia es:

```text
Cloud LLM

Your computer
     ↓
Internet
     ↓
OpenAI / Anthropic / Google
```

frente a:

```text
Local LLM

Your computer
     ↓
Ollama
     ↓
Model
```

Sin necesidad de enviar necesariamente tus datos a una API externa.

### Ventajas

```text
✓ Local
✓ Privacidad
✓ Puede funcionar offline
✓ Open source software
✓ Fácil de probar
```

### Inconvenientes

```text
✗ Necesita RAM
✗ Necesita CPU/GPU
✗ Los modelos grandes requieren mucho hardware
```

---

# OpenRouter

**OpenRouter no crea principalmente sus propios LLMs.**

Es una especie de:

> **Router / Gateway para modelos de IA**

En lugar de programar:

```text
OpenAI API

Anthropic API

Google API

DeepSeek API
```

puedes utilizar:

```text
             OpenRouter
                 |
       ┌─────────┼──────────┐
       ↓         ↓          ↓
    OpenAI   Anthropic    Google
       ↓         ↓          ↓
     GPT       Claude      Gemini
```

Tu aplicación utiliza:

```text
OpenRouter API
```

y seleccionas qué modelo quieres utilizar.

Por ejemplo conceptualmente:

```python
model = "anthropic/claude-sonnet"
```

o:

```python
model = "openai/gpt"
```

o:

```python
model = "google/gemini"
```

---

# ¿Para qué sirve OpenRouter?

Imagina que desarrollas tu Agent así:

```text
Operations Agent
       |
       ↓
   OpenRouter
       |
       ├── OpenAI
       │
       ├── Claude
       │
       ├── Gemini
       │
       └── DeepSeek
```

Puedes cambiar de modelo sin tener que rediseñar toda tu aplicación.

También permite comparar fácilmente:

```text
precio

velocidad

latencia

calidad
```

entre diferentes modelos.

---

# Diferencia fundamental

La forma correcta de clasificarlos sería:

| Nombre | ¿Crea modelos? | ¿Ejecuta modelos? | ¿Router? | ¿Local? |
|---|:---:|:---:|:---:|:---:|
| **OpenAI** | ✅ | ✅ | ❌ | ❌ |
| **Anthropic** | ✅ | ✅ | ❌ | ❌ |
| **Google** | ✅ | ✅ | ❌ | ❌ |
| **DeepSeek** | ✅ | ✅ | ❌ | Puede |
| **Groq** | ❌ principalmente | ✅ | ❌ | ❌ |
| **Ollama** | ❌ | ✅ | ❌ | ✅ |
| **OpenRouter** | ❌ principalmente | ✅ / conecta | ✅ | ❌ |

---

# Forma fácil de recordarlo

```text
MODEL MAKERS
============

OpenAI
  └── GPT

Anthropic
  └── Claude

Google
  └── Gemini

DeepSeek
  └── DeepSeek models
```

```text
MODEL PROVIDER / INFERENCE
==========================

Groq
  └── Ejecuta modelos MUY rápido
```

```text
LOCAL MODEL RUNNER
==================

Ollama
  └── Ejecuta modelos en tu ordenador
```

```text
MODEL ROUTER
============

OpenRouter
  └── Una API
        |
        ├── OpenAI
        ├── Anthropic
        ├── Google
        ├── DeepSeek
        └── muchos otros
```

---

# Artificial Analysis

**Artificial Analysis** es una web independiente dedicada a comparar modelos y proveedores de IA.

Website:

https://artificialanalysis.ai

Permite comparar modelos por parámetros como:

```text
Intelligence

Price

Speed

Latency

Context Window

Reasoning

Benchmarks
```

Por ejemplo:

```text
                    Artificial Analysis

                         MODELS
                           |
        ┌──────────────────┼──────────────────┐
        ↓                  ↓                  ↓
   Intelligence          Speed              Cost
        ↓                  ↓                  ↓
    Benchmark           tokens/s        $ / tokens
```

Es especialmente útil porque no tienes que fiarte solamente de:

```text
OpenAI diciendo que GPT es bueno

Anthropic diciendo que Claude es bueno

Google diciendo que Gemini es bueno
```

Puedes consultar benchmarks independientes y comparar distintos modelos.

Actualmente Artificial Analysis permite comparar cientos de modelos y métricas como **inteligencia, coste por tarea, velocidad de salida, latencia y ventana de contexto**.

---

# Resumen para memorizar

```text
OpenAI
    ↓
GPT

Anthropic
    ↓
Claude
    ├── Haiku
    ├── Sonnet
    └── Opus

Google
    ↓
Gemini
    ├── Flash-Lite
    ├── Flash
    └── Pro

DeepSeek
    ↓
DeepSeek models
    ├── V → general
    └── R → reasoning


Groq
    ↓
FAST INFERENCE PROVIDER


Ollama
    ↓
RUN MODELS LOCALLY


OpenRouter
    ↓
ROUTE REQUESTS TO MANY MODELS


Artificial Analysis
    ↓
COMPARE MODELS
    ├── Intelligence
    ├── Cost
    ├── Speed
    └── Latency
```
