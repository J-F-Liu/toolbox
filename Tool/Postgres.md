# Postgres Database

### Install

```
docker pull postgres
docker run --name some-postgres \
     -p 5432:5432 \
    -e POSTGRES_USER=postgres \
    -e POSTGRES_PASSWORD=mysecretpassword \
    -d postgres

docker pull dpage/pgadmin4
docker run -p 5433:80 \
    -e "PGADMIN_DEFAULT_EMAIL=user@domain.com" \
    -e "PGADMIN_DEFAULT_PASSWORD=SuperSecret" \
    -d dpage/pgadmin4
```

mysqldump --compatible=postgresql --default-character-set=utf8 -r DATABASE.mysql -u MYSQL_USER -p -h MYSQL_HOST DATABASE

docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d -p 3306:3306 mysql:5.7
mysql -u root -p -h localhost --protocol=tcp libra < libra_prod_20190827.sql

docker run --security-opt seccomp=unconfined --rm dimitri/pgloader pgloader mysql://lkltest@10.10.0.85/libra postgresql://postgres:mysecretpassword@10.10.0.85/libra

psql -d libra -f libra_prod_20190827.sql -U postgres -W -h localhost -p 5432
