# adaptive-rag-langgraph
# Sistema de RAG Adaptativo con LangGraph, Google Gemini y Tavily

Este repositorio contiene la implementación de un sistema Agentic RAG (Adaptive Retrieval-Augmented Generation) robusto, autónomo y de costo cero, construido utilizando el ecosistema de LangChain, LangGraph, Google Gemini y Tavily Search. 

El proyecto está basado en el artículo técnico de Medium *"Building an Adaptive RAG System with LangGraph, OpenAI, and Tavily"*, adaptando inteligentemente sus componentes hacia la infraestructura gratuita de Google AI Studio para fines académicos y de desarrollo sin costo.¨(se adaptó el código para utilizar Gemini con la guía de Gemini)

---

## 🛠️ Arquitectura y Flujo de Trabajo (Adaptive RAG)

A diferencia de un RAG tradicional que es lineal y rígido, este sistema actúa como un **Agente Inteligente con toma de decisiones no lineales**. El flujo de control implementado sigue la siguiente lógica:

1. **Enrutamiento Inteligente (Router):** Evalúa la pregunta del usuario utilizando `gemini-1.5-flash` y clasifica si debe responderse usando la base de datos de conocimiento local (`vectorstore`) o mediante internet en tiempo real (`web_search`).
2. **Evaluación de Relevancia (Document Grader):** Si se consulta la base de datos, el agente califica si los fragmentos extraídos de `ChromaDB` son realmente útiles semánticamente para responder. Si no sirven, redirige automáticamente el flujo al buscador web.
3. **Búsqueda Web de Respaldo (Tavily Search):** Si el conocimiento local es insuficiente o el tema es ajeno, el agente utiliza la API de Tavily para extraer los 3 mejores contextos actualizados de internet.
4. **Verificación de Alucinaciones (Hallucination Grader):** Antes de entregar la respuesta final, un nodo "juez" evalúa de forma binaria si la respuesta generada por el LLM está estrictamente fundamentada en los hechos proporcionados o si el modelo inventó datos (alucinación). Si detecta alucinación, el agente vuelve a procesar el texto automáticamente.

---

## 🚀 Tecnologías y Herramientas Utilizadas

- **LangGraph:** Orquestación del estado, definición de nodos y bordes condicionales basados en grafos cíclicos.
- **LangChain:** Framework base para conectar prompts, modelos de lenguaje y herramientas de vectores.
- **Google Gemini (gemini-1.5-flash):** Modelo de lenguaje principal (LLM) configurado para el razonamiento estructurado y la evaluación de calidad.
- **Google GenAI Embeddings (gemini-embedding-001):** Modelo oficial de Google para la vectorización matemática del texto.
- **ChromaDB:** Base de datos vectorial local embebida para el almacenamiento del conocimiento corporativo/financiero.
- **Tavily Search API:** Motor de búsqueda optimizado para agentes autónomos de Inteligencia Artificial.
- **Python Dotenv:** Gestión segura de credenciales locales de forma aislada.

---

## 📂 Estructura del Repositorio

El proyecto se ha organizado siguiendo los estándares profesionales de desarrollo de software:

```text
├── .gitignore               # Exclusiones de Git (evita subir credenciales .env o temporales)
├── README.md                # Documentación principal del proyecto (este archivo)
├── requirements.txt         # Listado de dependencias del proyecto con sus versiones
│
├── notebooks/               # Cuadernos interactivos de Jupyter
│   └── adaptive_rag.ipynb   # Pipeline completo del Agente RAG funcional

**Consideraciones:**

-Se evidenció dificultades para utilizar Gemini, debido a la compatibilidad de librerías
-Se evidenció errores para configurar el Generador de Vectores, sobre la siguiente función GoogleGenerativeAIEmbeddings(
    model="models/embedding-001") el modelo no estaba actualizado. Asimismo en solo una oportunidad de logró configurar con el 100%, luego se generó un bucle en la celda y no se ejecutó las sigientes líneas
-El código propuesto es teórico, debido a los problemas mencionados.