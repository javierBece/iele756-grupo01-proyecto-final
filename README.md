# Fuerza Laboral en Escala: Detección y corrección de sesgos por apalancamiento en datos comunales
**Curso:** IELE756 -- Preparación y Análisis de Datos  
**Profesor:** Leo Ferres, PhD  
**Ayudantes:** Alan Spikin, Antuan Vayisqui
**Integrantes:** Javier Becerra Muñoz, Jose Pino Muñoz  
**Grupo:** Grupo 1  

Este repositorio contiene la entrega del Proyecto Final para el ramo de Preparación y Análisis de Datos. En este trabajo, nos enfocamos en **reducir el alcance y aumentar la profundidad** para aislar y defender una única anomalía estadística identificada en nuestro pipeline.

---

## 1. Comunas Asignadas
Nuestras tres comunas de estudio de la Región Metropolitana son:
* **La Granja** (Código Comuna: 13111)
* **Macul** (Código Comuna: 13118)
* **San Ramón** (Código Comuna: 13131)

---

## 2. La Anomalía Defendida: Fuerza Laboral en Escala: Detección y corrección de sesgos por apalancamiento en datos comunales
Durante la Tarea 3, al correlacionar las variables sociodemográficas y de salud, encontramos una correlación de Pearson de **-0.51** (fuertemente negativa y contraintuitiva) entre la **tasa de dependencia demográfica** (`dependency_ratio`) y la **tasa de desempleo** (`pct_unemployed`) a nivel comunal en la Región Metropolitana. 

La teoría y la lógica económica indican que las comunas con mayor cantidad de dependientes (niños de 0-14 años y adultos mayores de 65+) deberían asociarse con mayores tasas de inactividad o desempleo, es decir, una correlación positiva. El hallazgo de que ocurriera exactamente lo contrario sugería un fenómeno anómalo.

En este proyecto demostramos que esta relación negativa es un **espejismo estadístico causado por un bug de integración de datos y escala**. Tres comunas (**Conchalí, La Cisterna y Quinta Normal**) tenían sus tasas de empleo subidas en escala `0-1` (fracción) en lugar de `0-100` (porcentaje). Debido a un bug en el pipeline de consolidación (`100 - tasa_empleo` en lugar de `100 * (1 - tasa_empleo)`), estas tres comunas registraron tasas de desempleo del **99.3%**, actuando como puntos de altísimo apalancamiento (*high-leverage outliers*). Al corregir este error de escala, la correlación real de la RM pasa a ser de **0.0872** (positiva y cercana a cero), resolviendo por completo la anomalía.

---

## 3. Estructura del Proyecto
* **`final_anomaly.ipynb`**: Notebook principal de Jupyter que realiza la carga de datos de la Tarea 3, genera la Headline Figure inicial, ejecuta las dos comprobaciones de hipótesis alternativas en código y concluye el análisis. Su tiempo de ejecución es de **~3 segundos**.
* **`requirements.txt`**: Archivo de requisitos que especifica las librerías necesarias para reproducir el análisis.
* **`figs/`**: Directorio que contiene las figuras generadas:
  * `headline.png`: Gráfico principal que muestra la correlación inicial y los 3 outliers extremos.
  * `corrected_correlation.png`: Gráfico corregido tras solucionar el error de escala.
  * `migration_confounding.png`: Gráfico de dispersión de comprobación de confusión por variables de migración.

---

## 4. Instalación y Reproducibilidad
Para reproducir el análisis y volver a generar todas las figuras de forma exacta:

1. Clonar el repositorio:
   ```bash
   git clone <URL_DEL_REPOSITORIO>
   cd <NOMBRE_DEL_REPOSITORIO>/Tarea\ final/
   ```

2. Instalar las dependencias usando `pip`:
   ```bash
   pip install -r requirements.txt
   ```

3. Abrir y ejecutar todas las celdas del notebook `final_anomaly.ipynb` en Jupyter Lab, VS Code o Google Colab.

---

## 5. Declaración de Uso de IA (AI-Use Disclosure)
Durante el desarrollo de este proyecto final, utilizamos **Gemini (Advanced)** como asistente de codificación de inteligencia artificial. La herramienta fue utilizada específicamente para:
* **Auditoría de Datos y Debugging**: Nos ayudó a identificar la discrepancia de escala (fracción vs. porcentaje) entre las comunas consolidando la base de datos de la clase.
* **Redacción de Código**: Asistencia en la generación de scripts de visualización con `seaborn` y `matplotlib`, incluyendo el cálculo y ajuste de anotaciones de outliers en la Headline Figure.
* **Estructuración y Formateo**: Ayuda en la estructuración de la narrativa del notebook y el formateo Markdown del README para cumplir con los estándares de entrega.

Toda la lógica analítica, la interpretación de los coeficientes, el diseño de las pruebas alternativas y las conclusiones fueron validadas y son responsabilidad de nuestro grupo.
