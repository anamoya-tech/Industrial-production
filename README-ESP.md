# 📊 Industrial Production Analysis

Proyecto de análisis de datos aplicado a una línea de producción industrial de piezas metálicas, en un contexto similar al sector automoción.

El objetivo es identificar ineficiencias operativas, analizar mermas, estudiar paradas de producción y proponer recomendaciones de mejora basadas en datos.

---

## 🚀 Resumen del proyecto

Este proyecto simula un caso real de análisis de datos en una fábrica industrial.

A partir de un dataset inicialmente sucio, se realizó un flujo completo de trabajo:

1. Diseño del problema de negocio  
2. Creación de un dataset simulado con errores reales  
3. Limpieza y validación de datos en SQL  
4. Análisis exploratorio  
5. Modelado de medidas DAX en Power BI  
6. Construcción de un dashboard interactivo  
7. Extracción de insights y recomendaciones de negocio  

El resultado final es un dashboard que permite detectar fases críticas, comparar turnos, analizar líneas de producción y priorizar acciones de mejora.

---

## 🧠 Pregunta de negocio

> ¿Cómo podemos mejorar la eficiencia de la producción reduciendo las paradas y las mermas en las distintas fases del proceso?

---

## 🏭 Contexto

El proceso productivo analizado está compuesto por cinco fases:

- Corte
- Mecanizado
- Tratamiento térmico
- Control de calidad
- Expedición

Cada fase puede presentar problemas relacionados con:

- defectos o mermas
- paradas de producción
- pérdida de eficiencia
- diferencias entre turnos
- diferencias entre líneas de producción

---

## 📂 Estructura del repositorio

```text
Industrial-production/
│
├── data/
│   ├── dataset_produccion_industrial_raw.csv
│   ├── dataset_produccion_clean.csv
│   └── data-quality.md
│
├── docs/
│   ├── notebook.md
│   └── ia.md
│
├── power-bi/
│   ├── dashboard.png
│   ├── dashboard-preview.png
│   └── interactive-dashboard.md
│
└── README.md
```

---

## 📁 Archivos principales

| Archivo | Descripción |
|---|---|
| [`data/dataset_produccion_industrial_raw.csv`](data/dataset_produccion_industrial_raw.csv) | Dataset original simulado, con errores intencionados |
| [`data/dataset_produccion_clean.csv`](data/dataset_produccion_clean.csv) | Dataset limpio y preparado para análisis |
| [`data/data-quality.md`](data/data-quality.md) | Resumen de problemas de calidad de datos y decisiones de limpieza |
| [`docs/notebook.md`](docs/notebook.md) | Diario técnico completo del proyecto, paso a paso |
| [`docs/ia.md`](docs/ia.md) | Explicación del uso de IA durante el proyecto |
| [`power-bi/dashboard.png`](power-bi/dashboard.png) | Captura del dashboard final |
| [`power-bi/dashboard-preview.png`](power-bi/dashboard-preview.png) | Vista previa del dashboard |
| [`power-bi/interactive-dashboard.md`](power-bi/interactive-dashboard.md) | Información sobre el dashboard interactivo en Power BI |

---

## 🛠️ Herramientas utilizadas

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

## 📊 Dataset

El dataset fue generado de forma simulada para representar un entorno industrial realista.

Se diseñó intencionadamente con errores para poder trabajar un proceso completo de limpieza y validación.

### Variables principales

| Grupo | Variables |
|---|---|
| Identificación | `id_registro`, `fecha`, `operario_id` |
| Producción | `unidades_producidas`, `unidades_defectuosas` |
| Proceso | `fase`, `turno`, `linea_produccion` |
| Tiempos | `tiempo_produccion_min`, `tiempo_parada_min` |
| Paradas | `hubo_parada`, `motivo_parada` |

### Problemas incluidos en el dataset raw

- valores nulos
- errores tipográficos
- fechas en distintos formatos
- categorías inconsistentes
- valores booleanos no normalizados
- unidades defectuosas superiores a producidas
- tiempos fuera de rango
- paradas sin motivo informado

---

## 🧹 Limpieza de datos

La limpieza se realizó en PostgreSQL usando una estrategia en dos fases:

1. Importar los datos en una tabla raw con todos los campos como texto.
2. Crear una tabla limpia con tipos correctos y reglas de validación.

### Decisiones principales

| Problema detectado | Decisión aplicada |
|---|---|
| Datos inválidos | Convertir a `NULL` |
| Defectos > producción | Convertir a `NULL` |
| Tiempos irreales | Convertir a `NULL` |
| Paradas sin motivo | Etiquetar como `desconocido` |
| Línea no informada | Mantener como `No registrada` |
| Registros parcialmente válidos | Conservarlos para no perder información útil |

Se priorizó la calidad del dato frente a la completitud.

---

## ✅ Resultado de la limpieza

| Métrica | Resultado |
|---|---:|
| Registros iniciales | 5.201 |
| Registros finales limpios | 4.673 |
| Nulos en variables críticas | 0 |
| Paradas sin motivo tras limpieza | 0 |
| Incoherencias lógicas | 0 |

El dataset final quedó validado y preparado para análisis en Power BI.

---

## 📈 KPIs principales

| KPI | Resultado |
|---|---:|
| Producción total | 1,65 M uds. |
| Defectos totales | 71,36 K uds. |
| Merma global | 4,33% |
| Registros con parada | 22,90% |
| Eficiencia | 53,30 piezas/h |

---

## 📊 Dashboard Power BI

El dashboard final se diseñó para comunicar rápidamente los principales problemas del proceso productivo.

![Dashboard final](power-bi/dashboard.png)

### Estructura del dashboard

#### Parte superior

KPIs principales:

- producción total
- defectos totales
- merma
- paradas
- eficiencia

#### Parte central

Análisis principal:

- merma por fase vs media global
- paradas por fase del proceso
- paradas por motivo

#### Parte inferior

Comparación operativa:

- eficiencia por turno
- comparación entre líneas
- parada vs merma

---

## 📌 Visualizaciones utilizadas

| Análisis | Visual utilizado |
|---|---|
| KPIs principales | Tarjetas |
| Merma por fase vs media global | Gráfico combinado de columnas y línea |
| Paradas por fase | Barras horizontales |
| Paradas por motivo | Gráfico de anillo |
| Eficiencia por turno | Gráfico de líneas |
| Comparación entre líneas | Matriz |
| Parada vs merma | Tarjetas comparativas |

Se eligieron distintos tipos de visuales para evitar un dashboard repetitivo y mejorar la lectura ejecutiva.

---

## 🧮 Medidas DAX principales

```DAX
Produccion Total =
SUM(produccion_tabla_clean[unidades_producidas])
```

```DAX
Defectos Totales =
SUM(produccion_tabla_clean[unidades_defectuosas])
```

```DAX
Unidades Validas =
[Produccion Total] - [Defectos Totales]
```

```DAX
% Merma =
DIVIDE([Defectos Totales], [Produccion Total], 0)
```

```DAX
Total Paradas =
CALCULATE(
    COUNTROWS(produccion_tabla_clean),
    produccion_tabla_clean[hubo_parada] = TRUE()
)
```

```DAX
% Paradas =
DIVIDE(
    [Total Paradas],
    COUNTROWS(produccion_tabla_clean),
    0
)
```

```DAX
Eficiencia =
DIVIDE([Unidades Validas], [Tiempo Total Min], 0)
```

```DAX
Eficiencia Hora =
[Eficiencia] * 60
```

La eficiencia se transformó de unidades por minuto a piezas por hora para que el KPI fuera más claro a nivel de negocio.

---

## 🔍 Principales insights

### 1. Control de calidad concentra la mayor merma

La fase con mayor porcentaje de merma es **control de calidad**, con un **7,34%**, por encima de la media global.

Este resultado indica que los defectos se detectan en esta fase, pero no necesariamente se originan ahí.

---

### 2. Mecanizado es la fase más crítica en paradas

La fase de **mecanizado** concentra el mayor número de paradas, con **363 interrupciones**.

Esto la convierte en una fase prioritaria desde el punto de vista operativo.

---

### 3. Las causas de parada son multifactoriales

El principal motivo de parada es **error de operario**, pero las causas están bastante repartidas entre:

- errores humanos
- falta de material
- mantenimiento
- fallos de máquina
- motivos desconocidos

Esto indica que no existe una única causa del problema.

---

### 4. El turno de noche tiene menor eficiencia

El turno de noche no presenta una merma mucho mayor que el resto, pero sí una menor eficiencia.

El problema parece estar más relacionado con el rendimiento operativo que con la calidad.

---

### 5. La línea B requiere atención prioritaria

La línea B combina:

- mayor merma
- menor eficiencia
- mayor número de paradas

Por tanto, es la línea prioritaria para una revisión operativa.

---

### 6. Las paradas se asocian con una merma ligeramente superior

Los registros con parada presentan una merma del **4,43%**, frente al **4,30%** de los registros sin parada.

La diferencia no es muy alta, pero sugiere que las interrupciones pueden afectar a la estabilidad del proceso.

---

## 💡 Insight especial

Al utilizar el dashboard de forma interactiva, se detectó que algunos patrones cambian al filtrar por fase.

A nivel global, el turno más eficiente es el de tarde. Sin embargo, al filtrar por **Tratamiento térmico**, el turno más eficiente pasa a ser el de mañana.

Esto demuestra que las decisiones no deben basarse solo en indicadores globales. Es necesario analizar cruces entre:

- fase
- turno
- línea
- paradas
- eficiencia

---

## 🚀 Recomendaciones de negocio

### Prioridad 1: revisar mecanizado

Mecanizado concentra el mayor número de paradas y puede estar relacionado con defectos detectados posteriormente.

Acciones recomendadas:

- revisar maquinaria
- analizar causas de parada
- comprobar mantenimiento preventivo
- estudiar si las paradas coinciden con aumentos de merma

---

### Prioridad 2: investigar el origen real de los defectos

Control de calidad detecta la mayor merma, pero el origen puede estar en fases anteriores.

Acciones recomendadas:

- clasificar tipos de defectos
- rastrear defectos hacia fases previas
- revisar mecanizado y tratamiento térmico
- incorporar controles intermedios

---

### Prioridad 3: analizar la línea B

La línea B combina peor calidad, menor eficiencia y más paradas.

Acciones recomendadas:

- comparar la línea B con A y C
- revisar maquinaria y procedimientos
- analizar turnos asignados
- estudiar si trabaja con cargas o materiales diferentes

---

### Prioridad 4: revisar el turno de noche

El turno de noche presenta menor eficiencia operativa.

Acciones recomendadas:

- revisar carga de trabajo
- analizar disponibilidad de personal
- comprobar soporte de mantenimiento
- estudiar tiempos de respuesta ante incidencias

---

### Prioridad 5: mejorar la calidad del registro de paradas

La categoría `desconocido` indica una limitación en la captura de datos.

Acciones recomendadas:

- hacer obligatorio registrar el motivo de parada
- crear una lista cerrada de motivos
- formar al personal en el registro de incidencias
- revisar periódicamente los datos incompletos

---

## 🧠 Conclusión

El análisis muestra que la eficiencia de la producción no depende de un único factor.

El problema es multifactorial y combina:

- calidad
- paradas
- turnos
- líneas de producción
- captura de datos

El dashboard permite pasar de datos sucios a decisiones concretas, identificando dónde intervenir primero y qué acciones pueden generar mayor impacto.

---

## 🤖 Uso de IA en el proyecto

La IA se utilizó como herramienta de apoyo en distintas fases del proyecto:

- generación del dataset simulado
- creación de errores intencionados
- apoyo en estrategias de limpieza
- revisión de enfoques SQL
- estructuración del análisis
- mejora de la documentación

La IA no sustituyó el análisis. Las decisiones importantes fueron revisadas manualmente y adaptadas al contexto del proyecto.

Más detalle en [`docs/ia.md`](docs/ia.md).

---

## 📚 Documentación adicional

- [Notebook técnico completo](docs/notebook.md)
- [Uso de IA en el proyecto](docs/ia.md)
- [Calidad de datos](data/data-quality.md)
- [Dashboard interactivo](power-bi/interactive-dashboard.md)

---

## 📌 Estado del proyecto

Proyecto finalizado.

Incluye:

- dataset raw
- dataset limpio
- limpieza y validación documentada
- análisis exploratorio
- modelado DAX
- dashboard Power BI
- insights de negocio
- recomendaciones accionables

---

## 👤 Autora

**Ana Moya**  
Repositorio desarrollado como proyecto de portfolio para perfil **Data Analyst / Data Junior**.
