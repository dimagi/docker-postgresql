FROM postgres:9.6

RUN apt-get update && apt-get install -y --no-install-recommends --no-upgrade \
    gcc \
    make \
    postgresql-$PG_MAJOR-plproxy \
    postgresql-server-dev-$PG_MAJOR \
    python-docutils \
    unzip \
    wget

RUN sed -i 's/$/ 9.4/' /etc/apt/sources.list.d/pgdg.list

RUN apt-get update && apt-get install -y --no-install-recommends --no-upgrade \
		postgresql-9.4=9.4.17-1.pgdg80+1 \
		postgresql-contrib-9.4=9.4.17-1.pgdg80+1 \
    postgresql-9.4-plproxy \
    postgresql-server-dev-9.4 \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN wget --quiet --no-check-certificate https://github.com/markokr/pghashlib/archive/master.zip -O pghashlib.zip \
  && unzip pghashlib.zip \
  && cd pghashlib-master \
  && PG_CONFIG=/usr/lib/postgresql/$PG_MAJOR/bin/pg_config make \
  && PG_CONFIG=/usr/lib/postgresql/$PG_MAJOR/bin/pg_config make install \
  && PG_CONFIG=/usr/lib/postgresql/9.4/bin/pg_config make \
  && PG_CONFIG=/usr/lib/postgresql/9.4/bin/pg_config make install \
  && ldconfig

RUN sed -ri "s/^#?(shared_preload_libraries\s*=\s*)\S+/\1'pg_stat_statements'/" /usr/share/postgresql/$PG_MAJOR/postgresql.conf.sample
RUN sed -ri "s/^#?(shared_preload_libraries\s*=\s*)\S+/\1'pg_stat_statements'/" /usr/share/postgresql/9.4/postgresql.conf.sample

ENV PGBINOLD /usr/lib/postgresql/9.4/bin
ENV PGBINNEW /usr/lib/postgresql/9.6/bin

ENV PGDATAOLD /var/lib/postgresql/9.4/data
ENV PGDATANEW /var/lib/postgresql/9.6/data

RUN mkdir -p "$PGDATAOLD" "$PGDATANEW" \
	&& chown -R postgres:postgres /var/lib/postgresql

WORKDIR /var/lib/postgresql

COPY docker-upgrade /usr/local/bin/

ENTRYPOINT ["docker-upgrade"]

# recommended: --link
CMD ["pg_upgrade"]
