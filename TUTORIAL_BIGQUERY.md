# Tutorial: explorar GDELT en BigQuery Studio

La meta es comprobar qué registros de GDELT existen alrededor de la Sentencia C-221/1994. Este ejercicio no descarga artículos de prensa. GDELT Events codifica sucesos reportados en noticias; no incluye el texto de esas noticias.

## 1. Abre BigQuery Studio

1. Inicia sesión en Google Cloud y abre BigQuery Studio.
2. En el selector de proyecto, elige `cesed-gdelt-sentencia-1994`.
3. Pulsa **Consulta en SQL** / **SQL query** para abrir una pestaña.
4. Al pegar cada archivo `.txt`, revisa el nombre del proyecto en las consultas que crean tablas. La tabla pública se escribe con tres partes: proyecto, conjunto de datos y tabla.

La interfaz puede mostrar nombres distintos según idioma y versión. No crees credenciales ni subas datos para estas consultas públicas.

## 2. Aprende a consultar con cuidado

Una consulta es una instrucción para leer o resumir filas. `SELECT` elige columnas, `FROM` indica la tabla y `WHERE` limita filas. Antes de pulsar **Ejecutar**, usa la opción de consulta de prueba o mira **Bytes procesados estimados**. Si no ves una estimación, no ejecutes aún: busca la opción de estimación en la interfaz.

## 3. Comprueba las columnas y años disponibles

Abre `sql/01_esquema_y_cobertura.txt`. Ejecuta primero las consultas de metadatos que no leen los registros completos. Después comprueba las columnas de la tabla. BigQuery distingue mayúsculas y minúsculas según el campo y el contexto; usa los nombres tal como aparecen en el esquema. La tabla histórica puede cambiar o tener diferencias de esquema. Si una consulta da error de columna, vuelve al esquema y adapta el nombre antes de continuar.

## 4. Mira una muestra de Colombia en 1994

Abre `sql/02_muestra_colombia_1994.txt`. Activa la estimación. La consulta selecciona pocas columnas y solo 20 filas. El límite de filas reduce el resultado que ves, pero no siempre reduce los bytes leídos; la condición de fecha y país no garantiza una consulta barata si la tabla no está particionada.

Comprueba si el código de país de acción `CO` corresponde a Colombia en esta tabla y si las filas tienen sentido. Las filas son eventos que tuvieron lugar en Colombia, no necesariamente noticias sobre la sentencia.

## 5. Busca candidatos en torno al fallo

El fallo se emitió el 5 de mayo de 1994. Abre `sql/03_busqueda_ventana_sentencia.txt` y empieza con el rango corto que trae el archivo. Puedes probar una ventana más amplia cambiando las fechas, pero primero revisa los bytes estimados. Inspecciona actores, lugar, código del evento y menciones. No hay un campo de texto que permita buscar las palabras “dosis personal” en el contenido de los artículos.

## 6. Compara con el contexto temporal

Abre `sql/04_resumen_mensual_y_tono.txt`. Esta consulta agrupa actividad relacionada con Colombia por mes en 1993–1995. Es un contexto general para detectar cambios de volumen o tono; no demuestra que un cambio se deba al fallo. `AvgTone` es una medición automática del texto procesado y no clasifica de forma fiable apoyo o rechazo a la sentencia.

## 7. Examina actores y tipos de eventos

Abre `sql/05_actores_y_tipos_evento.txt`. Los actores son entidades que el sistema identificó como participantes en un evento. Un actor ausente no prueba que no apareciera en la noticia. Códigos de evento se explican en el codebook y la documentación CAMEO.

## 8. Valida una muestra a mano

Abre `sql/06_validacion_manual.txt`. Lee las filas antes de atribuirlas al fallo. La tabla puede no ofrecer titular, texto, URL o medio para registros de 1994. Si no puedes comprobar la relación con una fuente periodística, etiqueta la fila como “candidato no validado”.

## 9. (Opcional) Guarda un extracto pequeño

Solo cuando ya confirmaste el esquema y revisaste la consulta, puedes ejecutar `sql/07_guardar_extracto_opcional.txt`. Sustituye el identificador de proyecto y dataset si corresponde. El SQL crea una tabla con un subconjunto de columnas y años. En Sandbox, las tablas pueden expirar automáticamente después de 60 días. No hagas esta copia si aún no la necesitas.

## 10. Exporta y registra resultados

En la tabla de resultados, usa **Guardar resultados** / **Descargar CSV** si está disponible. Guarda los CSV en una carpeta `results/` y conserva una nota con el archivo SQL, fecha de ejecución, bytes procesados, filas devueltas y problemas. Los CSV deben contener solo resultados pequeños. No descargues ni subas grandes volúmenes sin una razón y una revisión del costo.

## Errores comunes

- **No existe la columna:** consulta el esquema y reemplaza el nombre; no adivines.
- **Permiso denegado al proyecto público:** asegúrate de que la consulta usa `gdelt-bq.full.events` exactamente y de que sigues en el proyecto propio para ejecutar.
- **Estimación muy alta:** cancela, reduce columnas/rango o no ejecutes. El `LIMIT` no es una garantía de menos bytes escaneados.
- **Cero resultados:** esto no prueba que la prensa no cubriera la sentencia; puede reflejar el diseño y cobertura de GDELT.
- **Filas que parecen irrelevantes:** es esperable. La tabla registra eventos, no temas jurídicos; clasifica manualmente y no presentes la búsqueda como exhaustiva.
