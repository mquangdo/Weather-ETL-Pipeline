# Weather ETL Pipeline

This repository contains an Apache Airflow ETL workflow that retrieves current weather data from the Open-Meteo API and stores it in PostgreSQL.

## What this project does

The DAG in `/home/runner/work/Weather-ETL-Pipeline/Weather-ETL-Pipeline/mquangdo/Weather-ETL-Pipeline/dags/etl_weather_dag.py` runs daily and performs:

1. **Extract** weather data from Open-Meteo using the Airflow HTTP connection `open_meteo_api`
2. **Transform** the API response into a simplified weather record
3. **Load** the transformed data into PostgreSQL using `postgres_default`

The current coordinates are set to London:
- Latitude: `51.5074`
- Longitude: `-0.1278`

## Data loaded to PostgreSQL

The pipeline creates (if needed) and inserts into:

- Table: `weather_data`
- Columns:
  - `latitude` (FLOAT)
  - `longitude` (FLOAT)
  - `temperature` (FLOAT)
  - `windspeed` (FLOAT)
  - `winddirection` (FLOAT)
  - `weathercode` (INT)
  - `timestamp` (TIMESTAMP, default `CURRENT_TIMESTAMP`)

## Repository structure

- `/home/runner/work/Weather-ETL-Pipeline/Weather-ETL-Pipeline/mquangdo/Weather-ETL-Pipeline/dags/etl_weather_dag.py` - Weather ETL DAG
- `/home/runner/work/Weather-ETL-Pipeline/Weather-ETL-Pipeline/mquangdo/Weather-ETL-Pipeline/dags/exampledag.py` - Astronomer example DAG
- `/home/runner/work/Weather-ETL-Pipeline/Weather-ETL-Pipeline/mquangdo/Weather-ETL-Pipeline/tests/dags/test_dag_example.py` - DAG integrity and import tests
- `/home/runner/work/Weather-ETL-Pipeline/Weather-ETL-Pipeline/mquangdo/Weather-ETL-Pipeline/docker-compose.yml` - Local PostgreSQL service

## Local setup

1. Start PostgreSQL:
   ```bash
   docker compose up -d postgres
   ```
2. Start Airflow project (Astro Runtime based):
   ```bash
   astro dev start
   ```
3. Ensure Airflow connections exist:
   - `open_meteo_api` with host `https://api.open-meteo.com`
   - `postgres_default` pointing to your PostgreSQL instance
4. Trigger DAG `weather_etl_pipeline` from the Airflow UI.

## Validation

This repository includes DAG tests in `/home/runner/work/Weather-ETL-Pipeline/Weather-ETL-Pipeline/mquangdo/Weather-ETL-Pipeline/tests/dags/test_dag_example.py`.

Run tests in an environment that has `pytest` and Airflow installed:

```bash
pytest -q
```
