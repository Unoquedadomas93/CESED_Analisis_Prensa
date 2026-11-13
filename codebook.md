# Codebook (guía de campos)

Los nombres siguientes son los esperados en la tabla histórica de eventos. Confírmalos en BigQuery antes de usar las consultas.

| Campo | Explicación sencilla | Precaución |
|---|---|---|
| `GLOBALEVENTID` | Identificador de una fila de evento | No es un identificador de artículo |
| `SQLDATE` | Fecha asociada al evento, como número `YYYYMMDD` | Puede no ser la fecha de publicación de la noticia |
| `Year`, `MonthYear` | Año y mes en formatos numéricos | Confirma su tipo en el esquema |
| `Actor1Name`, `Actor2Name` | Entidades que GDELT identificó como actores | Puede faltar o estar mal identificado |
| `Actor1CountryCode`, `Actor2CountryCode` | Código de país relacionado con el actor | No equivale siempre a nacionalidad de periodista o medio |
| `EventCode`, `EventRootCode` | Código CAMEO del tipo de evento y su familia | Es una clasificación del suceso, no una etiqueta temática de la sentencia |
| `QuadClass` | Agrupación amplia del tipo de evento | No expresa una postura editorial |
| `GoldsteinScale` | Escala asociada a impacto/conflicto del tipo de evento | No es una medida del impacto real de la sentencia |
| `NumMentions` | Menciones detectadas para el evento | No necesariamente son artículos únicos o todos los artículos publicados |
| `NumSources` | Fuentes que mencionan el evento según el procesamiento de GDELT | En registros antiguos puede ser incompleto |
| `NumArticles` | Conteo asociado a artículos que mencionan el evento | No da el texto ni el nombre del medio |
| `AvgTone` | Puntuación automática de tono de textos asociados | No es sentimiento humano ni posición a favor/en contra |
| `ActionGeo_CountryCode` | País donde se sitúa la acción del evento | No indica dónde se publicó o leyó la noticia |
| `ActionGeo_FullName` | Nombre descriptivo de la ubicación de la acción | Verifica valores y calidad geográfica |
| `SOURCEURL` | URL/cita de fuente, si el esquema y el registro la proporcionan | No asumir que está presente o poblado en 1994; inspecciona primero |

**Distinción importante:** “evento situado en Colombia”, “artículo sobre Colombia” y “artículo publicado por prensa colombiana” son tres conceptos distintos. Esta extracción con `ActionGeo_CountryCode = 'CO'` solo aproxima el primero.

Los códigos CAMEO se organizan jerárquicamente; `EventRootCode` agrupa familias y `EventCode` identifica categorías más concretas. No interpretes el código como una valoración política del fallo.
