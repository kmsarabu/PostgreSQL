Dockerfile for PostgreSQL on CentOS 7

Takes PostgreSQL version as an argument & builds the docker image w/ following options. 

	--with-perl 
	--with-python 
	--with-tcl 
	--with-gssapi 
	--with-openssl 
	--with-pam 
	--with-ldap 
	--with-uuid=ossp 
	--with-libxml 
	--with-libxslt 
	--with-segsize=4 
	--with-wal-segsize=256

Please modify Dockerfile to expose the port if needed and/or to make any other changes. 

Steps:
-----

  $ git clone https://github.com/kmsarabu/Docker.git

  ## to build PostgreSQL 11beta1:
  $ docker build -t local/postgresql:11beta1 --build-arg PGVERSION=11beta1 .

  ## to build PostgreSQL v10.4:
  $ docker build -t local/postgresql:10.4 --build-arg PGVERSION=10.4 .

  ## Run the Docker Image
  #### creates a testdb PostgreSQL Cluster with version 11beta1 on port 4556

  $ docker run -e PGSID=testdb -e PGPORT=4556 -e PGPASSWORD=krishna123 local/postgresql:11beta1

  $ docker ps |egrep postgres

  c8e56302d433 docker.vmntech.com:5000/postgresql:11beta1   "/app/bin/dbe/postg..."   53 seconds ago      Up 52 seconds  elegant_clarke

  $ docker exec -it c8e56302d433 /bin/bash
  bash-4.2$ psql
  psql (11beta1)
  Type "help" for help.
  postgres=# \q
  bash-4.2$ echo $PGHOME
  /app/bin/postgres/product/server/11beta1
