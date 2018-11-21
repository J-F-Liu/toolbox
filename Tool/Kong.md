# Kong

https://konghq.com/

Kong is an open source, fast, scalable, and distributed Microservice Abstraction Layer (also known as a API Gateway, or API Middleware). Kong runs in front of any RESTful API and provide functionalities and services such as requests routing, authentication, rate limiting, etc.

## Install
```
docker network create kong-net

docker run -d --name kong-database \
               --network=kong-net \
               -p 5432:5432 \
               -e "POSTGRES_USER=kong" \
               -e "POSTGRES_DB=kong" \
               postgres:9.6

docker run --rm \
     --network=kong-net \
     -e "KONG_DATABASE=postgres" \
     -e "KONG_PG_HOST=kong-database" \
     kong:latest kong migrations up


docker run -d --name kong \
     --network=kong-net \
     -e "KONG_DATABASE=postgres" \
     -e "KONG_PG_HOST=kong-database" \
     -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
     -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
     -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
     -p 8000:8000 \
     -p 8443:8443 \
     -p 8001:8001 \
     -p 8444:8444 \
     kong:latest

docker run -d --name kong-dashboard \
    --network=kong-net \
    -p 8088:8080 \
    pgbi/kong-dashboard start --kong-url http://172.18.0.3:8001
```

## Configure

Service entities are abstractions of each of your own upstream APIs and microservices.

Route entities define rules to match client requests. Every request matching a given Route will be proxied to its associated Service.

Each Route is associated with a Service, and a single Service can have many Routes.

Plugins allow you to easily add new features to your API or make your API easier to manage.

Consumers are associated to individuals or applications using your API. They can be used for tracking, access management, and more.
