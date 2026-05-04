# 📊 Proyecto de Análisis de Producción Industrial

Proyecto de análisis de datos enfocado en la optimización de una línea de producción industrial de piezas metálicas, en un entorno similar al sector automoción.

El objetivo principal del proyecto es identificar ineficiencias operativas, analizar mermas, estudiar paradas de producción y detectar oportunidades de mejora mediante **SQL, Power BI y análisis de negocio**.

Este documento recoge el workflow completo del proyecto, incluyendo el diseño del dataset, la limpieza técnica, la validación de datos, el análisis exploratorio, las medidas DAX y las decisiones tomadas para construir el dashboard final.

---

## Índice

1. Definición del problema
2. Objetivo y preguntas de negocio
3. Diseño del dataset
4. Creación / obtención de datos
5. Limpieza de datos en SQL
6. Validación de datos
7. Preparación técnica para Power BI
8. Análisis exploratorio inicial
9. Análisis específico de negocio
10. Visualización en Power BI
11. Estado actual del proyecto
12. Próximos pasos

---

# 🟩 BLOQUE 1: Definición del problema

## Contexto del proyecto

El proyecto analiza una línea de producción industrial compuesta por cinco fases principales:

* Corte
* Mecanizado
* Tratamiento térmico
* Control de calidad
* Expedición

Cada fase puede presentar ineficiencias que afectan al rendimiento global de la producción, a la calidad del producto final y al cumplimiento de los plazos de entrega.

## Problema principal

La producción presenta ineficiencias reflejadas en tres áreas principales:

* Paradas frecuentes en determinadas fases del proceso.
* Diferencias de rendimiento entre turnos.
* Generación de mermas o piezas defectuosas.

Estas ineficiencias reducen la productividad y generan costes adicionales para la empresa.

## Impacto en negocio

Las ineficiencias detectadas pueden provocar:

* Reducción de la producción útil.
* Pérdida de tiempo operativo.
* Incremento de costes por mermas.
* Disminución de la calidad del producto final.
* Posibles retrasos en entregas.
* Dificultad para identificar el origen real de los problemas si no se analizan los datos correctamente.

## Enfoque analítico

El análisis se plantea desde una perspectiva de negocio, utilizando datos para:

* Detectar patrones de ineficiencia.
* Identificar fases críticas del proceso.
* Analizar la relación entre paradas, turnos, líneas de producción y calidad.
* Diferenciar puntos de detección de posibles puntos de origen.
* Proponer recomendaciones basadas en evidencia.

---

# 🟩 BLOQUE 2: Objetivo y preguntas de negocio

## Objetivo general

Analizar el rendimiento de la línea de producción para identificar ineficiencias, reducir mermas y optimizar el uso de recursos en las distintas fases del proceso productivo.

## Objetivos específicos

* Evaluar la eficiencia de cada fase del proceso.
* Analizar la frecuencia y el impacto de las paradas.
* Identificar diferencias de rendimiento entre turnos.
* Medir el nivel de mermas y su impacto en la producción.
* Detectar posibles cuellos de botella.
* Comparar el comportamiento de las líneas de producción.
* Crear un dashboard que permita transformar los datos en decisiones.

## Preguntas de negocio

### Producción

* ¿Cuál es la producción media por turno?
* ¿Qué fase del proceso es menos eficiente?
* ¿Dónde se pierde más tiempo productivo?

### Paradas

* ¿Cuántas paradas se producen en cada fase?
* ¿Qué fases concentran más paradas?
* ¿Cuáles son los principales motivos de parada?
* ¿Existen patrones de paradas según el turno?

### Mermas

* ¿Qué porcentaje de la producción total se convierte en merma?
* ¿En qué fase se generan más productos defectuosos?
* ¿Existe relación entre paradas y aumento de mermas?
* ¿Qué turno presenta mayor tasa de defectos?

### Turnos

* ¿Qué turno produce más?
* ¿Qué turno es más eficiente?
* ¿El turno de noche realmente es menos eficiente?

### Líneas de producción

* ¿Línea A, B o C produce mejor?
* ¿Qué línea tiene más merma?
* ¿Hay alguna línea claramente problemática?

## Pregunta central del proyecto

> ¿Cómo podemos mejorar la eficiencia de la producción reduciendo las paradas y las mermas en las distintas fases del proceso?

---

# 🟩 BLOQUE 3: Diseño del dataset

## Tabla principal

Se diseñó una única tabla principal:

```sql
produccion_linea
```

Cada fila representa una operación de producción en una fase concreta del proceso.

## Variables del dataset

### Identificación

| Variable           | Descripción                         |
| ------------------ | ----------------------------------- |
| `id_registro`      | Identificador único del registro    |
| `fecha`            | Fecha de la operación               |
| `turno`            | Turno de trabajo                    |
| `linea_produccion` | Línea donde se realizó la operación |

### Proceso

| Variable | Descripción                 |
| -------- | --------------------------- |
| `fase`   | Fase del proceso productivo |

Valores esperados:

* `corte`
* `mecanizado`
* `tratamiento_termico`
* `control_calidad`
* `expedicion`

### Producción

| Variable               | Descripción                    |
| ---------------------- | ------------------------------ |
| `unidades_producidas`  | Número de unidades producidas  |
| `unidades_defectuosas` | Número de unidades defectuosas |

### Tiempos

| Variable                | Descripción                     |
| ----------------------- | ------------------------------- |
| `tiempo_produccion_min` | Tiempo de producción en minutos |
| `tiempo_parada_min`     | Tiempo de parada en minutos     |

### Paradas

| Variable        | Descripción                |
| --------------- | -------------------------- |
| `hubo_parada`   | Indica si hubo parada o no |
| `motivo_parada` | Motivo de la parada        |

### Personal

| Variable      | Descripción                |
| ------------- | -------------------------- |
| `operario_id` | Identificador del operario |

## Decisiones de diseño

* Se trabajó con una única tabla para simplificar el análisis inicial.
* Se incluyó `fase` para analizar el rendimiento por etapa.
* Se añadieron `unidades_defectuosas` para medir mermas.
* Se incorporaron variables relacionadas con paradas para analizar su impacto operativo.
* Se incluyó `turno` para comparar rendimiento entre franjas de trabajo.
* Se incluyó `linea_produccion` para detectar si el problema estaba concentrado en una línea concreta.
* La estructura se diseñó pensando en su posterior uso en SQL y Power BI.

---

# 🟩 BLOQUE 4: Creación / obtención de datos

Se generó un dataset simulado de más de 5000 registros, intencionadamente sucio, para representar un entorno real de trabajo.

## Problemas incluidos en el dataset

El dataset se generó con errores deliberados para simular problemas habituales en datos reales:

* Valores nulos.
* Duplicados.
* Errores de formato.
* Inconsistencias en texto.
* Datos incoherentes.
* Categorías escritas con variantes.
* Fechas en distintos formatos.
* Campos booleanos no normalizados.

## Objetivo de esta decisión

El objetivo no era trabajar con un dataset perfecto, sino practicar un flujo realista de análisis:

* Importación de datos sucios.
* Diagnóstico inicial.
* Limpieza técnica.
* Normalización.
* Validación.
* Análisis exploratorio.
* Visualización en Power BI.

---

# 🟩 BLOQUE 5: Limpieza de datos en SQL

La limpieza se realizó en **PostgreSQL** utilizando **pgAdmin4**.

## 5.1 Creación de tabla raw

Primero se creó una tabla raw con todos los campos en formato `TEXT`.

Esta decisión permitió importar el CSV aunque existieran errores de formato, fechas mal escritas o valores no compatibles con tipos numéricos.

```sql
CREATE TABLE produccion_linea (
    id_registro TEXT,
    fecha TEXT,
    turno TEXT,
    linea_produccion TEXT,
    fase TEXT,
    unidades_producidas TEXT,
    unidades_defectuosas TEXT,
    tiempo_produccion_min TEXT,
    tiempo_parada_min TEXT,
    hubo_parada TEXT,
    motivo_parada TEXT,
    operario_id TEXT
);
```

## 5.2 Exploración inicial

Se revisó la carga de datos con:

```sql
SELECT COUNT(*) AS total_filas
FROM produccion_linea;
```

Resultado inicial:

```text
5201 filas
```

También se visualizaron las primeras filas:

```sql
SELECT *
FROM produccion_linea
LIMIT 10;
```

## 5.3 Problemas detectados

Durante la exploración inicial se detectaron los siguientes problemas:

* Nulos en `linea_produccion`, `motivo_parada`, `operario_id` y variables numéricas.
* Fechas en múltiples formatos.
* Errores tipográficos en `turno`.
* Variantes de texto en `fase`.
* Valores inconsistentes en `hubo_parada`.
* Motivos de parada escritos de diferentes formas.
* Valores incoherentes en producción y defectos.

Ejemplos de variantes detectadas en `motivo_parada`:

* `error operario`, `error_operario`, `Error_operario`
* `fallo máquina`, `fallo_maquina`, `fallomaquina`
* `falta material`, `falta_material`, `faltamaterial`
* `mantenimiento`, `mantenimienot`, `Mantenimiento`

---

# 🟩 BLOQUE 5.1: Creación de tabla limpia

Después de importar la tabla raw, se creó una tabla limpia con tipos de datos adecuados.

```sql
CREATE TABLE produccion_limpia (
    id_registro INTEGER,
    fecha DATE,
    turno VARCHAR(10),
    linea_produccion CHAR(1),
    fase VARCHAR(20),
    unidades_producidas INTEGER,
    unidades_defectuosas INTEGER,
    tiempo_produccion_min INTEGER,
    tiempo_parada_min INTEGER,
    hubo_parada BOOLEAN,
    motivo_parada VARCHAR(20),
    operario_id VARCHAR(10),
    fecha_carga TIMESTAMP DEFAULT NOW(),
    registro_con_errores BOOLEAN DEFAULT FALSE
);
```

## Estrategia de limpieza aplicada

Se aplicaron las siguientes acciones:

* Conversión de fechas a tipo `DATE`.
* Normalización de texto con `LOWER`, `TRIM` y reemplazos.
* Estandarización de categorías.
* Validación de datos numéricos.
* Conversión de `hubo_parada` a booleano.
* Corrección de motivos de parada.
* Conversión de valores incoherentes a `NULL`.

## Decisiones clave de limpieza

| Situación detectada                      | Decisión aplicada                         |
| ---------------------------------------- | ----------------------------------------- |
| Dato inválido                            | Convertir a `NULL`                        |
| Registro con algún campo inválido        | No eliminar si todavía aporta información |
| Defectos mayores que unidades producidas | Convertir a `NULL`                        |
| Tiempos irreales                         | Convertir a `NULL`                        |
| Parada sin motivo                        | Etiquetar como `desconocido`              |
| Línea de producción no informada         | Mantener como nulo inicialmente           |

## Justificación

Se priorizó la **calidad del dato frente a la completitud**.

Eliminar todos los registros con algún problema habría reducido el dataset y eliminado información útil. Por eso se optó por conservar registros válidos parcialmente y marcar como `NULL` los valores que no eran fiables.

---

# 🟩 BLOQUE 5.2: Validación de datos

Tras la limpieza se realizó una validación final para comprobar el estado del dataset.

## Validación de nulos

```sql
SELECT
    COUNT(*) AS total_filas,
    COUNT(*) FILTER (WHERE fecha IS NULL) AS nulos_fecha,
    COUNT(*) FILTER (WHERE turno IS NULL) AS nulos_turno,
    COUNT(*) FILTER (WHERE linea_produccion IS NULL) AS nulos_linea,
    COUNT(*) FILTER (WHERE fase IS NULL) AS nulos_fase,
    COUNT(*) FILTER (WHERE unidades_producidas IS NULL) AS nulos_up,
    COUNT(*) FILTER (WHERE unidades_defectuosas IS NULL) AS nulos_ud,
    COUNT(*) FILTER (WHERE tiempo_produccion_min IS NULL) AS nulos_tp,
    COUNT(*) FILTER (WHERE tiempo_parada_min IS NULL) AS nulos_tparada,
    COUNT(*) FILTER (WHERE hubo_parada IS NULL) AS nulos_hubo_parada,
    COUNT(*) FILTER (WHERE motivo_parada IS NULL AND hubo_parada = TRUE) AS nulos_motivo_con_parada,
    COUNT(*) FILTER (WHERE operario_id IS NULL) AS nulos_operario
FROM produccion_limpia;
```

## Resultado final antes del último ajuste

| Validación                          | Resultado |
| ----------------------------------- | --------: |
| Total filas                         |      4673 |
| Nulos en `fecha`                    |         0 |
| Nulos en `turno`                    |         0 |
| Nulos en `fase`                     |         0 |
| Nulos en `hubo_parada`              |         0 |
| Nulos en `linea_produccion`         |       146 |
| Nulos en `unidades_producidas`      |       241 |
| Nulos en `unidades_defectuosas`     |       289 |
| Nulos en `tiempo_produccion_min`    |        91 |
| Nulos en `tiempo_parada_min`        |        18 |
| Nulos en `motivo_parada` con parada |        74 |
| Nulos en `operario_id`              |       201 |

## Decisión sobre paradas sin motivo

Se detectaron 74 registros donde existía una parada, pero no se había registrado el motivo.

En lugar de eliminarlos, se etiquetaron como `desconocido`.

```sql
UPDATE produccion_limpia
SET motivo_parada = 'desconocido'
WHERE motivo_parada IS NULL
AND hubo_parada = TRUE;
```

Resultado:

```text
UPDATE 74
```

## Validación de coherencia

Se comprobó que no existieran unidades defectuosas superiores a las producidas:

```sql
SELECT *
FROM produccion_limpia
WHERE unidades_defectuosas > unidades_producidas;
```

Resultado:

```text
0 filas
```

También se comprobó que no existieran paradas sin motivo:

```sql
SELECT *
FROM produccion_limpia
WHERE motivo_parada IS NULL
AND hubo_parada = TRUE;
```

Resultado:

```text
0 filas
```

## Conclusión de limpieza

El dataset final queda:

* Limpio.
* Validado.
* Coherente.
* Preparado para análisis exploratorio.
* Preparado para visualización en Power BI.

Los valores nulos restantes se consideran controlados y justificados, ya que provienen de datos incoherentes o variables dependientes.

---

# 🟩 BLOQUE 5.3: Preparación técnica para Power BI

Se exportó la tabla `produccion_limpia` a CSV y se revisó en Excel antes de cargarla en Power BI.

## Ajustes realizados

* Revisión de cabeceras.
* Comprobación de columnas.
* Revisión de valores nulos.
* Transformación de valores booleanos.
* Conversión de `t/f` a `VERDADERO/FALSO`.
* Carga del dataset en Power BI.
* Conversión de `hubo_parada` a tipo booleano en Power Query.

## Resultado

El dataset quedó correctamente cargado en Power BI con:

* Columnas detectadas correctamente.
* Tipos numéricos adecuados.
* Fechas reconocidas.
* Booleanos preparados.
* Estructura lista para análisis visual.

---

# 🟩 BLOQUE 6: Análisis exploratorio inicial (EDA)

El análisis exploratorio inicial se realizó en SQL para entender el comportamiento general de los datos.

---

## 6.1 KPI global

```sql
SELECT
    COUNT(*) AS total_registros,
    SUM(unidades_producidas) AS total_unidades_producidas,
    SUM(unidades_defectuosas) AS total_unidades_defectuosas,
    SUM(unidades_producidas - unidades_defectuosas) AS total_unidades_validas,
    ROUND(AVG(unidades_producidas), 2) AS media_unidades_producidas,
    ROUND(AVG(unidades_defectuosas), 2) AS media_unidades_defectuosas,
    ROUND(
        SUM(unidades_defectuosas)::NUMERIC / NULLIF(SUM(unidades_producidas), 0) * 100,
        2
    ) AS porcentaje_merma_global
FROM produccion_limpia
WHERE unidades_producidas IS NOT NULL
AND unidades_defectuosas IS NOT NULL;
```

## Resultados

| Métrica              |          Resultado |
| -------------------- | -----------------: |
| Registros analizados |               4281 |
| Producción total     | 1.591.258 unidades |
| Defectos             |    70.876 unidades |
| Producción útil      | 1.520.382 unidades |
| Media producción     |       371 unidades |
| Media defectos       |     16,56 unidades |
| Merma global         |              4,45% |

## Insight

Aunque la merma global parece baja en porcentaje, supone más de 70.000 unidades defectuosas. Esto indica que las pérdidas no son puntuales, sino estructurales dentro del proceso productivo.

---

## 6.2 Merma por fase

| Fase                  | Merma |
| --------------------- | ----: |
| `control_calidad`     | 7,32% |
| `mecanizado`          | 4,80% |
| `tratamiento_termico` | 4,02% |
| `corte`               | 3,06% |
| `expedicion`          | 2,29% |

## Insights

* `control_calidad` presenta la mayor merma.
* Esta fase actúa como punto de detección, no necesariamente como origen del problema.
* `mecanizado` aparece como posible fase crítica previa.
* `expedicion` muestra el mejor rendimiento.

---

## 6.3 Paradas generales

| Estado     | Porcentaje |
| ---------- | ---------: |
| Sin parada |     77,10% |
| Con parada |     22,90% |

## Insight

Aproximadamente 1 de cada 4 registros presenta paradas. Esto sugiere que las interrupciones no son incidencias aisladas, sino un problema estructural.

---

## 6.4 Motivos de parada

| Motivo           | Porcentaje |
| ---------------- | ---------: |
| `error_operario` |     25,61% |
| `falta_material` |     23,08% |
| `mantenimiento`  |     22,24% |
| `fallo_maquina`  |     22,15% |
| `desconocido`    |      6,92% |

## Insights

* El principal motivo de parada es `error_operario`.
* Las causas están repartidas entre factores humanos, técnicos y logísticos.
* `mantenimiento` + `fallo_maquina` representan casi el 44% de las paradas.
* La categoría `desconocido` evidencia limitaciones en la captura de datos.

---

## 6.5 Análisis por turno

| Turno  | Merma |
| ------ | ----: |
| Tarde  | 4,36% |
| Mañana | 4,33% |
| Noche  | 4,30% |

## Insights

* La merma es estable entre turnos.
* El problema de calidad no depende directamente del horario.
* El turno noche produce menos, pero no mejora significativamente la calidad.
* Menos producción no implica mejor calidad.

---

# 🟩 BLOQUE 7: Análisis específico de negocio

En este bloque se profundiza en preguntas concretas de negocio usando Power BI y medidas DAX.

---

## 7.1 Medidas DAX creadas

### Producción total

```DAX
Produccion Total = SUM(produccion_tabla_clean[unidades_producidas])
```

Calcula todas las unidades producidas del dataset.

### Defectos totales

```DAX
Defectos Totales = SUM(produccion_tabla_clean[unidades_defectuosas])
```

Calcula todas las unidades defectuosas registradas.

### Unidades válidas

```DAX
Unidades Validas = [Produccion Total] - [Defectos Totales]
```

Calcula la producción útil, descontando los defectos.

### Tiempo total

```DAX
Tiempo Total Min =
SUM(produccion_tabla_clean[tiempo_produccion_min])
+ SUM(produccion_tabla_clean[tiempo_parada_min])
```

Suma el tiempo productivo y el tiempo de parada.

### Eficiencia

```DAX
Eficiencia = DIVIDE([Unidades Validas], [Tiempo Total Min], 0)
```

Calcula cuántas unidades válidas se producen por minuto.

### % Merma

```DAX
% Merma = DIVIDE([Defectos Totales], [Produccion Total], 0)
```

Calcula la tasa de defectos sobre la producción total.

### Total paradas

```DAX
Total Paradas =
CALCULATE(
    COUNTROWS(produccion_tabla_clean),
    produccion_tabla_clean[hubo_parada] = TRUE()
)
```

Cuenta los registros en los que hubo una parada.

### % Paradas

```DAX
% Paradas =
DIVIDE(
    [Total Paradas],
    COUNTROWS(produccion_tabla_clean),
    0
)
```

Calcula el porcentaje de registros con parada.

---

## 7.2 Eficiencia por turno

### Preguntas respondidas

* ¿Qué turno produce más?
* ¿Qué turno tiene mejor relación entre producción y defectos?
* ¿El turno noche realmente es menos eficiente?

### Resultados

| Turno  | Producción total | Defectos | Unidades válidas | Tiempo total | Eficiencia | % Merma |
| ------ | ---------------: | -------: | ---------------: | -----------: | ---------: | ------: |
| Tarde  |          547.782 |   23.866 |          523.916 |      540.498 |       0,97 |   4,36% |
| Mañana |          619.414 |   26.816 |          592.598 |      631.385 |       0,94 |   4,33% |
| Noche  |          480.869 |   20.677 |          460.192 |      603.007 |       0,76 |   4,30% |

### Conclusiones

* El turno de mañana produce más.
* El turno de tarde es el más eficiente.
* El turno de noche tiene la merma más baja, pero también la eficiencia más baja.
* El problema del turno noche no parece estar en la calidad, sino en la eficiencia operativa y el volumen producido.

---

## 7.3 Defectos y mermas por fase

### Preguntas respondidas

* ¿Qué fase concentra más defectos?
* ¿Qué fase debería revisarse primero?
* ¿Dónde puede estar el origen del problema?

### Resultados

| Fase                  | Producción total | Defectos | Unidades válidas | % Merma | % defectos sobre total |
| --------------------- | ---------------: | -------: | ---------------: | ------: | ---------------------: |
| `control_calidad`     |          374.355 |   27.482 |          346.873 |   7,34% |                    39% |
| `mecanizado`          |          260.250 |   12.613 |          247.637 |   4,85% |                    18% |
| `tratamiento_termico` |          298.925 |   12.114 |          286.811 |   4,05% |                    17% |
| `corte`               |          337.768 |   10.438 |          327.330 |   3,09% |                    15% |
| `expedicion`          |          377.667 |    8.712 |          368.955 |   2,31% |                    12% |

### Conclusiones

* `control_calidad` concentra más defectos: 27.482 unidades.
* Representa aproximadamente el 39% del total de defectos.
* Debe revisarse primero como punto de detección.
* El posible origen se encuentra en fases anteriores, especialmente `mecanizado`.

### Insight

`control_calidad` detecta el problema, pero no necesariamente lo genera. `mecanizado` aparece como una fase crítica por su merma elevada, su volumen de defectos y su naturaleza técnica.

---

## 7.4 Análisis de paradas

### Preguntas respondidas

* ¿Qué motivos generan más impacto?
* ¿Dónde se concentran más paradas?
* ¿Las paradas afectan a la merma?

### 7.4.1 Motivos que generan más impacto

El principal motivo de parada es:

* `error_operario`

Después aparecen, con valores similares:

* `falta_material`
* `mantenimiento`
* `fallo_maquina`

Finalmente aparece:

* `desconocido`

### Insight

El principal motivo está relacionado con errores de operario, lo que sugiere oportunidades de mejora en formación, procedimientos o supervisión.

Además, las causas están repartidas entre factores humanos, técnicos y logísticos, por lo que no existe una única causa aislada.

---

### 7.4.2 Paradas por fase

| Fase                  | Paradas |
| --------------------- | ------: |
| `mecanizado`          |     363 |
| `tratamiento_termico` |     234 |
| `corte`               |     207 |
| `control_calidad`     |     142 |
| `expedicion`          |     124 |

### Conclusión

Las paradas se concentran principalmente en `mecanizado`, con 363 interrupciones.

### Insight

`mecanizado` es una fase crítica desde el punto de vista operativo. Además, al coincidir con una merma elevada, refuerza la hipótesis de que parte de los defectos detectados en control de calidad pueden originarse en fases técnicas anteriores.

---

### 7.4.3 Relación entre paradas y merma

| Hubo parada | Producción total | Defectos | % Merma |
| ----------- | ---------------: | -------: | ------: |
| False       |        1.292.415 |   55.598 |   4,30% |
| True        |          355.650 |   15.761 |   4,43% |

### Conclusión

Los registros con parada tienen una merma ligeramente superior.

### Insight

La diferencia no es muy elevada, pero sugiere que las interrupciones pueden afectar negativamente a la calidad del proceso productivo.

---

## 7.5 Comparación entre líneas de producción

### Preguntas respondidas

* ¿Línea A, B o C produce mejor?
* ¿Qué línea tiene más merma?
* ¿Hay alguna línea claramente problemática?

Para gestionar registros sin línea informada, se creó una columna calculada:

```DAX
Linea Produccion Limpia =
VAR Linea = TRIM(produccion_tabla_clean[linea_produccion])
RETURN
IF(
    ISBLANK(Linea) || Linea = "",
    "No registrada",
    Linea
)
```

### Resultados

| Línea         | Producción total | Defectos | Unidades válidas | % Merma | Eficiencia | Paradas |
| ------------- | ---------------: | -------: | ---------------: | ------: | ---------: | ------: |
| A             |          559.554 |   24.302 |          535.252 |   4,34% |       0,89 |     355 |
| B             |          521.641 |   23.069 |          498.572 |   4,42% |       0,87 |     359 |
| C             |          514.246 |   21.827 |          492.419 |   4,24% |       0,90 |     322 |
| No registrada |           52.624 |    2.161 |           50.463 |   4,11% |       0,90 |      34 |

### Conclusiones

* La línea A produce más.
* La línea C tiene la mejor eficiencia.
* La línea B tiene la mayor merma.
* La línea B también tiene la menor eficiencia y el mayor número de paradas.
* Por tanto, la línea B es la más problemática y debería revisarse con prioridad.

### Nota sobre `No registrada`

La categoría `No registrada` corresponde a registros sin información de línea de producción. Se mantiene para conservar trazabilidad, pero no se interpreta como una línea real.

---

## 7.6 Cierre del bloque

### ¿Dónde está el mayor problema de calidad?

El mayor problema de calidad se detecta en `control_calidad`, con la mayor tasa de merma y el mayor volumen de defectos.

Sin embargo, el posible origen puede encontrarse en fases anteriores, especialmente `mecanizado`.

### ¿Las paradas influyen en la merma?

Sí. Los registros con parada presentan una merma ligeramente superior a los registros sin parada.

### ¿Hay alguna línea que requiere atención prioritaria?

Sí. La línea B requiere atención prioritaria porque combina:

* Mayor porcentaje de merma.
* Menor eficiencia.
* Mayor número de paradas.

### Recomendaciones de negocio

* Revisar la fase de `mecanizado`.
* Analizar en detalle la línea B.
* Reforzar la formación de operarios.
* Mejorar procedimientos operativos.
* Optimizar el mantenimiento preventivo.
* Revisar la planificación de materiales.
* Mejorar el registro de datos, especialmente los casos `desconocido`.

### Conclusión del bloque

El problema no se concentra en un único punto. Responde a una combinación de factores técnicos, humanos y operativos.

Los principales focos de atención son:

* `control_calidad` como punto de detección.
* `mecanizado` como posible origen operativo.
* Línea B como línea prioritaria de revisión.

---

# 🟩 BLOQUE 8: Visualización en Power BI

## Objetivo del bloque

Convertir los análisis realizados en un dashboard claro, visual e interactivo que permita comunicar los insights de negocio.

El dashboard debe ayudar a responder rápidamente:

* Dónde se concentran los defectos.
* Qué fases generan más problemas.
* Qué motivos explican las paradas.
* Qué líneas requieren atención.
* Qué acciones de mejora pueden priorizarse.

## Estado inicial antes del dashboard

El dataset ya estaba:

* Limpio en SQL.
* Validado.
* Exportado correctamente.
* Cargado en Power BI.
* Tipado correctamente.
* Preparado para visualización.

---

## 8.1 KPIs principales del dashboard

Se diseñó la zona superior del dashboard con los indicadores principales del proyecto:

* Producción total.
* Defectos totales.
* Merma.
* Paradas.
* Eficiencia.

## Medidas formateadas para tarjetas KPI

Para que las tarjetas fueran más claras y profesionales, se crearon medidas DAX formateadas.

### Producción total formateada

```DAX
Produccion Total M =
FORMAT([Produccion Total] / 1000000, "0.00") & " M uds."
```

Permite mostrar la producción total como `1,65 M uds.` en lugar de `1.648.065`.

### Defectos totales formateados

```DAX
Defectos Totales K =
FORMAT([Defectos Totales] / 1000, "0.00") & " K uds."
```

Permite mostrar los defectos como `71,36 K uds.` en lugar de `71.359`.

### Eficiencia formateada

```DAX
Eficiencia KPI =
FORMAT([Eficiencia], "0.00") & " uds/min"
```

Permite mostrar la eficiencia como `0,89 uds/min`.

## Resultado de los KPIs

| KPI              | Resultado mostrado |
| ---------------- | -----------------: |
| Producción total |        1,65 M uds. |
| Defectos totales |       71,36 K uds. |
| Merma            |              4,33% |
| Paradas          |             22,90% |
| Eficiencia       |       0,89 uds/min |

## Decisiones visuales

* Se centraron los valores dentro de las tarjetas.
* Se usaron títulos claros y homogéneos.
* Se ajustó el diseño para que la parte superior funcionara como resumen ejecutivo.
* Se añadió un título principal al dashboard: **Eficiencia en producción industrial**.

---

## 8.2 Diseño de la parte central del dashboard

La parte central del dashboard se diseñó para responder tres preguntas principales:

* ¿Dónde se concentra la merma?
* ¿Dónde se producen más paradas?
* ¿Cuáles son los principales motivos de parada?

Se decidió no utilizar únicamente gráficos de barras para evitar un dashboard repetitivo. Por eso se combinaron distintos tipos de visualizaciones:

* Gráfico combinado de columnas y línea.
* Gráfico de barras horizontales.
* Gráfico de anillo.

---

## 8.3 Gráfico: Merma por fase vs media global

Se creó un gráfico combinado de columnas y línea para analizar la merma por fase y compararla contra la media global.

### Campos utilizados

* Eje X: `Fase Corta`
* Columnas: `% Merma`
* Línea: `Merma Media Global`

### Columna calculada: Fase Corta

Se creó una columna calculada para mostrar nombres más limpios y cortos en el eje del gráfico.

```DAX
Fase Corta =
SWITCH(
    produccion_tabla_clean[fase],
    "control_calidad", "Calidad",
    "tratamiento_termico", "Trat. térmico",
    "mecanizado", "Mecanizado",
    "corte", "Corte",
    "expedicion", "Expedición",
    produccion_tabla_clean[fase]
)
```

### Medida: Merma Media Global

Se creó una medida para mostrar la merma media global como línea de referencia.

```DAX
Merma Media Global =
CALCULATE(
    [% Merma],
    REMOVEFILTERS(produccion_tabla_clean[Fase Corta]),
    REMOVEFILTERS(produccion_tabla_clean[fase])
)
```

### Problema corregido

Inicialmente la línea repetía los valores de cada fase, por lo que no representaba una media global real.

Para solucionarlo, se utilizó `REMOVEFILTERS` sobre `Fase Corta` y sobre la columna original `fase`. Así la medida ignoró el filtro de fase y quedó como una línea horizontal fija.

### Resultado

La línea quedó fija en torno al valor global de merma:

```text
4,33%
```

### Insight del gráfico

> Control de calidad presenta la mayor merma (7,34%), por encima de la media global. El posible origen del problema podría estar en fases previas como mecanizado.

### Aclaración importante

El `% Merma` no suma 100% porque no representa una distribución del total, sino una tasa dentro de cada fase.

La fórmula es:

```DAX
% Merma = DIVIDE([Defectos Totales], [Produccion Total], 0)
```

Es decir:

```text
% Merma = defectos de la fase / producción de la fase
```

Por eso cada porcentaje responde a:

> De todo lo producido en esta fase, ¿qué parte salió defectuosa?

Una medida que sí sumaría 100% sería `% defectos sobre total`, porque respondería a:

> De todos los defectos del proyecto, ¿qué parte corresponde a cada fase?

---

## 8.4 Gráfico: Paradas por fase del proceso

Se creó un gráfico de barras horizontales para visualizar dónde se concentran más paradas.

### Campos utilizados

* Eje Y: `Fase Corta`
* Eje X: `Total Paradas`

### Orden

El gráfico se ordenó de mayor a menor por `Total Paradas`.

### Resultados principales

| Fase          | Paradas |
| ------------- | ------: |
| Mecanizado    |     363 |
| Trat. térmico |     234 |
| Corte         |     207 |
| Calidad       |     142 |
| Expedición    |     124 |

### Insight del gráfico

> Mecanizado concentra 363 paradas, por lo que se identifica como la fase más crítica desde el punto de vista operativo.

---

## 8.5 Gráfico: Paradas por motivo

Para hacer el dashboard más visual, se sustituyó el gráfico de barras por un gráfico de anillo.

## Columna calculada: Motivo Parada Limpio

Se creó una columna limpia para mostrar los motivos de parada con nombres más profesionales.

```DAX
Motivo Parada Limpio =
SWITCH(
    produccion_tabla_clean[motivo_parada],
    "error_operario", "Error de operario",
    "fallo_maquina", "Fallo de máquina",
    "falta_material", "Falta de material",
    "mantenimiento", "Mantenimiento",
    "desconocido", "Desconocido",
    produccion_tabla_clean[motivo_parada]
)
```

### Campos utilizados

* Leyenda: `Motivo Parada Limpio`
* Valores: `Total Paradas`

### Resultados principales

| Motivo            | Paradas | Porcentaje |
| ----------------- | ------: | ---------: |
| Error de operario |     274 |     25,61% |
| Falta de material |     247 |     23,08% |
| Mantenimiento     |     238 |     22,24% |
| Fallo de máquina  |     237 |     22,15% |
| Desconocido       |      74 |      6,92% |

### Decisión sobre `Desconocido`

Se decidió mantener la categoría `Desconocido`, ya que representa una limitación real de calidad de datos.

No se ocultó porque aporta información importante: existen paradas registradas sin motivo asociado, lo que indica margen de mejora en la captura de datos.

### Insight del gráfico

> El error de operario es la principal causa de parada, aunque las incidencias se reparten entre factores humanos, técnicos y logísticos.

---

## 8.6 Ajustes visuales del dashboard

Durante el diseño se revisaron varios aspectos visuales:

* Se añadió un título principal: **Eficiencia en producción industrial**.
* Se ajustaron colores y fondos para mantener una estética homogénea.
* Se redujeron nombres largos en los ejes mediante columnas calculadas.
* Se añadieron cajas de insight debajo de cada visual.
* Se combinó un gráfico de columnas, barras horizontales y donut para evitar monotonía.
* Se añadió una referencia visual de la media global en el gráfico de merma.
* Se usaron nombres más legibles para fases y motivos.
* Se mantuvo la coherencia visual entre KPIs, gráficos e insights.

---

## 8.7 Estado actual del dashboard

Al finalizar esta fase, quedó terminada la parte superior y central del dashboard:

* KPIs principales terminados.
* Gráfico de merma por fase vs media global terminado.
* Gráfico de paradas por fase terminado.
* Gráfico de paradas por motivo terminado.
* Insights de negocio añadidos.
* Diseño visual más limpio, profesional y orientado a portfolio.

---

## 8.8 Próximo paso del dashboard

El siguiente bloque será la parte inferior del dashboard, donde se analizará:

* Eficiencia por turno.
* Comparación entre líneas de producción.
* Relación entre paradas y merma.

Distribución prevista:

```text
[ Eficiencia por turno ]   [ Comparación entre líneas ]   [ Parada vs merma ]
```

---

# Estado actual del proyecto

Hasta este punto, el proyecto cuenta con:

* Problema de negocio definido.
* Preguntas de negocio claras.
* Dataset diseñado y generado.
* Limpieza técnica realizada en SQL.
* Validación de datos documentada.
* Dataset preparado para Power BI.
* EDA inicial completado.
* Análisis específico de negocio realizado.
* Parte superior y central del dashboard construida.

---

# Próximos pasos

## En Power BI

* Construir la parte inferior del dashboard.
* Añadir análisis de eficiencia por turno.
* Añadir comparación entre líneas.
* Añadir relación entre paradas y merma.
* Incorporar filtros interactivos.
* Revisar diseño final del dashboard.

## En documentación

* Añadir capturas del dashboard final.
* Crear una sección de conclusiones ejecutivas.
* Añadir recomendaciones finales de negocio.
* Preparar el README del repositorio para GitHub.

## En portfolio

* Explicar el proyecto de forma clara para reclutadores.
* Mostrar decisiones técnicas y de negocio.
* Incluir SQL, DAX, limpieza, análisis y dashboard.
* Destacar la capacidad de transformar datos sucios en insights accionables.

---

# Conclusión provisional

El análisis muestra que el problema de eficiencia en la producción no se concentra en un único punto, sino que responde a una combinación de factores de calidad, operación y captura de datos.

Los principales focos detectados son:

* `control_calidad` como principal punto de detección de defectos.
* `mecanizado` como posible origen operativo del problema.
* Línea B como línea prioritaria de revisión.
* Error de operario como principal motivo de parada.
* Categoría `desconocido` como señal de mejora en calidad de datos.

El dashboard permite comunicar estos hallazgos de forma visual, clara y orientada a la toma de decisiones.
