### 🔍 Calidad de datos

El dataset presenta valores nulos en algunas variables no críticas o derivados del proceso de limpieza.

Estos nulos se deben a:

- Eliminación de valores incoherentes (ej: defectos > producción)
- Datos inválidos o mal formateados en el origen
- Variables dependientes (ej: motivo_parada solo aplica si hubo parada)

Se priorizó la **calidad y coherencia del dato** frente a la completitud, manteniendo únicamente información fiable para el análisis.

**El archivo CLEAN es .csv UTF-8
