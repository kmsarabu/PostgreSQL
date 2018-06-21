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

