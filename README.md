# AST1-CEIA-UBA-TP3
Repositorio del Trabajo Práctico 3 de la materia **Análisis de Series de Tiempo I** de la Carrera de Especialización en Inteligencia Artificial (CEIA – UBA).

---

## 👥 Alumno

- **Emiliano Iparraguirre** (<emiliano.iparraguirre22@gmail.com>)  

---

## 📘 Descripción general

Este proyecto corresponde al **Trabajo Práctico N.º 3 de la asignatura Análisis de Series Temporales (AST1)**, del **posgrado en Inteligencia Artificial (CEIA – UBA)**.  
El desafío principal fue desarrollar un proceso completo de **análisis, modelado y evaluación de series temporales** utilizando datos semanales de ventas de productos elaborados (*fiambres y chacinados*), pertenecientes a una empresa del sector alimenticio.

A lo largo del trabajo se abordaron **distintas metodologías de pronóstico** (SARIMA, Holt-Winters, SARIMAX, Fourier y modelos con variables exógenas), evaluando su precisión mediante métricas estandarizadas y comparativas.

---

## 🔍 Objetivo del trabajo

El presente trabajo tiene como objetivo aplicar técnicas de **análisis de series de tiempo** para predecir la **demanda semanal del producto “Salamin picado fino – 1ra marca (Código 1)”** de una empresa nacional del sector alimenticio, especializada en la elaboración de fiambres y embutidos.  
El horizonte de proyección considerado es de **52 semanas (un año)**, utilizando datos históricos de ventas semanales comprendidos entre los años **2016 y 2020**.

A partir de los datos, se busca **evaluar y comparar distintos enfoques de modelado temporal** —incluyendo modelos **SARIMA**, **Holt-Winters (ETS)** y **SARIMAX**— con el fin de determinar cuál ofrece **mayor precisión y estabilidad predictiva**, contribuyendo a la **planificación de producción y abastecimiento** de la empresa.

### Pregunta de investigación

> ¿Cuál será la demanda semanal del producto Salamin picado fino (Código 1) durante las próximas 52 semanas y qué modelo de series temporales (SARIMA, SARIMAX o Holt-Winters) permite obtener el pronóstico más preciso para la empresa procesadora de alimentos?

---

## 🔍 Objetivos específicos

1. **Explorar y limpiar la serie original** (`y`) correspondiente al producto base (COD1).  
2. **Analizar la estacionariedad** mediante gráficos, test ADF/KPSS y primeras diferencias.  
3. **Evaluar transformaciones Box-Cox / logarítmicas** para estabilizar la varianza.  
4. **Ajustar distintos modelos de pronóstico:**
   - SARIMA manual y auto.arima con drift.
   - Holt-Winters (ETS aditivo y multiplicativo).
   - SARIMAX con términos de Fourier (estacionalidad larga).
   - SARIMAX con variables exógenas (productos complementarios y sustitutos).  
5. **Comparar el desempeño de los modelos** con métricas cuantitativas (RMSE, MAE, MAPE, R², % error <30%).  
6. **Visualizar los pronósticos y los intervalos de confianza** a 52 semanas.  
7. **Extraer conclusiones y lecciones aprendidas** sobre la aplicabilidad de cada técnica.

---

## 🧩 Modelos implementados

### 1️⃣ SARIMA
Modelos estacionales sin variables externas:
- `(1,1,1)(1,1,1)[52]`
- `(3,1,1)(1,0,1)[52]` con drift (auto.arima)

Estos modelos sirvieron como **línea base comparativa**, permitiendo evaluar el impacto de estacionalidad y drift.

---

### 2️⃣ Holt-Winters (ETS)
Modelos aditivo y multiplicativo con periodo estacional `s=52`.  
Se evaluaron variantes con y sin tendencia amortiguada.  
El mejor modelo (por AIC) fue el de tipo **aditivo con suavizado exponencial**, logrando **el menor RMSE (1611.67)** y **MAPE de 17.3%**.

---

### 3️⃣ SARIMAX + Fourier
Extiende SARIMA incorporando **componentes sinusoidales** para capturar estacionalidades largas.  
El número óptimo de pares seno/coseno (K) se seleccionó por BIC, resultando en un modelo con **MAPE ≈ 21.5%** y **buen equilibrio entre error y estabilidad**.

---

### 4️⃣ SARIMAX + Variables exógenas
Se incorporaron tres series externas provenientes de productos relacionados:
- **COD2** → Salamin picado grueso (1ra marca) — *producto complementario*.  
- **COD24** → Salamin picado fino (2da marca) — *producto sustituto*.  
- **COD25** → Salamin picado grueso (2da marca) — *producto sustituto del anterior*.

Las variables fueron **estandarizadas (z-score)** para garantizar comparabilidad de magnitudes.  
El modelo logró **MAPE ≈ 25%**, mostrando que las variables complementarias aportan valor, aunque la relación puede requerir rezagos o transformaciones no lineales para maximizar el beneficio predictivo.

---

## 📈 Evaluación comparativa

| Modelo                                | RMSE     | MAE      | MAPE (%) | R²     | % error < 30% | N_obs |
|--------------------------------------|----------:|----------:|----------:|-------:|---------------:|------:|
| SARIMA (1,1,1)(1,1,1)                | 3040.6 | 2686.2 | 37.36 | -2.61 | 26.9 | 52 |
| SARIMA (3,1,1)(1,0,1)[52] con drift  | 2530.4 | 2000.6 | 26.06 | -1.50 | 53.8 | 52 |
| Holt-Winters                         | 1611.7 | 1259.5 | 17.30 | -0.01 | 80.8 | 52 |
| SARIMAX + Fourier                    | 1872.9 | 1515.5 | 21.52 | -0.37 | 75.0 | 52 |
| SARIMAX + Exógenas                   | 2299.9 | 1873.9 | 25.02 | -1.06 | 57.7 | 52 |

📊 **Conclusión:**  
El modelo **Holt-Winters** alcanzó el mejor desempeño general, seguido de **SARIMAX + Fourier**, confirmando que las **técnicas de suavizado y estacionalidad flexible** son más eficaces para capturar la dinámica de la demanda semanal.

---

## 🧠 Conclusiones finales

- Los modelos con **estacionalidad explícita (Holt-Winters, Fourier)** mostraron la mejor capacidad de generalización.  
- Las **variables exógenas** aportaron información relevante, pero su impacto fue moderado; se sugiere explorar **rezagos** o **modelos híbridos (ML + econométricos)** en futuros trabajos.  
- Los **modelos SARIMA simples** funcionan como base sólida, pero con limitaciones para captar variaciones externas o de largo plazo.

> En síntesis: la serie presenta una estacionalidad clara, con fluctuaciones anuales y patrones recurrentes que los métodos de suavizado logran modelar con alta precisión.

---

## 🧩 Lecciones aprendidas y desafíos superados

- **Homogeneización temporal:** algunas series tenían semanas faltantes o diferencias de frecuencia; se resolvió mediante `asfreq('W-MON')` y `reindex()`.  
- **Estandarización de exógenas:** se aplicó z-score para evitar dominancia de variables con mayor escala.  
- **Optimización de modelos SARIMA:** se priorizó el criterio BIC y la convergencia estable, dado el alto costo computacional de combinaciones estacionales.  
- **Control de estacionariedad:** se emplearon pruebas ADF y KPSS en cada transformación.  
- **Interpretación de resultados:** se observó que las relaciones entre productos complementarios no siempre son lineales ni contemporáneas, lo que abre futuras líneas de análisis.