# Asistente del Reglamento Académico Duoc UC

Proyecto personal para el ramo de IA. La idea era hacer algo útil, así que armé un chatbot que responde preguntas sobre el reglamento académico de Duoc UC usando el PDF oficial.

\---

## ¿Qué hace?

Le puedes hacer preguntas en lenguaje normal como:

* "¿Cuál es la nota mínima para aprobar?"
* "¿Cuántas veces puedo reprobar una asignatura?"
* "¿Cuánto es el mínimo de asistencia?"

Y el sistema busca la respuesta directamente en el reglamento y te dice en qué artículo está.

\---

## ¿Cómo funciona por dentro?

Básicamente el flujo es este:

1. Lee el PDF del reglamento y extrae todo el texto
2. Lo divide en fragmentos por artículo usando expresiones regulares
3. Convierte cada fragmento en un vector numérico (embeddings) con `sentence-transformers`
4. Guarda esos vectores en un índice FAISS para poder buscar rápido
5. Cuando haces una pregunta, busca los 3 fragmentos más parecidos semánticamente
6. Le manda esos fragmentos + tu pregunta al modelo de lenguaje
7. El modelo responde citando el artículo correspondiente

\---

## Tecnologías que usé

* **pdfplumber** — para leer el PDF
* **sentence-transformers** — para generar los embeddings (modelo `all-MiniLM-L6-v2`)
* **FAISS** — para la búsqueda vectorial
* **LangChain** — para conectar todo con el LLM
* **GitHub Models (gpt-4o-mini)** — como modelo de lenguaje, es gratis con token de GitHub

\---

## Requisitos

* Python 3.12
* Token de GitHub Models (gratis, solo necesitas cuenta de GitHub)
* Jupyter Notebook o VS Code con extensión Jupyter

\---

## Instalación

Clona el repo e instala las dependencias:

```bash
git clone https://github.com/JDL-DUOC/Not\_Python\_IA\_Jaime\_Delgadillo\_1
cd Not\_Python\_IA\_Jaime\_Delgadillo\_1
pip install -r Requeriments.txt
```

\---

## Configuración

Para usar GitHub Models necesitas un token:

1. Ve a [https://github.com/marketplace/models](https://github.com/marketplace/models) y activa el acceso
2. Genera un token en [https://github.com/settings/tokens](https://github.com/settings/tokens)
3. Pégalo en el bloque 1 del notebook donde dice `TU\_GITHUB\_TOKEN\_AQUI`

\---

## Cómo ejecutarlo

Abre el notebook y ejecuta los bloques en este orden:

```
Bloque 1 → Bloque 2 → Bloque 3 → Bloque 4 → Bloque 5 → Bloque 6
```



\---

## Cosas que mejoraría con más tiempo

* El modelo de embeddings está entrenado en inglés principalmente, así que a veces no encuentra el fragmento exacto en español
* Los artículos muy largos se cortan a 600 caracteres, lo que puede hacer que se pierda algo de información
* Podría agregar una interfaz gráfica simple con Gradio o Streamlit

\---

## Autor

Jaime Delgadillo — Duoc UC

