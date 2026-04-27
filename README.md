# 📊 Análisis de Producción Industrial

Proyecto de análisis de datos enfocado en la optimización de procesos productivos en un entorno industrial (tipo automoción), mediante el estudio de mermas, paradas y eficiencia operativa.

---

## 🎯 Objetivo

Identificar ineficiencias en la producción y proponer mejoras a través del análisis de:

- Mermas (defectos)
- Paradas en producción
- Rendimiento por fase y turno

👉 Enfoque orientado a negocio: reducir costes, mejorar calidad y optimizar recursos.

---

## 🏭 Contexto

Se analiza una línea de producción compuesta por 5 fases:

- Corte  
- Mecanizado  
- Tratamiento térmico  
- Control de calidad  
- Expedición  

---

## 📂 Dataset

- Dataset simulado (más de 5000 registros)
- Diseñado intencionadamente con errores para simular un entorno real

### Variables principales:

- Producción: `unidades_producidas`, `unidades_defectuosas`
- Tiempos: `tiempo_produccion_min`, `tiempo_parada_min`
- Operativa: `fase`, `turno`, `linea_produccion`
- Paradas: `hubo_parada`, `motivo_parada`
- Identificación: `fecha`, `operario_id`

---

## 🧹 Limpieza de datos (SQL - PostgreSQL)

Se trabajó con dos tablas:

- `produccion_linea` → datos raw (texto)
- `produccion_limpia` → datos tratados

### Procesos aplicados:

- Normalización de texto (`LOWER`, `TRIM`)
- Estandarización de categorías
- Conversión de fechas a formato `DATE`
- Validación de datos numéricos
- Eliminación de incoherencias:
  - valores negativos
  - defectos > producción
  - tiempos irreales

### Decisiones clave:

- Datos inválidos → `NULL`
- No eliminación de registros
- Paradas sin motivo → `"desconocido"`

### Resultado:

- Registros finales: **4.673**
- Dataset consistente y preparado para análisis

---

## ✅ Validación

Se verificó que:

- No hay nulos en variables críticas (`fecha`, `turno`, `fase`, `hubo_parada`)
- No existen incoherencias lógicas
- Las categorías están normalizadas
- Todas las paradas tienen motivo asociado

👉 Existen valores nulos controlados en variables no críticas o derivadas del proceso de limpieza, manteniendo la coherencia del dataset.

---

## 📊 Análisis Exploratorio (EDA)

### 🔹 KPI GLOBAL

- Producción total: **1.591.258**
- Defectos: **70.876**
- Producción útil: **1.520.382**
- Merma global: **4,45%**

💡 La merma es baja en porcentaje pero significativa en volumen → impacto real en negocio.

---

### ⚙️ Por fase

- Mayor merma: **control de calidad (7,32%)**
- Menor merma: **expedición**

💡 Los defectos se detectan al final, pero se originan en fases anteriores (especialmente mecanizado).

---

### 🛑 Paradas

- 22,9% de los registros presentan paradas

💡 Aproximadamente 1 de cada 4 procesos se ve interrumpido → problema estructural.

---

### 🔍 Motivos de parada

- Error operario → principal causa (~25%)
- Distribución equilibrada entre causas

💡 Problema multifactorial:
- humano
- técnico (maquinaria/mantenimiento)
- logístico (materiales)

👉 ~44% de las paradas están relacionadas con maquinaria.

👉 Existe un % de "desconocido" → limitación en calidad de datos.

---

### ⏱️ Turnos

- Merma estable en todos los turnos (~4,3%)
- Menor producción en turno noche

💡 El problema no depende del turno → es estructural.

---

## 🧠 Conclusiones

- La merma es un problema estructural
- Los defectos se generan antes de ser detectados
- Las paradas afectan significativamente a la eficiencia
- No existe una única causa → problema complejo
- Hay margen de mejora en:
  - formación
  - mantenimiento
  - logística
  - calidad de datos

---

## 🛠️ Tecnologías utilizadas

- PostgreSQL (pgAdmin4)
- SQL

---

## 🚀 Próximos pasos

- 📊 Visualización en Power BI
- 📈 Creación de dashboard interactivo
- 🧩 Storytelling de datos orientado a negocio

---

## 📁 Estructura del proyecto
data/
├── raw/
├── clean/

images/
media/

powerBI/
docs/

---

## 📌 Notas

- Dataset simulado con fines educativos
- Proyecto orientado a portfolio Data Analyst / Data Junior
