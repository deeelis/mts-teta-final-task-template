# MTS Teta Final Task Template

Для запуска требуется [PostgreSQL](https://www.postgresql.org/)
и [Clickhouse](https://clickhouse.com/).

Docker-команды:

```shell
# Postgres
docker run --name mts-teta-postgres -e POSTGRES_PASSWORD=password -e POSTGRES_USER=user -e POSTGRES_DB=mts-teta-database -p 5432:5432 -d postgres

# Clickhouse
docker run --name mts-teta-clickhouse -e CLICKHOUSE_DB=db -e CLICKHOUSE_USER=username -e CLICKHOUSE_PASSWORD=password -p 8123:8123 -d yandex/clickhouse-server
```

Либо можно воспользоваться конфигурацией для [Docker Compose](https://docs.docker.com/compose/)
. [Файл приложен в репозитории](docker-compose.yml). Для запуска достаточно выполнить
команду `docker-compose up`, находясь в той же директории, где и файл `docker-compose.yml`

К Postgres можно цепляться через [DBeaver](https://dbeaver.io/). К Clickhouse тоже, но также
доступен web-интерфейс в браузере по ссылке [http://localhost:8123/play](http://localhost:8123/play)
. Обратите внимание, что в web-интерфейсе Clickhouse в правом верхнем углу нужно вписать логин и
пароль, который вы передавали в качестве переменных окружения (`CLICKHOUSE_USER`
и `CLICKHOUSE_PASSWORD`) при старте Docker-контейнера. Если у вас есть IDEA Ultimate, то и к
Clickhouse, и к PostgreSQL можно цепляться прямо из IDE (
вкладка [Databases](https://www.jetbrains.com/help/idea/database-tool-window.html)).

Важный момент относительно PostgreSQL. По умолчанию DBeaver цепляется к БД, которая
называется `postgres` (она создается автоматически). Но при старте Docker-контейнера мы явно говорим
о том, что нужно дополнительно создать БД с
названием `mts-teta-database` (`POSTGRES_DB=mts-teta-database`). Так что все таблицы приложение при
старте создаст именно в `mts-teta-database`.

Чтобы посмотреть, какие данные записались в Clickhouse, сделайте запрос:

```sql
SELECT *
FROM db.event
```

Если вы выполняете Docker-команды по умолчанию, не меняя пользователей, названия баз и порты, то
достаточно просто запустить приложение через [main](src/main/java/com/mts/teta/DemoApplication.java)
. Иначе же нужно также предварительно поправить конфиги
в [application.properties](src/main/resources/application.properties).

Для запуска требуется Java 17. 
