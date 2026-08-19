# 🏔️ AndesGuide AI: Sistema Asistente para la Planificación Técnica y Generación de Fichas Visuales de Senderismo

---

## 📌 Resumen del Proyecto

**AndesGuide AI** es una Prueba de Concepto (POC) desarrollada en Google Colab que combina técnicas avanzadas de **Ingeniería de Prompts** (*Role Prompting*, *Structured Markdown Output* y *Prompt Chaining*) para automatizar la gestión logística y de seguridad en excursiones de montaña.

El sistema procesa variables técnicas de una ruta (altitud, clima, desnivel y nivel del grupo) mediante la API gratuita de **Google Gemini 2.0 Flash**, generando de manera autónoma una **Ficha Técnica y Matriz de Riesgos** estructurada, así como un **Prompt Visual en inglés** optimizado para modelos de texto a imagen (*knolling / flat lay view*). Esto reduce el tiempo de preparación administrativa de 4 horas a menos de 5 segundos, con costo operativo nulo.

---

## ❓ Problemática y Solución

| Problemática Identificada | Solución Propuesta (*AndesGuide AI*) |
| --- | --- |
| **Falta de estandarización técnica:** Informes informales o desestructurados por chat. | **Formatos estandarizados en Markdown** con tablas logísticas y protocolos de rescate. |
| **Insumo alto de tiempo:** Entre 4 y 8 horas por ruta para diseñar itinerarios y listas. | **Generación en cadena (*Prompt Chaining*)** en < 5 segundos mediante LLM. |
| **Ausencia de material visual:** Diseñar infografías requiere software y habilidades costosas. | **Traducción automática a prompts gráficos** para maquetación visual (*knolling layout*). |

---

## ⚙️ Arquitectura del Pipeline Modular

El flujo de trabajo se divide en 3 módulos consecutivos donde la salida de cada etapa alimenta la siguiente:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        VARIABLES DE ENTRADA                            │
│    (Nombre de la ruta, Altitud, Desnivel, Clima, Nivel del grupo)      │
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│ MÓDULO 1: Procesamiento Lógico y Evaluación de Seguridad               │
│ Engine: Gemini 2.0 Flash (System Instruction: Guía UIAGM/WFR)          │
│ Output: Ficha Técnica + Matriz de Riesgos en Markdown                  │
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│ MÓDULO 2: Traducción Contextual Instruccional (Prompt Chaining)        │
│ Engine: Gemini 2.0 Flash (System Instruction: Director de Arte)         │
│ Output: Prompt fotográfico en inglés (Estilo Knolling/Flat Lay)         │
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│ MÓDULO 3: Generación Visual y Salida (Texto a Imagen)                  │
│ Engine: NightCafe / Leonardo AI / Bing Image Creator / DALL-E 3        │
│ Output: Infografía visual del equipamiento en formato PNG              │
└────────────────────────────────────────────────────────────────────────┘

```

---

## 🛠️ Herramientas y Tecnologías

* **Lenguaje:** Python 3.10+
* **Entorno de Ejecución:** Google Colab
* **Modelo LLM:** Google Gemini 3.6 Flash (`google-genai` SDK)
* **Generación Visual:** NightCafe / Bing Image Creator / Leonardo AI
* **Técnicas de Fast Prompting Aplicadas:**
* **Role Prompting:** Asignación explícita de roles (*Guía UIAGM/WFR* y *Director de Arte*).
* **Prompt Chaining:** Inyección del texto generado en el Módulo 1 como contexto directo del Módulo 2.
* **Structured Markdown Constraints:** Restricción estricta de salida en esquemas y tablas Markdown.
* **Zero-Shot Task Transformation:** Traducción de listas técnicas en español a comandos de diseño en inglés técnico.



---

## 📂 Estructura del Repositorio

```text
AndesGuide_AI/
│
├── README.md                      # Presentación y documentación general
├── AndesGuide_AI_POC.ipynb         # Notebook ejecutable en Google Colab
└── assets/
    ├── logo_andesguide.png        # Logo de la portada
    ├── pipeline_diagram.png       # Esquema del flujo modular
    └── gear_infographic_output.png# Infografía gráfica generada por IA

```

---

## 🚀 Guía de Instalación y Ejecución

### 1. Clonar el Repositorio

```bash
git clone https://github.com/ivancho15/AndesGuide_AI.git
cd AndesGuide_AI

```

### 2. Abrir en Google Colab

Sube el archivo `AndesGuide_AI_POC.ipynb` a tu cuenta de Google Colab o ejecútalo directamente desde GitHub.

### 3. Configurar API Key

Obtén tu clave de API gratuita en [Google AI Studio](https://aistudio.google.com/) y guárdala en los **Secretos de Colab** (`userdata`) bajo el nombre `GEMINI_API_KEY`, o ingresándola interactivamente al ejecutar el cuaderno.

### 4. Instalar Dependencias

```python
!pip install -q google-genai

```

---

## 📸 Resultados de la Prueba de Concepto

**Caso de Prueba Real:** Volcán Rumiñahui Sur (4,630 msnm, Parque Nacional Cotopaxi, Ecuador).

### 1. Salida de Texto (Ficha de Seguridad)

La ejecución del Módulo 1 genera una tabla de resumen logístico, lista de equipamiento por capas térmicas, matriz de riesgos (altitud, clima, terreno) y protocolo de emergencias.

### 2. Salida Visual (Infografía de Equipamiento)

Prompt procesado en el Módulo 3 a partir de la instrucción en inglés generada por Gemini:

---

## 📊 Auditoría de Tokens y Viabilidad Económica

| Componente | Consumo Estimado | Costo en Plan Gratuito (Gemini API) | Costo Modelo Tradicional |
| --- | --- | --- | --- |
| **Módulo 1 (Texto)** | ~880 tokens | **$0.00 USD** | $25.00 - $50.00 USD (Redactor) |
| **Módulo 2 (Prompting)** | ~920 tokens | **$0.00 USD** | $25.00 - $50.00 USD (Diseñador) |
| **Módulo 3 (Imagen)** | 1 Crédito / Imagen | **$0.00 USD (Free Tier)** | Session fotográfica / Maquetación |
| **Total por Ruta** | **~1,800 tokens** | **$0.00 USD** | **$50.00 - $100.00 USD** |

---

## 👤 Autor

* **Iván José Marcano Mota**
* **Curso:** Prompt Engineering
* **Repositorio:** [ivancho15/AndesGuide_AI](https://www.google.com/search?q=https://github.com/ivancho15/AndesGuide_AI)

---

## 📜 Licencia

Este proyecto se distribuye bajo la licencia **MIT**. Consulta el archivo `LICENSE` para más información.
