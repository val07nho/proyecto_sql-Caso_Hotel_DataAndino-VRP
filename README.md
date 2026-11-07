# Hotel DataAndino | Análisis de cancelaciones con SQL

**¿Cuánto le cuestan al hotel las reservas que nunca se concretan?**

Este proyecto analiza las cancelaciones de reservas hoteleras para cuantificar su impacto económico, identificar patrones de comportamiento y aportar evidencia para la revisión de contratos con agencias y políticas de cancelación.

Fue desarrollado como proyecto final del curso **SQL para Data Analyst — Data Academy Latam**, utilizando **SQL en Databricks** y un Job para ejecutar los notebooks de forma secuencial.

---

## 🎯 Problema de negocio

Hotel DataAndino paga a las agencias del canal `TA/TO` una comisión de **USD 1,50 por cada reserva generada**, incluso cuando posteriormente se cancela.

Además, según los supuestos del caso, cada cancelación realizada con **menos de tres días de anticipación** genera un costo operativo adicional de **USD 120**.

El análisis busca responder:

- ¿Cuánto se paga en comisiones por reservas canceladas?
- ¿Cuál es el costo estimado de las cancelaciones de última hora?
- ¿Qué países concentran más cancelaciones?
- ¿Cómo varía la tasa de cancelación según hijos, solicitudes especiales y modificaciones?
- ¿Qué tipo de hotel presenta el mayor precio promedio entre las reservas canceladas?

## 📊 Principales resultados

| Indicador | Resultado |
|---|---:|
| Total de reservas | 119 390 |
| Reservas canceladas | 44 224 |
| Tasa global de cancelación | 37,04 % |
| Reservas canceladas del canal TA/TO | 40 152 |
| Comisiones por reservas canceladas | USD 60 228 |
| Cancelaciones con menos de tres días de anticipación | 3 204 |
| Costo operativo estimado por cancelaciones tardías | USD 384 480 |
| **Costo combinado estimado bajo los supuestos del caso** | **USD 444 708** |

El costo combinado suma dos conceptos distintos: las comisiones de reservas canceladas y el costo operativo de las cancelaciones tardías. Una misma reserva puede generar ambos costos.

Estos importes corresponden al conjunto de datos analizado; no representan una estimación anual ni ingresos totales perdidos.

## 🔎 Hallazgos del análisis

### 1. La presencia de hijos presenta una diferencia moderada

| Segmento | Reservas | Tasa de cancelación |
|---|---:|---:|
| Sin hijos | 110 058 | 37,22 % |
| Con hijos | 9 332 | 34,92 % |

Las reservas con hijos registran **2,30 puntos porcentuales menos de cancelación**. La diferencia respalda una asociación con menor cancelación, pero no garantiza que la estancia se concrete.

### 2. Las solicitudes especiales se asocian con menor cancelación

| Solicitudes especiales | Reservas | Tasa de cancelación |
|---:|---:|---:|
| 0 | 70 318 | 47,72 % |
| 1 | 33 226 | 22,02 % |
| 2 | 12 969 | 22,10 % |
| 3 | 2 497 | 17,86 % |
| 4 | 340 | 10,59 % |
| 5 | 40 | 5,00 % |

Las reservas sin solicitudes presentan la mayor tasa de cancelación. La tendencia general disminuye al aumentar las solicitudes, aunque no lo hace en todos los pasos.

El segmento con cinco solicitudes contiene solo **40 reservas**, por lo que su resultado debe interpretarse con cautela.

### 3. Las reservas modificadas presentan menor cancelación

| Modificaciones | Reservas | Tasa de cancelación |
|---|---:|---:|
| Sin cambios | 101 314 | 40,85 % |
| De 1 a 3 cambios | 17 433 | 15,59 % |
| Más de 3 cambios | 643 | 18,04 % |

El grupo con **1 a 3 cambios** presenta la menor tasa de cancelación. Sin embargo, realizar más cambios no implica una reducción continua del riesgo.

### 4. El precio promedio de las cancelaciones es similar entre hoteles

| Tipo de hotel | Cancelaciones | ADR promedio de reservas canceladas |
|---|---:|---:|
| Resort Hotel | 11 122 | 105,79 |
| City Hotel | 33 102 | 104,69 |

Resort Hotel presenta el mayor ADR promedio entre las reservas canceladas, con una diferencia de **1,10** respecto a City Hotel.

Esta comparación no permite determinar si un precio más alto aumenta la probabilidad de cancelar. El ADR se presenta en las unidades del dataset, sin asumir que corresponde a USD.

### 5. Portugal concentra el mayor número de cancelaciones

| País | Reservas canceladas |
|---|---:|
| Portugal | 27 519 |
| Reino Unido | 2 453 |
| España | 2 177 |
| Francia | 1 934 |
| Italia | 1 333 |

Este ranking compara **cantidades**, no tasas. Un mayor volumen de cancelaciones puede estar relacionado con un mayor volumen de reservas.

## 💡 Recomendaciones de negocio

- **Revisar los contratos con agencias:** evaluar comisiones vinculadas a estancias concretadas o mecanismos de devolución por cancelaciones.
- **Evaluar medidas frente a cancelaciones tardías:** probar garantías y confirmaciones previas, midiendo su efecto sobre costos y conversión.
- **Combinar indicadores para el seguimiento:** utilizar solicitudes especiales, modificaciones y otras características para orientar análisis posteriores, sin asumir que un único factor predice la cancelación.
- **Profundizar en los mercados de mayor volumen:** comparar tasas y costos por país antes de aplicar políticas diferenciadas.

Las recomendaciones son propuestas para evaluar. El proyecto no mide todavía el ahorro real que produciría su implementación.

## 🛠️ Herramientas y conocimientos aplicados

| Herramienta | Uso |
|---|---|
| SQL de Databricks | Consultas, transformaciones y análisis |
| Databricks | Ejecución de notebooks y almacenamiento de tablas |
| Delta Lake | Tablas de trabajo |
| Python, pandas y Spark | Carga inicial del CSV |
| Databricks Jobs | Orquestación secuencial de notebooks |
| GitHub | Versionado y documentación |

Las respuestas a los **20 ejercicios** se desarrollaron en SQL. Python se utilizó únicamente para la carga inicial.

**Conceptos SQL utilizados:**

- Creación, inserción, actualización y eliminación de datos.
- Filtros con `WHERE`, `LIKE` y `BETWEEN`.
- Agregaciones con `COUNT`, `SUM` y `AVG`.
- Segmentación con `CASE WHEN`.
- Agrupación y ordenamiento de resultados.
- Conversión de tipos y construcción de fechas.
- Funciones `TO_DATE`, `DATE_SUB`, `DATEDIFF`, `ROUND` y `CEIL`.
- `LEFT JOIN`, `INNER JOIN`, `FULL OUTER JOIN` y `RIGHT JOIN`.
- Validación de unicidad en las claves utilizadas para los cruces.

## 📁 Organización del proyecto

Los notebooks se encuentran en la carpeta [`Querys/`](./Querys/) y cubren las siguientes etapas:

| Etapa | Contenido |
|---|---|
| Creación de tablas | Carga del dataset y preparación de las tablas |
| Validación de datos | Revisiones de apoyo sobre estructura y campos relevantes |
| Parte 1 | Ejercicios 1–10: exploración, transformaciones y modificaciones |
| Parte 2 | Ejercicios 11–16: impacto financiero y JOINs |
| Parte 3 | Ejercicios 17–20: hipótesis y conclusiones |

Las conclusiones se documentan junto con las consultas y sus resultados.

## ⚙️ Pipeline de ejecución

Se configuró un Job con las siguientes dependencias:

1. `Creacion_de_Tablas`
2. `Validacion_Datos`
3. `Seccion_1`
4. `Seccion_2`
5. `Seccion_3`

Cada tarea depende de la finalización exitosa de la anterior. Se verificó una ejecución completa con las cinco tareas en estado **Succeeded**.

Para evitar acumulaciones al repetir el proceso, el catálogo `tb_paises` se recrea antes de insertar sus registros. La comprobación posterior debe mostrar **177 filas y 177 códigos distintos**.

## 🧹 Criterios de preparación y calidad

- Se conservó `hotel_bookings` como tabla de origen y se utilizó `tb_reservas` para el análisis.
- Las modificaciones de bebés y la eliminación de precios cero se aplicaron únicamente a `tb_reservas_prueba`.
- Se construyó `arrival_date` a partir del año, mes y día de llegada.
- `reservation_status_date` se interpretó con el formato `d/M/yy`.
- Para las cancelaciones tardías, se comprobó que no existieran diferencias negativas entre llegada y cancelación.
- Se verificó la unicidad de `tb_paises.cod_pais` para evitar multiplicar reservas durante los JOINs.
- Se conservaron las correspondencias `CN` y `CHN` con China, y `TMP` con Timor Oriental.

Se identificaron **cuatro reservas con `children` nulo y `babies = 0`**. La regla del ejercicio las clasifica como “Sin Hijos”; se documentó esta limitación y su impacto mínimo sobre la comparación.

## 🧪 Simulaciones de vendedores y facturas

El dataset original no contiene identificadores de reserva, asignaciones de vendedores ni facturas.

Para demostrar los ejercicios de `FULL OUTER JOIN` y `RIGHT JOIN`, se crearon tablas con **datos ficticios identificados como simulación**:

- `tb_reservas_simuladas`
- `tb_vendedores`
- `tb_facturas`

Estas tablas permiten representar reservas sin vendedor, vendedores sin reservas y facturas sin una reserva coincidente. **No se utilizaron para calcular los indicadores ni las conclusiones económicas del hotel.**

## ▶️ Cómo reproducir el proyecto

1. Obtener el CSV desde la fuente indicada.
2. Importar los notebooks en un entorno Databricks con permisos de lectura y escritura.
3. Adaptar la ruta del archivo en el notebook de carga.
4. Crear o seleccionar el catálogo y esquema de trabajo.
5. Ajustar las referencias a `cursodataacademy.proyecto_sql_vnrp` si se utiliza otro entorno.
6. Ejecutar los notebooks en el orden indicado o configurar un Job con las mismas dependencias.
7. Revisar los resultados y verificar que el catálogo de países mantenga una fila por código.

El repositorio contiene el código y la documentación. La ejecución requiere un entorno Databricks propio o acceso autorizado al entorno de trabajo.

## 📌 Alcance y limitaciones

- El análisis es descriptivo: identifica asociaciones, no relaciones causales.
- Los costos de USD 1,50 y USD 120 son supuestos del caso académico.
- No se construyó un modelo predictivo ni se evaluaron intervenciones comerciales.
- Los segmentos con pocos registros requieren cautela.
- Los resultados corresponden al dataset analizado y no deben generalizarse automáticamente a otros hoteles o periodos.

## 📚 Fuente de datos

[Hotel Bookings — Análisis de cancelaciones hoteleras, Kaggle](https://www.kaggle.com/datasets/datacertlaboratoria/sql-proyecto-1-analisis-de-cancelaciones-hoteleras?select=hotel_bookings.csv)

El archivo cargado contiene **119 390 registros y 31 columnas originales**.

## 👩‍💻 Autoría

**Valeria Nahomie Ramos Poquioma**.