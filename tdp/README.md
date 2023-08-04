# Airflow Packing
Dependency archive for Airflow 2.6.2 with Python 3.9.

From your airflow and after checkout the adequate branch, you must first build the image (one image for specific airflow|python) that will build the release with:

```bash
docker build . -t airflow-tdp-builder-py3.9 -f tdp/Dockerfile
```
Then, you can execute the build container with the following arguments:

```bash
mkdir tdp/target
docker run --rm -v "./tdp/target:/opt/tdp/airflow/target" \
    -it airflow-tdp-builder-py3.9  \
    bash
```

And now, execute the following commands from the container: 

```bash
TDP_VERSION=TDP-1.0.0
AIRFLOW_VERSION=2.6.2
AIRFLOW=airflow-$AIRFLOW_VERSION-$TDP_VERSION
ARCHIVE=$AIRFLOW.tar.gz
AIRFLOW_EXTRA=[flower,redis,kerberos,celery,apache.hdfs,apache.hive,apache.spark]
mkdir -p target/$AIRFLOW/dependencies
python3 -m venv venv
source venv/bin/activate
python3 -m pip install --upgrade pip wheel
python3 -m pip install psycopg2-binary setuptools python-ldap
python3 -m pip install apache-airflow${AIRFLOW_EXTRA}==$AIRFLOW_VERSION --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-$AIRFLOW_VERSION/constraints-3.9.txt"
python3 -m pip freeze > target/${AIRFLOW}/requirements.txt
python3 -m pip wheel --no-cache-dir --wheel-dir target/${AIRFLOW}/dependencies -r target/${AIRFLOW}/requirements.txt
deactivate
cd target 
tar -czvf $ARCHIVE ${AIRFLOW}
sha256sum $ARCHIVE | sed 's| .*/| |' > $ARCHIVE.sha256
```

This command generates a `tar.gz` file of the release at tdp/target/