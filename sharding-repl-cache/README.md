Порядок инициализации шардирования:
1. Поднятие контейнеров:

docker-compose up -d

2. Инициализация сервера конфигурации

docker-compose exec configsvr01 sh -c "mongosh < /scripts/init-configserver.js"

3. Инициализация шардов

docker-compose exec shard01-a sh -c "mongosh < /scripts/init-shard01.js"

docker-compose exec shard02-a sh -c "mongosh < /scripts/init-shard02.js"

4. Инициализация роутера.
docker-compose exec router01 sh -c "mongosh < /scripts/init-router.js"

5. Инициализация кластера Redis:

docker exec -it redis_1 sh -c 'echo "yes" | redis-cli --cluster create 173.17.0.21:6379 173.17.0.22:6379 173.17.0.23:6379 173.17.0.24:6379 173.17.0.25:6379 173.17.0.26:6379 --cluster-replicas 1'
