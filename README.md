# Batch_streaming_Prediction_results_with_spark_mlflow
Streaming prediction results built with random forest via mlflow

This project reads churn data with spark, uses pyspark ml library RandomForest classification model to predict whether a subscriber will exit or will not.
Batch streaming has been applied to read prediüction results writes streaming data to postgres DB. This algoritm is controlled with spark's mlflow.

In order to use spark's mlflow, you sholuld perform tasks below:

1. Hadoop services must be running
2. activate your virtual environment for spark
3. Install dependencies of psycopg2 package, mlflow and necessary libs for model development.
```
sudo yum -y groupinstall "Development Tools"
sudo yum -y install python3-devel
sudo yum -y install postgresql-libs
sudo yum -y install postgresql-devel
pip install psycopg2
pip install mlflow
pip install pyspark==3.0.0
pip install keras
pip install tensorflow  

4. Prepare postgresql as backend store  
` CREATE DATABASE mlflow OWNER train ENCODING 'UTF8'; `

5. Artifact store as HDFS  
` hdfs dfs -mkdir /user/train/mlflow `

5. Artifact store as HDFS  
` hdfs dfs -mkdir /user/train/mlflow `
5.1. Run mlflow server with hdfs artifact
```
(venvspark) [train@localhost mlflow]$ mlflow server \
--backend-store-uri postgresql+psycopg2://train:Ankara06@localhost:5432/mlflow \
--default-artifact-root hdfs://localhost:9000/user/train/mlflow \
--host 0.0.0.0  > mlflow_server.log 2>&1 &


6. See the postgresql tables are created.

```
postgres=# \c mlflow
psql (9.2.24, server 10.13)
WARNING: psql version 9.2, server version 10.0.
         Some psql features might not work.
You are now connected to database "mlflow" as user "postgres".
mlflow=# \dt
               List of relations
 Schema |         Name          | Type  | Owner
--------+-----------------------+-------+-------
 public | alembic_version       | table | train
 public | experiment_tags       | table | train
 public | experiments           | table | train
 public | latest_metrics        | table | train
 public | metrics               | table | train
 public | model_version_tags    | table | train
 public | model_versions        | table | train
 public | params                | table | train
 public | registered_model_tags | table | train
 public | registered_models     | table | train
 public | runs                  | table | train
 public | tags                  | table | train
(12 rows)

7. Open browser and enter [http://localhost:5000/](http://localhost:5000/)
