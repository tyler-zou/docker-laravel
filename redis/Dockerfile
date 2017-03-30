FROM daocloud.io/daocloud/dao-redis:master-init

MAINTAINER Tyler <zzxaire@163.com>

COPY redis.conf /usr/local/etc/redis/redis.conf

VOLUME /data

EXPOSE 6379

CMD ["redis-server"]
