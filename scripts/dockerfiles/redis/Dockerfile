FROM cgr.dev/chainguard/wolfi-base

# Install Redis 7.x (latest available major version in Wolfi)
# Install tini as init process to reap zombie processes from healthchecks
RUN apk add --no-cache 'redis>=7' 'redis<8' redis-cli bash tini && \
  mkdir -p /data && \
  chown -R 1001:1001 /data

ENV REDIS_DATA_DIR="/data" \
  REDIS_REPLICATION_MODE="master" \
  ALLOW_EMPTY_PASSWORD="yes" \
  REDIS_TLS_ENABLED="no" \
  REDIS_PORT="6379"

USER 1001

WORKDIR /data

COPY --chown=1001:1001 entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh

# Use tini as init to properly reap zombie processes from healthcheck commands
ENTRYPOINT ["/sbin/tini", "--", "/usr/local/bin/entrypoint.sh"]
