Порядок инициализации шардирования:
1. Указание в compose-файле 16 различных контейнеров:

    Конфигуратор1,
    Конфигуратор2,
    Конфигуратор3,
    Роутер,
    Шард1a,
    Шард2a,
    Шард1b,
    Шард2b,
    Шард1c,
    Шард2c,
    Redis1,
    Redis2,
    Redis3,
    Redis4,
    Redis5,
    Redis6
2. Инициализация серверов конфигурации mongo
rs.initiate(
  {
    _id : "config_server1",
       configsvr: true,
    members: [
      { _id : 0, host : "configSrv:27010" }
    ]
  }
);

rs.initiate(
  {
    _id : "config_server2",
       configsvr: true,
    members: [
      { _id : 0, host : "configSrv:27011" }
    ]
  }
);

rs.initiate(
  {
    _id : "config_server3",
       configsvr: true,
    members: [
      { _id : 0, host : "configSrv:27012" }
    ]
  }
);

3. Инициализация шардов для каждой реплики mongo.

rs.initiate(
    {
      _id : "shard1a",
      members: [
        { _id : 0, host : "shard1:27013" },
      ]
    }
);

rs.initiate(
    {
      _id : "shard1b",
      members: [
        { _id : 0, host : "shard1:27014" },
      ]
    }
);

rs.initiate(
    {
      _id : "shard2a",
      members: [
        { _id : 0, host : "shard1:27015" },
      ]
    }
);

rs.initiate(
    {
      _id : "shard2b",
      members: [
        { _id : 0, host : "shard1:27016" },
      ]
    }
);

rs.initiate(
    {
      _id : "shard3a",
      members: [
        { _id : 0, host : "shard1:27017" },
      ]
    }
);

rs.initiate(
    {
      _id : "shard3b",
      members: [
        { _id : 0, host : "shard1:27018" },
      ]
    }
);
4. Инициализация роутера mongo

sh.addShard( "shard1a/shard1a:27013");

sh.addShard( "shard1b/shard1b:27014");

sh.addShard( "shard2a/shard2a:27015");

sh.addShard( "shard2b/shard2b:27016");

sh.addShard( "shard3a/shard3a:27017");

sh.addShard( "shard3b/shard3b:27018");

sh.enableSharding("somedb");

sh.shardCollection("somedb.helloDoc", { "name" : "hashed" } )
5. Инициализация кластера Redis:

    docker exec -it redis_1

    echo "yes" | redis-cli --cluster create   173.17.0.21:6379   173.17.0.22:6379   173.17.0.23:6379   173.17.0.24:6379   173.17.0.25:6379   173.17.0.26:6379   --cluster-replicas 1 