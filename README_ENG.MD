# **Docker Swarm and docker-compose configuration for Kong API Gateway**
Configuration for easy deployment **Kong API Gateway** and **Konga Dashboard**, with **Docker Swarm** or **docker-compose**.

# Properties
* Includes configurations for local deployment, with logging to **Graylog** or STDOUT
* Automatic migrations with Kong-migrations service
* **Konga Dashboard** used by default
* Uses official **Kong**, **MongoDB** and **PostgreSQL** images
* Sensitive data interpolation with environment variables
* Persistence data with ```volumes``` **Docker** mechanism
* Example of **Gitlab CI** configuration for deployment
* **cAdvisor** provided for monitoring containers

# Requirements
Required for launch in Swarm cluster:
* **Docker**
* **Docker Swarm** cluster
* (optional) **Graylog** - by default services send theirl logs to Graylog

Requred for local deployment:
* **Docker**
* **docker-compose**

# List of services:
* **Kong**
* **Kong-migrations** (Copy of **Kong**, applies migrations)
* **PostgreSQL**
* **Konga**
* **MongoDB**
* **cAdvisor**

# Configuration
For configuring service it needs to set environment variables before deploy stack:

* **KONG_REPLICAS** - number of replicas **Kong** (Docker Swarm only)
* **POSTGRES_USER** - **PostgreSQL** user for Kong
* **POSTGRES_DB** - **PostgreSQL** db for Kong
* **GRAYLOG_ADDR** - **Graylog** URL for send logs
* **GRAYLOG_PORT** - **Graylog** PORT for send logs (UDP by default)
* **ENVIRONMENT** (optional) - environment, passes to **Konga Dashboard**
* **KONGA_TOKEN_SECRET** (optional) - JWT token for **Konga Dashboard**

For configure local deployment use ```.env``` file, example exists in repo.

# Launch

> Important! On the first launch Kong appies migrations to PostgreSQL, so Kong will be in restarting state ~30 second. After Kong-migrations container will be stopped and Kong container is up - Kong will be ready to use.

On manager node of **Docker Swarm** cluster use command for launch stack with logging to Graylog:
```
docker stack deploy -c docker-compose.yml ${STACK_NAME}
```
On manager node of **Docker Swarm** cluster use command for launch stack with logging to STDOUT:
```
docker stack deploy -c docker-compose-stdout.yml ${STACK_NAME}
```
For local deployment use command:
```
docker-compose -f docker-compose-stdout.yml up -d
```
# Usage

* By default **Konga Dashboard** runs on http://localhost:1337

# To-do

* Improve parametrizing
* Backup ```volumes``` with configuration

# Useful links

* [Docker docs](https://docs.docker.com/)

* [Docker Swarm docs](https://docs.docker.com/engine/swarm/)

* [Docker-compose install](https://docs.docker.com/compose/install/)

* [Kong docs](https://docs.konghq.com/)

* [GitHub Konga Dashboard](https://github.com/pantsel/konga)
