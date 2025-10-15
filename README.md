# Generación de Tweets en el Estilo de Gustavo Petro mediante Modelos de Lenguaje y Análisis de Estilo Político

**Autores:**  
Juan Esteban Hunter Malaver, Dylan Macgiver Rivero Esteves, Fredy Alexander González Pobre, César Arthuro Lemos Silva

---

## **Resumen**
Este trabajo presenta el diseño y desarrollo de un sistema generativo capaz de producir mensajes en redes sociales con el estilo lingüístico característico del expresidente colombiano Gustavo Petro.  
El proyecto tiene aplicaciones en investigación lingüística, análisis discursivo, generación automática de contenido y enseñanza de técnicas de procesamiento de lenguaje natural (NLP).  

Se emplean múltiples estrategias de representación textual y modelos de generación, desde cadenas de Markov hasta redes neuronales recurrentes (LSTM), a fin de contrastar enfoques clásicos y modernos de modelado del lenguaje.

---

## **I. Entendimiento del Negocio**

### **A. Objetivo del Negocio y del Proyecto**
El objetivo principal es construir un sistema capaz de generar tweets en el estilo del autor (Gustavo Petro) a partir de un tema o *prompt* definido.  
Este sistema busca contribuir al estudio del discurso político, la generación de texto controlada y el desarrollo de herramientas de apoyo en redacción de contenido automatizado.

**Valor de negocio:**  
La propuesta permite la creación de herramientas de simulación de tono, análisis de consistencia discursiva y apoyo en tareas educativas relacionadas con el procesamiento del lenguaje político.

**Consideración ética:**  
Todo texto generado debe incluir una advertencia explícita (por ejemplo, *“Contenido generado por IA; no representa opiniones reales”*), respetar las licencias de datos y mitigar riesgos de desinformación o suplantación de identidad.

---

### **B. Descripción del Conjunto de Textos Seleccionado**
Se emplea el dataset **“jhonparra18/petro-tweets”** disponible en la plataforma **Hugging Face**.

| Atributo | Descripción |
|-----------|-------------|
| **Idioma** | Español |
| **Formato** | Parquet |
| **Tamaño** | ~19.8k registros |
| **Columnas principales** | `date`, `user` (petrogustavo), `tweet` |
| **Licencia** | MIT |

La colección contiene mensajes originales publicados por el autor en Twitter, conformando un corpus representativo del estilo político y discursivo objeto de estudio.

---

### **C. Obtención y Carga del Conjunto de Datos**
El acceso al dataset se realiza mediante la librería **`datasets`** de Hugging Face o por lectura directa del archivo Parquet desde Google Colab (`pandas.read_parquet("hf://...")`).  
Esta estrategia garantiza la trazabilidad y reproducibilidad del proceso de carga y análisis de datos.

---

## **II. Entendimiento y Preparación del Conjunto de Texto**

### **A. Visualización y Representación Semántica**
Con el fin de caracterizar el corpus, se aplican representaciones basadas en **TF-IDF** sobre texto lematizado. Esto permite la identificación de las palabras más distintivas y su ponderación según relevancia contextual.

**Se generó:**
- Una lista de las 25 palabras más frecuentes del corpus.  
- Un gráfico de barras con los términos de mayor peso TF-IDF, reflejando los conceptos dominantes en el discurso del autor.  
- Totalidad de tweets.  
- Longitud promedio de los tweets.  
- Promedio de palabras diferentes por tweet en el corpus.  
- Tweet más corto y más largo.  
- Histograma de cantidad de tweets por año.  
- Cantidad de tweets por año.  
- Comportamiento día y hora de frecuencia de publicación.  

---

### **B. Estrategia de Preprocesamiento y Limpieza**
El flujo de preprocesamiento se divide en tres *pipelines*, ajustados a las necesidades de los modelos a emplear:

| **Pipeline** | **Propósito** | **Operaciones Principales** | **Uso Principal** |
|---------------|---------------|-----------------------------|-------------------|
| **Lematizado Extenso** | Análisis de contenido | Eliminación de puntuación, *stopwords*, URLs, menciones y hashtags; lematización completa. | Vectorización TF-IDF y análisis temático (LDA). |
| **Markov** | Generación de texto base (*baseline*) | Eliminación de URLs y menciones; conservación de puntuación, mayúsculas y emojis. | Modelo de Cadenas de Markov. |
| **LSTM** | Generación avanzada mediante red neuronal | Limpieza ligera (URLs, menciones); normalización de acentos; preservación de estilo. | Modelo neuronal generativo (LSTM). |

---

## **III. Transformación del Texto y Extracción de Características**

### **A. Estrategias Implementadas**

#### **1. TF-IDF (Análisis Descriptivo y Temático)**
Se utiliza **`TfidfVectorizer`** aplicado al texto lematizado.  
Esta técnica convierte el corpus en una matriz dispersa ponderada que refleja la importancia relativa de cada término, permitiendo análisis cuantitativos de frecuencia y relevancia semántica.

#### **2. Secuenciación para Modelos Neuronales (LSTM)**
Se aplica un proceso de tokenización y normalización mediante **`Tokenizer`** y **`pad_sequences`**, transformando las palabras en secuencias numéricas uniformes, adecuadas como entrada para el modelo LSTM.

---

### **B. Detección de Temas No Supervisada (LDA sobre TF-IDF)**
La técnica **Latent Dirichlet Allocation (LDA)** se aplica sobre la representación TF-IDF para descubrir los principales ejes temáticos del corpus.  
Este enfoque permite identificar los tópicos dominantes en el discurso político del autor, facilitando el análisis de coherencia temática y evolución lingüística.

---

## **IV. Modelos de Generación de Texto**

### **A. Generación Baseline**
Se implementa un modelo de **Cadenas de Markov**, empleando *n-gramas* de palabras.  
Este modelo constituye una referencia inicial para evaluar la coherencia y fluidez de las secuencias generadas mediante un enfoque probabilístico clásico.

### **B. Generación Avanzada (LSTM)**
El modelo avanzado se basa en una **Red Neuronal Recurrente (LSTM)** configurada para predecir la siguiente palabra dentro de una secuencia dada.  
Esta arquitectura permite capturar dependencias a largo plazo en el texto, reproduciendo patrones estilísticos y estructuras sintácticas del autor de manera más realista.

---

## **V. Conclusiones y Trabajo Futuro**
El sistema propuesto representa una aproximación integral para el estudio y la generación de lenguaje político mediante técnicas de **NLP**.  
La combinación de métodos tradicionales (Markov, TF-IDF) y neuronales (LSTM) permite evaluar tanto la riqueza lingüística como la capacidad de generalización de los modelos.

**Trabajo futuro:**
- Incorporación de mecanismos de control ético y filtros de sesgo.  
- Extensión del sistema hacia modelos de atención (*Transformers*).  
- Mejora de la coherencia contextual y el realismo discursivo.

---
