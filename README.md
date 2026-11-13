# Prensa y la Sentencia C-221 de 1994: guía de búsqueda en GDELT

Esta carpeta contiene un tutorial paso a paso para explorar qué información de GDELT puede ayudar a estudiar la cobertura de prensa sobre la Sentencia C-221 de 1994, que despenalizó el porte y consumo de la dosis personal en Colombia. La sentencia es del **5 de mayo de 1994**.

## Qué podrás averiguar (y qué no)

GDELT no es un archivo de periódicos ni un buscador de artículos históricos. Para 1994, la tabla histórica `gdelt-bq.full.events` contiene eventos codificados por máquina a partir de noticias. Sus campos describen fecha, actores, lugar y tipo de evento, junto con medidas de menciones/artículos y tono. Una fila **no equivale necesariamente a un artículo** y los campos de tono no son una lectura humana de la postura editorial.

Con este tutorial podrás:

- revisar qué esquema y cobertura tiene la tabla pública;
- explorar eventos asociados a Colombia alrededor del 5 de mayo de 1994;
- examinar registros cercanos y comparar con un período de referencia;
- guardar en tu proyecto solo las filas y columnas que decidas analizar.

No podrás recuperar de esta tabla el texto completo, titulares, citas, enlaces fiables ni una lista exhaustiva de periódicos colombianos e internacionales que publicaron sobre el fallo. La tabla Global Knowledge Graph (GKG), con datos a nivel de documento y campos como temas y organizaciones editoriales, comenzó en 2015; no sirve para localizar artículos de 1994. Los eventos de GDELT tampoco dicen por sí mismos que una fila trate de la sentencia. Las consultas producen **candidatos** que requieren validación y no una medición completa de “lo que dijo la prensa”. Para estudiar lo que publicaron El Tiempo, El Espectador u otros medios colombianos hará falta consultar hemerotecas y archivos periodísticos.

## Archivos y orden de trabajo

Abre los `.txt` de `sql/` en un editor de texto y copia el contenido a una pestaña de consulta de BigQuery Studio. Se usa `.txt` para que sean fáciles de abrir; su contenido es SQL. Ejecuta las consultas en este orden:

1. `sql/01_esquema_y_cobertura.txt` — columnas, años disponibles y metadatos; empieza por estas consultas pequeñas.
2. `sql/02_muestra_colombia_1994.txt` — muestra de eventos de Colombia en 1994.
3. `sql/03_busqueda_ventana_sentencia.txt` — ventana alrededor del fallo, buscando actores y lugares relacionados.
4. `sql/04_resumen_mensual_y_tono.txt` — contexto temporal para 1993–1995.
5. `sql/05_actores_y_tipos_evento.txt` — actores y clases de eventos más frecuentes.
6. `sql/06_validacion_manual.txt` — filas para inspección humana cerca del fallo.
7. `sql/07_guardar_extracto_opcional.txt` — copia reducida en tu propio proyecto, solo después de revisar el costo y el resultado.

El archivo [TUTORIAL_BIGQUERY.md](TUTORIAL_BIGQUERY.md) explica cada paso en español sencillo. [codebook.md](codebook.md) explica los campos y límites de interpretación. [FUENTES.md](FUENTES.md) reúne referencias.

## Antes de empezar

Necesitas acceso a BigQuery Studio con el proyecto `cesed-gdelt-sentencia-1994` seleccionado (o cambiar el nombre del proyecto en las consultas). Este proyecto está configurado en modo Sandbox según las notas iniciales. Google documenta para Sandbox hasta 1 TiB de consultas procesadas al mes y 10 GiB de almacenamiento, y expiración predeterminada de tablas a los 60 días. Comprueba los límites vigentes en la documentación oficial antes de crear tablas.

**Antes de cada consulta a `gdelt-bq.full.events`:** abre el editor de consulta, activa el cálculo de bytes procesados / consulta de prueba (dry run), y revisa la estimación. No ejecutes una consulta si el volumen mostrado te sorprende. La limitación de columnas ayuda con el costo, pero la tabla histórica puede no estar particionada; filtrar fechas no garantiza por sí solo una lectura pequeña.

## Reglas para este proyecto

- No pongas contraseñas, tokens, claves o archivos de credenciales en esta carpeta ni en GitHub.
- No uses `SELECT *` en consultas a tablas grandes.
- Primero inspecciona el esquema; los nombres de campos de las consultas deben confirmarse en tu BigQuery.
- Anota la fecha, consulta, bytes estimados y observaciones cuando exportes resultados.
- Si guardas una tabla en Sandbox, puede expirar. Vuelve a crearla ejecutando el paso de extracción si ocurre.
- Los resultados de eventos son aproximaciones algorítmicas, no artículos periodísticos ni prueba de causalidad.

## Confirmación de la sentencia

La referencia correcta para este proyecto es **C-221/1994**, no C-322/1994. La Corte Constitucional fecha la providencia el 5 de mayo de 1994. Consulta el texto oficial enlazado en [FUENTES.md](FUENTES.md).
