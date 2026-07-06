# Titanic — NiFi → Airflow → Hive Pipeline

Ingestion-to-warehouse pipeline: Apache NiFi lands the Titanic dataset into HDFS, then
an Airflow-orchestrated PySpark job cleans and transforms it into a Hive table, with
survival-analysis queries on top.

## Stack
Apache NiFi · Apache Airflow · PySpark · Apache Hive · HDFS

## Pipeline
1. **Ingest (NiFi)** — download `titanic.csv`, move it across staging directories, and
   load it into HDFS. Shell: [src/ingest.sh](src/ingest.sh)

![NiFi flow](img/nifi.png)

2. **Transform (Spark)** — drop `SibSp`/`Parch`, impute age by sex, set null cabins to 0.
   [src/titanic-pyspark.py](src/titanic-pyspark.py)
3. **Orchestrate (Airflow)** — a DAG loads the transformed data into Hive `titanicdb`.
   [src/titanic.py](src/titanic.py)

![Airflow DAG](img/dag.png)

## Analysis
Survivors by sex and by class, plus oldest and youngest survivors — queried in Hive.

![Survival analysis](img/table.png)

---
*Built on the public Titanic dataset.*
