# Metabase

<!--
https://github.com/tlrobinson/metabase-proxy-examples
https://github.com/heroku-examples/analytics-with-kafka-redshift-metabase
https://github.com/fhsgoncalves/metabase-sparksql-databricks-driver
https://configr.com/blog/medo-de-bi-pelo-alto-investimento-conheca-o-metabase/
-->

## Supported Databases

- Amazon Redshift
- BigQuery
- Druid
- Google Analytics
- H2
- MongoDB
- MySQL
- PostgreSQL
- Presto
- Snowflake
- Spark SQL
- SQL Server
- SQLite

## References

- [DB2 Driver](https://github.com/dludwig-jrt/metabase-db2-driver)

## CLI

### Installation

#### Homebrew

```sh
brew install metabase
```

### Services

```sh
# Homebrew
brew services start metabase
```

## Helm

### References

- [Configuration](https://github.com/pmint93/helm-charts/tree/master/charts/metabase#configuration)

### Repository

```sh
helm repo add pmint93 'https://pmint93.github.io/helm-charts'
helm repo update
```

### Install

```sh
#
export INGRESS_HOST='127.0.0.1'

#
kubectl create ns metabase
```

```sh
#
helm install metabase pmint93/metabase \
  --namespace metabase \
  --version 1.0.0 \
  -f <(cat << EOF
ingress:
  enabled: true
  hosts:
  - metabase.${INGRESS_HOST}.nip.io
EOF
)
```

### Status

```sh
kubectl rollout status deploy/metabase \
  -n metabase
```

### Logs

```sh
kubectl logs \
  -l 'app=metabase' \
  -n metabase \
  -f
```

### DNS

```sh
dig @10.96.0.10 metabase.metabase.svc.cluster.local +short
nslookup metabase.metabase.svc.cluster.local 10.96.0.10
```

#### ExternalDNS

```sh
dig @10.96.0.10 "metabase.${INGRESS_HOST}.nip.io" +short
nslookup "metabase.${INGRESS_HOST}.nip.io" 10.96.0.10
```

### Delete

```sh
helm uninstall metabase \
  -n metabase

kubectl delete ns metabase \
  --grace-period=0 \
  --force
```

## Docker

### Network

```sh
docker network create workbench \
  --subnet 10.1.1.0/24
```

### Running

```sh
docker run -d \
  $(echo "$DOCKER_RUN_OPTS") \
  -h postgres \
  -e POSTGRES_USER='metabase' \
  -e POSTGRES_PASSWORD='metabase' \
  -e POSTGRES_DB='metabase' \
  -v metabase-postgres-data:/var/lib/postgresql/data \
  -p 5432:5432 \
  --name metabase-postgres \
  --network workbench \
  docker.io/library/postgres:11.2-alpine
```

```sh
docker run -d \
  $(echo "$DOCKER_RUN_OPTS") \
  -h metabase \
  -e MB_DB_TYPE='postgres' \
  -e MB_DB_DBNAME='metabase' \
  -e MB_DB_PORT='5432' \
  -e MB_DB_USER='metabase' \
  -e MB_DB_PASS='metabase' \
  -e MB_DB_HOST='metabase-postgres' \
  -p 3000:3000 \
  --name metabase \
  --network workbench \
  docker.io/metabase/metabase:v0.34.1
```

> Wait! This process take a while.

```sh
docker logs -f metabase | sed '/Metabase Initialization COMPLETE/ q'
```

```sh
echo -e '[INFO]\thttp://127.0.0.1:3000'
```

### Database

```sh
# Backup
PGPASSWORD='metabase' pg_dump \
  -d metabase \
  -h 127.0.0.1 \
  -U metabase \
  > /path/to/dump/metabase-$(gdate +%Y-%m-%d-%H-%M).sql

# Restore
# TODO
```

### Remove

```sh
docker rm -f metabase-postgres metabase

docker volume rm metabase-postgres-data
```

## Host

### Build

<!--
https://www.metabase.com/docs/latest/operations-guide/running-metabase-on-debian.html
-->

```sh
#
export METABASE_VERSION=0.34.1

#
mkdir -p /usr/local/src/metabase && cd "$_"

# cURL
curl -L "https://github.com/metabase/metabase/archive/v$METABASE_VERSION.tar.gz" | \
  tar -xzC /usr/local/src/metabase --transform s/metabase-// && \
    cd ./$METABASE_VERSION

#
lein install-for-building-drivers

#
yarn install
yarn build

#
lein ring server
# nohup lein ring server &
# nohup lein ring server-headless &
# nohup lein ring server-headless 8080 > /dev/null 2>&1 &

#
rm ./metabase.db.*
```

```sh
echo -e '[INFO]\thttp://127.0.0.1:3000'
```

### Tips

<!-- #### Proxy

TODO -->

<!--
https://stackoverflow.com/questions/57881019/i-cant-get-atom-text-editor-leiningen-proto-repl-to-work-for-programming-c
-->

## General

### Tips

#### Google Search Operators

```txt
allintitle:"Metabase" site:.com "Sign in to Metabase"
```

#### Ldap

1. Settings
2. Admin
3. Authentication
4. LDAP
   - Server Settings

#### Druid

1. Settings
2. Admin
3. Databases -> Add database
   - Name: wikipedia
   - Host: http://druid-broker
   - Save
