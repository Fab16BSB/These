![Doctorate Thesis](https://img.shields.io/badge/Doctorate-Thesis-ffb6c1)
![University: Paris 8](https://img.shields.io/badge/University-Paris%208-red)
![deep: learning](https://img.shields.io/badge/deep-learning-blue)
![python](https://img.shields.io/badge/python-brightgreen)
![Contributors](https://img.shields.io/badge/contributor-1-orange)
![Stars](https://img.shields.io/github/stars/Fab16BSB/These?color=orange)
![Fork](https://img.shields.io/github/forks/Fab16BSB/These?color=orange)
![Watchers](https://img.shields.io/github/watchers/Fab16BSB/These?color=orange)

## 🌍 Versiones multilingües del README

| 🇫🇷 Français | 🇬🇧 English | 🇪🇸 Español |
|-------------|------------|------------|
| [README.fr.md](./README.fr.md) | [README.md](./README.md) | ¡Estás aquí! |

---

# Detección de anomalías en tiempo real en un flujo de vídeo

## 📌 Contexto  

Realicé una **tesis CIFRE** (Convención Industrial de Formación por la Investigación) con una duración de **3 años** (del 4 de diciembre de 2019 al 4 de diciembre de 2022), titulada **_Detección de anomalías en tiempo real en un flujo de vídeo_**.  

Durante este período, ocupé el puesto de **investigador** en la startup [**Othello** 🚀](http://www.othello.group), lo que me permitió llevar a cabo mis trabajos de investigación directamente en la empresa, mientras estaba supervisado por el **Laboratorio de Inteligencia Artificial y Semántica de Datos (LIASD)** 🧠 de la Universidad **Paris 8** 📚, favoreciendo así la transferencia de conocimientos entre el mundo industrial y la investigación 🤝.

---

## 🎯 Objetivo  

El objetivo de este proyecto era desarrollar una solución de **detección de anomalías mediante inteligencia artificial** 🤖, en el marco de la convocatoria destinada a **garantizar la seguridad de los Juegos Olímpicos de París 2024** 🏅.  
Entre las restricciones impuestas:  

- El modelo debía funcionar **en tiempo real** para permitir una intervención rápida y garantizar así la seguridad de las personas involucradas.  
- Los recursos disponibles eran limitados: **sin acceso a un servidor GPU** ni a **cámaras de vigilancia reales**, lo que aumentó el desafío técnico del proyecto.  
- El modelo debía ser **ligero y optimizado**, de manera que pudiera funcionar sin necesidad de un servidor GPU potente, lo cual era esencial dadas las limitaciones de recursos.  

---

## 💡 Postulado  

Como seres humanos, podemos identificar fácilmente una situación anómala en una escena o en un video:  
- un **incendio** gracias a la presencia de humo o llamas 🔥  
- una **pelea** observando las interacciones entre varias personas 👥  
- un **accidente de tráfico** vigilando los vehículos 🚗  
- un **peligro** potencial al detectar un arma blanca o de fuego 🗡️🔫  
- Además, un accidente de tráfico **no puede ocurrir** si ningún vehículo es visible en la pantalla.

Estas observaciones me inspiraron a combinar en este proyecto:  
1. **Análisis espacial**: detección de objetos y poses humanas potencialmente sospechosas  
2. **Análisis temporal**: seguimiento de las acciones de los objetos e individuos a lo largo del tiempo  

El objetivo es crear una arquitectura que combine **análisis espacial y temporal**, reproduciendo de manera simplificada la forma en que los humanos interpretan una escena observando los objetos y su dinámica para comprender la situación en su conjunto, con el fin de evaluar si este enfoque puede superar los métodos clásicos.

---

## ⚙️ Entorno técnico  

Todo el proyecto se desarrolló en un ordenador portátil proporcionado por la empresa, equipado con los siguientes componentes:  
- **Memoria RAM:** 32 GB  
- **Procesador:** Intel Core i9, 16 núcleos a 2,3 GHz  
- **Tarjeta gráfica:** Nvidia GeForce RTX 2080 con 8 GB de RAM dedicada  

En cuanto al software, el desarrollo y entrenamiento de los modelos se realizó con:  
- **Python**  
- **Keras** para el diseño y entrenamiento de los modelos de inteligencia artificial

---

## 📊 Conjunto de datos  

### 1️⃣ Videos para la detección de anomalías

Ningún conjunto de datos público era suficientemente adecuado para entrenar nuestro modelo de manera eficaz:  
- Falta de volumen y variedad  
- Calidad insuficiente  
- Presencia de anomalías que no representan consecuencias directas sobre la seguridad de las personas involucradas  

Por lo tanto, diseñamos nuestro propio conjunto de datos.

⚠️ *Este conjunto de datos es propietario y no puede ser compartido ni difundido.* ⚠️

#### 🏷️ Clases y distribución

El conjunto de datos está compuesto por **4 clases**:  
- **Fight** (pelea)  
- **Shooting** (disparo)  
- **Fire** (incendio)  
- **Normal** (sin problemas detectados)  

Está **desequilibrado**, ya que algunas anomalías son más raras que otras. Su distribución es la siguiente:  

![Distribución de clases](images/dataset_distribution.png)  

| Clase     | Videos de entrenamiento (n) | Duración entrenamiento | Videos de validación (n) | Duración validación |
|-----------|-----------------------------|----------------------|--------------------------|-----------------------|
| Fight     | 587                         | 0h50                 | 391                      | 0h31                  |
| Fire      | 237                         | 3h40                 | 61                       | 0h46                  |
| Shooting  | 247                         | 0h17                 | 64                       | 0h08                  |

#### 🖼️ Preprocesamiento y aumento de datos

- Cada video que contiene una anomalía fue **recortado manualmente** para conservar solo la porción relevante.  
- Los videos se cargaron mediante un **video generator** (ver [repo GitHub](lien_vers_repo)) para formar **secuencias de 20 imágenes de tamaño 112x122**.  
- Se aplicó un **aumento de datos realista** para simular situaciones plausibles:  
  - Modificación de la **luminosidad**  
  - **Flip horizontal**  
  - **Zoom**  

### 2️⃣ Imágenes para la detección de objetos

De acuerdo con el postulado de **combinar la detección de objetos con la detección de anomalías**, creamos un **segundo conjunto de datos basado en imágenes**, enfocado en las entidades presentes en la pantalla y relacionadas con las anomalías.

#### 🏷️ Clases y volumen

Las clases y volúmenes aproximados son:  
- **Armas de fuego**: ~10 000 imágenes 🔫  
- **Llamas**: >2 000 imágenes 🔥  
- **Personas**: cada individuo presente en las imágenes 👤  

También se añadieron imágenes **sin ninguna de estas clases** para el aprendizaje y la validación, con el fin de manejar mejor las situaciones normales.

#### 🖼️ Anotación y uso

- Las imágenes fueron **etiquetadas manualmente**.

---

## 🧠 Modelo

El sistema de detección combina varias arquitecturas para procesar los flujos de video tanto en el **plano espacial** como en el **plano temporal**:  

- **CNN + RNN**: utilizado para el **análisis temporal** de las secuencias de video.  
- **YOLOv3, YOLOv4, YOLOv7**: utilizados para el **análisis espacial** de las imágenes.  

![Diagrama del sistema](images/these_architecture1.png)  
![Diagrama del sistema](images/these_architecture2.png)  

### 🔄 Combinación de modelos

Los modelos pueden combinarse de diferentes maneras:  
1. **Serie**: YOLO se utiliza para un **preprocesamiento espacial**, y sus resultados se envían luego al CNN+RNN para el análisis temporal.  
   ![Diagrama del sistema](images/modele_serie.png)
   
2. **Paralelo**: ambos modelos realizan la detección simultáneamente, y sus resultados se **fusionan** para producir la predicción final.  
![Diagrama del sistema](images/modele_parallele.png)

También se integra un **módulo de explicabilidad** para analizar ciertas predicciones, aunque **no funciona en tiempo real**.  

### 🖥 CNN + RNN
![Diagrama CNN + RNN](images/modele.png)  

- **CNN**: VGG19  
- **RNN**: GRU con **1024 neuronas**  
- seguido de un **MLP de 3 capas** (1024 → 512 → 128 neuronas)  
  - **Dropout:** 50% en todas las capas  
  - **Regularización L2:** coeficiente 0.01  
- **Entrenamiento:**  
  - Entrenamiento por lotes (batch training) sobre nuestros videos  
  - **200 epochs**  
  - Optimizador: **Stochastic Gradient Descent (SGD)** con **learning rate 0.01**  

### 🖼 YOLO

Los modelos **YOLOv3, YOLOv4 y YOLOv7** fueron **reentrenados con nuestro conjunto de imágenes**, permitiendo una detección adaptada a las clases definidas en nuestro dataset.

---

## 📈 Resultados

### 🖼️ 1. Detección de objetos

Los siguientes resultados muestran la detección de los objetos clave en imágenes extraídas de nuestros videos.  
Estos resultados ilustran la capacidad de nuestro modelo YOLO para localizar con precisión las entidades importantes para la seguridad.

![Ejemplo de detección de arma de fuego](images/yolo_result.png)
![Ejemplo de detección de llama](images/yolo_result_2.png)

---

### 🎬 2. Análisis de video

Para ilustrar el rendimiento de nuestro enfoque, presentamos a continuación varios ejemplos de análisis de video.  
Cada secuencia combina la **detección espacial** (personas, objetos, llamas, etc.) con un **análisis temporal** basado en VGG-GRU, permitiendo resaltar las anomalías en tiempo real.  

📸 **Capturas de pantalla**  
- Detección de un inicio de incendio.  
![Ejemplo detección de incendio (imagen)](images/incendie.png)  

- Detección de un accidente de tráfico.  
![Ejemplo detección de accidente (imagen)](images/accident.png)  

🎥 **Ejemplos de videos**  
- [▶️ Detección de incendio en Notre-Dame (ver video)](https://youtu.be/Zg4AAycii1M)  
- [▶️ Detección de una pelea durante un partido de rugby (ver video)](https://youtu.be/vdMqYTSrXok) 

---

### 🏆 3. Premio a la mejor demostración

Video que ilustra nuestro primer resultado que recibió el **premio a la mejor demostración en EGC 2022**.  

▶️ [Ver video de demostración](https://www.youtube.com/watch?v=EGHUEPMI4c8)

---

### 🧐 Explicabilidad del modelo  

Para comprender mejor el funcionamiento interno de nuestro sistema, hemos integrado un **módulo de explicabilidad**.  
Este cumple un doble propósito:  
- **Analizar las predicciones** del modelo para identificar sus razones.  
- **Ayudar en el ajuste de hiperparámetros** destacando las zonas o características más influyentes.  

---

#### 🔍 Visualización de predicciones  
Estos ejemplos ilustran cómo la explicabilidad resalta las regiones relevantes para la detección de anomalías:  

![Ejemplo de explicabilidad](images/explicabilite.png)  
![Ejemplo de explicabilidad](images/explicabilite_2.png)  

---

#### ⚙️ Ayuda en la configuración del modelo  
La explicabilidad también se utilizó para analizar el comportamiento de las distintas capas (GRU y MLP), permitiendo afinar el diseño de la arquitectura y su parametrización.  

![Explicabilidad GRU](images/explicabilite_gru.png)  
![Explicabilidad MLP](images/explicabilite_mlp.png)

---

### 📊 5. Estadísticas y rendimiento

Para evaluar nuestro enfoque, probamos diferentes **arquitecturas de combinación** entre YOLO (para el análisis espacial) y VGG-GRU (para el análisis temporal).  
Se compararon dos configuraciones principales:  

1. **Modo serie**: las salidas de YOLO se utilizan directamente como entrada para VGG-GRU.  
2. **Modo paralelo**: YOLO y VGG-GRU realizan sus predicciones por separado, luego sus resultados se combinan para producir la decisión final.  

Las siguientes tablas presentan el rendimiento obtenido para estas dos configuraciones en términos de **precisión, recall, F1-score y matriz de confusión**.

#### Rendimiento de YOLO + VGG-GRU en modo serie

| Métrica    | Exactitud | Precisión | Recall  | F1-Score |
|------------|-----------|-----------|---------|----------|
| Valor      | 87.3%     | 87.6%     | 87.3%   | 87.1%    |

#### Matriz de confusión en porcentaje para evaluación en serie

| Truth \ Predicted | Pelea | Disparo | Incendio | Normal |
|------------------|-------|---------|----------|---------|
| **Pelea**        | 60.5% | 2.4%    | 1.3%     | 35.8%   |
| **Disparo**      | 10%   | 55.6%   | 14.8%    | 19.6%   |
| **Incendio**     | 15.5% | 10.6%   | 48%      | 25.9%   |
| **Normal**       | 3.4%  | 0.6%    | 1%       | 95%     |

#### Rendimiento de YOLO + VGG-GRU en modo paralelo

| Métrica    | Exactitud | Precisión | Recall  | F1-Score |
|------------|-----------|-----------|---------|----------|
| Valor      | 78.42%    | 85.60%    | 78.42%  | 81.16%   |

#### Matriz de confusión en porcentaje para evaluación en paralelo

| Truth \ Predicted | Pelea  | Disparo | Incendio | Normal |
|-------------------|--------|---------|----------|--------|
| **Pelea**         | 63.66% | 6.58%   | 1.93%    | 27.83% |
| **Disparo**       | 9.94%  | 66.06%  | 9.33%    | 14.67% |
| **Incendio**      | 13.66% | 15.73%  | 57.71%   | 12.9%  |
| **Normal**        | 7.43%  | 5.96%   | 3.98%    | 82.63% |

---

Finalmente, evaluamos ambos enfoques (paralelo y serie) desde el punto de vista del **tiempo de ejecución** para verificar su aplicabilidad en condiciones reales.  
Los resultados muestran que:  
- El **modo paralelo** permite un procesamiento más rápido, con un tiempo medio cercano al tiempo real para ciertos videos.  
- El **modo serie**, aunque más preciso, tiene un costo temporal significativamente mayor.  

Las siguientes tablas resumen estas mediciones para diferentes videos de evaluación.

#### Tiempo de ejecución de YOLO + VGG-GRU en paralelo

| Duración del video | FPS del video | Promedio de detecciones | Tiempo de procesamiento |
|--------------------|---------------|-----------------------|---------------------------|
| 16s                | 33            | 601ms                 | 15s                       |
| 44s                | 30            | 533ms                 | 35s                       |
| 9s                 | 30            | 994ms                 | 12s                       |
| 35s                | 30            | 1.1s                  | 57s                       |
| 23s                | 30            | 1s06                  | 35s                       |
| 1min 43            | 30            | 758ms                 | 116s (1min 56)            |
| 50s                | 30            | 826ms                 | 61s                       |
| 1min 05            | 30            | 886ms                 | 83s (1min 23)             |
| 2s                 | 30            | 847ms                 | 847ms                     |
| 9s                 | 30            | 870ms                 | 11s                       |
| 2s                 | 30            | 1s                    | 1s                        |

#### Tiempo de ejecución de YOLO + VGG-GRU en serie

| Duración del video | FPS del video | Promedio de detecciones | Tiempo de procesamiento |
|--------------------|---------------|-----------------------|---------------------------|
| 16s                | 33            | 1s                    | 26s                       |
| 44s                | 30            | 1s                    | 71s (1min 11)             |
| 9s                 | 30            | 1.5s                  | 20s                       |
| 35s                | 30            | 1.5s                  | 81s (1min 21)             |
| 23s                | 30            | 1.5s                  | 48s                       |
| 1min 43            | 30            | 1.2s                  | 193s (3min 13)            |
| 50s                | 30            | 1.3s                  | 102s (1min 42)            |
| 1min 05            | 30            | 1.4s                  | 134s (2min 14)            |
| 2s                 | 30            | 1.3s                  | 1.3s                      |
| 9s                 | 30            | 1.3s                  | 17s                       |
| 2s                 | 30            | 1.5s                  | 1.5s                      |

---

## ⚠️ Limitaciones y Confidencialidad

- **Número limitado de clases**: solo se trataron algunas clases, ya que la recolección y el etiquetado de datos son procesos largos y costosos.

- **Sin comparación con otras arquitecturas**: el proyecto no permite un benchmark directo con soluciones existentes.

- **Tecnologías no utilizadas**: no se exploraron autoencoders ni Vision Transformers, debido a limitaciones de tiempo y capacidad de cómputo; además, estas arquitecturas no siempre son adecuadas para el procesamiento en **tiempo real**.

Todos los conjuntos de datos utilizados, así como el código desarrollado en el marco de esta tesis, son **propiedad exclusiva de la empresa Othello**.  
Por razones de confidencialidad y protección de la propiedad intelectual, no pueden compartirse públicamente.

---

## 📚 Publicaciones / Artículos

### 🎓 Tesis

- 🇫🇷 **Détection d’anomalies en temps réel dans un flux vidéo** (2023)  
  Autor: F. Poirier  
  [Enlace HAL](https://hal.science/tel-04792952)  

- 🇬🇧 **Real-Time Anomaly Detection in Video Streams** (2023)  
  Autor: F. Poirier  
  Tesis traducida  
  [Enlace HAL](https://hal.science/tel-04824941)  
  [Enlace ArXiv](https://arxiv.org/abs/2411.19731)  

### 📝 Artículos científicos

- 🇬🇧 **From CNN to CNN RNN Adapting Visualization Techniques for Time-Series Anomaly Detection** (2025, enviado)  
  Autores: F. Poirier, M. Lamolle  
  En proceso de envío a la revista *Engineering Applications of Artificial Intelligence*

- 🇬🇧 **From CNN to CNN+ RNN: Adapting Visualization Techniques for Time-Series Anomaly Detection** (2024)  
  Autor: F. Poirier  
  [Enlace ArXiv](https://arxiv.org/abs/2411.04707)  

- 🇬🇧 **Real-Time Anomalies Detection on Video** (2024)  
  Autor: F. Poirier  
  [Enlace ArXiv](https://arxiv.org/abs/2410.18051)  

- 🇬🇧 **Hybrid Architecture for Real-Time Video Anomaly Detection: Integrating Spatial and Temporal Analysis** (2024)  
  Autor: F. Poirier  
  [Enlace ArXiv](https://arxiv.org/abs/2410.15909)  

- 🇬🇧 **Enhancing Anomaly Detection in Videos using a Combined YOLO and a VGG GRU Approach** (2023)  
  Autores: F. Poirier, R. Jaziri, C. Srour, G. Bernard  
  Conferencia: 20th ACS/IEEE International Conference on Computer Systems and Applications (AICCSA)  
  [Enlace IEEE](https://ieeexplore.ieee.org/abstract/document/10479307)  

- 🇫🇷 **Détection d’anomalies en temps réel dans le flux vidéo** (2022)  
  Autores: F. Poirier, R. Jaziri, C. Srour, G. Bernard  
  [Enlace HAL](https://hal.science/hal-04810781)  

### 🏆 Posters y distinciones

- 🇫🇷 **Détection d’anomalies en temps réel dans le flux vidéo**  
  Poirier Fabien, R. Jaziri, C. Srour, G. Bernard  
  Poster presentado en EGC 2022  
  [Enlace HAL](https://hal.science/hal-04830165v1/document)  
  🏅 **Premio a la Mejor Demostración** EGC 2022
