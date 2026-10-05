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
