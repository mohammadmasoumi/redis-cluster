# redis-cluster
Redis cluster with single master and sentinel nodes


## Deployment

`docker-compose.yml` 

```docker
services:
  redis-sentinel:
    image: bitnami/redis-sentinel:latest
    ports:
      - '26379-26381:26379'
    environment:
      - REDIS_MASTER_HOST=redis-master
      - REDIS_MASTER_PASSWORD=str0ng_passw0rd
    networks:
      - redis-network
    depends_on:
      - redis-master
    restart: on-failure:5

  redis-master:
    image: bitnami/redis:latest
    container_name: redis-master
    ports:
      - '6379'
    environment:
      - REDIS_REPLICATION_MODE=master
      - REDIS_PASSWORD=str0ng_passw0rd
    networks:
      - redis-network
    restart: on-failure:5

  redis-replica-1:
    image: bitnami/redis:latest
    container_name: redis-replica-1
    ports:
      - '6379'
    environment:
      - REDIS_REPLICATION_MODE=slave
      - REDIS_MASTER_HOST=redis-master
      - REDIS_MASTER_PASSWORD=str0ng_passw0rd
      - REDIS_PASSWORD=str0ng_passw0rd
    volumes:
      - redis-replica-1-data:/var/lib/redis
      - ./configs/redis-replica-1.conf:/usr/local/etc/redis/redis.conf
    networks:
      - redis-network
    depends_on:
      - redis-master
    restart: on-failure:5

  redis-replica-2:
    image: bitnami/redis:latest
    container_name: redis-replica-2
    ports:
      - '6379'
    environment:
      - REDIS_REPLICATION_MODE=slave
      - REDIS_MASTER_HOST=redis-master
      - REDIS_MASTER_PASSWORD=str0ng_passw0rd
      - REDIS_PASSWORD=str0ng_passw0rd
    volumes:
      - redis-replica-2-data:/var/lib/redis
      - ./configs/redis-replica-2.conf:/usr/local/etc/redis/redis.conf
    networks:
      - redis-network
    depends_on:
      - redis-master
    restart: on-failure:5

  redis-replica-3:
    image: bitnami/redis:latest
    container_name: redis-replica-3
    ports:
      - '6379'
    environment:
      - REDIS_REPLICATION_MODE=slave
      - REDIS_MASTER_HOST=redis-master
      - REDIS_MASTER_PASSWORD=str0ng_passw0rd
      - REDIS_PASSWORD=str0ng_passw0rd
    volumes:
      - redis-replica-3-data:/var/lib/redis
      - ./configs/redis-replica-3.conf:/usr/local/etc/redis/redis.conf
    networks:
      - redis-network
    depends_on:
      - redis-master
    restart: on-failure:5

networks:
  redis-network:
    driver: bridge
  
volumes:
  redis-persistence: null
  redis-replica-1-data: null
  redis-replica-2-data: null
  redis-replica-3-data: null

```