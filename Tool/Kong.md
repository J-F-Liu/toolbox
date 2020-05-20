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

docker run --rm -it --name kong \
     -e "KONG_DATABASE=off" \
     -e "KONG_DECLARATIVE_CONFIG=/etc/kong/kong.yml" \
     -e "KONG_NGINX_HTTP_INCLUDE=/etc/kong/static-files.nginx.conf" \
     -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
     -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
     -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
     -p 8000:8000 \
     -p 8443:8443 \
     -p 8001:8001 \
     -p 8444:8444 \
     -v /Users/Junfeng/ZhimoProjects/school-crm/kong-config:/etc/kong \
     kong:1.2

docker run -d --name kong-dashboard \
    --network=kong-net \
    -p 8088:8080 \
    pgbi/kong-dashboard start --kong-url http://172.18.0.3:8001
```

```
npm install -g kong-dashboard
kong-dashboard start --kong-url http://localhost:8001 --port 8088
```

```
kong reload
kong restart # need to restart after plugins configuration is changed
```

## Configure

Service entities are abstractions of each of your own upstream APIs and microservices.

Route entities define rules to match client requests. Every request matching a given Route will be proxied to its associated Service.

Each Route is associated with a Service, and a single Service can have many Routes.

Plugins allow you to easily add new features to your API or make your API easier to manage.

Consumers are associated to individuals or applications using your API. They can be used for tracking, access management, and more.

The upstream object represents a virtual hostname and can be used to loadbalance incoming requests over multiple services (targets).

## Monitor

```
curl http://localhost:8001/plugins -d name=prometheus
curl http://localhost:8001/metrics
docker run -d -p 3000:3000 grafana/grafana
docker run -d -p 8056:9090 -v ~/.config/prometheus.yml:/etc/prometheus/prometheus.yml --name prometheus prom/prometheus
```

```prometheus.yml
global:
  scrape_interval:     15s
  evaluation_interval: 15s

rule_files:
  # - "first.rules"
  # - "second.rules"

scrape_configs:
  - job_name: kong_prometheus
    static_configs:
      - targets: ['localhost:8001']
```

[Grafana Kong Dashboard](https://grafana.com/dashboards/7424)
