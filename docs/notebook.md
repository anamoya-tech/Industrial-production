# 📘 Notebook técnico / Diario del proyecto  
# 📊 Análisis de Producción Industrial

Este documento recoge el workflow completo del proyecto de análisis de producción industrial. Funciona como **notebook técnico** o **diario de trabajo**, y está pensado para documentar con detalle todas las decisiones tomadas durante el desarrollo.

El proyecto se centra en la optimización de una línea de producción industrial de piezas metálicas, en un entorno similar al sector automoción. El objetivo principal es identificar ineficiencias operativas, analizar mermas, estudiar paradas de producción y detectar oportunidades de mejora mediante SQL, Power BI y análisis de negocio.

A diferencia del `README.md`, que debe ser más breve y fácil de leer para recruiters, este notebook explica el proceso paso a paso: diseño del dataset, limpieza técnica, validación de datos, análisis exploratorio, medidas DAX, construcción del dashboard final, insights, conclusiones y recomendaciones.

---

## Índice

1. [Bloque 1: Definición del problema](#-bloque-1-definición-del-problema)
2. [Bloque 2: Objetivo y preguntas de negocio](#-bloque-2-objetivo-y-preguntas-de-negocio)
3. [Bloque 3: Diseño del dataset](#-bloque-3-diseño-del-dataset)
4. [Bloque 4: Creación / obtención de datos](#-bloque-4-creación--obtención-de-datos)
5. [Bloque 5: Limpieza de datos en SQL](#-bloque-5-limpieza-de-datos-en-sql)
6. [Bloque 6: Validación de datos](#-bloque-6-validación-de-datos)
7. [Bloque 7: Preparación técnica para Power BI](#-bloque-7-preparación-técnica-para-power-bi)
8. [Bloque 8: Análisis exploratorio inicial](#-bloque-8-análisis-exploratorio-inicial-eda)
9. [Bloque 9: Análisis específico de negocio](#-bloque-9-análisis-específico-de-negocio)
10. [Bloque 10: Visualización en Power BI](#-bloque-10-visualización-en-power-bi)
11. [Bloque 11: Insights y conclusiones](#-bloque-11-insights-y-conclusiones)
12. [Bloque 12: Recomendaciones](#-bloque-12-recomendaciones)
13. [Bloque 13: Estado final del proyecto](#-bloque-13-estado-final-del-proyecto)
14. [Bloque 14: Aprendizajes](#-bloque-14-aprendizajes-del-proyecto)
15. [Bloque 15: Conclusión final](#-bloque-15-conclusión-final-del-proyecto)
16. [Tecnologías utilizadas](#-tecnologías-utilizadas)
17. [Uso de IA en el proyecto](#-uso-de-ia-en-el-proyecto)
18. [Cierre](#-cierre)

---

# 🟩 BLOQUE 1: Definición del problema

## Contexto del proyecto

El proyecto analiza una línea de producción industrial compuesta por cinco fases principales:

1. Corte  
2. Mecanizado  
3. Tratamiento térmico  
4. Control de calidad  
5. Expedición  

Cada fase puede presentar ineficiencias que afectan al rendimiento global de la producción, a la calidad del producto final y al cumplimiento de los plazos de entrega.

---

## Problema principal

La producción presenta ineficiencias reflejadas en tres áreas principales:

- Paradas frecuentes en determinadas fases del proceso.
- Diferencias de rendimiento entre turnos.
- Generación de mermas o piezas defectuosas.

Estas ineficiencias reducen la productividad y generan costes adicionales para la empresa.

---

## Impacto en negocio

Las ineficiencias detectadas pueden provocar:

- Reducción de la producción útil.
- Pérdida de tiempo operativo.
- Incremento de costes por mermas.
- Disminución de la calidad del producto final.
- Posibles retrasos en entregas.
- Dificultad para identificar el origen real de los problemas si no se analizan los datos correctamente.

---

## Enfoque analítico

El análisis se plantea desde una perspectiva de negocio, utilizando datos para:

- Detectar patrones de ineficiencia.
- Identificar fases críticas del proceso.
- Analizar la relación entre paradas, turnos, líneas de producción y calidad.
- Diferenciar puntos de detección de posibles puntos de origen.
- Proponer recomendaciones basadas en evidencia.

---

# 🟩 BLOQUE 2: Objetivo y preguntas de negocio

## Objetivo general

Analizar el rendimiento de la línea de producción para identificar ineficiencias, reducir mermas y optimizar el uso de recursos en las distintas fases del proceso productivo.

---

## Objetivos específicos

- Evaluar la eficiencia de cada fase del proceso.
- Analizar la frecuencia y el impacto de las paradas.
- Identificar diferencias de rendimiento entre turnos.
- Medir el nivel de mermas y su impacto en la producción.
- Detectar posibles cuellos de botella.
- Comparar el comportamiento de las líneas de producción.
- Crear un dashboard que permita transformar los datos en decisiones.

---

## Preguntas de negocio

### Producción

- ¿Cuál es la producción media por turno?
- ¿Qué fase del proceso es menos eficiente?
- ¿Dónde se pierde más tiempo productivo?

### Paradas

- ¿Cuántas paradas se producen en cada fase?
- ¿Qué fases concentran más paradas?
- ¿Cuáles son los principales motivos de parada?
- ¿Existen patrones de paradas según el turno?

### Mermas

- ¿Qué porcentaje de la producción total se convierte en merma?
- ¿En qué fase se generan más productos defectuosos?
- ¿Existe relación entre paradas y aumento de mermas?
- ¿Qué turno presenta mayor tasa de defectos?

### Turnos

- ¿Qué turno produce más?
- ¿Qué turno es más eficiente?
- ¿El turno de noche realmente es menos eficiente?

### Líneas de producción

- ¿Línea A, B o C produce mejor?
- ¿Qué línea tiene más merma?
- ¿Hay alguna línea claramente problemática?

---

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

---

## Variables del dataset

### Identificación

| Variable | Descripción |
|---|---|
| `id_registro` | Identificador único del registro |
| `fecha` | Fecha de la operación |
| `turno` | Turno de trabajo |
| `linea_produccion` | Línea donde se realizó la operación |

### Proceso

| Variable | Descripción |
|---|---|
| `fase` | Fase del proceso productivo |

Valores esperados:

- `corte`
- `mecanizado`
- `tratamiento_termico`
- `control_calidad`
- `expedicion`

### Producción

| Variable | Descripción |
|---|---|
| `unidades_producidas` | Número de unidades producidas |
| `unidades_defectuosas` | Número de unidades defectuosas |

### Tiempos

| Variable | Descripción |
|---|---|
| `tiempo_produccion_min` | Tiempo de producción en minutos |
| `tiempo_parada_min` | Tiempo de parada en minutos |

### Paradas

| Variable | Descripción |
|---|---|
| `hubo_parada` | Indica si hubo parada o no |
| `motivo_parada` | Motivo de la parada |

### Personal

| Variable | Descripción |
|---|---|
| `operario_id` | Identificador del operario |

---

## Decisiones de diseño

- Se trabajó con una única tabla para simplificar el análisis inicial.
- Se incluyó `fase` para analizar el rendimiento por etapa.
- Se añadieron `unidades_defectuosas` para medir mermas.
- Se incorporaron variables relacionadas con paradas para analizar su impacto operativo.
- Se incluyó `turno` para comparar rendimiento entre franjas de trabajo.
- Se incluyó `linea_produccion` para detectar si el problema estaba concentrado en una línea concreta.
- La estructura se diseñó pensando en su posterior uso en SQL y Power BI.

---

# 🟩 BLOQUE 4: Creación / obtención de datos

Se generó un dataset simulado de más de 5000 registros, intencionadamente sucio, para representar un entorno real de trabajo.

---

## Problemas incluidos en el dataset

El dataset se generó con errores deliberados para simular problemas habituales en datos reales:

- Valores nulos.
- Duplicados.
- Errores de formato.
- Inconsistencias en texto.
- Datos incoherentes.
- Categorías escritas con variantes.
- Fechas en distintos formatos.
- Campos booleanos no normalizados.

---

## Objetivo de esta decisión

El objetivo no era trabajar con un dataset perfecto, sino practicar un flujo realista de análisis:

- Importación de datos sucios.
- Diagnóstico inicial.
- Limpieza técnica.
- Normalización.
- Validación.
- Análisis exploratorio.
- Visualización en Power BI.

---

# 🟩 BLOQUE 5: Limpieza de datos en SQL

La limpieza se realizó en PostgreSQL utilizando pgAdmin4.

---

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

---

## 5.2 Exploración inicial

Se revisó la carga de datos con:

```sql
SELECT COUNT(*) AS total_filas
FROM produccion_linea;
```

Resultado inicial:

| Métrica | Resultado |
|---|---:|
| Total filas | 5201 |

También se visualizaron las primeras filas:

```sql
SELECT *
FROM produccion_linea
LIMIT 10;
```

---

## 5.3 Problemas detectados

Durante la exploración inicial se detectaron los siguientes problemas:

- Nulos en `linea_produccion`, `motivo_parada`, `operario_id` y variables numéricas.
- Fechas en múltiples formatos.
- Errores tipográficos en `turno`.
- Variantes de texto en `fase`.
- Valores inconsistentes en `hubo_parada`.
- Motivos de parada escritos de diferentes formas.
- Valores incoherentes en producción y defectos.

Ejemplos de variantes detectadas en `motivo_parada`:

```text
error operario, error_operario, Error_operario
fallo máquina, fallo_maquina, fallomaquina
falta material, falta_material, faltamaterial
mantenimiento, mantenimienot, Mantenimiento
```

---

## 5.4 Creación de tabla limpia

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

---

## 5.5 Estrategia de limpieza aplicada

Se aplicaron las siguientes acciones:

- Conversión de fechas a tipo `DATE`.
- Normalización de texto con `LOWER`, `TRIM` y reemplazos.
- Estandarización de categorías.
- Validación de datos numéricos.
- Conversión de `hubo_parada` a booleano.
- Corrección de motivos de parada.
- Conversión de valores incoherentes a `NULL`.

---

## 5.6 Decisiones clave de limpieza

| Situación detectada | Decisión aplicada |
|---|---|
| Dato inválido | Convertir a `NULL` |
| Registro con algún campo inválido | No eliminar si todavía aporta información |
| Defectos mayores que unidades producidas | Convertir a `NULL` |
| Tiempos irreales | Convertir a `NULL` |
| Parada sin motivo | Etiquetar como `desconocido` |
| Línea de producción no informada | Mantener inicialmente como nulo |

---

## Justificación

Se priorizó la calidad del dato frente a la completitud.

Eliminar todos los registros con algún problema habría reducido el dataset y eliminado información útil. Por eso se optó por conservar registros válidos parcialmente y marcar como `NULL` los valores que no eran fiables.

---

# 🟩 BLOQUE 6: Validación de datos

Tras la limpieza se realizó una validación final para comprobar el estado del dataset.

---

## 6.1 Validación de nulos

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

---

## 6.2 Resultado final antes del último ajuste

| Validación | Resultado |
|---|---:|
| Total filas | 4673 |
| Nulos en fecha | 0 |
| Nulos en turno | 0 |
| Nulos en fase | 0 |
| Nulos en `hubo_parada` | 0 |
| Nulos en `linea_produccion` | 146 |
| Nulos en `unidades_producidas` | 241 |
| Nulos en `unidades_defectuosas` | 289 |
| Nulos en `tiempo_produccion_min` | 91 |
| Nulos en `tiempo_parada_min` | 18 |
| Nulos en motivo con parada | 74 |
| Nulos en `operario_id` | 201 |

---

## 6.3 Decisión sobre paradas sin motivo

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

---

## 6.4 Validación de coherencia

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

---

## 6.5 Conclusión de limpieza

El dataset final queda:

- Limpio.
- Validado.
- Coherente.
- Preparado para análisis exploratorio.
- Preparado para visualización en Power BI.

Los valores nulos restantes se consideran controlados y justificados, ya que provienen de datos incoherentes o variables dependientes.

---

# 🟩 BLOQUE 7: Preparación técnica para Power BI

Se exportó la tabla `produccion_limpia` a CSV y se revisó en Excel antes de cargarla en Power BI.

---

## Ajustes realizados

- Revisión de cabeceras.
- Comprobación de columnas.
- Revisión de valores nulos.
- Transformación de valores booleanos.
- Conversión de `t/f` a `VERDADERO/FALSO`.
- Carga del dataset en Power BI.
- Conversión de `hubo_parada` a tipo booleano en Power Query.

---

## Resultado

El dataset quedó correctamente cargado en Power BI con:

- Columnas detectadas correctamente.
- Tipos numéricos adecuados.
- Fechas reconocidas.
- Booleanos preparados.
- Estructura lista para análisis visual.

---

# 🟩 BLOQUE 8: Análisis exploratorio inicial (EDA)

El análisis exploratorio inicial se realizó en SQL para entender el comportamiento general de los datos.

---

## 8.1 KPI global

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

### Resultados

| Métrica | Resultado |
|---|---:|
| Registros analizados | 4281 |
| Producción total | 1.591.258 unidades |
| Defectos | 70.876 unidades |
| Producción útil | 1.520.382 unidades |
| Media producción | 371 unidades |
| Media defectos | 16,56 unidades |
| Merma global | 4,45% |

### Insight

Aunque la merma global parece baja en porcentaje, supone más de 70.000 unidades defectuosas. Esto indica que las pérdidas no son puntuales, sino estructurales dentro del proceso productivo.

---

## 8.2 Merma por fase

| Fase | Merma |
|---|---:|
| `control_calidad` | 7,32% |
| `mecanizado` | 4,80% |
| `tratamiento_termico` | 4,02% |
| `corte` | 3,06% |
| `expedicion` | 2,29% |

### Insights

- `control_calidad` presenta la mayor merma.
- Esta fase actúa como punto de detección, no necesariamente como origen del problema.
- `mecanizado` aparece como posible fase crítica previa.
- `expedicion` muestra el mejor rendimiento.

---

## 8.3 Paradas generales

| Estado | Porcentaje |
|---|---:|
| Sin parada | 77,10% |
| Con parada | 22,90% |

### Insight

Aproximadamente 1 de cada 4 registros presenta paradas. Esto sugiere que las interrupciones no son incidencias aisladas, sino un problema estructural.

---

## 8.4 Motivos de parada

| Motivo | Porcentaje |
|---|---:|
| `error_operario` | 25,61% |
| `falta_material` | 23,08% |
| `mantenimiento` | 22,24% |
| `fallo_maquina` | 22,15% |
| `desconocido` | 6,92% |

### Insights

- El principal motivo de parada es `error_operario`.
- Las causas están repartidas entre factores humanos, técnicos y logísticos.
- `mantenimiento` + `fallo_maquina` representan casi el 44% de las paradas.
- La categoría `desconocido` evidencia limitaciones en la captura de datos.

---

## 8.5 Análisis por turno

| Turno | Merma |
|---|---:|
| Tarde | 4,36% |
| Mañana | 4,33% |
| Noche | 4,30% |

### Insights

- La merma es estable entre turnos.
- El problema de calidad no depende directamente del horario.
- El turno noche produce menos, pero no mejora significativamente la calidad.
- Menos producción no implica mejor calidad.

---

# 🟩 BLOQUE 9: Análisis específico de negocio

En este bloque se profundiza en preguntas concretas de negocio usando Power BI y medidas DAX.

---

## 9.1 Medidas DAX creadas

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

### Eficiencia por hora

```DAX
Eficiencia Hora = [Eficiencia] * 60
```

Convierte la eficiencia a piezas válidas por hora, una unidad más clara para negocio.

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

## 9.2 Eficiencia por turno

### Preguntas respondidas

- ¿Qué turno produce más?
- ¿Qué turno tiene mejor relación entre producción y defectos?
- ¿El turno noche realmente es menos eficiente?

### Resultados

| Turno | Producción total | Defectos | Unidades válidas | Tiempo total | Eficiencia | % Merma |
|---|---:|---:|---:|---:|---:|---:|
| Tarde | 547.782 | 23.866 | 523.916 | 540.498 | 0,97 | 4,36% |
| Mañana | 619.414 | 26.816 | 592.598 | 631.385 | 0,94 | 4,33% |
| Noche | 480.869 | 20.677 | 460.192 | 603.007 | 0,76 | 4,30% |

### Conclusiones

- El turno de mañana produce más.
- El turno de tarde es el más eficiente.
- El turno de noche tiene la merma más baja, pero también la eficiencia más baja.
- El problema del turno noche no parece estar en la calidad, sino en la eficiencia operativa y el volumen producido.

---

## 9.3 Defectos y mermas por fase

### Preguntas respondidas

- ¿Qué fase concentra más defectos?
- ¿Qué fase debería revisarse primero?
- ¿Dónde puede estar el origen del problema?

### Resultados

| Fase | Producción total | Defectos | Unidades válidas | % Merma | % defectos sobre total |
|---|---:|---:|---:|---:|---:|
| `control_calidad` | 374.355 | 27.482 | 346.873 | 7,34% | 39% |
| `mecanizado` | 260.250 | 12.613 | 247.637 | 4,85% | 18% |
| `tratamiento_termico` | 298.925 | 12.114 | 286.811 | 4,05% | 17% |
| `corte` | 337.768 | 10.438 | 327.330 | 3,09% | 15% |
| `expedicion` | 377.667 | 8.712 | 368.955 | 2,31% | 12% |

### Conclusiones

- `control_calidad` concentra más defectos: 27.482 unidades.
- Representa aproximadamente el 39% del total de defectos.
- Debe revisarse primero como punto de detección.
- El posible origen se encuentra en fases anteriores, especialmente `mecanizado`.

### Insight

`control_calidad` detecta el problema, pero no necesariamente lo genera. `mecanizado` aparece como una fase crítica por su merma elevada, su volumen de defectos y su naturaleza técnica.

---

## 9.4 Análisis de paradas

### Preguntas respondidas

- ¿Qué motivos generan más impacto?
- ¿Dónde se concentran más paradas?
- ¿Las paradas afectan a la merma?

---

### 9.4.1 Motivos que generan más impacto

El principal motivo de parada es:

- `error_operario`

Después aparecen, con valores similares:

- `falta_material`
- `mantenimiento`
- `fallo_maquina`

Finalmente aparece:

- `desconocido`

### Insight

El principal motivo está relacionado con errores de operario, lo que sugiere oportunidades de mejora en formación, procedimientos o supervisión.

Además, las causas están repartidas entre factores humanos, técnicos y logísticos, por lo que no existe una única causa aislada.

---

### 9.4.2 Paradas por fase

| Fase | Paradas |
|---|---:|
| `mecanizado` | 363 |
| `tratamiento_termico` | 234 |
| `corte` | 207 |
| `control_calidad` | 142 |
| `expedicion` | 124 |

### Conclusión

Las paradas se concentran principalmente en `mecanizado`, con 363 interrupciones.

### Insight

`mecanizado` es una fase crítica desde el punto de vista operativo. Además, al coincidir con una merma elevada, refuerza la hipótesis de que parte de los defectos detectados en control de calidad pueden originarse en fases técnicas anteriores.

---

### 9.4.3 Relación entre paradas y merma

| Hubo parada | Producción total | Defectos | % Merma |
|---|---:|---:|---:|
| False | 1.292.415 | 55.598 | 4,30% |
| True | 355.650 | 15.761 | 4,43% |

### Conclusión

Los registros con parada tienen una merma ligeramente superior.

### Insight

La diferencia no es muy elevada, pero sugiere que las interrupciones pueden afectar negativamente a la calidad del proceso productivo.

---

## 9.5 Comparación entre líneas de producción

### Preguntas respondidas

- ¿Línea A, B o C produce mejor?
- ¿Qué línea tiene más merma?
- ¿Hay alguna línea claramente problemática?

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

| Línea | Producción total | Defectos | Unidades válidas | % Merma | Eficiencia | Paradas |
|---|---:|---:|---:|---:|---:|---:|
| A | 559.554 | 24.302 | 535.252 | 4,34% | 0,89 | 355 |
| B | 521.641 | 23.069 | 498.572 | 4,42% | 0,87 | 359 |
| C | 514.246 | 21.827 | 492.419 | 4,24% | 0,90 | 322 |
| No registrada | 52.624 | 2.161 | 50.463 | 4,11% | 0,90 | 34 |

### Conclusiones

- La línea A produce más.
- La línea C tiene la mejor eficiencia.
- La línea B tiene la mayor merma.
- La línea B también tiene la menor eficiencia y el mayor número de paradas.
- Por tanto, la línea B es la más problemática y debería revisarse con prioridad.

### Nota sobre `No registrada`

La categoría `No registrada` corresponde a registros sin información de línea de producción. Se mantiene para conservar trazabilidad, pero no se interpreta como una línea real.

---

## 9.6 Cierre del bloque

### ¿Dónde está el mayor problema de calidad?

El mayor problema de calidad se detecta en `control_calidad`, con la mayor tasa de merma y el mayor volumen de defectos.

Sin embargo, el posible origen puede encontrarse en fases anteriores, especialmente `mecanizado`.

### ¿Las paradas influyen en la merma?

Sí. Los registros con parada presentan una merma ligeramente superior a los registros sin parada.

### ¿Hay alguna línea que requiere atención prioritaria?

Sí. La línea B requiere atención prioritaria porque combina:

- Mayor porcentaje de merma.
- Menor eficiencia.
- Mayor número de paradas.

### Recomendaciones iniciales de negocio

- Revisar la fase de mecanizado.
- Analizar en detalle la línea B.
- Reforzar la formación de operarios.
- Mejorar procedimientos operativos.
- Optimizar el mantenimiento preventivo.
- Revisar la planificación de materiales.
- Mejorar el registro de datos, especialmente los casos `desconocido`.

### Conclusión del bloque

El problema no se concentra en un único punto. Responde a una combinación de factores técnicos, humanos y operativos.

Los principales focos de atención son:

- `control_calidad` como punto de detección.
- `mecanizado` como posible origen operativo.
- Línea B como línea prioritaria de revisión.

---

# 🟩 BLOQUE 10: Visualización en Power BI

## Objetivo del bloque

Convertir los análisis realizados en un dashboard claro, visual e interactivo que permita comunicar los insights de negocio.

El dashboard debe ayudar a responder rápidamente:

- Dónde se concentran los defectos.
- Qué fases generan más problemas.
- Qué motivos explican las paradas.
- Qué líneas requieren atención.
- Qué acciones de mejora pueden priorizarse.

---

## Estado inicial antes del dashboard

El dataset ya estaba:

- Limpio en SQL.
- Validado.
- Exportado correctamente.
- Cargado en Power BI.
- Tipado correctamente.
- Preparado para visualización.

---

## 10.1 KPIs principales del dashboard

Se diseñó la zona superior del dashboard con los indicadores principales del proyecto:

- Producción total.
- Defectos totales.
- Merma.
- Paradas.
- Eficiencia.

---

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

Inicialmente la eficiencia se mostraba como `0,89 uds/min`, pero se decidió convertirla a piezas por hora porque era una unidad más comprensible para negocio.

```DAX
Eficiencia KPI =
FORMAT([Eficiencia Hora], "0.00") & " piezas/h"
```

---

## Resultado de los KPIs

| KPI | Resultado mostrado |
|---|---:|
| Producción total | 1,65 M uds. |
| Defectos totales | 71,36 K uds. |
| Merma | 4,33% |
| Paradas | 22,90% |
| Eficiencia | 53,30 piezas/h |

---

## Decisiones visuales

- Se centraron los valores dentro de las tarjetas.
- Se usaron títulos claros y homogéneos.
- Se ajustó el diseño para que la parte superior funcionara como resumen ejecutivo.
- Se añadió un título principal al dashboard: **Eficiencia en producción industrial**.

---

## 10.2 Diseño de la parte central del dashboard

La parte central del dashboard se diseñó para responder tres preguntas principales:

- ¿Dónde se concentra la merma?
- ¿Dónde se producen más paradas?
- ¿Cuáles son los principales motivos de parada?

Se decidió no utilizar únicamente gráficos de barras para evitar un dashboard repetitivo. Por eso se combinaron distintos tipos de visualizaciones:

- Gráfico combinado de columnas y línea.
- Gráfico de barras horizontales.
- Gráfico de anillo.

---

## 10.3 Gráfico: Merma por fase vs media global

Se creó un gráfico combinado de columnas y línea para analizar la merma por fase y compararla contra la media global.

### Campos utilizados

- Eje X: `Fase Corta`
- Columnas: `% Merma`
- Línea: `Merma Media Global`

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

Control de calidad presenta la mayor merma, con **7,34%**, por encima de la media global. El posible origen del problema podría estar en fases previas como mecanizado.

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

## 10.4 Gráfico: Paradas por fase del proceso

Se creó un gráfico de barras horizontales para visualizar dónde se concentran más paradas.

### Campos utilizados

- Eje Y: `Fase Corta`
- Eje X: `Total Paradas`

### Orden

El gráfico se ordenó de mayor a menor por `Total Paradas`.

### Resultados principales

| Fase | Paradas |
|---|---:|
| Mecanizado | 363 |
| Trat. térmico | 234 |
| Corte | 207 |
| Calidad | 142 |
| Expedición | 124 |

### Insight del gráfico

Mecanizado concentra 363 paradas, por lo que se identifica como la fase más crítica desde el punto de vista operativo.

---

## 10.5 Gráfico: Paradas por motivo

Para hacer el dashboard más visual, se sustituyó el gráfico de barras por un gráfico de anillo.

### Columna calculada: Motivo Parada Limpio

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

- Leyenda: `Motivo Parada Limpio`
- Valores: `Total Paradas`

### Resultados principales

| Motivo | Paradas | Porcentaje |
|---|---:|---:|
| Error de operario | 274 | 25,61% |
| Falta de material | 247 | 23,08% |
| Mantenimiento | 238 | 22,24% |
| Fallo de máquina | 237 | 22,15% |
| Desconocido | 74 | 6,92% |

### Decisión sobre `Desconocido`

Se decidió mantener la categoría `Desconocido`, ya que representa una limitación real de calidad de datos.

No se ocultó porque aporta información importante: existen paradas registradas sin motivo asociado, lo que indica margen de mejora en la captura de datos.

### Insight del gráfico

El error de operario es la principal causa de parada, aunque las incidencias se reparten entre factores humanos, técnicos y logísticos.

---

## 10.6 Ajustes visuales del dashboard

Durante el diseño se revisaron varios aspectos visuales:

- Se añadió un título principal: **Eficiencia en producción industrial**.
- Se ajustaron colores y fondos para mantener una estética homogénea.
- Se redujeron nombres largos en los ejes mediante columnas calculadas.
- Se añadieron cajas de insight debajo de cada visual.
- Se combinó un gráfico de columnas, barras horizontales y donut para evitar monotonía.
- Se añadió una referencia visual de la media global en el gráfico de merma.
- Se usaron nombres más legibles para fases y motivos.
- Se mantuvo la coherencia visual entre KPIs, gráficos e insights.

---

## 10.7 Estado de la parte superior y central

Al finalizar esta fase, quedó terminada la parte superior y central del dashboard:

- KPIs principales terminados.
- Gráfico de merma por fase vs media global terminado.
- Gráfico de paradas por fase terminado.
- Gráfico de paradas por motivo terminado.
- Insights de negocio añadidos.
- Diseño visual más limpio, profesional y orientado a portfolio.

---

## 10.8 Rediseño de la parte inferior del dashboard

Una vez terminada la parte superior y central del dashboard, se trabajó la parte inferior para completar el análisis operativo.

La idea inicial era usar gráficos sencillos, pero se decidió mejorar la variedad visual del dashboard para que no quedara compuesto únicamente por barras y columnas.

La parte inferior quedó orientada a responder tres preguntas:

- ¿Qué turno es más eficiente?
- ¿Qué línea de producción requiere atención prioritaria?
- ¿Las paradas están relacionadas con una mayor merma?

Distribución final:

```text
[ Eficiencia por turno ]   [ Comparación entre líneas ]   [ Parada vs merma ]
```

---

## 10.8.1 Visual: Eficiencia por turno

Inicialmente se había planteado un gráfico de columnas para comparar la eficiencia entre mañana, tarde y noche.

Sin embargo, para aportar más variedad visual al dashboard, se decidió sustituirlo por un **gráfico de líneas**.

La lógica del gráfico es mostrar la evolución del rendimiento a lo largo de la secuencia diaria:

```text
mañana → tarde → noche
```

### Problema detectado

Power BI ordenaba los turnos de forma alfabética, lo que no representaba correctamente el orden real del día.

Para solucionarlo, se creó una tabla auxiliar:

```DAX
Dim Turno =
DATATABLE(
    "turno", STRING,
    "orden_turno", INTEGER,
    {
        {"mañana", 1},
        {"tarde", 2},
        {"noche", 3}
    }
)
```

Después se creó una relación entre:

```text
Dim Turno[turno] → produccion_tabla_clean[turno]
```

Con cardinalidad:

```text
Uno a varios (1:*)
```

Finalmente, se ordenó la columna `turno` por `orden_turno`.

### Campos utilizados

```text
Eje X: Dim Turno[turno]
Eje Y: Eficiencia Hora
```

### Resultado

| Turno | Eficiencia |
|---|---:|
| Mañana | 56,3 piezas/h |
| Tarde | 58,2 piezas/h |
| Noche | 45,8 piezas/h |

### Insight

El turno de tarde presenta la mayor eficiencia global, mientras que el turno de noche muestra el rendimiento más bajo.

Esto confirma que el problema del turno de noche no está principalmente en la calidad, sino en la capacidad operativa y el ritmo de producción.

---

## 10.8.2 Visual: Comparación entre líneas

Para comparar las líneas de producción se utilizó una **matriz**.

Este visual se mantuvo porque permite comparar varias métricas al mismo tiempo de forma clara:

- producción
- merma
- eficiencia
- paradas

### Campos utilizados

```text
Filas:
Linea Produccion Limpia

Valores:
Produccion Total
% Merma
Eficiencia Hora
Total Paradas
```

### Ajustes realizados

Se renombraron las columnas dentro del visual para mejorar la lectura:

| Nombre original | Nombre mostrado |
|---|---|
| `Linea Produccion Limpia` | Línea |
| `Produccion Total` | Producción |
| `% Merma` | Merma |
| `Eficiencia Hora` | Piezas/h |
| `Total Paradas` | Paradas |

También se mantuvo la categoría `No registrada`.

Esta categoría no representa una línea real, sino registros donde no se informó la línea de producción. Se decidió conservarla para mantener trazabilidad y mostrar una limitación real de calidad de datos.

### Resultado

| Línea | Producción | Merma | Piezas/h | Paradas |
|---|---:|---:|---:|---:|
| A | 559.554 | 4,34% | 53,50 | 355 |
| B | 521.641 | 4,42% | 52,46 | 359 |
| C | 514.246 | 4,24% | 53,90 | 322 |
| No registrada | 52.624 | 4,11% | 53,80 | 34 |
| Total | 1.648.065 | 4,33% | 53,30 | 1070 |

### Insight

La línea B requiere atención prioritaria, ya que combina:

- mayor porcentaje de merma
- menor eficiencia
- mayor número de paradas

Aunque la línea A produce más volumen, la línea B es la que presenta peor equilibrio operativo.

---

## 10.8.3 Visual: Parada vs merma

Inicialmente se creó un gráfico de columnas para comparar la merma entre registros con parada y sin parada.

Sin embargo, como solo se comparaban dos estados, el gráfico resultaba poco diferencial. Por eso se decidió sustituirlo por **tarjetas comparativas**.

El objetivo era comunicar el resultado de forma más ejecutiva y rápida.

### Medidas creadas

```DAX
Merma Sin Parada =
CALCULATE(
    [% Merma],
    produccion_tabla_clean[hubo_parada] = FALSE()
)
```

```DAX
Merma Con Parada =
CALCULATE(
    [% Merma],
    produccion_tabla_clean[hubo_parada] = TRUE()
)
```

```DAX
Diferencia Merma Parada =
[Merma Con Parada] - [Merma Sin Parada]
```

Para mostrar correctamente la diferencia entre porcentajes, se creó una medida en puntos porcentuales:

```DAX
Diferencia Merma Parada KPI =
FORMAT([Diferencia Merma Parada] * 100, "+0.00;-0.00;0.00") & " pp"
```

### Resultado

| Estado | Merma |
|---|---:|
| Sin parada | 4,30% |
| Con parada | 4,43% |
| Diferencia | +0,13 pp |

### Decisión importante

Se utilizó `pp` porque la diferencia entre dos porcentajes debe expresarse en **puntos porcentuales**, no como porcentaje relativo.

### Insight

Los registros con parada presentan una merma ligeramente superior. La diferencia es reducida, pero sugiere una posible relación entre interrupciones y pérdida de calidad.

---

## 10.9 Tooltips e insights interactivos

Durante el diseño del dashboard se añadieron inicialmente cajas de texto con insights debajo de cada gráfico.

Aunque estos textos ayudaban a interpretar los visuales, el dashboard quedaba demasiado cargado visualmente.

Por este motivo, se decidió sustituir los insights visibles por **tooltips personalizados**.

---

## Motivo de la decisión

El objetivo era conseguir un dashboard:

- más limpio
- menos saturado
- más profesional
- más interactivo
- más orientado a portfolio

La decisión permite que el usuario consulte la interpretación de cada visual al pasar el cursor por encima, sin que la página principal quede llena de texto.

---

## Implementación

Se creó una página específica llamada:

```text
Tooltip_Merma
```

Después se configuró como página de información sobre herramientas:

```text
Formato de página → Información de página → Permitir el uso como información sobre herramientas
```

También se cambió el tamaño de página a:

```text
Información sobre herramientas
```

Posteriormente, se asignó el tooltip al gráfico correspondiente desde:

```text
Formato visual → General → Información sobre herramientas
Tipo → Página del informe
Página → Tooltip_Merma
```

---

## Nota añadida al dashboard

Para que el usuario supiera que existen insights ocultos, se añadió una nota discreta en el dashboard:

```text
Pasa el cursor sobre cada visual para ver el insight.
```

---

## Insights preparados

### Merma por fase vs media global

```text
Control de calidad presenta la mayor merma (7,34%), por encima de la media global. Esta fase actúa como punto de detección; el posible origen del problema podría estar en fases previas como mecanizado.
```

### Paradas por fase

```text
Mecanizado concentra el mayor número de paradas, con 363 interrupciones. Esto la convierte en la fase más crítica desde el punto de vista operativo.
```

### Paradas por motivo

```text
El error de operario es la principal causa de parada, con 274 registros. Aun así, las incidencias están bastante repartidas entre factores humanos, técnicos y logísticos.
```

### Eficiencia por turno

```text
El turno de tarde presenta la mayor eficiencia, con 58,16 piezas válidas por hora. El turno de noche muestra el rendimiento más bajo, con 45,79 piezas/h.
```

### Comparación entre líneas

```text
La línea B requiere atención prioritaria porque combina la mayor merma (4,42%), la menor eficiencia (52,46 piezas/h) y el mayor número de paradas (359).
```

### Parada vs merma

```text
Los registros con parada presentan una merma ligeramente superior: 4,43% frente a 4,30% sin parada. La diferencia es reducida, pero sugiere una posible relación entre interrupciones y pérdida de calidad.
```

---

## 10.10 Resultado final del dashboard

El dashboard final quedó organizado en tres niveles:

### Parte superior

KPIs principales:

- Producción total
- Defectos totales
- Merma
- Paradas
- Eficiencia

### Parte central

Análisis principal:

- Merma por fase vs media global
- Paradas por fase del proceso
- Paradas por motivo

### Parte inferior

Comparación operativa:

- Eficiencia por turno
- Comparación entre líneas
- Parada vs merma

### Visuales utilizados

El dashboard combina distintos tipos de visuales:

- tarjetas KPI
- gráfico combinado de columnas y línea
- barras horizontales
- gráfico de anillo
- gráfico de líneas
- matriz
- tarjetas comparativas

Esta variedad permite que el dashboard sea más atractivo visualmente sin perder claridad analítica.

---

# 🟩 BLOQUE 11: Insights y conclusiones

En este bloque se interpretan los resultados obtenidos durante el análisis y la visualización en Power BI.

El objetivo es transformar los datos en conclusiones útiles para la toma de decisiones, identificando patrones, problemas, posibles causas y recomendaciones de negocio.

---

## 11.1 Resumen ejecutivo

El análisis global muestra una producción total de aproximadamente **1,65 millones de unidades**, con **71,36 mil unidades defectuosas** y una **merma global del 4,33%**.

Aunque el porcentaje de merma no parece excesivamente alto en términos relativos, el volumen absoluto de defectos sí es relevante para una fábrica industrial. Esto significa que pequeñas mejoras porcentuales podrían traducirse en una reducción importante de piezas defectuosas.

Los principales focos detectados son:

- `control_calidad` concentra la mayor merma.
- `mecanizado` acumula el mayor número de paradas.
- El turno de noche presenta la menor eficiencia global.
- La línea B es la más crítica en el análisis general.
- Las paradas se asocian con una merma ligeramente superior.
- Algunos patrones cambian al analizar el dashboard de forma interactiva.

La conclusión general es que el problema no depende de una única causa, sino de una combinación de factores relacionados con fases del proceso, turnos, líneas de producción, paradas y calidad del registro de datos.

---

## 11.2 Insight 1: La merma se concentra en control de calidad

La fase con mayor merma es `control_calidad`, con un **7,34%**, claramente por encima de la media global del **4,33%**.

Este resultado indica que la mayor proporción de defectos se registra en esta fase. Sin embargo, control de calidad puede actuar más como punto de detección que como origen real del problema.

Es decir, los defectos podrían estar generándose en fases anteriores y detectándose posteriormente durante el control final.

Por este motivo, no conviene interpretar esta fase como única responsable, sino como una señal para investigar el proceso anterior.

Posibles fases a revisar:

- mecanizado
- tratamiento térmico
- corte

---

## 11.3 Insight 2: Mecanizado es la fase más crítica en paradas

La fase de `mecanizado` registra el mayor número de paradas, con **363 interrupciones**.

Este dato convierte a mecanizado en una fase crítica desde el punto de vista operativo.

Una alta concentración de paradas puede afectar al ritmo de producción, generar retrasos y reducir la estabilidad del proceso.

Además, mecanizado también aparece como una fase relevante dentro del análisis de calidad, ya que podría estar relacionada con defectos que se detectan más adelante en control de calidad.

---

## 11.4 Insight 3: Las causas de parada son multifactoriales

El principal motivo de parada es `error_operario`, con **274 registros**, lo que representa aproximadamente el **25,61%** de las paradas.

Sin embargo, el resto de motivos también tienen un peso importante:

- falta de material
- mantenimiento
- fallo de máquina
- desconocido

Esto indica que las paradas no se explican por una única causa.

El problema tiene carácter multifactorial y combina factores humanos, técnicos y logísticos.

La categoría `desconocido` también es relevante porque muestra una limitación en la calidad del dato. Si algunas paradas no se registran correctamente, será más difícil identificar causas reales y proponer acciones precisas.

---

## 11.5 Insight 4: El turno de noche presenta la menor eficiencia global

En el análisis global, la eficiencia por turno muestra este patrón:

| Turno | Eficiencia |
|---|---:|
| Mañana | 56,3 piezas/h |
| Tarde | 58,2 piezas/h |
| Noche | 45,8 piezas/h |

El turno de tarde es el más eficiente a nivel global, mientras que el turno de noche presenta el menor rendimiento.

Esto no significa necesariamente que el turno de noche tenga más defectos. El problema principal parece estar más relacionado con la cantidad de piezas válidas producidas por hora.

Por tanto, el turno de noche debería analizarse desde una perspectiva operativa:

- ritmo de producción
- disponibilidad de personal
- respuesta ante incidencias
- supervisión
- mantenimiento disponible
- carga de trabajo

---

## 11.6 Insight 5: La línea B requiere atención prioritaria

La matriz de comparación entre líneas muestra que la línea B combina varios indicadores negativos:

| Línea | Producción | Merma | Piezas/h | Paradas |
|---|---:|---:|---:|---:|
| A | 559.554 | 4,34% | 53,50 | 355 |
| B | 521.641 | 4,42% | 52,46 | 359 |
| C | 514.246 | 4,24% | 53,90 | 322 |
| No registrada | 52.624 | 4,11% | 53,80 | 34 |

La línea B presenta:

- la mayor merma
- la menor eficiencia
- el mayor número de paradas

Aunque las diferencias no son extremas, la línea B destaca porque concentra varios indicadores negativos al mismo tiempo.

Por este motivo, debería ser una de las prioridades en una revisión operativa.

---

## 11.7 Insight 6: Las paradas se relacionan con una merma ligeramente superior

El análisis de parada vs merma muestra:

| Estado | Merma |
|---|---:|
| Sin parada | 4,30% |
| Con parada | 4,43% |
| Diferencia | +0,13 pp |

Los registros con parada presentan una merma ligeramente superior.

La diferencia es pequeña, por lo que no se puede afirmar que las paradas sean la causa principal de la merma.

Sin embargo, sí puede decirse que existe una asociación entre interrupciones y una ligera pérdida de calidad.

La interpretación correcta es:

> Las paradas pueden afectar a la estabilidad del proceso, pero no explican por sí solas el problema de merma.

---

## 11.8 Insight especial del dashboard interactivo

Uno de los hallazgos más interesantes aparece al utilizar el dashboard de forma interactiva.

Al filtrar por la fase **Tratamiento térmico**, se observa que varios indicadores cambian respecto al análisis global.

En el análisis general, el turno más eficiente es **tarde**. Sin embargo, al seleccionar únicamente **Tratamiento térmico**, el turno más eficiente pasa a ser **mañana**.

Resultados aproximados al filtrar por Tratamiento térmico:

| Turno | Eficiencia en Tratamiento térmico |
|---|---:|
| Mañana | 53,3 piezas/h |
| Tarde | 50,1 piezas/h |
| Noche | 40,2 piezas/h |

Este hallazgo es importante porque demuestra que la eficiencia no debe interpretarse solo a nivel global.

Aunque el turno de tarde sea el más eficiente en el conjunto del proceso, existen fases concretas donde otro turno puede obtener mejores resultados.

La conclusión es que el rendimiento depende del cruce entre:

- fase del proceso
- turno
- condiciones operativas
- tipo de tarea realizada

Por tanto, las decisiones de mejora no deberían aplicarse igual a todos los turnos. Es necesario analizar cada fase de forma específica.

Este insight aporta valor porque demuestra que el dashboard no solo resume datos, sino que permite explorar patrones ocultos mediante filtros interactivos.

---

## 11.9 Problemas detectados

A partir del análisis se detectan los siguientes problemas principales:

### Problema 1: concentración de merma en control de calidad

Control de calidad concentra el mayor porcentaje de merma. Esto puede indicar que los defectos se acumulan o se detectan en una fase final del proceso.

No obstante, el origen real podría encontrarse en fases anteriores.

### Problema 2: alto número de paradas en mecanizado

Mecanizado registra el mayor número de interrupciones. Esto puede afectar negativamente al flujo productivo y provocar pérdidas de tiempo operativo.

### Problema 3: menor eficiencia del turno de noche

El turno de noche produce menos piezas válidas por hora que mañana y tarde. Esto indica una pérdida de rendimiento operativo que debe investigarse.

### Problema 4: línea B como foco crítico

La línea B combina mayor merma, menor eficiencia y más paradas. Esto la convierte en una candidata clara para una revisión prioritaria.

### Problema 5: registros con motivo desconocido

La existencia de paradas clasificadas como `desconocido` limita la calidad del análisis. Si no se registra correctamente la causa de una parada, es difícil diseñar acciones correctivas eficaces.

### Problema 6: los patrones cambian al filtrar

El caso de Tratamiento térmico demuestra que los indicadores globales pueden ocultar comportamientos específicos. Esto implica que las decisiones basadas solo en datos agregados pueden ser incompletas.

---

## 11.10 Conclusión del bloque

El análisis permite concluir que la eficiencia de la producción industrial no depende de un único factor.

La merma global es del **4,33%**, pero existen focos claros de mejora en fases, turnos y líneas concretas.

Los puntos más relevantes son:

- Control de calidad concentra la mayor merma.
- Mecanizado acumula el mayor número de paradas.
- El turno de noche presenta la menor eficiencia global.
- La línea B combina los peores indicadores globales.
- Las paradas se asocian con una merma ligeramente superior.
- El patrón de eficiencia cambia al filtrar por fases como Tratamiento térmico.

La principal conclusión es que el problema es **multifactorial**.

No conviene actuar únicamente sobre un indicador global, sino analizar la producción por fase, turno y línea.

El dashboard permite detectar estos patrones de forma visual e interactiva, facilitando la toma de decisiones basada en datos.

---

# 🟩 BLOQUE 12: Recomendaciones

En este bloque se plantean acciones concretas a partir de los resultados del análisis.

El objetivo es convertir los insights en decisiones operativas que puedan ayudar a reducir mermas, disminuir paradas y mejorar la eficiencia global de la producción.

Las recomendaciones se priorizan según impacto esperado y urgencia.

---

## 12.1 Prioridad 1: Revisar mecanizado

La fase de mecanizado debe ser una de las primeras áreas de intervención, ya que concentra el mayor número de paradas del proceso.

Aunque la mayor merma se detecta en control de calidad, mecanizado aparece como una fase crítica porque puede afectar tanto a la continuidad operativa como a la calidad posterior del producto.

### Acciones recomendadas

- Revisar el estado de la maquinaria de mecanizado.
- Analizar las causas exactas de las 363 paradas registradas.
- Comprobar si las paradas se concentran en máquinas, operarios o turnos concretos.
- Revisar la frecuencia del mantenimiento preventivo.
- Analizar si las paradas de mecanizado coinciden con aumentos de defectos en fases posteriores.
- Documentar mejor cada incidencia técnica.

### Objetivo

Reducir interrupciones en una fase crítica y evitar que los problemas operativos afecten a fases posteriores.

---

## 12.2 Prioridad 2: Investigar control de calidad como punto de detección

La fase de control de calidad presenta la mayor merma, con un **7,34%**.

Sin embargo, esta fase puede estar actuando como punto de detección y no necesariamente como origen del problema.

Por tanto, no se recomienda actuar solo sobre control de calidad, sino rastrear hacia atrás el origen de los defectos.

### Acciones recomendadas

- Clasificar los tipos de defectos detectados en control de calidad.
- Relacionar cada defecto con la fase anterior donde pudo originarse.
- Analizar si los defectos proceden principalmente de mecanizado, tratamiento térmico o corte.
- Crear un registro más detallado de defectos.
- Revisar si hay patrones por turno, línea u operario.
- Incorporar controles intermedios antes de llegar a control de calidad.

### Objetivo

Pasar de detectar defectos al final del proceso a prevenirlos antes de que lleguen a control de calidad.

---

## 12.3 Prioridad 3: Intervenir sobre la línea B

La línea B debe revisarse con prioridad porque concentra el peor equilibrio operativo:

- mayor merma
- menor eficiencia
- mayor número de paradas

Aunque las diferencias con las otras líneas no son extremas, la combinación de indicadores negativos convierte a la línea B en un punto claro de mejora.

### Acciones recomendadas

- Comparar procesos de la línea B con las líneas A y C.
- Revisar maquinaria, tiempos y mantenimiento de la línea B.
- Analizar si trabaja con materiales, productos o cargas diferentes.
- Revisar qué turnos operan con mayor frecuencia en esa línea.
- Identificar si existen operarios o equipos asociados a más incidencias.
- Aplicar una auditoría operativa específica sobre la línea B.

### Objetivo

Detectar si la línea B tiene un problema técnico, organizativo o de carga de trabajo que esté afectando a su rendimiento.

---

## 12.4 Prioridad 4: Analizar el turno de noche

El turno de noche presenta la menor eficiencia global, con aproximadamente **45,8 piezas/h**, frente a valores superiores en mañana y tarde.

El problema no parece ser principalmente de calidad, sino de rendimiento operativo.

### Acciones recomendadas

- Revisar la carga de trabajo del turno de noche.
- Analizar disponibilidad de personal y supervisión.
- Comprobar si hay menos soporte de mantenimiento durante la noche.
- Estudiar tiempos de respuesta ante paradas.
- Comparar procedimientos entre turnos.
- Identificar si determinadas fases empeoran especialmente durante la noche.
- Revisar descansos, fatiga y organización del equipo.

### Objetivo

Entender por qué el turno de noche produce menos piezas válidas por hora y corregir los factores que reducen su rendimiento.

---

## 12.5 Prioridad 5: Analizar cruces entre fase y turno

Uno de los hallazgos más importantes del dashboard interactivo es que el patrón global puede cambiar al filtrar por fase.

A nivel global, el turno de tarde es el más eficiente. Sin embargo, al seleccionar Tratamiento térmico, el turno de mañana pasa a ser el más eficiente.

Esto demuestra que las decisiones no deben tomarse únicamente con datos agregados.

### Acciones recomendadas

- Crear una tabla de eficiencia por fase y turno.
- Identificar el mejor turno para cada fase.
- Comparar las prácticas del turno de mañana en tratamiento térmico.
- Revisar si existen diferencias de personal, maquinaria o planificación.
- Replicar buenas prácticas de un turno en otros cuando tenga sentido.
- No aplicar una solución general sin validar primero el comportamiento por fase.

### Objetivo

Evitar decisiones demasiado generales y diseñar mejoras específicas según la combinación fase-turno.

---

## 12.6 Prioridad 6: Mejorar el registro de paradas

El motivo `desconocido` aparece como una categoría dentro de las paradas.

Esto no debe eliminarse del análisis, porque representa una limitación real en la calidad del dato.

Sin embargo, sí debería reducirse en el futuro.

### Acciones recomendadas

- Hacer obligatorio registrar el motivo de parada.
- Crear una lista cerrada de motivos.
- Evitar campos libres cuando sea posible.
- Añadir una opción “otros” solo si se acompaña de explicación.
- Formar al personal en el registro correcto de incidencias.
- Revisar semanalmente los registros incompletos.
- Crear alertas cuando aumenten los motivos desconocidos.

### Objetivo

Mejorar la calidad del dato para que los futuros análisis permitan identificar causas con mayor precisión.

---

## 12.7 Prioridad 7: Reforzar formación y procedimientos

El principal motivo de parada es error de operario, por lo que existe una oportunidad clara de mejora mediante formación y estandarización de procedimientos.

### Acciones recomendadas

- Revisar instrucciones de trabajo.
- Crear checklists operativos por fase.
- Formar a los operarios en los errores más frecuentes.
- Analizar si los errores se concentran en determinados turnos o fases.
- Establecer sesiones de mejora continua con los equipos.
- Compartir buenas prácticas entre turnos.
- Revisar si la interfaz de registro o los procedimientos inducen a error.

### Objetivo

Reducir paradas asociadas a errores humanos y mejorar la estabilidad del proceso.

---

## 12.8 Prioridad 8: Reforzar mantenimiento preventivo

Los motivos relacionados con mantenimiento y fallo de máquina tienen un peso importante dentro de las paradas.

Esto indica que no basta con reaccionar cuando ocurre una incidencia. La empresa debería reforzar acciones preventivas.

### Acciones recomendadas

- Revisar calendario de mantenimiento.
- Priorizar máquinas de mecanizado.
- Analizar fallos repetitivos por equipo.
- Crear indicadores de paradas por máquina.
- Revisar si el mantenimiento se realiza antes de que aumenten las incidencias.
- Coordinar producción y mantenimiento para reducir tiempos improductivos.

### Objetivo

Reducir fallos técnicos y evitar que las máquinas generen paradas recurrentes.

---

## 12.9 Prioridad 9: Mejorar la gestión de materiales

La falta de material aparece como uno de los motivos principales de parada.

Esto indica un posible problema logístico o de planificación.

### Acciones recomendadas

- Revisar stock mínimo por fase.
- Analizar si la falta de material se concentra en una línea o turno.
- Mejorar coordinación entre almacén y producción.
- Crear alertas de inventario.
- Revisar tiempos de reposición.
- Analizar si determinadas fases consumen material más rápido de lo previsto.

### Objetivo

Evitar que la producción se detenga por falta de disponibilidad de materiales.

---

## 12.10 Plan de acción recomendado

Para que las recomendaciones sean aplicables, se propone organizarlas en tres niveles.

### Corto plazo

Acciones rápidas y de bajo coste:

- Mejorar el registro de motivos de parada.
- Revisar la categoría `desconocido`.
- Analizar en detalle la línea B.
- Revisar el turno de noche.
- Crear una tabla fase-turno para detectar patrones específicos.

### Medio plazo

Acciones que requieren coordinación:

- Auditar la fase de mecanizado.
- Revisar procedimientos de operarios.
- Reforzar formación en errores frecuentes.
- Ajustar planificación de materiales.
- Comparar prácticas entre turnos.

### Largo plazo

Acciones estructurales:

- Implementar mantenimiento preventivo más avanzado.
- Crear controles intermedios de calidad.
- Automatizar el seguimiento de incidencias.
- Construir un dashboard recurrente de producción.
- Establecer reuniones periódicas de mejora continua basadas en datos.

---

## 12.11 Impacto esperado

Si la empresa aplica estas recomendaciones, podría conseguir:

- reducción de paradas en fases críticas
- menor merma
- mayor producción útil
- mejor eficiencia en turnos menos productivos
- mejor trazabilidad de incidencias
- mayor capacidad de anticipación
- decisiones operativas basadas en datos

La mejora más importante no sería solo reducir defectos o paradas, sino crear un sistema de seguimiento continuo que permita detectar problemas antes de que se conviertan en pérdidas significativas.

---

## 12.12 Conclusión del bloque

Las recomendaciones muestran que el análisis no debe quedarse en la visualización de datos.

El verdadero valor aparece cuando los resultados se convierten en acciones concretas.

La empresa debería empezar por revisar:

1. Mecanizado, por su alto número de paradas.
2. Control de calidad, como punto principal de detección de defectos.
3. Línea B, por combinar los peores indicadores.
4. Turno de noche, por su baja eficiencia global.
5. Cruces fase-turno, porque los patrones cambian según el contexto.
6. Calidad del registro de datos, para mejorar futuros análisis.

La principal recomendación es actuar de forma priorizada y basada en evidencia, evitando decisiones generales que no tengan en cuenta las diferencias entre fases, turnos y líneas.

Este bloque diferencia el proyecto porque convierte un dashboard descriptivo en una herramienta de mejora operativa real.

---

# 🟩 BLOQUE 13: Estado final del proyecto

Al finalizar el proyecto, se ha completado un flujo completo de análisis de datos aplicado a un caso industrial.

El proyecto incluye:

- definición del problema de negocio
- diseño del dataset
- generación de datos simulados
- limpieza en SQL
- validación de calidad de datos
- análisis exploratorio
- análisis específico de negocio
- modelado con medidas DAX
- dashboard interactivo en Power BI
- insights de negocio
- recomendaciones accionables

---

# 🟩 BLOQUE 14: Aprendizajes del proyecto

Este proyecto permitió trabajar un flujo completo similar al que podría encontrarse en un entorno profesional de análisis de datos.

---

## Aprendizajes técnicos

- Importar datos sucios en PostgreSQL.
- Diseñar una tabla raw para evitar errores de carga.
- Crear una tabla limpia con tipos adecuados.
- Limpiar fechas, textos, categorías y booleanos.
- Validar incoherencias lógicas.
- Trabajar con nulos controlados.
- Crear medidas DAX en Power BI.
- Diseñar un dashboard interactivo.
- Usar filtros, tooltips y visualizaciones variadas.

---

## Aprendizajes analíticos

- Diferenciar un punto de detección de un punto de origen.
- No interpretar porcentajes sin revisar el volumen absoluto.
- No asumir causalidad solo por observar relación entre variables.
- Analizar datos globales y también cruces específicos.
- Detectar limitaciones en la calidad del dato.
- Traducir resultados técnicos en decisiones de negocio.

---

## Aprendizajes de negocio

- Una merma baja en porcentaje puede tener alto impacto en volumen.
- Las paradas no siempre tienen una única causa.
- La eficiencia debe analizarse por fase, turno y línea.
- Los dashboards deben facilitar decisiones, no solo mostrar gráficos.
- Las recomendaciones deben ser concretas, priorizadas y accionables.

---

# 🟩 BLOQUE 15: Conclusión final del proyecto

El proyecto demuestra cómo un dataset inicialmente sucio puede convertirse en una herramienta útil para la toma de decisiones.

A través de SQL se transformaron datos inconsistentes en una base limpia y validada. Después, mediante Power BI, se construyó un dashboard que permite explorar la producción desde distintas perspectivas.

Los principales hallazgos fueron:

- la mayor merma se detecta en control de calidad
- mecanizado concentra el mayor número de paradas
- el turno de noche tiene menor eficiencia operativa
- la línea B requiere atención prioritaria
- las paradas se asocian con una merma ligeramente superior
- los patrones globales pueden cambiar al filtrar por fase

La conclusión principal es que la eficiencia industrial no depende de un único factor. El problema es multifactorial y requiere analizar conjuntamente calidad, producción, paradas, turnos y líneas.

El valor del proyecto está en haber pasado de datos sucios a recomendaciones concretas de negocio.

---

# 🧰 Tecnologías utilizadas

- PostgreSQL
- pgAdmin4
- SQL
- Excel
- Power BI Desktop
- Power Query
- DAX
- GitHub
- IA generativa como apoyo

---

# 🤖 Uso de IA en el proyecto

La inteligencia artificial se utilizó como herramienta de apoyo en distintas fases del proyecto.

---

## Usos principales

- Generación del dataset simulado.
- Creación de datos intencionadamente sucios.
- Apoyo en estrategias de limpieza.
- Revisión de enfoques SQL.
- Apoyo en la estructuración del análisis.
- Redacción y mejora de insights.
- Organización de la documentación.

---

## Criterio aplicado

La IA se utilizó como apoyo, no como sustituto del análisis.

Las decisiones importantes se revisaron manualmente:

- qué variables incluir
- cómo limpiar los datos
- qué valores transformar a `NULL`
- qué registros conservar
- qué visuales usar
- cómo interpretar los resultados
- qué recomendaciones proponer

El foco principal fue comprender los datos y convertirlos en conclusiones útiles.

---

# 📌 Cierre

Este notebook documenta el workflow completo del proyecto.

El resultado final es un caso práctico de análisis de datos aplicado a producción industrial, con foco en:

- limpieza de datos
- análisis exploratorio
- visualización
- pensamiento analítico
- toma de decisiones basada en datos
- comunicación de insights de negocio

El proyecto queda preparado para documentarse en el README del repositorio y presentarse como pieza de portfolio para perfil Data Analyst / Data Junior.
