# EC. Primer Parcial — Proyecto Práctico HPC

**Asignatura:** Cómputo de Alto Rendimiento  
**Docente:** Javier Moya  
**Entrega:** Repositorio de GitHub  
**Modalidad:** Trabajo colaborativo  
**Metodología:** Git, GitHub y GitFlow  

---

## Integrantes

- **Integrante 1:** Elliot González
- **Integrante 2:** Diego Almaraz
- **Integrante 3:** Patricia Botello
- **Integrante 4:** Ignacio Rodríguez

---

# 1. Objetivo

El objetivo del proyecto es desarrollar y evaluar un programa en Python que compare una ejecución secuencial con una ejecución paralela sobre una cantidad considerable de datos.

Además, se aplican conceptos de:

- Cómputo de Alto Rendimiento (HPC);
- procesamiento secuencial y paralelo;
- medición de tiempos;
- Speedup;
- eficiencia;
- Git y GitHub;
- GitFlow y Pull Requests;
- reproducibilidad.

El objetivo no es asumir que una mayor cantidad de workers siempre produce mejores resultados, sino comprobarlo mediante mediciones.

---

# 2. Dataset

Se utiliza **Covertype (Forest CoverType)**, un dataset con aproximadamente 581 mil registros y 54 características de entrada.

No se utiliza para entrenar un modelo de Machine Learning. Sus características numéricas se emplean como entrada para un procesamiento matemático repetitivo.

Se eligió porque contiene suficientes datos y porque cada fila puede procesarse de manera independiente.

---

# 3. Operación matemática

Para cada fila se aplica:

\[
f(x) = \sum_{j=1}^{54} \left(\sqrt{x_j^2 + 1} + \sin(x_j)\right)
\]

Cada fila es independiente de las demás, por lo que el trabajo puede repartirse posteriormente entre varios workers.

---

# 4. Implementación secuencial

La versión secuencial procesa las filas una después de otra y funciona como referencia inicial.

El tiempo se mide con:

```python
time.perf_counter()
```

Las mediciones secuenciales se almacenan en:

```text
resultados/secuencial.csv
```

---

# 5. Implementación paralela

La versión paralela utiliza `ProcessPoolExecutor` para distribuir bloques de datos entre varios procesos.

Se prueban:

- 1 worker;
- 2 workers;
- 4 workers.

Cada configuración se ejecuta 3 veces.

Los resultados se almacenan en:

```text
resultados/paralelo.csv
```

---

# 6. Experimentación

A partir de las tres ejecuciones de cada configuración se calcula el tiempo promedio.

El Speedup se obtiene mediante:

\[
S_p = \frac{T_1}{T_p}
\]

donde `T1` es el tiempo promedio con 1 worker y `Tp` es el tiempo promedio con `p` workers.

La eficiencia se calcula mediante:

\[
E_p = \frac{S_p}{p}
\]

El análisis numérico se almacena en:

```text
resultados/analisis.csv
```

Los valores de Speedup y eficiencia se calculan mediante código, no manualmente.

---

# 7. Resultados actuales

Los tiempos reales registrados hasta esta etapa son:
# Análisis de rendimiento: procesamiento secuencial y paralelo

## Descripción

Este proyecto compara el rendimiento de una implementación secuencial y una implementación paralela para el procesamiento de datos.

La versión paralela utiliza varios workers para distribuir el procesamiento mediante `ProcessPoolExecutor`.

## Resultados

Se realizaron tres ejecuciones para cada configuración de workers:

- 1 worker
- 2 workers
- 4 workers

Los tiempos promedio obtenidos fueron:

| Configuración | Tiempo promedio (s) |
|---|---:|
| Secuencial | 2.626654 |
| Paralelo - 1 worker | 7.697102 |
| Paralelo - 2 workers | 4.947311 |
| Paralelo - 4 workers | 3.435260 |

El análisis basado en 1 worker como referencia es:

| Workers | Speedup | Eficiencia |
|---:|---:|---:|
| 1 | 1.0000 | 1.0000 |
| 2 | 1.5558 | 0.7779 |
| 4 | 2.2406 | 0.5602 |

Dentro de las configuraciones paralelas, 4 workers obtiene el menor tiempo. Sin embargo, con las mediciones actuales la versión secuencial sigue siendo más rápida que la versión paralela.

Esto es un resultado válido: la paralelización introduce costos adicionales relacionados con la creación de procesos, distribución de datos y combinación de resultados.

---

# 8. Gráficas

La etapa de gráficas utilizará los resultados anteriores para generar mediante Python:

- workers vs. tiempo de ejecución;
- workers vs. Speedup;
- workers vs. eficiencia.

Las gráficas pertenecen a la rama:

```text
feature/graficas
```

---

# 9. Estructura del proyecto

```text
Examencito/
│
├── notebooks/
│   ├── 01_secuencial.ipynb
│   ├── 02_paralelo.ipynb
│   ├── 03_experimentos.ipynb
│   └── 04_graficas.ipynb
│
├── resultados/
│   ├── secuencial.csv
│   ├── paralelo.csv
│   ├── analisis.csv
│   ├── tiempo_workers.png
│   ├── speedup.png
│   └── eficiencia.png
│
├── README.md
├── requirements.txt
└── .gitignore
```

El README es general para el proyecto. Antes del merge, cada rama `feature/...` puede contener únicamente los archivos que correspondan a su etapa.

---

# 10. División del trabajo

## Integrante 1 — `feature/secuencial`

- carga y preparación del dataset;
- operación matemática;
- procesamiento secuencial;
- medición del tiempo;
- validación.

## Integrante 2 — `feature/paralelo`

- implementación paralela;
- distribución del trabajo;
- ejecución con 1, 2 y 4 workers;
- registro de tiempos.

## Integrante 3 — `feature/experimentos`

- promedio de las 3 ejecuciones;
- cálculo de Speedup;
- cálculo de eficiencia;
- generación de `resultados/analisis.csv`;
- validación de métricas.

## Integrante 4 — `feature/graficas`

- gráficas de rendimiento;
- interpretación visual;
- apoyo en la documentación y conclusiones.

---

# 11. GitFlow

Las ramas principales son:

```text
main
develop
feature/...
```

El flujo esperado es:

```text
feature/* -> Pull Request -> develop -> Pull Request -> main
```

No se desarrolla directamente sobre `main`.

Cada integrante debe realizar como mínimo:

- 3 commits significativos;
- 1 rama `feature`;
- 1 Pull Request;
- 1 revisión de un Pull Request de otro integrante;
- participación directa en código.

---

# 12. Reproducibilidad

Instalar dependencias:

```bash
pip install -r requirements.txt
```

Abrir Jupyter:

```bash
jupyter notebook
```

Los notebooks deben ejecutarse en orden según la etapa correspondiente.

El dataset se descarga mediante código y los resultados experimentales se guardan en archivos CSV.

---

# 13. Análisis final pendiente

Antes del merge final hacia `main` se deberá responder:

1. ¿La ejecución paralela fue más rápida que la secuencial?
2. ¿Qué número de workers obtuvo el menor tiempo?
3. ¿Duplicar el número de workers duplicó el rendimiento? ¿Por qué?
4. ¿Por qué el problema seleccionado puede paralelizarse?
5. ¿En qué momento agregar más workers deja de ser beneficioso?
6. ¿Qué limitaciones tiene el hardware utilizado?
7. ¿Este experimento representa HPC o demuestra principios utilizados en HPC?

---

# 14. Estado antes del merge final

```text
[x] Implementación secuencial
[x] Implementación paralela
[x] 3 pruebas con 1 worker
[x] 3 pruebas con 2 workers
[x] 3 pruebas con 4 workers
[x] Promedios
[x] Speedup
[x] Eficiencia
[x] Integrar gráficas
[ ] Completar análisis final
[ ] Revisar Pull Requests
[ ] Merge de feature/* hacia develop
[ ] Pull Request develop -> main
```

---

# 15. Conclusión provisional

Los resultados muestran que aumentar el número de workers mejora el tiempo dentro de la implementación paralela, pero el incremento de rendimiento no es proporcional al número de workers.

Además, para este problema y estas mediciones, la versión secuencial continúa siendo más rápida que la versión paralela. Esto permite observar de forma práctica que paralelizar un programa tiene costos y que utilizar más procesos no garantiza automáticamente un mejor rendimiento.
## Speedup

Para analizar la escalabilidad de la versión paralela se tomó como referencia el tiempo de ejecución con 1 worker.

La fórmula utilizada es:

**Speedup = T1 / Tp**

donde `T1` corresponde al tiempo con 1 worker y `Tp` al tiempo con el número de workers evaluado.

| Workers | Speedup |
|---:|---:|
| 1 | 1.0000 |
| 2 | 1.5560 |
| 4 | 2.2406 |

Los resultados muestran que el Speedup aumenta al incrementar el número de workers. Con 4 workers se obtiene el mejor Speedup de las configuraciones evaluadas.

## Efficiency

La eficiencia se calculó mediante:

**Efficiency = Speedup / número de workers**

| Workers | Efficiency |
|---:|---:|
| 1 | 1.0000 |
| 2 | 0.7780 |
| 4 | 0.5602 |

La eficiencia disminuye conforme aumenta el número de workers. Esto indica que el incremento de recursos no produce un aumento proporcional del rendimiento.

## Comparación con la versión secuencial

Aunque la versión paralela mejora su tiempo de ejecución al utilizar más workers, en estas pruebas la implementación secuencial continúa siendo más rápida.

El tiempo promedio secuencial fue de aproximadamente **2.63 segundos**, mientras que la mejor configuración paralela, con 4 workers, obtuvo aproximadamente **3.44 segundos**.

Esto puede explicarse por el costo adicional asociado con la creación de procesos y con la distribución y recopilación de los datos.

## Conclusiones

Dentro de las configuraciones paralelas evaluadas, 4 workers representa la mejor alternativa, ya que obtiene el menor tiempo de ejecución y el mayor Speedup.

Sin embargo, para este problema y bajo las condiciones de la prueba, la implementación secuencial sigue siendo más rápida.

El análisis completo, incluyendo las tablas y las gráficas de tiempo, Speedup y Efficiency, se encuentra en:

`notebooks/03_analisis_graficas.ipynb`

Los resultados numéricos del análisis se encuentran en:

`resultados/analisis.csv`
