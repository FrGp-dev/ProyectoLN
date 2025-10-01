## 1. Entendimiento del Negocio
---
### 1.1 Objetivo del Negocio y del Proyecto

* **Objetivo General:** Construir un sistema capaz de generar **tweets con el estilo del autor (Gustavo Petro)** dado un tema o *prompt*, para usos de **investigación lingüística**, **generación de contenido automático controlado** y **análisis de estilo**.
* **Valor de Negocio:** Creación de herramientas para **simulación de tono**, apoyo en redacción de comunicados en tono similar (con avisos éticos), **análisis de consistencia discursiva**, y **enseñanza de técnicas de NLP** aplicadas a textos políticos.
* **Nota Ética:** El uso de modelos que imitan a personas públicas debe incluir **rotulado claro** (p. ej. "generado por IA, no es real"), cumplir licencias y considerar **riesgos de desinformación**.

---
### 1.2 Descripción del Conjunto de Textos Seleccionado

| Característica | Detalle | Fuente |
| :--- | :--- | :--- |
| **Dataset** | `jhonparra18/petro-tweets` | Hugging Face |
| **Idioma** | **Español** | Hugging Face |
| **Formato** | **Parquet** | Hugging Face |
| **Tamaño** | $\approx$ **19.8k tweets** (split *train*, 19.8k filas) | Hugging Face |
| **Columnas Clave** | **Date**, **User** (`petrogustavo`), **Tweet** | Hugging Face |

---
### 1.3 Proceso de Carga u Obtención de Textos

* **Método:** Usaremos la **API de Hugging Face / datasets** o `pandas.read_parquet("hf://...")` para Colab.
* **Licencia:** **MIT** (ver página). Hugging Face

---
## 2. Entendimiento y Preparación del Conjunto de Texto
---
### 2.1 Análisis Exploratorio del Conjunto de Textos

**Realizado:**

* **Conteo de tweets**, **frecuencias**.
* **Longitud de tweets** (*tokens* y caracteres), **distribución del vocabulario**, **top-N palabras/hashtags**, *top-mentions*, **emojis frecuentes**.
* **Detección de duplicados** y tweets vacíos.
---
### 2.2 Visualizaciones Sugeridas

* **Histograma de longitud de tweets** (caracteres y *tokens*).
* **Top-30 palabras / hashtags**.
* **Palabras clave**.
* **Distribución del uso de *emojis***.

---
### 2.3 Preprocesamiento en NLP (Resumen)

Usamos **Regex en español**, con **tres *pipelines* distintos** según modelo (ya discutido y adaptado):

1.  **N-gramas:** limpieza $\to$ minúsculas $\to$ tokenización $\to$ lematización $\to$ eliminar puntuacion.
2.  **Markov:** limpieza (**URLs y menciones**) $\to$ **mantener formato original** (*hashtags*, *emojis*, conjugaciones).
3.  **Redes Neuronales:** **limpieza mínima** (eliminar URLs y menciones), **mantener mayúsculas, *hashtags* y *emojis*** para preservar estilo.

El preprocesado se encuentra en el **Repositorio del proyecto**:
[https://github.com/FrGp-dev/ProyectoLN/blob/0090adef39927d89e00d025a9175918d78656e95/petrosky.ipynb](https://github.com/FrGp-dev/ProyectoLN/blob/0090adef39927d89e00d025a9175918d78656e95/petrosky.ipynb)
