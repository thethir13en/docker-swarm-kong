# **Docker Swarm and docker-compose Configuration for Kong API Gateway**
Данная конфигурация позволяет быстро развернуть **Kong API Gateway** и **Konga Dashboard**, при помощи оркестраторов **Docker Swarm** и **docker-compose**.

# Характеристики
* Различные конфигурации для запуска локально, с логгированием в **Graylog** и STDOUT
* Присутствует контейнер осуществляющий миграции в БД
* **Konga Dashboard** присутствует по-умолчанию
* Использует официальные образы **Kong**, **MongoDB**, **PostgreSQL**
* Чувствительные данные передаются переменными
* Персистентные данные используют мехнизм ```volumes``` **Docker**
* Использует **Gitlab CI** для быстрого развертывания (пример в репозитории)
* Присутствует **cAdvisor** для отслеживания метрик контейнеров

# Требования
Для запуска стека необходимы:
* **Docker**
* кластер **Docker Swarm**, он может состоять из 1 ноды
* (опционально) **Graylog** - по-умолчанию сервисы шлют туда логи

Для локального запуска стека необходимы:
* **Docker**
* **docker-compose**

# Список сервисов:
* **Kong**
* **Kong-migrations** (Инстанс сервиса **Kong**, выполняющий исключительно миграции)
* **PostgreSQL**
* **Konga**
* **MongoDB**
* **cAdvisor**

# Конфигурация
Для настройки параметров необходимо создать переменные окружения перед развертыванием стека:

* **KONG_REPLICAS** - количество реплик контейнера с сервисом **Kong** (только Docker Swarm)
* **POSTGRES_USER** - пользователь **PostgreSQL**, используется Kong для авторизации в БД
* **POSTGRES_DB** - база **PostgreSQL**, используется Kong для хранения конфигурации
* **GRAYLOG_ADDR** - адрес **Graylog** для отправки логов сервисов
* **GRAYLOG_PORT** - порт **Graylog** для отправки логов сервисов
* **ENVIRONMENT** (опционально) - окружение развертывания, значение передается в **Konga Dashboard**
* **KONGA_TOKEN_SECRET** (опционально) - JWT токен для **Konga Dashboard**

Для конфигурации локального развертывания используйте файл ```.env```, пример есть в репозитории

# Запуск

> Важно! При первом запуске происходят миграции в базу данных PostgreSQL, в следствие чего Kong не будет доступен порядка ~30 секунд. Когда контейнер migrations остановится можно будет пользоваться Kong.

На мастер-ноде, в кластере **Docker Swarm** выполните команду, для запуска стека с логгированием в Graylog:
```
docker stack deploy -c docker-compose.yml ${НАЗВАНИЕ_ВАШЕГО_СТЕКА}
```
Для запуска стека с выводом логов в STDOUT выполните команду:
```
docker stack deploy -c docker-compose-stdout.yml ${НАЗВАНИЕ_ВАШЕГО_СТЕКА}
```
Для локального запуска (без кластера Docker Swarm) выполните команду:
```
docker-compose -f docker-compose-stdout.yml up -d
```
# Использование

* По-умолчанию **Konga Dashboard** запускается на порту http://localhost:1337

# To-do

* Улучшеное параметризирование
* Резервное копирование ```volumes``` с конфигурациями

# Ссылки

* [Документация Docker](https://docs.docker.com/)

* [Документация Docker Swarm](https://docs.docker.com/engine/swarm/)

* [Установка docker-compose](https://docs.docker.com/compose/install/)

* [Документация Kong](https://docs.konghq.com/)

* [GitHub Konga Dashboard](https://github.com/pantsel/konga)
