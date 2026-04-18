# Asistente del Reglamento Académico Duoc UC

Sistema de preguntas y respuestas basado en RAG (Retrieval-Augmented Generation) que permite consultar el Reglamento Académico de Duoc UC en lenguaje natural, utilizando GitHub Models como motor de IA.

---

## Descripción general

Este proyecto implementa un asistente académico inteligente que responde preguntas sobre el Reglamento Académico de Duoc UC. El sistema extrae el texto del reglamento desde un PDF, lo fragmenta por artículos, genera embeddings semánticos para cada fragmento y los almacena en un índice de búsqueda vectorial. Cuando el usuario hace una pregunta, el sistema recupera los fragmentos más relevantes y los entrega como contexto a un modelo de lenguaje (LLM) para generar una respuesta precisa.

---

## Arquitectura del sistema

```
PDF del Reglamento
        ↓
  Extracción de texto (pdfplumber)
        ↓
  Fragmentación por artículos (regex)
        ↓
  Generación de embeddings (SentenceTransformer)
        ↓
  Índice vectorial FAISS
        ↓
  Pregunta del usuario
        ↓
  Búsqueda de fragmentos relevantes (FAISS)
        ↓
  Construcción del prompt (LangChain)
        ↓
  Modelo LLM (GitHub Models - gpt-4o-mini)
        ↓
  Respuesta citando el artículo correspondiente
```

---

## Tecnologías utilizadas

| Componente | Tecnología | Propósito |
|---|---|---|
| Extracción PDF | pdfplumber | Leer y extraer texto del reglamento |
| Fragmentación | Python re (regex) | Dividir el texto por artículos |
| Embeddings | sentence-transformers (all-MiniLM-L6-v2) | Convertir texto a vectores numéricos |
| Búsqueda vectorial | FAISS (IndexFlatL2) | Encontrar fragmentos semánticamente similares |
| Orquestación LLM | LangChain | Construir y enviar prompts al modelo |
| Modelo de lenguaje | GitHub Models (gpt-4o-mini) | Generar respuestas en lenguaje natural |

---

## Decisiones técnicas

**¿Por qué fragmentar por artículos?**
El reglamento está estructurado en artículos numerados, lo que permite una fragmentación semánticamente coherente. Cada fragmento corresponde a una unidad de información completa, lo que mejora la precisión de la búsqueda.

**¿Por qué FAISS con IndexFlatL2?**
Para un corpus pequeño como este reglamento (93 fragmentos), IndexFlatL2 realiza búsqueda exhaustiva exacta sin necesidad de índices aproximados. Es simple, preciso y eficiente para este volumen de datos.

**¿Por qué all-MiniLM-L6-v2?**
Es un modelo de embeddings liviano (384 dimensiones), rápido y con buen rendimiento en tareas de similitud semántica. No requiere GPU y funciona bien con textos en español.

**¿Por qué GitHub Models?**
Permite acceder a modelos de OpenAI de forma gratuita usando un token de GitHub, sin necesidad de tarjeta de crédito ni cuota de pago. Es ideal para desarrollo y pruebas académicas.

**¿Por qué k=3 fragmentos?**
Se recuperan los 3 fragmentos más relevantes para construir el contexto. Este valor balancea precisión (contexto suficiente) con eficiencia (menos tokens enviados al modelo).

---

## Estructura del proyecto

```
 proyecto/
├──  Not_Python_IA_Jaime_Delgadillo_1.ipynb   # Notebook principal
├──  RES-VRA-03-2024-NUEVO-REGLAMENTO-ACADÉMICO63-1.pdf  # PDF del reglamento
├──  requirements.txt                          # Dependencias del proyecto
└──  README.md                                 # Este archivo
```

---

## Requisitos previos

- Python 3.12 o superior
- Token de GitHub Models habilitado (ver sección de configuración)
- Jupyter Notebook o VS Code con extensión Jupyter

---

## Instalación

1. Clona el repositorio:
```bash
git clone https://github.com/JDL-DUOC/Not_Python_IA_Jaime_Delgadillo_1
cd Not_Python_IA_Jaime_Delgadillo_1
```

2. Instala las dependencias:
```bash
pip install -r Requeriments.txt
```

---

## Configuración del token de GitHub Models

1. Ve a [https://github.com/marketplace/models](https://github.com/marketplace/models)
2. Genera un token personal (classic) con permisos básicos
3. Activa el acceso a GitHub Models en [https://github.com/marketplace/models](https://github.com/marketplace/models)
4. Copia el token y pégalo en el bloque 1 del notebook donde dice `TU_GITHUB_TOKEN_AQUI`

---

## Ejecución

Abre el notebook `Not_Python_IA_Jaime_Delgadillo_1.ipynb` y ejecuta los bloques en este orden:

| Bloque | Descripción |
|---|---|
| Bloque 1 | Configuración del modelo LLM (GitHub Models) |
| Bloque 2 | Extracción del PDF y fragmentación por artículos |
| Bloque 3 | Generación de embeddings e índice FAISS |
| Bloque 4 | Definición de la función `buscar_fragmentos()` |
| Bloque 5 | Definición de la función `responder_pregunta()` |
| Bloque 6 | Prueba con una pregunta o chat interactivo |



---

## Ejemplos de uso

```
Pregunta: ¿Cuál es la nota mínima para aprobar?
Respuesta: Según el Artículo 37, la nota mínima para aprobar una asignatura es 4,0,
lo que corresponde al 60% de logro de los aprendizajes.

Pregunta: ¿Cuántas veces puedo reprobar una asignatura?
Respuesta: Según el Artículo 42, reprobar una asignatura por tercera vez es causal
de eliminación académica.

Pregunta: ¿Cuál es el porcentaje mínimo de asistencia?
Respuesta: Según el Artículo 30, el porcentaje mínimo de asistencia es del 70%
tanto para actividades teóricas como prácticas.
```

---

## Limitaciones conocidas

- El sistema responde únicamente con información contenida en el reglamento cargado. Preguntas fuera de ese contexto recibirán una respuesta indicando que no se encontró la información.
- La precisión depende de la calidad de la fragmentación. Artículos muy largos son truncados a 600 caracteres, lo que puede omitir información al final del artículo.
- El modelo de embeddings utilizado (`all-MiniLM-L6-v2`) fue entrenado principalmente en inglés, por lo que el rendimiento en español puede ser subóptimo en algunos casos.

---

## Autor

**Jaime Delgadillo**  
Proyecto académico — Duoc UC
