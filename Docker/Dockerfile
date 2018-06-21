FROM centos:centos7
ARG PGVERSION

ENV     PGSID=${PGSID:-kridb}                                   \
        PGPORT=${PGPORT:-5430}                                  \
	PGBASE=/app/bin/postgres 				\
	PGVERSION=${PGVERSION:-10.4}				\
        PGHOME=/app/bin/postgres/product/server/${PGVERSION}     \
	PSYCOPG2=$PGHOME/psycopg2 				\
	PG_CONFIG=$PGHOME/bin/pg_config 			\
        PATH=${PATH}:/app/bin/scripts/postgres:${PGHOME}/bin         \
        PGDATA=/app/db/postgres/data001/${PGSID}                 \
        PGDATABASE=${PGDATABASE:-postgres}                      \
        PGUSER=${PGUSER:-postgres}                              \
        PGPASSWORD=${PGPASSWORD:-krishna}

RUN mkdir -p /app/tmp

ADD https://ftp.postgresql.org/pub/source/v${PGVERSION}/postgresql-${PGVERSION}.tar.gz /app/tmp/postgresql-${PGVERSION}.tar.gz
ADD https://github.com/psycopg/psycopg2/archive/2_7_4.tar.gz /app/tmp/psycopg2_2.7.4.tar.gz
ADD https://github.com/2ndQuadrant/repmgr/archive/v4.0.5.tar.gz /app/tmp/repmgr_4.0.5.tar.gz

RUN yum -y install ksh libevent libevent-devel tcl tcl-devel uuid uuid-devel openldap openldap-devel libxslt libxslt-devel libxml2 libxml2-devel pam pam-devel openssl openssl-devel krb5-devel krb5-libs zip zip-devel perl-ExtUtils-Embed perl-ExtUtils-MakeMaker perl-ExtUtils-Install gcc libgcc gcc-c++ readline readline-devel python-devel sudo

RUN 	mkdir -p /app/bin/scripts/postgres	\
	&& mkdir -p /app/bin/postgres/config \
	&& mkdir -p /app/data/postgres/logs \
	&& groupadd -g 101 dba \
	&& useradd -s /bin/ksh -d /app/bin/postgres -G dba -p postgres -u 101 postgres \
	&& echo "postgres ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/postgres \
	&& chown -hR postgres:dba /app/bin/postgres \
	&& chown -hR postgres:dba /app/data/postgres \
	&& chown -hR postgres:dba /app/bin/scripts/postgres \
	&& su - postgres \
	&& cd /app/tmp \
	&& tar -xzvf postgresql-${PGVERSION}.tar.gz \
	&& cd postgresql-${PGVERSION} \
	&& ./configure --prefix=/app/bin/postgres/product/server/${PGVERSION} --with-perl --with-python --with-tcl --with-gssapi --with-openssl --with-pam --with-ldap --with-uuid=ossp --with-libxml --with-libxslt --with-segsize=4 --with-wal-segsize=256 \
	&& make world \
	&& make install-world \
	&& cd /app/tmp \
	&& tar -xzvf psycopg2_2.7.4.tar.gz \
	&& cd psycopg2-2_7_4 \
	&& python setup.py build_ext --pg-config /app/bin/postgres/product/server/${PGVERSION}/bin/pg_config install --install-base /app/bin/postgres/product/server/${PGVERSION}/psycopg2 --install-platbase /app/bin/postgres/product/server/$PGVERSION/psycopg2 --install-purelib /app/bin/postgres/product/server/$PGVERSION/psycopg2 --install-platlib /app/bin/postgres/product/server/$PGVERSION/psycopg2 --install-lib /app/bin/postgres/product/server/$PGVERSION/psycopg2 --install-headers /app/bin/postgres/product/server/$PGVERSION/psycopg2 --install-scripts /app/bin/postgres/product/server/$PGVERSION/psycopg2 --install-data /app/bin/postgres/product/server/$PGVERSION/psycopg2 \
	&& cd /app/tmp \
	&& tar -xzvf repmgr_4.0.5.tar.gz \
	&& cd repmgr-4.0.5 \
	&& ./configure --prefix=/app/bin/postgres/product/server/$PGVERSION PG_CONFIG=/app/bin/postgres/product/server/$PGVERSION/bin/pg_config \
	&& make \
	&& make install \
	&& chown -hR postgres:dba /app/bin/postgres

RUN     sudo mkdir -p /app/bin/scripts/postgres \
        && sudo mkdir -p /app/bin/postgres/bin/config \
        && sudo mkdir -p /app/db/postgres/data001 \
        && sudo chown -hR postgres:dba /app/db/postgres/data001 \
        && sudo chown -hR postgres:dba /app/bin/scripts/postgres \
        && sudo chown -hR postgres:dba /app/bin/postgres/bin/config

COPY    scripts/* /app/bin/scripts/postgres/

USER 101

ENTRYPOINT ["/app/bin/scripts/postgres/entry_db"]
CMD ["/app/bin/scripts/postgres/cr_start_db"]

