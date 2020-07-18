FROM redis:5.0-alpine
#COPY redis.conf /usr/local/etc/redis/redis.conf
ENTRYPOINT ["docker-entrypoint.sh"]

CMD [ "redis-server", "/data/redis.conf" ]