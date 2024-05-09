## Setting up our PostgreSQL environment

Deploy a namespace to hold our resources:

```
kubectl create ns postgresql
```

Create our secret for our first PostgreSQL instance:

```
  kubectl -n postgresql create secret generic postgresql \
    --from-literal POSTGRES_USER="postgresadmin" \
    --from-literal POSTGRES_PASSWORD="admin123" \
    --from-literal POSTGRES_DB="postgresdb" \
    --from-literal REPLICATION_USER="replicationuser" \
    --from-literal REPLICATION_PASSWORD="replicationPassword"
```

Deploy our PostgreSQL instance:

```
kubectl -n postgresql apply -f ./statefulset.yaml
```

### Check our installation

```
kubectl -n postgresql get pods

# check the database logs
kubectl -n postgresql logs postgres-0

```

Let's check our instance further:

```
kubectl -n postgresql exec -it postgres-0 -- bash

# login to postgres
psql --username=postgresadmin postgresdb

# see our replication user created
\du

#create a table(optional)
CREATE TABLE customers (firstname text, customer_id serial, date_created timestamp);

#show the table
\dt

# quit out of postgresql
\q

# check the data directory
ls -l /data/pgdata

```
