# PostgreSQL on Kubernetes

## Steps to Setup PostgreSQL Statefulset in Kubernetes

Note: The Dockerfile image build instructions are in Docker folder.

$ echo -n 'PostGreSQL' | base64
  and update the passwd info pgpass.yaml

### Create Secret with PostgreSQL postgres user password.

$ kubectl create -f pgpass.yaml

### Create Storage Class.

$ kubectl create -f lsc.yaml


### Create persistent Volumes

Note: please make sure that the API server started with options
       PersistentLocalVolumes=true,VolumeScheduling=true,MountPropagation=true

On Worker Node kubeslave1:
  $ mkdir -p /app/db/postgres/data001/bb /app/db/postgres/data001/data

$ kubectl create -f pv_slave1_data.yaml
$ kubectl create -f pv_slave1_bin.yaml

### Create StatefulSet

$ kubectl create -f pgdev1_sf.yaml

### Create NodePort Service

$ kubectl create -f pgdev1_sf.yaml

### Listing Statefulset

$ kubectl get statefulset
NAME      DESIRED   CURRENT   AGE
pgdev1    1         1         18h

$ kubectl get po
NAME       READY     STATUS    RESTARTS   AGE
pgdev1-0   1/1       Running   0          18h

$ kubectl exec -ti pgdev1-0 -- /bin/bash
bash-4.2$ ps -eaf |grep postgres
postgres     1     0  0 Jun20 ?        00:00:00 postgres -D /app/db/postgres/data001/pgdev1 -c config_file=/app/bin/postgres/bin/config/pgdev1/pgdev1_postgresql.conf
postgres    28     1  0 Jun20 ?        00:00:00 postgres: pgdev1: logger process   
postgres    30     1  0 Jun20 ?        00:00:00 postgres: pgdev1: checkpointer process   
postgres    31     1  0 Jun20 ?        00:00:00 postgres: pgdev1: writer process   
postgres    32     1  0 Jun20 ?        00:00:00 postgres: pgdev1: wal writer process   
postgres    33     1  0 Jun20 ?        00:00:00 postgres: pgdev1: autovacuum launcher process   
postgres    34     1  0 Jun20 ?        00:00:00 postgres: pgdev1: archiver process   
postgres    35     1  0 Jun20 ?        00:00:00 postgres: pgdev1: stats collector process   
postgres    36     1  0 Jun20 ?        00:00:00 postgres: pgdev1: bgworker: logical replication launcher   
postgres  1148     0  0 06:56 ?        00:00:00 /bin/bash
postgres  1152  1148  0 06:56 ?        00:00:00 ps -eaf
postgres  1153  1148  0 06:56 ?        00:00:00 grep postgres


