FROM python:3.9

RUN echo "krb5-config krb5-config/default_realm string" | debconf-set-selections && \
    echo "krb5-config krb5-config/kerberos_servers string" | debconf-set-selections && \
    echo "krb5-config krb5-config/admin_server string" | debconf-set-selections && \
    DEBIAN_FRONTEND=noninteractive apt-get update

RUN apt-get -q update \
    && apt-get -q install -y --no-install-recommends \
    build-essential \
    tar \
    krb5-user \
    libkrb5-dev \
    gcc \
    graphviz \
    graphviz-dev \
    curl \
    libpq-dev \
    libffi-dev \
    make \
    libsasl2-dev \
    libbz2-dev \
    libsqlite3-dev \
    xz-utils \
    libssl-dev \
    unixodbc-dev \
    libldap2-dev \
    python3-ldap \
    unzip \
    jq && \
    apt-get clean

RUN mkdir -p /opt/tdp/airflow
VOLUME /opt/tdp/airflow
WORKDIR /opt/tdp/airflow