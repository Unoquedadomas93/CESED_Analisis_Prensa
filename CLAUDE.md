# CESED-GDELT-Sentencia-1994: project brief

Handoff from a planning conversation. Nothing has been built yet: no repo, no queries run, no data extracted. Items marked **[unverified]** come from memory of GDELT/BigQuery documentation and must be checked before being relied on.

## About the user

- Complete beginner in SQL. Explain each query in plain language, introduce one concept at a time, and keep queries short and commented.
- Works in the BigQuery Studio web console (UI in Spanish) and is about to create a GitHub repository for this project.
- Google Cloud project: `CESED-GDELT-Sentencia-1994` (project ID appears as `cesed-gdelt-sentencia-1994`). It is in the BigQuery **sandbox** (no billing configured).

## Goal

Analyze media coverage in Colombia around an event in 1994-95 using GDELT on BigQuery.

**The event has not been specified yet.** The project name suggests Sentencia C-221 of May 1994 (Constitutional Court ruling on the personal dose), but this is a guess. Ask the user to confirm the event, its exact date, and the key actors before writing analysis queries.

Also ask: is the interest in international attention to Colombia, or in what the Colombian press itself said? That decides whether GDELT is the main source or a complement.

## Critical data limitation

GDELT most likely cannot show Colombian newspaper coverage for 1994-95. **[unverified]**

- Only the Events table (`gdelt-bq.full.events`, 1979 to present) covers those years.
- For that period the underlying sources are mostly international wires and English-language services (AFP, AP, BBC Monitoring, Xinhua, NYT and similar), not El Tiempo or El Espectador.
- `SOURCEURL` is only populated from 2013; the Global Knowledge Graph (themes, outlet lists) starts in 2015. There is no per-outlet information for 1994.
- No field names an event directly. Relevance has to be inferred from date, location, actors and CAMEO event codes, then checked by reading sample rows.

So the question GDELT can answer is: how did the volume, tone and type of reported events located in Colombia change around the event date, compared with a baseline? Coverage by the Colombian press would need a newspaper archive (El Tiempo's digital archive goes back to about 1990 **[unverified]**) as a second arm.

State this limitation in the README and in any write-up.

## Method

1. Confirm the event, date, actors and relevant event types with the user.
2. Create a dataset in the user's project (suggested name `sentencia_1994`).
3. Explore: schema and about 100 sample rows for Colombia in 1994.
4. Extract once: save Colombia events for 1993-1996 (a year of baseline each side) into a small table. All later queries run on that table.
5. Analyze: volume over time, tone over time, top actors, event types.
6. Validate: read rows near the event date to check they relate to the event.
7. Visualize and write up, with limitations documented.

## Planned repository layout

```
README.md                              research question, data source, limitations, how to rerun
codebook.md                            columns and CAMEO codes used, in plain language
sql/01_explore.sql                     schema check and sample rows
sql/02_extract_colombia_1993_1996.sql  creates the small working table
sql/03_volume_over_time.sql            events and article counts per month or week
sql/04_tone_over_time.sql              average tone per period
sql/05_actors_and_event_types.sql      who appears, what kinds of events
sql/06_validation_sample.sql           rows near the event date for manual reading
results/                               exported CSVs and charts
```

## How queries get run

- GitHub holds the text of the queries and notes; BigQuery runs them. Nothing needs uploading to Google Cloud.
- For now the user copies each `.sql` file into a "Consulta en SQL" tab in BigQuery Studio and runs it there, then exports results as CSV into `results/`.
- Later option: the `bq` command-line tool or Python, once queries are stable.

## Rules for this project

- **Check scan size before every query on `gdelt-bq.full.events`.** Select only needed columns, never `SELECT *`. The sandbox allows about 1 TB of queries per month and 10 GB of storage, and sandbox tables expire after 60 days **[unverified]**. Remind the user the extract will need re-creating if it expires.
- Never commit credentials, key files or tokens. Add them to `.gitignore` from the start.
- Small result CSVs can be committed; large extracts stay in BigQuery.
- Keep SQL to five ideas where possible: `SELECT`, `WHERE`, `GROUP BY`, `ORDER BY`, `CREATE TABLE AS`.

## Starting points for the first queries

Column names below are from memory **[unverified]**; confirm against the schema in `01_explore.sql` first.

- Date: `SQLDATE` (integer, YYYYMMDD), also `MonthYear`, `Year`
- Location: `ActionGeo_CountryCode` (FIPS code, Colombia = `'CO'`), `ActionGeo_FullName`
- Actors: `Actor1Name`, `Actor1CountryCode`, `Actor1Type1Code`, and the `Actor2` equivalents
- Event type: `EventCode`, `EventRootCode`, `QuadClass`, `GoldsteinScale`
- Attention: `NumMentions`, `NumSources`, `NumArticles`
- Tone: `AvgTone`

Sketch of the extract (adjust after checking the schema and the scan estimate):

```sql
-- Saves a small copy of events located in Colombia, 1993-1996
CREATE TABLE `cesed-gdelt-sentencia-1994.sentencia_1994.colombia_1993_1996` AS
SELECT
  SQLDATE, MonthYear, Year,
  Actor1Name, Actor1CountryCode, Actor1Type1Code,
  Actor2Name, Actor2CountryCode, Actor2Type1Code,
  EventCode, EventRootCode, QuadClass, GoldsteinScale,
  NumMentions, NumSources, NumArticles, AvgTone,
  ActionGeo_FullName, ActionGeo_CountryCode
FROM `gdelt-bq.full.events`
WHERE ActionGeo_CountryCode = 'CO'
  AND Year BETWEEN 1993 AND 1996;
```

## First tasks

1. Ask the user for the event, date, actors, and international vs Colombian-press focus.
2. Set up the repo skeleton above with `.gitignore` and a README stating the limitation.
3. Write `01_explore.sql` and have the user run it to verify the unverified claims (date coverage, column names, whether `SOURCEURL` is empty for 1994).
4. Then write `02_extract_colombia_1993_1996.sql`.
