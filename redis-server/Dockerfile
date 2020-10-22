# a customized version of https://hub.docker.com/r/redislabs/redismod/dockerfile

FROM redislabs/redisearch:latest as redisearch
FROM redislabs/redisgraph:latest as redisgraph
FROM redislabs/redisgears:latest

ENV LD_LIBRARY_PATH /usr/lib/redis/modules
ENV REDISGRAPH_DEPS libgomp1

WORKDIR /data
RUN set -ex; \
    apt-get update; \
    apt-get install -y --no-install-recommends ${REDISGRAPH_DEPS};

COPY --from=redisearch ${LD_LIBRARY_PATH}/redisearch.so ${LD_LIBRARY_PATH}/
COPY --from=redisgraph ${LD_LIBRARY_PATH}/redisgraph.so ${LD_LIBRARY_PATH}/

# ENV PYTHONPATH /usr/lib/redis/modules/deps/cpython/Lib
ENTRYPOINT ["redis-server"]
CMD ["--loadmodule", "/usr/lib/redis/modules/redisearch.so", \
    "--loadmodule", "/usr/lib/redis/modules/redisgraph.so", \
    "--loadmodule", "/var/opt/redislabs/lib/modules/redisgears.so", \
    "PythonHomeDir", "/opt/redislabs/lib/modules/python3", \
    "--appendonly", "yes" ]
